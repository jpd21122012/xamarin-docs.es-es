---
title: Core ML 2 en Xamarin. iOS
description: En este documento se describen las actualizaciones de Core ML disponibles como parte de iOS 12. En concreto, se examinan las mejoras de rendimiento asociadas a la nueva API de predicción por lotes.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/15/2018
ms.openlocfilehash: 6245873385caa23e37d5499daa822fa0b699ac1e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032035"
---
# <a name="core-ml-2-in-xamarinios"></a>Core ML 2 en Xamarin. iOS

Core ML es una tecnología de aprendizaje automático disponible en iOS, macOS, tvOS y watchos. Permite a las aplicaciones realizar predicciones basadas en modelos de aprendizaje automático.

En iOS 12, Core ML incluye una API de procesamiento por lotes. Esta API hace que los ML principales sean más eficaces y proporciona mejoras de rendimiento en escenarios en los que un modelo se utiliza para realizar una secuencia de predicciones.

## <a name="sample-app-marshabitatcoremltimer"></a>Aplicación de ejemplo: MarsHabitatCoreMLTimer

Para mostrar las predicciones por lotes con Core ML, eche un vistazo a la aplicación de ejemplo [MarsHabitatCoreMLTimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer) . En este ejemplo se usa un modelo de ML básico entrenado para predecir el costo de la creación de un hábitat en Mars, basado en varias entradas: el número de paneles solares, el número de invernaderos y el número de acres.

Los fragmentos de código de este documento proceden de este ejemplo.

## <a name="generate-sample-data"></a>Generar datos de ejemplo

En `ViewController`, el método de `ViewDidLoad` de la aplicación de ejemplo llama a `LoadMLModel`, que carga el modelo de ML Core incluido:

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

A continuación, la aplicación de ejemplo crea 100.000 objetos `MarsHabitatPricerInput` que se usarán como entrada para las predicciones de ML básicas secuenciales. Cada ejemplo generado tiene un valor aleatorio establecido para el número de paneles solares, el número de invernaderos y el número de acres:

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

Al pulsar cualquiera de los tres botones de la aplicación se ejecutan dos secuencias de predicciones: una que usa un bucle de `for` y otra con el nuevo método de `GetPredictions` por lotes introducido en iOS 12:

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## <a name="for-loop"></a>for (bucle)

La versión de bucle `for` de la prueba recorre en iteración el número especificado de entradas, llamando [`GetPrediction`](xref:CoreML.MLModel.GetPrediction*) para cada una de ellas y descartando el resultado. El método multiplica el tiempo que se tarda en realizar las predicciones:

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="getpredictions-new-batch-api"></a>GetPredictions (nueva API de batch)

La versión de batch de la prueba crea un objeto `MLArrayBatchProvider` a partir de la matriz de entrada (ya que se trata de un parámetro de entrada necesario para el método `GetPredictions`), crea una [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
objeto que evita que los cálculos de predicción se limiten a la CPU y usa la API de `GetPredictions` para capturar las predicciones, descartando de nuevo el resultado:

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="results"></a>Resultados

En el simulador y el dispositivo, `GetPredictions` finaliza más rápidamente que las predicciones de ML principales basadas en bucle.

## <a name="related-links"></a>Vínculos relacionados

- [Aplicación de ejemplo: MarsHabitatCoreMLTimer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
- [Novedades de Core ML, parte 1 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [Novedades de Core ML, parte 2 (WWDC 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Introducción a Core ML en Xamarin. iOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [Core ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [Trabajar con modelos Core ML](https://developer.apple.com/machine-learning/build-run-models/)
