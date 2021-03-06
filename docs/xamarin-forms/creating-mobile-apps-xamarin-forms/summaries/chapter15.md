---
title: Resumen del capítulo 15. La interfaz interactiva
description: 'Creación de aplicaciones móviles con Xamarin.Forms: resumen del capítulo 15. La interfaz interactiva'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F54E86F4-1CDA-474E-9B09-242060C2C13D
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 5f96d2f4b619bbb10bb58e9b1b5dc7007c1ce888
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131103"
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Resumen del capítulo 15. La interfaz interactiva

[![Descargar ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)

En este capítulo se exploran ocho derivados de `View` que permiten la interacción con el usuario.

## <a name="view-overview"></a>Ver información general

Xamarin. Forms contiene 20 clases de instancias que se derivan de `View` pero no `Layout`. Seis de estos se han tratado en los capítulos anteriores:

- `Label`: [ **capítulo 2. Anatomía de una aplicación**](chapter02.md)
- `BoxView`: [ **capítulo 3. Desplazar la pila**](chapter03.md)
- `Button`: [ **capítulo 6. Clics de botón**](chapter06.md)
- `Image`: [ **capítulo 13. Mapas de bits**](chapter13.md)
- `ActivityIndicator`: [ **capítulo 13. Mapas de bits**](chapter13.md)
- `ProgressBar`: [ **capítulo 14. AbsoluteLayout**](chapter14.md)

Las vistas de ocho en este capítulo eficazmente permiten al usuario interactuar con tipos de datos básicos. NET:

|Tipo de datos|Vistas|
|--- |--- |
|`Double`|[`Slider`](xref:Xamarin.Forms.Slider), [`Stepper`](xref:Xamarin.Forms.Stepper)|
|`Boolean`|[`Switch`](xref:Xamarin.Forms.Switch)|
|`String`|[`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), [`SearchBar`](xref:Xamarin.Forms.SearchBar)|
|`DateTime`|[`DatePicker`](xref:Xamarin.Forms.DatePicker), [`TimePicker`](xref:Xamarin.Forms.TimePicker)|

Estas vistas se pueden considerar como representaciones visuales de interactivas de los tipos de datos subyacente. Este concepto se explora más en el capítulo 16 del siguiente capítulo [ **. Enlace de datos**](chapter16.md).

Las vistas de seis restantes se tratan en los capítulos siguientes:

- `WebView`: [ **capítulo 16. Enlace de datos**](chapter16.md)
- `Picker`: [ **capítulo 19. Vistas de colección**](chapter19.md)
- `ListView`: [ **capítulo 19. Vistas de colección**](chapter19.md)
- `TableView`: [ **capítulo 19. Vistas de colección**](chapter19.md)
- `Map`: [ **capítulo 28. Ubicación y asignaciones**](chapter28.md)
- `OpenGLView`: no se trata en este libro (y no es compatible con las plataformas Windows)

## <a name="slider-and-stepper"></a>Control deslizante y motor paso a paso

Tanto [`Slider`](xref:Xamarin.Forms.Slider) como [`Stepper`](xref:Xamarin.Forms.Stepper) permiten al usuario elegir un valor numérico de un intervalo. La `Slider` es un intervalo continuo mientras que la `Stepper` implica valores discretos.

### <a name="slider-basics"></a>Conceptos básicos del control deslizante

El [`Slider`](xref:Xamarin.Forms.Slider) es una barra horizontal que representa un intervalo de valores de un mínimo de la izquierda a un máximo de la derecha. Define tres propiedades públicas:

- [`Value`](xref:Xamarin.Forms.Slider.Value) de tipo `double`, valor predeterminado de 0
- [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) de tipo `double`, valor predeterminado de 0
- [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) de tipo `double`, valor predeterminado de 1

Asegurarse de que son coherentes con las propiedades enlazables apoyar estas propiedades:

- Para las tres propiedades, el método [`coerceValue`](xref:Xamarin.Forms.BindableProperty.CoerceValueDelegate) especificado para la propiedad enlazable garantiza que `Value` se encuentra entre `Minimum` y `Maximum`.
- El método [`validateValue`](xref:Xamarin.Forms.BindableProperty.ValidateValueDelegate) en `MinimumProperty` devuelve `false` si `Minimum` se establece en un valor mayor o igual que `Maximum`y similar para `MaximumProperty`. Al devolver `false` del método `validateValue`, se genera una `ArgumentException`.

`Slider` activa el evento [`ValueChanged`](xref:Xamarin.Forms.Slider.ValueChanged) con un argumento [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) cuando cambia la propiedad `Value`, ya sea mediante programación o cuando el usuario manipula el `Slider`.

En el ejemplo [**SliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) se muestra el uso simple de la `Slider`.

### <a name="common-pitfalls"></a>Dificultades habituales

Tanto en código como en XAML, las propiedades `Minimum` y `Maximum` se establecen en el orden especificado. Asegúrese de inicializar estas propiedades para que `Maximum` sea siempre mayor que `Minimum`. De lo contrario, se producirá una excepción.

La inicialización de las propiedades de `Slider` puede hacer que cambie la propiedad `Value` y se desencadene el evento `ValueChanged`. Debe asegurarse de que el controlador de eventos `Slider` no tenga acceso a las vistas que todavía no se han creado durante la inicialización de la página.

El evento `ValueChanged` no se activa durante la inicialización de `Slider` a menos que cambie la propiedad `Value`. Puede llamar al controlador de `ValueChanged` directamente desde el código.

### <a name="slider-color-selection"></a>Selección de color del control deslizante

El programa [**RgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) contiene tres elementos `Slider` que permiten seleccionar de forma interactiva un color mediante la especificación de sus valores RGB:

[![Captura de pantalla triple de controles deslizantes de R G B](images/ch15fg03-small.png "Controles deslizantes RGB")](images/ch15fg03-large.png#lightbox "Controles deslizantes RGB")

En el ejemplo [**TextFade**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) se usan dos elementos `Slider` para desplace dos elementos `Label` a través de un `AbsoluteLayout` y se desvanece uno en el otro.

### <a name="the-stepper-difference"></a>La diferencia del motor paso a paso

El [`Stepper`](xref:Xamarin.Forms.Stepper) define las mismas propiedades y eventos que `Slider` pero la propiedad `Maximum` se inicializa en 100 y `Stepper` define una cuarta propiedad:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) de tipo `double`, inicializado en 1

Visualmente, el `Stepper` se compone de dos botones con la etiqueta **&ndash;** y **+** . Al presionar **&ndash;** se reduce `Value` `Increment` a un mínimo de `Minimum`. Al presionar **+** aumenta `Value` `Increment` a un máximo de `Maximum`.

Esto se muestra en el ejemplo [**StepperDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) .

## <a name="switch-and-checkbox"></a>Casilla de verificación y conmutador

El [`Switch`](xref:Xamarin.Forms.Switch) permite al usuario especificar un valor booleano.

### <a name="switch-basics"></a>Conceptos básicos de conmutador

Visualmente, el `Switch` se compone de un comando de alternancia que se puede desactivar y activar. La clase define una propiedad:

- [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) de tipo `bool`

`Switch` define un evento:

- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) acompañado de un objeto [`ToggledEventArgs`](xref:Xamarin.Forms.ToggledEventArgs) , que se desencadena cuando cambia la propiedad `IsToggled`.

El programa [**SwitchDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) muestra el `Switch`.

### <a name="a-traditional-checkbox"></a>Una casilla de verificación tradicional

Algunos desarrolladores podrían preferir una `CheckBox` más tradicional a la `Switch`. La biblioteca [**Xamarin. FormsBook. Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) contiene una clase `CheckBox` que se deriva de `ContentView`. los archivos [CheckBox. Xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) y [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) implementan `CheckBox`. `CheckBox` define tres propiedades (`Text`, `FontSize`y `IsChecked`) y un evento `CheckedChanged`.

En el ejemplo [**CheckBoxDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) se muestra este `CheckBox`.

## <a name="typing-text"></a>Escribir texto

Xamarin.Forms define tres vistas que permiten al usuario escribir y editar texto:

- [`Entry`](xref:Xamarin.Forms.Entry) para una sola línea de texto
- [`Editor`](xref:Xamarin.Forms.Editor) para varias líneas de texto
- [`SearchBar`](xref:Xamarin.Forms.SearchBar) para una sola línea de texto para fines de búsqueda.

`Entry` y `Editor` derivan de [`InputView`](xref:Xamarin.Forms.InputView), que deriva de `View`. `SearchBar` deriva directamente de `View`.

### <a name="keyboard-and-focus"></a>Teclado y el foco

En teléfonos y tabletas sin teclados físicos, los elementos `Entry`, `Editor`y `SearchBar` producen un teclado virtual emergente. La presencia de este teclado en la pantalla está relacionado con el foco de entrada. Una vista debe tener las propiedades [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) y [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) establecidas en `true` para obtener el foco de entrada.

Dos métodos, una propiedad de solo lectura y dos eventos relacionados con el foco de entrada. Todos se definen mediante `VisualElement`:

- El método [`Focus`](xref:Xamarin.Forms.VisualElement.Focus) intenta establecer el foco de entrada en un elemento y devuelve `true` si se realiza correctamente.
- El método [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus) quita el foco de entrada de un elemento
- La propiedad [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused) de solo lectura indica si el elemento tiene el foco de entrada
- El evento [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) indica cuándo un elemento obtiene el foco de entrada
- El evento [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) indica cuándo un elemento pierde el foco de entrada

### <a name="choosing-the-keyboard"></a>Elegir el teclado

La clase [`InputView`](xref:Xamarin.Forms.InputView) de la que derivan `Entry` y `Editor` solo define una propiedad:

- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) de tipo [`Keyboard`](xref:Xamarin.Forms.Keyboard)

Indica el tipo de teclado que se muestra. Algunos teclados están optimizados para números o URI.

La clase `Keyboard` permite definir un teclado con un método de [`Keyboard.Create`](xref:Xamarin.Forms.Keyboard.Create(Xamarin.Forms.KeyboardFlags)) estático con un argumento de tipo [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags), una enumeración con las siguientes marcas de bits:

- `None` establecer en 0
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) establecida en 1
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) establecido en 2
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) establecido en 4
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) establece en \xFFFFFFFF

Al utilizar el [`Editor`](xref:Xamarin.Forms.Editor) multilínea cuando se espera un párrafo o más texto, llamar a `Keyboard.Create` es un buen enfoque para seleccionar un teclado. En el caso de la [`Entry`](xref:Xamarin.Forms.Entry)de una sola línea, las siguientes propiedades de solo lectura estáticas de `Keyboard` son útiles:

- [`Default`](xref:Xamarin.Forms.Keyboard.Default)
- [`Text`](xref:Xamarin.Forms.Keyboard.Text)
- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat)
- [`Url`](xref:Xamarin.Forms.Keyboard.Url)
- [`Email`](xref:Xamarin.Forms.Keyboard.Email)
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone)
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) para los números positivos con o sin punto decimal.

El [`KeyboardTypeConverter`](xref:Xamarin.Forms.KeyboardTypeConverter) permite especificar estas propiedades en XAML tal y como se muestra en el programa [**EntryKeyboards**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) .

### <a name="entry-properties-and-events"></a>Eventos y propiedades de entrada

El [`Entry`](xref:Xamarin.Forms.Entry) de una sola línea define las siguientes propiedades:

- [`Text`](xref:Xamarin.Forms.InputView.Text) de tipo `string`, el texto que aparece en el `Entry`
- [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) de tipo `Color`
- [`FontFamily`](xref:Xamarin.Forms.Entry.FontFamily) de tipo `string`
- [`FontSize`](xref:Xamarin.Forms.Entry.FontSize) de tipo `double`
- [`FontAttributes`](xref:Xamarin.Forms.Entry.FontAttributes) de tipo `FontAttributes`
- [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) de tipo `bool`, que hace que los caracteres estén enmascarados
- [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) de tipo `string`, para el texto de color dimly que aparece en el `Entry` antes de que se escriba algo
- [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) de tipo `Color`

En el `Entry` también se definen dos eventos:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) con un objeto [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) , que se desencadena cuando cambia la propiedad `Text`
- [`Completed`](xref:Xamarin.Forms.Entry.Completed), se desencadena cuando el usuario finaliza y se descarta el teclado. El usuario indica la finalización de una manera específica de la plataforma

En el ejemplo [**QuadraticEquations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) se muestran estos dos eventos.

### <a name="the-editor-difference"></a>La diferencia del Editor

El [`Editor`](xref:Xamarin.Forms.Editor) multilínea define las mismas propiedades `Text` y `Font` que `Entry`, pero no las demás propiedades. `Editor` también define las dos mismas propiedades que `Entry`.

[**JustNotes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) es un programa de toma de notas de forma libre que guarda y restaura el contenido de la `Editor`.

### <a name="the-searchbar"></a>El SearchBar

El [`SearchBar`](xref:Xamarin.Forms.SearchBar) no se deriva de `InputView`, por lo que no tiene una propiedad `Keyboard`. Pero tiene todas las propiedades `Text`, `Font`y `Placeholder` que define `Entry`. Además, `SearchBar` define tres propiedades adicionales:

- [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) de tipo `Color`
- [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) de tipo [`ICommand`](xref:System.Windows.Input.ICommand) para su uso con enlaces de datos y MVVM
- [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) de tipo `Object`, para su uso con `SearchCommand`

El específico de la plataforma Cancelar botón borra el texto. El `SearchBar` también tiene un botón de búsqueda específico de la plataforma. Al presionar cualquiera de estos botones, se produce uno de los dos eventos que `SearchBar` define:

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) acompañado de un objeto [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs)
- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed)

En el ejemplo [**SearchBarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) se muestra el `SearchBar`.

## <a name="date-and-time-selection"></a>Selección de fecha y hora

Las vistas [`DatePicker`](xref:Xamarin.Forms.DatePicker) y [`TimePicker`](xref:Xamarin.Forms.TimePicker) implementan controles específicos de la plataforma que permiten al usuario especificar una fecha o una hora.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](xref:Xamarin.Forms.DatePicker) define cuatro propiedades:

- [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) de tipo `DateTime`, inicializado en el 1 de enero de 1900
- [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) de tipo `DateTime`, inicializado hasta el 31 de diciembre de 2100
- [`Date`](xref:Xamarin.Forms.DatePicker.Date) de tipo `DateTime`, inicializado en `DateTime.Today`
- [`Format`](xref:Xamarin.Forms.DatePicker.Format) de tipo `string`, la cadena de formato .net inicializada en "d", el patrón de fecha corta, lo que da como resultado una fecha que se muestra como "7/20/1969" en Estados Unidos.

Puede establecer las propiedades de `DateTime` en XAML si expresa las propiedades como elementos de propiedad y usa el formato de fecha corta ("7/20/1969") de referencia cultural invariable.   

El ejemplo [**DaysBetweenDates**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) calcula el número de días entre dos fechas seleccionadas por el usuario.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>El objeto TimePicker (o es una TimeSpanPicker?)

[`TimePicker`](xref:Xamarin.Forms.TimePicker) define dos propiedades y sin eventos:

- [`Time`](xref:Xamarin.Forms.TimePicker.Time) es de tipo `TimeSpan` en lugar de `DateTime`, lo que indica el tiempo transcurrido desde la medianoche.
- [`Format`](xref:Xamarin.Forms.TimePicker.Format) de tipo `string`, la cadena de formato .net inicializada en "t", el patrón de hora corta, lo que da lugar a una presentación de hora como "1:45 PM" en Estados Unidos.

El programa [**SetTimer**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) muestra cómo usar el `TimePicker` para especificar una hora para un temporizador. El programa sólo funciona si la mantiene en primer plano.

**SetTimer** también muestra cómo usar el método [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) de `Page` para mostrar un cuadro de alerta.

## <a name="related-links"></a>Vínculos relacionados

- [Capítulo 15 texto completo (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Ejemplos del capítulo 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Slider](~/xamarin-forms/user-interface/slider.md)
- [Entrada](~/xamarin-forms/user-interface/text/entry.md)
- [Editor](~/xamarin-forms/user-interface/text/editor.md)
- [DatePicker](~/xamarin-forms/user-interface/datepicker.md)
