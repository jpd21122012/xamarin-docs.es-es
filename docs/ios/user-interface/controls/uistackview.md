---
title: Vistas de pila en Xamarin. iOS
description: En este artículo se describe cómo usar el nuevo control UIStackView en una aplicación de Xamarin. iOS para administrar un conjunto de subvistas en una pila organizada horizontal o verticalmente.
ms.prod: xamarin
ms.assetid: 20246E87-2A49-438A-9BD7-756A1B50A617
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b4a8507d4d1497964f6b60307622ca3e1dc4cd90
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021801"
---
# <a name="stack-views-in-xamarinios"></a>Vistas de pila en Xamarin. iOS

_En este artículo se describe cómo usar el nuevo control UIStackView en una aplicación de Xamarin. iOS para administrar un conjunto de subvistas en una pila organizada horizontal o verticalmente._

> [!IMPORTANT]
> Tenga en cuenta que, aunque se admite StackView en el diseñador de iOS, pueden producirse errores de facilidad de uso al usar el canal estable. El cambio de los canales beta o alfa debe aliviar este problema. Hemos decidido presentar este tutorial con Xcode hasta que las correcciones necesarias se implementen en el canal estable.

El control de vista de pila (`UIStackView`) aprovecha la eficacia de las clases de diseño y tamaño automáticos para administrar una pila de subvistas, ya sea horizontal o verticalmente, que responde dinámicamente a la orientación y el tamaño de pantalla del dispositivo iOS.

El diseño de todas las subvistas adjuntas a una vista de pila se administra en función de las propiedades definidas por el desarrollador como el eje, la distribución, la alineación y el espaciado:

[![](uistackview-images/stacked01.png "Stack View layout diagram")](uistackview-images/stacked01.png#lightbox)

Cuando se usa un `UIStackView` en una aplicación de Xamarin. iOS, el desarrollador puede definir las subvistas dentro de un guión gráfico en el diseñador de iOS o agregando y quitando las C# subvistas en el código.

Este documento consta de dos partes: una guía de inicio rápido que le ayuda a implementar su primera vista de pila y, a continuación, algunos detalles técnicos sobre cómo funciona.

> [!VIDEO https://youtube.com/embed/p3po6507Ip8]

**Vídeo de UIStackView**

## <a name="uistackview-quickstart"></a>Inicio rápido de UIStackView

Como introducción rápida al control `UIStackView`, vamos a crear una interfaz simple que permite al usuario escribir una clasificación de 1 a 5. Usaremos dos vistas de pila: una para organizar la interfaz verticalmente en la pantalla del dispositivo y otra para organizar los iconos de clasificación de 1-5 horizontalmente a lo largo de la pantalla.

### <a name="define-the-ui"></a>Definir la interfaz de usuario

Inicie un nuevo proyecto de Xamarin. iOS y edite el archivo **Main. Storyboard** en la Interface Builder de Xcode. En primer lugar, arrastre una sola **vista de pila vertical** en el **controlador de vista**:

[![](uistackview-images/quick01.png "Drag a single Vertical Stack View on the View Controller")](uistackview-images/quick01.png#lightbox)

En el **Inspector de atributos**, establezca las siguientes opciones:

[![](uistackview-images/quick02.png "Set the Stack View options")](uistackview-images/quick02.png#lightbox)

Dónde:

- **Eje** : determina si la vista de pila organiza las subvistas **horizontal** o **verticalmente**.
- **Alineación** : controla cómo se alinean las subvistas dentro de la vista de pila.
- **Distribución** : controla cómo se ajusta el tamaño de las subvistas en la vista de pila.
- **Espaciado** : controla el espacio mínimo entre cada subvista de la vista de pila.
- **Referencia relativa** : si se activa, el espaciado vertical de cada subvista se derivará de su línea base.
- **Márgenes de diseño relativos** : coloca las subvistas en relación con los márgenes de diseño estándar.

Al trabajar con una vista de pila, puede pensar en la **alineación** como la ubicación X **e y de** la subvista y la **distribución** como el **alto** y el **ancho**.

> [!IMPORTANT]
> `UIStackView` está diseñada como una vista de contenedor sin representación y, como tal, no se dibuja en el lienzo como otras subclases de `UIView`. Por tanto, establecer propiedades como `BackgroundColor` o invalidar `DrawRect` no tendrá ningún efecto visual.

Continúe con el diseño de la interfaz de la aplicación agregando una etiqueta, ImageView, dos botones y una vista de pila horizontal para que sea similar a lo siguiente:

[![](uistackview-images/quick03.png "Laying out the Stack View UI")](uistackview-images/quick03.png#lightbox)

Configure la vista de pila horizontal con las siguientes opciones:

[![](uistackview-images/quick04.png "Configure the Horizontal Stack View options")](uistackview-images/quick04.png#lightbox)

Dado que no queremos que el icono que representa cada "punto" de la clasificación se estire cuando se agrega a la vista de pila horizontal, hemos establecido la **alineación** en **Center** y la **distribución** para **rellenar por igual**.

Por último, conecte las siguientes **salidas** y **acciones**:

[![](uistackview-images/quick05.png "The Stack View Outlets and Actions")](uistackview-images/quick05.png#lightbox)

### <a name="populate-a-uistackview-from-code"></a>Rellenar un UIStackView desde el código

Vuelva a Visual Studio para Mac y edite el archivo **ViewController.CS** y agregue el código siguiente:

```csharp
public int Rating { get; set;} = 0;
...

partial void IncreaseRating (Foundation.NSObject sender) {

    // Maximum of 5 "stars"
    if (++Rating > 5 ) {
        // Abort
        Rating = 5;
        return;
    }

    // Create new rating icon and add it to stack
    var icon = new UIImageView (new UIImage("icon.png"));
    icon.ContentMode = UIViewContentMode.ScaleAspectFit;
    RatingView.AddArrangedSubview(icon);

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });

}

partial void DecreaseRating (Foundation.NSObject sender) {

    // Minimum of zero "stars"
    if (--Rating < 0) {
        // Abort
        Rating =0;
        return;
    }

    // Get the last subview added
    var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];

    // Remove from stack and screen
    RatingView.RemoveArrangedSubview(icon);
    icon.RemoveFromSuperview();

    // Animate stack
    UIView.Animate(0.25, ()=>{
        // Adjust stack view
        RatingView.LayoutIfNeeded();
    });
}
```

Echemos un vistazo a algunas partes de este código en detalle. En primer lugar, usamos una `if` instrucciones para comprobar que no hay más de cinco "estrellas" o menor que cero.

Para agregar una nueva "estrella", se carga su imagen y se establece su **modo de contenido** en **ajuste de escala**:

```csharp
var icon = new UIImageView (new UIImage("icon.png"));
icon.ContentMode = UIViewContentMode.ScaleAspectFit;
```

Esto evita que el icono de "estrella" se distorsione cuando se agrega a la vista de pila.

A continuación, se agrega el nuevo icono de "estrella" a la colección de subvistas de la vista de pila:

```csharp
RatingView.AddArrangedSubview(icon);
```

Observará que se ha agregado el `UIImageView` a la propiedad `ArrangedSubviews` del `UIStackView`y no al `SubView`. Cualquier vista que desee que la vista de pila controle su diseño debe agregarse a la propiedad `ArrangedSubviews`.

Para quitar una vista previa de una vista de pila, primero se obtiene la subvista que se va a quitar:

```csharp
var icon = RatingView.ArrangedSubviews[RatingView.ArrangedSubviews.Length-1];
```

A continuación, es necesario quitarlo de la colección de `ArrangedSubviews` y de la vista superior:

```csharp
// Remove from stack and screen
RatingView.RemoveArrangedSubview(icon);
icon.RemoveFromSuperview();
```

Quitar una vista previa de la colección de `ArrangedSubviews` la saca del control de la vista de pila, pero no la quita de la pantalla.

### <a name="testing-the-ui"></a>Probar la interfaz de usuario

Con todos los elementos de interfaz de usuario y el código necesarios, ahora puede ejecutar y probar la interfaz. Cuando se muestra la interfaz de usuario, todos los elementos de la vista de pila vertical se espaciarán uniformemente de arriba abajo.

Cuando el usuario pulsa el botón **aumentar clasificación** , se agrega otra "estrella" a la pantalla (hasta un máximo de 5):

[![](uistackview-images/intro01.png "The sample app run")](uistackview-images/intro01.png#lightbox)

Las "estrellas" se centrarán automáticamente y se distribuirán uniformemente en la vista de pila horizontal. Cuando el usuario pulsa el botón **disminuir clasificación** , se quita una "estrella" (hasta que no quede ninguno).

## <a name="stack-view-details"></a>Detalles de la vista de pila

Ahora que tenemos una idea general de lo que es el control de `UIStackView` y cómo funciona, echemos un vistazo más profundo a algunas de sus características y detalles.

### <a name="auto-layout-and-size-classes"></a>Clases de diseño y tamaño automáticos

Como vimos anteriormente, cuando se agrega una subvista a una vista de pila, su diseño está totalmente controlado por esa vista de pila con las clases de diseño y tamaño automáticas para colocar y cambiar el tamaño de las vistas organizadas.

La vista de pila _anclará_ la primera y la última subvista de su colección a los bordes **superior** e **inferior** de las vistas de pila verticales o los bordes **izquierdo** y **derecho** de las vistas de pila horizontal. Si establece la propiedad `LayoutMarginsRelativeArrangement` en `true`, la vista ancla las subvistas a los márgenes relevantes en lugar del borde.

La vista de pila usa la propiedad `IntrinsicContentSize` de la subvista al calcular el tamaño de las subvistas a lo largo del `Axis` definido (excepto el `FillEqually Distribution`). El `FillEqually Distribution` cambia el tamaño de todas las subvistas para que tengan el mismo tamaño, con lo que se rellena la vista de pila a lo largo del `Axis`.

A excepción de los `Fill Alignment`, la vista de pila utiliza la propiedad `IntrinsicContentSize` de la subvista para calcular el tamaño de la vista perpendicular a la `Axis`especificada. En el `Fill Alignment`, se ajusta el tamaño de todas las subvistas para que rellenen la vista de pila perpendicular al `Axis`determinado.

### <a name="positioning-and-sizing-the-stack-view"></a>Colocar y ajustar el tamaño de la vista de pila

Mientras que la vista de pila tiene control total sobre el diseño de cualquier subvista (basándose en propiedades como `Axis` y `Distribution`), es necesario colocar la vista de pila (`UIStackView`) dentro de la vista primaria mediante las clases de diseño y tamaño automáticos.

Por lo general, esto significa anclar al menos dos bordes de la vista de pila para expandir y contraer, lo que define su posición. Sin restricciones adicionales, se cambiará automáticamente el tamaño de la vista de pila para ajustarse a todas sus subvistas de la manera siguiente:

- El tamaño a lo largo de su `Axis` será la suma de todos los tamaños de las subvistas más el espacio definido entre cada subvista.
- Si se `true`la propiedad `LayoutMarginsRelativeArrangement`, el tamaño de las vistas de pila también incluirá espacio para los márgenes.
- El tamaño perpendicular a la `Axis` se establecerá en la subvista más grande de la colección.

Además, puede especificar restricciones para el **alto** y el **ancho**de la vista de pila. En este caso, las subvistas se diseñarán (con el tamaño) para rellenar el espacio especificado por la vista de pila según lo determinen las propiedades `Distribution` y `Alignment`.

Si se `true`la propiedad `BaselineRelativeArrangement`, las subvistas se organizarán en función de la primera o la última línea de base de la subvista, en lugar de usar la posición **superior**, **inferior** o **central**- **Y** . Se calculan en el contenido de la vista de pila como se indica a continuación:

- Una vista de pila vertical devolverá la primera subvista de la primera línea de base y la última para la última. Si alguna de estas subvistas son vistas de pila, se usará su primera o última línea base.
- Una vista de pila horizontal usará su subvista más alta para la primera y la última línea de base. Si la vista más alta también es una vista de pila, usará la subvista más alta como línea de base.

> [!IMPORTANT]
> La alineación de línea base no funciona en tamaños de subvista expandidos o comprimidos, ya que la línea base se calculará en la posición equivocada. Para la alineación de línea base, asegúrese de que el **alto** de la subvista coincide con el **alto**de la vista de contenido intrínseca.

### <a name="common-stack-view-uses"></a>Usos comunes de la vista de pila

Hay varios tipos de diseño que funcionan bien con los controles de vista de pila. Según Apple, estos son algunos de los usos más comunes:

- **Defina el tamaño a lo largo del eje** : fijando ambos bordes a lo largo del `Axis` de la vista de pila y uno de los bordes adyacentes para establecer la posición, la vista de pila crecerá a lo largo del eje para ajustarse al espacio definido por sus subvistas.
- **Definir la posición de la subvista** : al anclar los bordes adyacentes de la vista de pila a su vista primaria, la vista de pila aumentará en ambas dimensiones para que quepan en las subvistas.
- **Definir el tamaño y la posición de la pila** : al anclar los cuatro bordes de la vista de pila a la vista primaria, la vista de pila organiza las subvistas en función del espacio definido en la vista de pila.
- **Defina el tamaño perpendicular al eje** : anclando ambos bordes perpendiculares al `Axis` de la vista de pila y uno de los bordes a lo largo del eje para establecer la posición, la vista de pila aumentará perpendicularmente al eje para ajustarse al espacio definido por sus subvistas.

### <a name="managing-the-appearance"></a>Administrar la apariencia

El `UIStackView` está diseñado como una vista de contenedor sin representación y, como tal, no se dibuja en el lienzo como otras subclases de `UIView`. Establecer propiedades como `BackgroundColor` o invalidar `DrawRect` no tendrá ningún efecto visual.

Hay varias propiedades que controlan el modo en que una vista de pila organizará su colección de subvistas:

- **Eje** : determina si la vista de pila organiza las subvistas **horizontal** o **verticalmente**.
- **Alineación** : controla cómo se alinean las subvistas dentro de la vista de pila.
- **Distribución** : controla cómo se ajusta el tamaño de las subvistas en la vista de pila.
- **Espaciado** : controla el espacio mínimo entre cada subvista de la vista de pila.
- **Referencia relativa** : si `true`, el espaciado vertical de cada subvista se derivará de su línea base.
- **Márgenes de diseño relativos** : coloca las subvistas en relación con los márgenes de diseño estándar.

Normalmente, usará una vista de pila para organizar un número pequeño de subvistas. Se pueden crear interfaces de usuario más complejas mediante el anidamiento de una o varias vistas de pila dentro de otras (como hicimos en el [Inicio rápido de UIStackView](#uistackview-quickstart) anterior).

Puede ajustar aún más la apariencia de la IU agregando restricciones adicionales a las subvistas (por ejemplo, para controlar el alto o el ancho). Sin embargo, se debe tener cuidado de no incluir restricciones conflictivas a las introducidas por la propia vista de pila.

### <a name="maintaining-arranged-views-and-sub-views-consistency"></a>Mantener la coherencia de vistas y vistas secundarias organizadas

La vista de pila garantizará que su propiedad `ArrangedSubviews` sea siempre un subconjunto de su propiedad `Subviews` mediante las siguientes reglas:

- Si se agrega una subvista a la colección de `ArrangedSubviews`, se agregará automáticamente a la colección `Subviews` (a menos que ya forme parte de la colección).
- Si se quita una subvista de la colección `Subviews` (quitada de la pantalla), también se quita de la colección `ArrangedSubviews`.
- Quitar una vista previa de la colección de `ArrangedSubviews` no la quita de la colección de `Subviews`. Por lo tanto, la vista de pila ya no lo colocará, pero seguirá estando visible en la pantalla.

La colección de `ArrangedSubviews` siempre es un subconjunto de la colección de `Subview`, pero el orden de las subvistas individuales dentro de cada colección es independiente y controlado por lo siguiente:

- El orden de las subvistas dentro de la colección `ArrangedSubviews` determina su orden de presentación dentro de la pila.
- El orden de las subvistas dentro de la colección `Subview` determina el orden Z (o la disposición en capas) en la vista de vuelta al frente.

### <a name="dynamically-changing-content"></a>Cambiar el contenido dinámicamente

Una vista de pila ajustará automáticamente el diseño de las subvistas cada vez que se agregue, quite u oculte una vista previa. El diseño también se ajustará si se ajusta alguna propiedad de la vista de pila (por ejemplo, su `Axis`).

Los cambios de diseño se pueden animar colocándolos en un bloque de animación, por ejemplo:

```csharp
// Animate stack
UIView.Animate(0.25, ()=>{
    // Adjust stack view
    RatingView.LayoutIfNeeded();
});
```

Muchas de las propiedades de la vista de pila se pueden especificar utilizando clases de tamaño dentro de un guion gráfico. Estas propiedades se animarán automáticamente como respuesta a los cambios de tamaño o orientación.

## <a name="summary"></a>Resumen

En este artículo se ha tratado el nuevo control de `UIStackView` (para iOS 9) para administrar un conjunto de subvistas en una pila organizada horizontal o verticalmente en una aplicación de Xamarin. iOS.
Comenzó con un ejemplo sencillo del uso de vistas de pila para crear una interfaz de usuario y finalizó con una visión más detallada de las vistas de pila y sus propiedades y características.

## <a name="related-links"></a>Vínculos relacionados

- [Ejemplos de iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 para desarrolladores](https://developer.apple.com/ios/pre-release/)
- [Novedades de iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Referencia de UIStackView](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)
- [Introducción a UIStackView (vídeo)](https://university.xamarin.com/lightninglectures/introducing-uistackview-on-ios9)
