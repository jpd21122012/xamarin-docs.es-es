---
title: Controles personalizados en el Xamarin Designer para iOS
description: El Xamarin Designer para iOS admite la representación de controles personalizados creados en el proyecto o a los que se hace referencia desde orígenes externos como el almacén de componentes de Xamarin.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: e8c38ec407d13a99e2990a6d4cf39b5a23728b1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003973"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Controles personalizados en el Xamarin Designer para iOS

_El Xamarin Designer para iOS admite la representación de controles personalizados creados en el proyecto o a los que se hace referencia desde orígenes externos como el almacén de componentes de Xamarin._

El Xamarin Designer para iOS es una herramienta eficaz para visualizar la interfaz de usuario de una aplicación y proporciona compatibilidad con la edición WYSIWYG para la mayoría de las vistas y los controladores de vistas de iOS. La aplicación también puede contener controles personalizados que extienden los que están integrados en iOS. Si estos controles personalizados se escriben teniendo en cuenta algunas instrucciones, también se pueden representar en el diseñador de iOS, lo que proporciona una experiencia de edición incluso más enriquecida. En este documento se examinan esas directrices.

## <a name="requirements"></a>Requisitos

En la superficie de diseño se representará un control que cumpla todos los requisitos siguientes:

1. Es una subclase directa o indirecta de [UIView](xref:UIKit.UIView) o [UIViewController](xref:UIKit.UIViewController). Otras subclases [NSObject](xref:Foundation.NSObject) aparecerán como un icono en la superficie de diseño.
2. Tiene un [RegisterAttribute](xref:Foundation.RegisterAttribute) para exponerlo a Objective-C.
3. Tiene [el constructor IntPtr necesario](~/ios/internals/api-design/index.md).
4. Implementa la interfaz [IComponent](xref:System.ComponentModel.IComponent) o tiene un [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) establecido en true.

Los controles definidos en el código que cumplan los requisitos anteriores aparecerán en el diseñador cuando su proyecto contenedor se compile para el simulador. De forma predeterminada, todos los controles personalizados aparecerán en la sección **componentes personalizados** del **cuadro de herramientas**. Sin embargo, el [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) se puede aplicar a la clase del control personalizado para especificar una sección diferente.

El diseñador no admite la carga de bibliotecas de otros fabricantes de Objective-C.

## <a name="custom-properties"></a>Propiedades personalizadas

Una propiedad declarada por un control personalizado aparecerá en el panel de propiedades si se cumplen las condiciones siguientes:

1. La propiedad tiene un captador y un establecedor públicos.
1. La propiedad tiene un [ExportAttribute](xref:Foundation.ExportAttribute) , así como un [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) establecido en true.
1. El tipo de propiedad es un tipo numérico, un tipo de enumeración, una cadena, bool, [SizeF](xref:System.Drawing.SizeF), [UIColor](xref:UIKit.UIColor)o [UIImage](xref:UIKit.UIImage). Esta lista de tipos admitidos puede expandirse en el futuro.

La propiedad también se puede decorar con un [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) para especificar la etiqueta que se muestra en el panel de propiedades.

## <a name="initialization"></a>Inicialización

En el caso de las subclases de `UIViewController`, debe usar el método [ViewDidLoad](xref:UIKit.UIViewController.ViewDidLoad) para el código que depende de las vistas que creó en el diseñador.

Por `UIView` y otras subclases de `NSObject`, el método [AwakeFromNib](xref:Foundation.NSObject.AwakeFromNib) es el lugar recomendado para realizar la inicialización del control personalizado una vez que se carga desde el archivo de diseño. Esto se debe a que las propiedades personalizadas establecidas en el panel de propiedades no se establecerán cuando se ejecute el constructor del control, pero se establecerán antes de que se llame a `AwakeFromNib`:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

Si el control también está diseñado para crearse directamente desde el código, puede que desee crear un método que tenga código de inicialización común, como el siguiente:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>Inicialización de propiedad y AwakeFromNib

Se debe tener cuidado cuando y dónde inicializar las propiedades diseñables en un componente personalizado para no sobrescribir los valores que se han establecido en el diseñador de iOS. Como ejemplo, tome el siguiente código:

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

El componente `CustomView` expone una propiedad `Counter` que el desarrollador puede establecer en el diseñador de iOS. Sin embargo, independientemente del valor que se establezca en el diseñador, el valor de la propiedad `Counter` siempre será cero (0). El motivo es el siguiente:

- Una instancia del `CustomControl` se infla a partir del archivo de guion gráfico.
- Se establecen todas las propiedades modificadas en el diseñador de iOS (por ejemplo, al establecer el valor de la `Counter` en dos (2), por ejemplo).
- Se ejecuta el método `AwakeFromNib` y se realiza una llamada al método `Initialize` del componente.
- Dentro `Initialize` el valor de la propiedad `Counter` se restablece a cero (0).

Para corregir la situación anterior, inicialice la propiedad `Counter` en otra parte (como en el constructor del componente) o no invalide el método `AwakeFromNib` y llame a `Initialize` si el componente no requiere ninguna inicialización adicional fuera de lo que está actualmente controlado por sus constructores.

## <a name="design-mode"></a>Modo de diseño

En la superficie de diseño, un control personalizado debe cumplir algunas restricciones:

- Los recursos del lote de aplicaciones no están disponibles en el modo de diseño. Las imágenes están disponibles cuando se cargan a través de [los métodos UIImage](xref:UIKit.UIImage) .
- Las operaciones asincrónicas, como las solicitudes Web, no se deben realizar en modo de diseño. La superficie de diseño no admite animaciones ni otras actualizaciones asincrónicas en la interfaz de usuario del control.

Un control personalizado puede implementar [IComponent](xref:System.ComponentModel.IComponent) y utilizar la propiedad [DesignMode](xref:System.ComponentModel.ISite.DesignMode) para comprobar si está en la superficie de diseño. En este ejemplo, la etiqueta mostrará "modo de diseño" en la superficie de diseño y "tiempo de ejecución" en tiempo de ejecución:

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null && Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

Siempre debe comprobar la propiedad `Site` de `null` antes de intentar tener acceso a cualquiera de sus miembros. Si `Site` es `null`, es seguro suponer que el control no se está ejecutando en el diseñador.
En el modo de diseño, `Site` se establecerá después de que se haya ejecutado el constructor del control y antes de que se llame a `AwakeFromNib`.

## <a name="debugging"></a>Depuración

Un control que cumpla los requisitos anteriores se mostrará en el cuadro de herramientas y se representará en la superficie.
Si un control no se representa, compruebe si hay errores en el control o una de sus dependencias.

A menudo, la superficie de diseño puede detectar las excepciones producidas por los controles individuales mientras continúa representando otros controles. El control defectuoso se reemplaza por un marcador de posición rojo y puede ver el seguimiento de la excepción haciendo clic en el icono de exclamación:

 ![](ios-designable-controls-overview-images/exception-box.png "A faulty control as red placeholder and the exception details")

Si los símbolos de depuración están disponibles para el control, el seguimiento tendrá nombres de archivo y números de línea.
Si hace doble clic en una línea del seguimiento de la pila, se saltará a esa línea en el código fuente.

Si el diseñador no puede aislar el control defectuoso, aparecerá un mensaje de advertencia en la parte superior de la superficie de diseño:

 ![](ios-designable-controls-overview-images/info-bar.png "A warning message at the top of the design surface")

La representación completa se reanudará cuando el control defectuoso se corrija o se quite de la superficie de diseño.

## <a name="summary"></a>Resumen

En este artículo se presentó la creación y la aplicación de controles personalizados en el diseñador de iOS. En primer lugar, se describen los requisitos que los controles deben cumplir para que se representen en la superficie de diseño y se expongan propiedades personalizadas en el panel de propiedades. A continuación, se examina el código subyacente de la inicialización del control y la propiedad DesignMode. Por último, se describe lo que sucede cuando se producen excepciones y cómo resolver este problema.
