---
title: Escalado de accesibilidad para tamaños de fuente con nombre en iOS
description: Funcionalidades específicas de plataforma permiten utilizar la funcionalidad que solo está disponible en una plataforma concreta, sin necesidad de implementar los representadores personalizados o los efectos. En este artículo se explica cómo consumir el tipo específico de la plataforma iOS que deshabilita el escalado de accesibilidad para los tamaños de fuente con nombre.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 13ca4945ed447ae811ee95e308238f04e58ac0bd
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200082"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Escalado de accesibilidad para tamaños de fuente con nombre en iOS

[![Descargar ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Este específico de la plataforma iOS deshabilita el escalado de accesibilidad para tamaños de fuente con nombre. Se consume en XAML estableciendo el `Application.EnableAccessibilityScalingForNamedFontSizes` propiedad enlazable a `false`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Como alternativa, pueden usarse desde C# mediante la API fluida:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

El `Application.On<iOS>` método especifica que solo se ejecutarán este específicos de la plataforma de iOS. El `Application.SetEnableAccessibilityScalingForNamedFontSizes` método, en el [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) espacio de nombres, se usa para deshabilitar los tamaños de fuente con nombre que se escalan mediante la configuración de accesibilidad de iOS. Además, se puede `Application.GetEnableAccessibilityScalingForNamedFontSizes` usar el método para devolver si los tamaños de fuente con nombre se escalan mediante la configuración de accesibilidad de iOS.

## <a name="related-links"></a>Vínculos relacionados

- [PlatformSpecifics (ejemplo)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creación funcionalidades específicas de plataforma](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API de iOSSpecific](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
