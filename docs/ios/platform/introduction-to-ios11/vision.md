---
title: Vision Framework en Xamarin. iOS
description: En este documento se describe cómo usar el marco de trabajo de iOS 11 Vision en Xamarin. iOS. En concreto, se describe la detección de rectángulos y la detección de caras.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/31/2017
ms.openlocfilehash: b58e7b1fffed3253d9765401d52f16b751db134d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032069"
---
# <a name="vision-framework-in-xamarinios"></a>Vision Framework en Xamarin. iOS

Vision Framework agrega una serie de nuevas características de procesamiento de imágenes a iOS 11, entre las que se incluyen:

- [Detección de rectángulos](#rectangles)
- [Detección de caras](#faces)
- Machine Learning el análisis de imágenes (se explica en [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Detección de códigos de barras
- Análisis de alineación de imagen
- Detección de texto
- Detección de horizonte
- Seguimiento de & de detección de objetos

![Fotografía con tres rectángulos detectados](vision-images/found-rectangles-tiny.png) ![Fotografía con dos caras detectadas](vision-images/xamarin-home-faces-tiny.png)

La detección y el Detección de caras de los rectángulos se describen con más detalle a continuación.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Detección de rectángulos

En el [ejemplo VisionRects](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles) se muestra cómo procesar una imagen y dibujar los rectángulos detectados en ella.

### <a name="1-initialize-the-vision-request"></a>1. inicializar la solicitud de visión

En `ViewDidLoad`, cree un `VNDetectRectanglesRequest` que haga referencia al método `HandleRectangles` que se llamará al final de cada solicitud:

También se debe establecer la propiedad `MaximumObservations`; de lo contrario, el valor predeterminado será 1 y solo se devolverá un resultado único.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. iniciar el procesamiento de la visión

El código siguiente inicia el procesamiento de la solicitud. En el ejemplo **VisionRects** , este código se ejecuta después de que el usuario haya seleccionado una imagen:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Este controlador pasa el `ciImage` al `VNDetectRectanglesRequest` de Vision Framework que se creó en el paso 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. controlar los resultados del procesamiento de la visión

Una vez completada la detección de rectángulos, el marco de trabajo ejecuta el método `HandleRectangles`, cuyo resumen se muestra a continuación:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Mostrar los resultados

El método `OverlayRectangles` del ejemplo **VisionRectangles** tiene tres funciones:

- Representación de la imagen de origen,
- Dibujar un rectángulo para indicar dónde se ha detectado cada uno de ellos y
- Agregar una etiqueta de texto para cada rectángulo mediante CoreGraphics.

Vea el [origen del ejemplo](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles) para el método CoreGraphics exacto.

![Fotografía con tres rectángulos detectados](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. procesamiento adicional

La detección de rectángulos suele ser solo el primer paso en una cadena de operaciones, como en [este ejemplo de CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), donde los rectángulos se pasan a un modelo de CoreML para analizar los dígitos escritos a mano.

<a name="faces" />

## <a name="face-detection"></a>Detección de caras

El [ejemplo VisionFaces](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces) funciona de forma similar al ejemplo **VisionRectangles** , mediante una clase de solicitud de visión diferente.

### <a name="1-initialize-the-vision-request"></a>1. inicializar la solicitud de visión

En `ViewDidLoad`, cree un `VNDetectFaceRectanglesRequest` que haga referencia al método `HandleRectangles` que se llamará al final de cada solicitud.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. iniciar el procesamiento de la visión

El código siguiente inicia el procesamiento de la solicitud. En el ejemplo **VisionFaces** , este código se ejecuta después de que el usuario haya seleccionado una imagen:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Este controlador pasa el `ciImage` al `VNDetectFaceRectanglesRequest` de Vision Framework que se creó en el paso 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. controlar los resultados del procesamiento de la visión

Una vez completada la detección de rostros, el controlador ejecuta el método `HandleRectangles` que realiza el control de errores y muestra los límites de los rostros detectados y llama al `OverlayRectangles` para dibujar los rectángulos delimitadores en la imagen original:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Mostrar los resultados

El método `OverlayRectangles` del ejemplo **VisionFaces** tiene tres funciones:

- Representación de la imagen de origen,
- Dibujar un rectángulo para cada una de las caras detectadas
- Agregar una etiqueta de texto para cada una de ellas mediante CoreGraphics.

Vea el [origen del ejemplo](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces) para el método CoreGraphics exacto.

![Fotografía con dos caras detectadas](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. procesamiento adicional

Vision Framework incluye funcionalidades adicionales para detectar características faciales, como los ojos y la boca. Use el tipo de `VNDetectFaceLandmarksRequest`, que devolverá `VNFaceObservation` resultados como en el paso 3 anterior, pero con datos de `VNFaceLandmark` adicionales.

## <a name="related-links"></a>Vínculos relacionados

- [Rectángulos de visión (ejemplo)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles)
- [Caras de visión (ejemplo)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces)
- [Avances en los filtros de imagen principal, metal, visión y mucho más (WWDC) (vídeo)](https://developer.apple.com/videos/play/wwdc2017/510/)
