---
title: Trabajar con barras de navegación de tvOS en Xamarin
description: En este documento se describe cómo trabajar con barras de navegación en una aplicación tvOS compilada con Xamarin. Describe cómo configurar las barras de navegación en un guión gráfico y responder a los eventos de estos botones.
ms.prod: xamarin
ms.assetid: 74E396B7-87F0-46F7-BC6C-827DB8884C97
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: aa376385b000b83a41fdcdc7a4d3c8bf1553f0a7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030475"
---
# <a name="working-with-tvos-navigation-bars-in-xamarin"></a>Trabajar con barras de navegación de tvOS en Xamarin

Las barras de navegación se pueden agregar a la parte superior de las vistas para mostrar un título y botones de barra de navegación opcionales. Normalmente se usan cuando el usuario ha navegado desde una página principal, como una vista de tabla, una colección o un menú a una vista, que muestra los detalles del elemento seleccionado.

[![](navigation-bars-images/navbar01.png "Sample Navigation Bar")](navigation-bars-images/navbar01.png#lightbox)

Además del título (que se muestra en el centro), las barras de navegación pueden contener uno o varios botones de barra de navegación (`UIBarButtonItem`) en los lados izquierdo y derecho de la barra.

> [!IMPORTANT]
> Las barras de navegación son completamente transparentes de forma predeterminada. Se debe tener cuidado para asegurarse de que el contenido de la barra de navegación permanece legible sobre el contenido que se encuentra debajo de él. Por ejemplo, cuando el contenido de una vista de tabla o una colección se desplaza bajo él.

<a name="Navigation-Bars-and-Storyboards" />

## <a name="navigation-bars-and-storyboards"></a>Barras de navegación y guiones gráficos

La manera más sencilla de trabajar con barras de navegación en una aplicación Xamarin. tvOS es agregarlos a la interfaz de usuario de la aplicación mediante el diseñador de iOS.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

1. En el **Panel de solución**, haga doble clic en `Main.storyboard` archivo y ábralo para su edición.
1. Arrastre una **barra de navegación** desde el **cuadro de herramientas** y colóquela en la vista de la parte superior de la pantalla:

    [![](navigation-bars-images/navbar02.png "A Navigation Bar")](navigation-bars-images/navbar02.png#lightbox)
1. Haga doble clic en la **barra de navegación** para seleccionar el **elemento de navegación**. En la pestaña **Widget** del **Panel de propiedades**, puede establecer el **título**:

    [![](navigation-bars-images/navbar03.png "Set the Title")](navigation-bars-images/navbar03.png#lightbox)
1. A continuación, puede agregar uno o varios **elementos de botón de barra** a cualquier extremo de la barra:

    [![](navigation-bars-images/navbar04.png "A Bar Button Item")](navigation-bars-images/navbar04.png#lightbox)
1. Por último, conecte los **elementos del botón de barra** a las acciones en la pestaña **eventos** del **Explorador de propiedades**:

    [![](navigation-bars-images/navbar05.png "A Bar Button Item Action")](navigation-bars-images/navbar05.png#lightbox)
1. Guarde los cambios.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. En el **Explorador de soluciones**, haga doble clic en `Main.storyboard` archivo y ábralo para su edición.
1. Arrastre una **barra de navegación** desde el **cuadro de herramientas** y colóquela en la vista de la parte superior de la pantalla:

    [![](navigation-bars-images/navbar02-vs.png "A Navigation Bar")](navigation-bars-images/navbar02-vs.png#lightbox)
1. Haga doble clic en la **barra de navegación** para seleccionar el **elemento de navegación**. En la pestaña **Widget** del **Explorador de propiedades**, puede establecer el **título**:

    [![](navigation-bars-images/navbar03-vs.png "Set the Title")](navigation-bars-images/navbar03-vs.png#lightbox)
1. A continuación, puede agregar uno o varios **elementos de botón de barra** a cualquier extremo de la barra:

    [![](navigation-bars-images/navbar04-vs.png "A Bar Button Items")](navigation-bars-images/navbar04-vs.png#lightbox)
1. Por último, conecte los **elementos del botón de barra** a las acciones en la pestaña **eventos** del **Explorador de propiedades**:

    [![](navigation-bars-images/navbar05-vs.png "A Bar Button Item Actions")](navigation-bars-images/navbar05-vs.png#lightbox)
1. Guarde los cambios.

-----

> [!IMPORTANT]
> Aunque es posible asignar eventos como `TouchUpInside` a un elemento de la interfaz de usuario (por ejemplo, un botón) en el diseñador de iOS, nunca se llamará porque Apple TV no tiene una pantalla táctil ni eventos táctiles de soporte técnico. Siempre debe usar el evento `Primary Action` al crear controladores de eventos para los elementos de la interfaz de usuario de tvOS.

El código siguiente proporciona un ejemplo de controladores de eventos en tres BarButtonItems diferentes: `ShowFirstHotel`, `ShowSecondHotel`y `ShowThirdHotel`. Cuando se hace clic en cada elemento, se cambia el `HotelImage` de la imagen de fondo. Esto se edita en el archivo de controlador de vista (ejemplo `ViewController.cs`):

```csharp
using System;
using Foundation;
using UIKit;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Custom Actions
        partial void ShowFirstHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel01.jpg");
        }

        partial void ShowSecondHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel02.jpg");
        }

        partial void ShowThirdHotel (UIBarButtonItem sender) {
            // Change background image
            HotelImage.Image = UIImage.FromFile("Motel03.jpg");
        }
        #endregion
    }
}
```

Siempre y cuando la propiedad `Enabled` de un botón esté `true` y no esté incluida en otro control o vista, se puede convertir en el elemento enfocado mediante Siri Remote.

Para obtener más información sobre cómo trabajar con guiones gráficos, vea nuestra [Guía de inicio rápido Hola, tvOS](~/ios/tvos/get-started/hello-tvos.md).

<a name="Summary" />

## <a name="summary"></a>Resumen

En este artículo se ha tratado el diseño y el trabajo con barras de navegación dentro de una aplicación Xamarin. tvOS.

## <a name="related-links"></a>Vínculos relacionados

- [Ejemplos de tvOS](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [Guías de la interfaz humana de tvOS](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Guía de programación de aplicaciones para tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
