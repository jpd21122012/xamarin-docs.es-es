---
title: Propiedades, métodos y eventos de controles comunes de Xamarin. Forms
description: En este artículo se describen las propiedades, los métodos y los eventos comunes definidos en la clase VisualElement, que se suelen usar en las clases derivadas.
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/21/2019
ms.openlocfilehash: 6d10e665c6461655440ddfb2c524cb56a14337f6
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121348"
---
# <a name="xamarinforms-common-control-properties-methods-and-events"></a>Propiedades, métodos y eventos de controles comunes de Xamarin. Forms

La clase de Xamarin `VisualElement` . Forms es la clase base para la mayoría de los controles que se usan en una aplicación de Xamarin. Forms. La `VisualElement` clase define muchas [propiedades](#properties), [métodos](#methods)y [eventos](#events) que se usan en las clases derivadas.

## <a name="properties"></a>Properties (Propiedades)

Las siguientes propiedades están disponibles en `VisualElement` las instancias de. Para obtener una lista completa, consulte las propiedades de la [API de VisualElement](xref:Xamarin.Forms.VisualElement#properties).

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

La `AnchorX` propiedad es un `double` valor que define el punto central en el eje X para las transformaciones como la escala y la rotación. El valor predeterminado es 0,5.

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

La `AnchorY` propiedad es un `double` valor que define el punto central en el eje X para las transformaciones como la escala y la rotación. El valor predeterminado es 0,5.

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

La `BackgroundColor` propiedad es un `Color` que determina el color de fondo del control. Si no se establece, el fondo será el `Color` objeto predeterminado, que se representa como transparente.

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

La `Behaviors` propiedad `List` es de`Behavior` objetos. Los comportamientos permiten asociar la funcionalidad reutilizable a los elementos agregándolos a `Behaviors` la lista. Para obtener más información sobre `Behavior` la clase, vea [comportamientos de Xamarin. Forms](~/xamarin-forms/app-fundamentals/behaviors/index.md).

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

La `Bounds` propiedad es un objeto de solo `Rectangle` lectura que representa el espacio ocupado por el control. El `Bounds` valor de la propiedad se asigna durante el ciclo de diseño. `Rectangle` Contienepropiedadesymétodosútilesparaprobarlaintersecciónylacontención`struct` de rectángulos. Para obtener más información, consulte la [API de rectángulo de Xamarin. Forms](xref:Xamarin.Forms.Rectangle).

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

La `Effects` propiedad `Element`es de objetos`Effect` , heredada de la clase (XREF: Xamarin. Forms. Element). `List` Los efectos permiten personalizar los controles nativos y se suelen usar para pequeños cambios de estilo. Para obtener más información sobre `Effect` la clase, vea [efectos de Xamarin. Forms](~/xamarin-forms/app-fundamentals/effects/index.md).

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

La `FlowDirection` propiedad es un `FlowDirection` valor de enumeración. La dirección del flujo se puede `MatchParent`establecer `LeftToRight`en, `RightToLeft` o y determina el orden y la dirección del diseño. La `FlowDirection` propiedad se utiliza normalmente para admitir idiomas que se leen de derecha a izquierda.

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

La `Height` propiedad es un valor de solo `double` lectura que describe el alto representado del control. La `Height` propiedad se calcula durante el ciclo de diseño y no se puede establecer directamente. El alto de un control se puede solicitar mediante la [propiedad HeightRequest](#heightrequest).

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

La `HeightRequest` propiedad es un `double` valor que determina el alto deseado del control. Es posible que el alto absoluto del control no coincida con el valor solicitado. Para obtener más información, consulte propiedades de la [solicitud](#request-properties).

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

La `InputTransparent` propiedad es un `bool` que determina si el control recibe datos proporcionados por el usuario. El valor predeterminado es `false`, asegurándose de que el elemento recibe la entrada. Esta propiedad se transfiere a los elementos secundarios cuando se establece. Si se `InputTransparent` establece la `true` propiedad en en una clase de diseño, todos los elementos del diseño no recibirán una entrada.

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

La `IsEnabled` propiedad es un `bool` valor que determina si el control reacciona a los datos proporcionados por el usuario. El valor predeterminado es `true`. Si establece esta propiedad en false, impedirá que el control acepte datos proporcionados por el usuario.

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

La `IsFocused` propiedad es un `bool` valor que describe si el control es actualmente el objeto que tiene el foco. Si se [`Focus`](#focus) llama al método en el control, el `IsFocused` valor se establecerá en true. Al llamar [`Unfocus`](#unfocus) al método, se establecerá esta propiedad en false.

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

La `IsTabStop` propiedad es un `bool` valor que define si el control recibe el foco cuando el usuario avanza por los controles con la tecla TAB. Si esta propiedad es false, la `TabIndex` propiedad no tendrá ningún efecto.

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

La `IsVisible` propiedad es un `bool` valor que determina si se representa el control. No se mostrarán los controles con la `IsVisible` propiedad establecida en false, por lo que no se tendrán en cuenta para los cálculos de espacio durante el ciclo de diseño y no podrán aceptar la entrada del usuario.

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

La `MinimumHeightRequest` propiedad es un `double` valor que determina cómo se controla el desbordamiento cuando dos elementos compiten por el espacio limitado. El establecimiento `MinimumHeightRequest` de la propiedad permite que el proceso de diseño escale el elemento hacia abajo hasta la dimensión mínima solicitada. Si no `MinimumHeightRequest` se especifica, el valor predeterminado es-1 y el proceso de diseño `HeightRequest` considerará como el valor mínimo. Esto significa que los elementos sin `MinimumHeightRequest` valor no tendrán un alto escalable.

Para obtener más información, vea [propiedades de solicitud mínima](#minimum-request-properties).

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

La `MinimumWidthRequest` propiedad es un `double` valor que determina cómo se controla el desbordamiento cuando dos elementos compiten por el espacio limitado. El establecimiento `MinimumWidthRequest` de la propiedad permite que el proceso de diseño escale el elemento hacia abajo hasta la dimensión mínima solicitada. Si no `MinimumWidthRequest` se especifica, el valor predeterminado es-1 y el proceso de diseño `WidthRequest` considerará como el valor mínimo. Esto significa que los elementos sin `MinimumWidthRequest` valor no tendrán un ancho escalable.

Para obtener más información, vea [propiedades de solicitud mínima](#minimum-request-properties).

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

La `Opacity` propiedad es un `double` valor de cero a uno que determina la opacidad del control durante la representación. El valor predeterminado de esta propiedad es 1,0. Se fijarán los valores fuera del intervalo de 0 a 1. La `Opacity` propiedad solo se aplica si la [`IsVisible`](#isvisible) propiedad es `true`. La opacidad se aplica de forma iterativa. Por lo tanto, si un control principal tiene una opacidad de 0,5 y su elemento secundario tiene una opacidad de 0,5, el elemento secundario se representará con un valor de opacidad de 0,25 vigente. Establecer la `Opacity` propiedad de un control de entrada en 0 tiene un comportamiento indefinido.

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

La propiedad `Parent` se hereda de la clase `Element`. Esta propiedad es un `Element` objeto que es el elemento primario del control. Normalmente `Parent` , la propiedad se establece automáticamente en un elemento cuando se agrega como elemento secundario de otro elemento.

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

La `Resources` propiedad es una `ResourceDictionary` instancia de que se rellena con pares clave-valor que normalmente se rellenan en tiempo de ejecución desde XAML. Este diccionario permite a los desarrolladores de aplicaciones reutilizar los objetos definidos en XAML en tiempo de compilación y en tiempo de ejecución. Las claves del diccionario se rellenan a `x:Key` partir del atributo de la etiqueta XAML. El objeto creado a partir de XAML se inserta `ResourceDictionary` en para la clave especificada. una vez inicializado.

Para obtener más información, vea [diccionarios de recursos](~/xamarin-forms/xaml/resource-dictionaries.md).

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

La `Rotation` propiedad es un `double` valor entre cero y 360 que define el giro sobre el eje Z en grados. El valor predeterminado de esta propiedad es 0. La rotación se aplica en relación [`AnchorX`](#anchorx) con [`AnchorY`](#anchory) los valores y.

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

La `RotationX` propiedad es un `double` valor entre cero y 360 que define el giro sobre el eje X en grados. El valor predeterminado de esta propiedad es 0. La rotación se aplica en relación [`AnchorX`](#anchorx) con [`AnchorY`](#anchory) los valores y.

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

La `RotationY` propiedad es un `double` valor entre cero y 360 que define el giro sobre el eje Y en grados. El valor predeterminado de esta propiedad es 0. La rotación se aplica en relación [`AnchorX`](#anchorx) con [`AnchorY`](#anchory) los valores y.

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

La `Scale` propiedad es un `double` valor que define la escala del control. El valor predeterminado de esta propiedad es 1,0. La escala se aplica en relación [`AnchorX`](#anchorx) con [`AnchorY`](#anchory) los valores y.

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

La `ScaleX` propiedad es un `double` valor que define la escala del control a lo largo del eje X. El valor predeterminado de esta propiedad es 1,0. La `ScaleX` propiedad se aplica en relación con [`AnchorX`](#anchorx) el valor.

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

La `ScaleY` propiedad es un `double` valor que define la escala del control a lo largo del eje Y. El valor predeterminado de esta propiedad es 1,0. La `ScaleY` propiedad se aplica en relación con [`AnchorY`](#anchory) el valor.

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

La propiedad `Style` se hereda de la clase `NavigableElement`. Esta propiedad es una instancia de la `Style` clase. La `Style` clase contiene desencadenadores, establecedores y comportamientos que definen la apariencia y el comportamiento de los elementos visuales. Para obtener más información, vea [estilos XAML de Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/index.md).

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

La `StyleClass` propiedad es una lista de `string` objetos que representan los nombres de `Style` las clases. Esta propiedad se hereda de la clase `NavigableElement`. La `StyleClass` propiedad permite aplicar varios atributos de estilo a una `VisualElement` instancia de. Para obtener más información, vea [clases de estilo de Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

La `TabIndex` propiedad es un `int` valor que define el orden de control al avanzar por los controles con la tecla TAB. La `TabIndex` propiedad es la implementación de la propiedad definida en la `ITabStopElement` interfaz, que la `VisualElement` clase implementa.

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

La `TranslationX` propiedad es un `double` valor que define la conversión Delta que se va a aplicar en el eje X. La traducción se aplica después del diseño y se utiliza normalmente para aplicar animaciones. La conversión de un elemento fuera de los límites de su contenedor primario My impide que se funcionen las entradas.

Para obtener más información, vea [animación en Xamarin. Forms](~/xamarin-forms/user-interface/animation/index.md).

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

La `TranslationY` propiedad es un `double` valor que define la conversión Delta que se va a aplicar en el eje Y. La traducción se aplica después del diseño y se utiliza normalmente para aplicar animaciones. La conversión de un elemento fuera de los límites de su contenedor primario My impide que se funcionen las entradas.

Para obtener más información, vea [animación en Xamarin. Forms](~/xamarin-forms/user-interface/animation/index.md).

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

La `Triggers` propiedad es de solo `List` lectura de `TriggerBase` objetos. Los desencadenadores permiten a los desarrolladores de aplicaciones expresar acciones en XAML que cambian la apariencia visual de los controles en respuesta a los cambios de eventos o propiedades. Para obtener más información, vea desencadenadores de [Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

La `Visual` propiedad es una `IVisual` instancia de que permite crear representadores y aplicarlos de forma `VisualElement` selectiva a las instancias de. La `Visual` propiedad se establece para que coincida con su elemento primario, por lo que la definición de un representador en un componente también se aplicará a todos los elementos secundarios de ese componente. Si no se establece ningún representador personalizado en un control o en sus antecesores, se usará el representador de Xamarin. Forms predeterminado. Para obtener más información, vea [Visual Xamarin. Forms](~/xamarin-forms/user-interface/visual/index.md).

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

La `Width` propiedad es un valor de solo `double` lectura que describe el ancho representado del control. La `Width` propiedad se calcula durante el ciclo de diseño y no se puede establecer directamente. El ancho de un control se puede solicitar mediante la [propiedad WidthRequest](#widthrequest).

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

La `WidthRequest` propiedad es un `double` valor que determina el ancho deseado del control. El ancho absoluto del control puede no coincidir con el valor solicitado. Para obtener más información, consulte propiedades de la [solicitud](#request-properties).

### [`X`](xref:Xamarin.Forms.VisualElement.X)

La `X` propiedad es un valor de solo `double` lectura que describe la posición X actual del control.

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

La `Y` propiedad es un valor de solo `double` lectura que describe la posición Y actual del control.

## <a name="methods"></a>Métodos

Los métodos siguientes están disponibles en la `VisualElement` clase. Para obtener una lista completa, consulte métodos de la [API de VisualElement](xref:Xamarin.Forms.VisualElement#methods).

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

El `FindByName` método se hereda de la `Element` clase y tiene la siguiente firma:

```csharp
public object FindByName (string name)
```

Este método busca el argumento proporcionado `name` en todos los elementos secundarios y devuelve el elemento que tiene el nombre especificado. Si no se encuentra ninguna coincidencia `null` , se devuelve.

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

El `Focus` método intenta establecer el foco en el elemento. Este método tiene la siguiente firma:

```csharp
public bool Focus ()
```

El `Focus` método devuelve `true` si se estableció correctamente el foco de `false` teclado y si la llamada al método no produjo un cambio de foco. El elemento debe ser capaz de recibir el foco para que este método funcione. La llamada `Focus` al método en elementos que están fuera de la definición o que no están en funcionamiento tiene un comportamiento indefinido.

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

El `Unfocus` método intenta quitar el foco en el elemento. Este método tiene la siguiente firma:

```csharp
public void Unfocus ()
```

El elemento ya debe tener el foco para que este método funcione.

## <a name="events"></a>Events

Los eventos siguientes están disponibles en la `VisualElement` clase. Para obtener una lista completa, vea [eventos de Xamarin. Forms VisualElement](xref:Xamarin.Forms.VisualElement#events).

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

El `Focused` evento se desencadena cuando la `VisualElement` instancia recibe el foco. Este evento no se traspasa a través de la pila de Xamarin. Forms, sino que se recibe directamente del control nativo. El establecedor de la [`IsFocused`](#isfocused) propiedad emite este evento.

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

El `SizeChanged` evento se desencadena cuando cambia `VisualElement` la `Height` instancia `Width` o las propiedades. Si los desarrolladores desean responder directamente al cambio de tamaño, en lugar de responder al evento posterior al cambio, deben implementar el [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*) método virtual en su lugar.

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

El `Unfocused` evento se desencadena cuando la `VisualElement` instancia pierde el foco. Este evento no se traspasa a través de la pila de Xamarin. Forms, sino que se recibe directamente del control nativo. El establecedor de la [`IsFocused`](#isfocused) propiedad emite este evento.

## <a name="units-of-measurement"></a>Unidades de medida

Todas las plataformas Android, iOS y UWP tienen unidades de medida diferentes que pueden variar en los dispositivos. Xamarin. Forms usa una unidad de medida independiente de la plataforma que normaliza las unidades entre dispositivos y plataformas. Hay 160 unidades por pulgada o 64 unidades por centímetro en Xamarin. Forms.

## <a name="request-properties"></a>Propiedades de solicitud

Las propiedades cuyos nombres contienen "Request" definen un valor deseado, que puede no coincidir con el valor representado real. Por ejemplo, `HeightRequest` puede establecerse en 150 pero si el diseño solo permite espacio para las unidades 100, la `Height` representación del control solo será 100. El tamaño representado se ve afectado por el espacio disponible y los componentes contenidos.

## <a name="minimum-request-properties"></a>Propiedades de solicitud mínimas

Las propiedades de solicitud `MinimumHeightRequest` mínima `MinimumWidthRequest`incluyen y, y están diseñadas para permitir un control más preciso sobre cómo los elementos controlan el desbordamiento entre sí. Sin embargo, el comportamiento de diseño relacionado con estas propiedades tiene algunas consideraciones importantes.

### <a name="unspecified-minimum-property-values"></a>Valores de propiedad mínimos no especificados

Si no se establece un valor mínimo, el valor predeterminado de la propiedad minimum es-1. El proceso de diseño omite este valor y considera que el valor absoluto es el mínimo. La consecuencia práctica de este comportamiento es que **no se** reducirá un elemento sin ningún valor mínimo especificado. **Se** reducirá un elemento con un valor mínimo especificado.

En el siguiente código XAML `BoxView` se muestran dos elementos `StackLayout`en un horizontal:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

La primera `BoxView` instancia solicita un ancho de 500 y no especifica un ancho mínimo. La segunda `BoxView` instancia solicita un ancho de 500 y un ancho mínimo de 250. Si el elemento `StackLayout` primario no es lo suficientemente ancho como para contener ambos componentes en el ancho solicitado, `BoxView` el proceso de diseño tendrá en cuenta la primera instancia para tener un ancho mínimo de 500 porque no se especifica ningún otro mínimo válido. La segunda `BoxView` instancia tiene permiso para reducir verticalmente a 250 y se reducirá para ajustarse hasta que su ancho alcance 250 unidades.

Si el comportamiento deseado es que la primera `BoxView` instancia se reduzca verticalmente sin ningún ancho mínimo `MinimumWidthRequest` , se debe establecer en un valor válido, como 0.

### <a name="minimum-and-absolute-property-values"></a>Valores de las propiedades Minimum y Absolute

El comportamiento es indefinido si el valor mínimo es mayor que el valor absoluto. Por ejemplo, si `MinimumWidthRequest` se establece en 100, la `WidthRequest` propiedad nunca debe superar 100. Al especificar un valor de propiedad mínimo, siempre debe especificar un valor absoluto para asegurarse de que el valor absoluto sea mayor que el valor mínimo.

### <a name="minimum-properties-within-a-grid"></a>Propiedades mínimas dentro de una cuadrícula

`Grid`los diseños tienen su propio sistema para ajustar el tamaño relativo de las filas y las columnas. El `MinimumWidthRequest` uso `MinimumHeightRequest` de o `Grid` dentro de un diseño no tendrá ningún efecto. Para obtener más información, consulte [cuadrícula de Xamarin. Forms](~/xamarin-forms/user-interface/layouts/grid.md).

## <a name="related-links"></a>Vínculos relacionados

* [Documentación de la API de VisualElement](xref:Xamarin.Forms.VisualElement)
