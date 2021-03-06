---
title: Diseño para tabletas y aplicaciones de escritorio
description: En este artículo se explica cómo optimizar los diseños de aplicaciones de Xamarin. Forms para tabletas, en lugar de teléfonos.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: f91d0127d0f2ffe37e3e0ff016dee551a679ad84
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72273112"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Diseño para tabletas y aplicaciones de escritorio

Xamarin. Forms admite todos los tipos de dispositivos disponibles en las plataformas admitidas, así que, además de los teléfonos, las aplicaciones también se pueden ejecutar en:

- iPad
- Tabletas Android,
- Tabletas y equipos de escritorio con Windows (que ejecutan Windows 10).

En esta página se describe brevemente:

- los [tipos de dispositivo](#Device_Types)admitidos y
- Cómo [optimizar](#optimize) los diseños para tabletas en comparación con los teléfonos.

<a name="Device_Types" />

## <a name="device-types"></a>Tipos de dispositivos

Los dispositivos de pantalla más grandes están disponibles para todas las plataformas compatibles con Xamarin. Forms.

### <a name="ipads-ios"></a>iPad (iOS)

La plantilla de Xamarin. Forms incluye automáticamente la compatibilidad con iPad mediante la configuración de la opción **info. plist > dispositivos** en **universal** (lo que significa que se admiten tanto iPhone como iPad).

Para proporcionar una experiencia de inicio agradable y asegurarse de que se utiliza la resolución de pantalla completa en todos los dispositivos, debe asegurarse de que se proporciona una [pantalla de inicio específica de iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (con un guion gráfico). Esto garantiza que la aplicación se represente correctamente en los dispositivos iPad mini, iPad y iPad Pro.

Antes de iOS 9, todas las aplicaciones requerían la pantalla completa en el dispositivo, pero algunos iPad ahora pueden realizar varias [tareas de pantalla dividida](~/ios/platform/multitasking.md).
Esto significa que la aplicación podría ocupar simplemente una columna delgada en el lateral de la pantalla, el 50% del ancho de la pantalla o toda la pantalla.

[![](tablet-images/ipad-sml.png "iPad Split Screen Example")](tablet-images/ipad.png#lightbox "iPad Split Screen Example")

La funcionalidad de pantalla dividida significa que debe diseñar la aplicación para que funcione bien con tan solo 320 píxeles de ancho o hasta 1366 píxeles de ancho.

### <a name="android-tablets"></a>Tabletas Android

El ecosistema de Android tiene una gran cantidad de tamaños de pantalla compatibles, desde pequeños teléfonos hasta grandes tabletas. Xamarin. Forms puede admitir todos los tamaños de pantalla, pero, al igual que con las otras plataformas, puede que desee ajustar la interfaz de usuario para dispositivos de mayor tamaño.

Cuando se admiten muchas resoluciones de pantalla diferentes, puede proporcionar los recursos de imagen nativa en distintos tamaños para optimizar la experiencia del usuario.
Revise la documentación de [los recursos de Android](~/android/app-fundamentals/resources-in-android/index.md) (y, en particular, [cree recursos para diferentes tamaños de pantalla](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) para más información sobre cómo estructurar las carpetas y los nombres de archivo en el proyecto de aplicación de Android para incluir recursos de imagen optimizados en la aplicación.

### <a name="windows-tablets-and-desktops"></a>Tabletas y escritorios de Windows

Para admitir tabletas y equipos de escritorio que ejecuten Windows, deberá usar la [compatibilidad con Windows UWP](~/xamarin-forms/platform/windows/installation/index.md), que crea aplicaciones universales que se ejecutan en Windows 10.

Se puede cambiar el tamaño de las aplicaciones que se ejecutan en tabletas y escritorios de Windows a dimensiones arbitrarias, además de ejecutarse en pantalla completa.

[![](tablet-images/splitscreen-sml.png "Windows Split Screen Example")](tablet-images/splitscreen.png#lightbox "Windows Split Screen Example")

<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optimizar para tabletas y equipos de escritorio

Puede ajustar la interfaz de usuario de Xamarin. Forms en función de si se usa un dispositivo de teléfono o tableta o escritorio. Esto significa que puede optimizar la experiencia del usuario en dispositivos de gran tamaño, como tabletas y equipos de escritorio.

### <a name="deviceidiom"></a>Dispositivo. expresión

Puede usar la clase [`Device`](~/xamarin-forms/platform/device.md) para cambiar el comportamiento de la aplicación o la interfaz de usuario. Con la enumeración `Device.Idiom` puede

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Este enfoque se puede expandir para realizar cambios significativos en los diseños de página individuales o incluso para representar páginas completamente diferentes en pantallas más grandes.

### <a name="leveraging-masterdetailpage"></a>Aprovechar MasterDetailPage

La [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) es ideal para pantallas de mayor tamaño, especialmente en el iPad, donde usa el [`UISplitViewController`](xref:UIKit.UISplitViewController) para proporcionar una experiencia de iOS nativa.

Revise [esta entrada de blog de Xamarin](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/) para ver cómo puede adaptar la interfaz de usuario para que los teléfonos usen un diseño y las pantallas más grandes puedan usar otro (con la `MasterDetailPage`).

## <a name="related-links"></a>Vínculos relacionados

- [Blog de Xamarin](https://devblogs.microsoft.com/xamarin/bringing-xamarin-forms-apps-to-tablets/)
- [Ejemplo de mis compradores](https://github.com/jamesmontemagno/myshoppe)
