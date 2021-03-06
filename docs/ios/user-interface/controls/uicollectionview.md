---
title: Vistas de colección en Xamarin. iOS
description: Las vistas de colección permiten mostrar el contenido mediante diseños arbitrarios. Permiten crear diseños similares a cuadrículas de forma sencilla, a la vez que también admiten diseños personalizados.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: b7f8452f0f085a8a15f188534851e8926d13f377
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021838"
---
# <a name="collection-views-in-xamarinios"></a>Vistas de colección en Xamarin. iOS

_Las vistas de colección permiten mostrar el contenido mediante diseños arbitrarios. Permiten crear diseños similares a cuadrículas de forma sencilla, a la vez que también admiten diseños personalizados._

Las vistas de colección, disponibles en la clase `UICollectionView`, son un nuevo concepto en iOS 6 que presenta la presentación de varios elementos en la pantalla mediante diseños. Los patrones para proporcionar datos a una `UICollectionView` para crear elementos e interactuar con dichos elementos siguen los mismos patrones de delegación y de origen de datos que se usan habitualmente en el desarrollo de iOS.

Sin embargo, las vistas de colección funcionan con un subsistema de diseño que es independiente del propio `UICollectionView`. Por lo tanto, simplemente proporcionar un diseño diferente puede cambiar fácilmente la presentación de una vista de colección.

iOS proporciona una clase de diseño denominada `UICollectionViewFlowLayout` que permite crear diseños basados en líneas, como una cuadrícula, sin trabajo adicional. Además, también se pueden crear diseños personalizados que permitan cualquier presentación que pueda imaginar.

## <a name="uicollectionview-basics"></a>Aspectos básicos de UICollectionView

La clase `UICollectionView` se compone de tres elementos diferentes:

- **Celdas** : vistas controladas por datos para cada elemento
- **Vistas complementarias** : vistas controladas por datos asociadas a una sección.
- **Vistas de decoración** : vistas no controladas por datos creadas por un diseño

## <a name="cells"></a>Celdas

Las celdas son objetos que representan un único elemento en el conjunto de datos que la vista de colección está presentando. Cada celda es una instancia de la clase `UICollectionViewCell`, que se compone de tres vistas diferentes, como se muestra en la ilustración siguiente:

 [![](uicollectionview-images/01-uicollectionviewcell.png "Each cell is composed of three different views, as shown here")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

La clase `UICollectionViewCell` tiene las siguientes propiedades para cada una de estas vistas:

- `ContentView`: esta vista contiene el contenido que presenta la celda. Se representa en el orden z superior de la pantalla.
- `SelectedBackgroundView`: las celdas tienen compatibilidad integrada para la selección. Esta vista se usa para indicar visualmente que se ha seleccionado una celda. Se representa justo debajo del `ContentView` cuando se selecciona una celda.
- `BackgroundView`: las celdas también pueden mostrar un fondo, que presenta el `BackgroundView`. Esta vista se representa debajo del `SelectedBackgroundView`.

Al establecer el `ContentView` de modo que sea menor que el `BackgroundView` y el `SelectedBackgroundView`, el `BackgroundView` se puede usar para enmarcar visualmente el contenido, mientras que la `SelectedBackgroundView` se mostrará cuando se seleccione una celda, como se muestra a continuación. :

 [![](uicollectionview-images/02-cells.png "The different cell elements")](uicollectionview-images/02-cells.png#lightbox)

Las celdas de la captura de pantalla anterior se crean heredando de `UICollectionViewCell` y estableciendo las propiedades `ContentView`, `SelectedBackgroundView` y `BackgroundView`, respectivamente, tal como se muestra en el código siguiente:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />

## <a name="supplementary-views"></a>Vistas complementarias

Las vistas complementarias son vistas que presentan información asociada a cada sección de una `UICollectionView`. Al igual que las celdas, las vistas complementarias están controladas por datos. Cuando las celdas presenten los datos de los elementos de un origen de datos, las vistas complementarias presentan los datos de la sección, como las categorías de libro en una estantería o el género de música en una biblioteca de música.

Por ejemplo, una vista complementaria podría usarse para presentar un encabezado para una sección determinada, tal como se muestra en la ilustración siguiente:

 [![](uicollectionview-images/02a-supplementary-view.png "A Supplementary View used to present a header for a particular section, as shown here")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Para usar una vista complementaria, primero debe registrarse en el método `ViewDidLoad`:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

A continuación, la vista debe devolverse mediante `GetViewForSupplementaryElement`, creada con `DequeueReusableSupplementaryView`y hereda de `UICollectionReusableView`. El siguiente fragmento de código generará el SupplementaryView que se muestra en la captura de pantalla anterior:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Las vistas complementarias son más genéricas que solo los encabezados y pies de página.
Se pueden colocar en cualquier parte de la vista de colección y pueden estar formadas por cualquier vista, por lo que su apariencia es totalmente personalizable.

 <a name="Decoration_Views" />

## <a name="decoration-views"></a>Vistas de decoración

Las vistas de decoración son vistas meramente visuales que se pueden mostrar en un `UICollectionView`. A diferencia de las celdas y las vistas complementarias, no están controladas por datos. Siempre se crean dentro de la subclase de un diseño y, posteriormente, pueden cambiar como el diseño del contenido. Por ejemplo, una vista de decoración podría usarse para presentar una vista de fondo que se desplaza con el contenido del `UICollectionView`, como se muestra a continuación:

 [![](uicollectionview-images/02c-decoration-view.png "Decoration View with a red background")](uicollectionview-images/02c-decoration-view.png#lightbox)

 El siguiente fragmento de código cambia el fondo a rojo en la clase samples `CircleLayout`:

 ```csharp
 public class MyDecorationView : UICollectionReusableView
  {
    [Export ("initWithFrame:")]
    public MyDecorationView (CGRect frame) : base (frame)
    {
      BackgroundColor = UIColor.Red;
    }
  }
 ```

## <a name="data-source"></a>Origen de datos

Como con otras partes de iOS, como `UITableView` y `MKMapView`, `UICollectionView` obtiene sus datos de un *origen de datos*, que se expone en Xamarin. iOS a través de la clase **`UICollectionViewDataSource`** . Esta clase es responsable de proporcionar contenido al `UICollectionView` como:

- **Celdas** : devueltas por `GetCell` método.
- **Vistas complementarias** : devueltos por `GetViewForSupplementaryElement` método.
- **Número de secciones** : devueltas por `NumberOfSections` método. El valor predeterminado es 1 si no se implementa.
- **Número de elementos por sección** : devueltos por `GetItemsCount` método.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Para mayor comodidad, la clase `UICollectionViewController` está disponible. Esto se configura automáticamente para que sea el delegado, que se describe en la sección siguiente, y el origen de datos para su `UICollectionView` vista.

Al igual que con `UITableView`, la clase `UICollectionView` solo llamará a su origen de datos para obtener las celdas de los elementos que se encuentran en la pantalla.
Las celdas que se desplazan fuera de la pantalla se colocan en una cola para su reutilización, como se muestra en la siguiente imagen:

 [![](uicollectionview-images/03-cell-reuse.png "Cells that scroll off the screen are placed in to a queue for reuse as shown here")](uicollectionview-images/03-cell-reuse.png#lightbox)

La reutilización de la celda se ha simplificado con `UICollectionView` y `UITableView`. Ya no es necesario crear una celda directamente en el origen de datos si no hay ninguna disponible en la cola reutilizable, ya que las celdas se registran con el sistema. Si una celda no está disponible al realizar la llamada para quitar la cola de la cola de reutilizar, iOS la creará automáticamente basándose en el tipo o NIB que se registró.
También está disponible la misma técnica para las vistas complementarias.

Por ejemplo, considere el siguiente código, que registra la clase `AnimalCell`:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Cuando un `UICollectionView` necesita una celda porque su elemento está en la pantalla, el `UICollectionView` llama al método `GetCell` de su origen de datos. De forma similar a como funciona con UITableView, este método es responsable de configurar una celda a partir de los datos de respaldo, que sería una clase `AnimalCell` en este caso.

En el código siguiente se muestra una implementación de `GetCell` que devuelve una instancia de `AnimalCell`:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

La llamada a `DequeReusableCell` es donde la celda se eliminará de la cola de reutilización o, si una celda no está disponible en la cola, se creará en función del tipo registrado en la llamada a `CollectionView.RegisterClassForCell`.

En este caso, mediante el registro de la clase `AnimalCell`, iOS creará un nuevo `AnimalCell` internamente y lo devolverá cuando se realice una llamada para quitar de la cola una celda, después de la cual se configura con la imagen contenida en la clase animal y devuelta para su presentación en el `UICollectionView`.

 <a name="Delegate" />

### <a name="delegate"></a>delegado

La clase `UICollectionView` usa un delegado de tipo `UICollectionViewDelegate` para admitir la interacción con el contenido de la `UICollectionView`. Esto permite controlar:

- **Selección de celdas** : determina si una celda está seleccionada.
- **Resaltado de celdas** : determina si una celda se está tocando actualmente.
- **Menús de celda** : menú que se muestra para una celda en respuesta a un gesto largo de presionar.

Al igual que con el origen de datos, el `UICollectionViewController` se configura de forma predeterminada como el delegado del `UICollectionView`.

 <a name="Cell_HighLighting" />

#### <a name="cell-highlighting"></a>Resaltado de celdas

Cuando se presiona una celda, la celda pasa a un estado resaltado y no se selecciona hasta que el usuario levanta el dedo de la celda. Esto permite un cambio temporal en el aspecto de la celda antes de que se seleccione realmente. Cuando se selecciona, se muestra la `SelectedBackgroundView` de la celda. En la siguiente ilustración se muestra el estado resaltado justo antes de que se produzca la selección:

 [![](uicollectionview-images/04-cell-highlight.png "This figure shows the highlighted state just before the selection occurs")](uicollectionview-images/04-cell-highlight.png#lightbox)

Para implementar el resaltado, se pueden usar los métodos `ItemHighlighted` y `ItemUnhighlighted` de la `UICollectionViewDelegate`. Por ejemplo, el código siguiente aplicará un fondo amarillo del `ContentView` cuando la celda esté resaltada y un fondo blanco cuando no se resalte, tal como se muestra en la imagen anterior:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />

#### <a name="disabling-selection"></a>Deshabilitar la selección

La selección está habilitada de forma predeterminada en `UICollectionView`. Para deshabilitar la selección, invalide `ShouldHighlightItem` y devuelva false como se muestra a continuación:

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Cuando el resaltado está deshabilitado, el proceso de selección de una celda también está deshabilitado. Además, también hay un método `ShouldSelectItem` que controla la selección directamente, aunque si se implementa `ShouldHighlightItem` y devuelve false, no se llama a `ShouldSelectItem`.

 `ShouldSelectItem` permite activar o desactivar la selección para cada elemento, cuando no se implemente `ShouldHighlightItem`. También permite resaltar sin selección, si se implementa `ShouldHighlightItem` y devuelve true, mientras que `ShouldSelectItem` devuelve false.

 <a name="Cell_Menus" />

#### <a name="cell-menus"></a>Menús de celda

Cada celda de una `UICollectionView` es capaz de mostrar un menú que permita que se admita opcionalmente cortar, copiar y pegar. Para crear un menú de edición en una celda:

1. Invalide `ShouldShowMenu` y devuelva true si el elemento debe mostrar un menú.
1. Invalide `CanPerformAction` y devuelva true para cada acción que pueda realizar el elemento, que será cualquier corte, copia o pegado.
1. Invalide `PerformAction` para realizar la operación de edición, copia de pegado.

En la captura de pantalla siguiente se muestra el menú cuando se presiona una celda larga:

 [![](uicollectionview-images/04a-menu.png "This screenshot show the menu when a cell is long pressed")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />

## <a name="layout"></a>Diseño

`UICollectionView` admite un sistema de diseño que permite que el posicionamiento de todos sus elementos, celdas, vistas complementarias y vistas de decoración se administre independientemente del `UICollectionView` mismo.
Con el sistema de diseño, una aplicación puede admitir diseños como el tipo de cuadrícula que se ha visto en este artículo, así como proporcionar diseños personalizados.

 <a name="Layout_Basics" />

### <a name="layout-basics"></a>Conceptos básicos del diseño

Los diseños de un `UICollectionView` se definen en una clase que hereda de `UICollectionViewLayout`. La implementación del diseño es responsable de la creación de los atributos de diseño para cada elemento del `UICollectionView`. Hay dos maneras de crear un diseño:

- Use la `UICollectionViewFlowLayout` integrada.
- Proporcione un diseño personalizado heredando de `UICollectionViewLayout`.

 <a name="Flow_Layout" />

### <a name="flow-layout"></a>Diseño de flujo

La clase `UICollectionViewFlowLayout` proporciona un diseño basado en líneas que es adecuado para organizar el contenido en una cuadrícula de celdas como hemos visto.

Para usar un diseño de flujo:

- Cree una instancia de `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
```

- Pase la instancia al constructor del `UICollectionView`:

```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

Esto es todo lo que se necesita para diseñar el contenido en una cuadrícula. Además, cuando cambia la orientación, el `UICollectionViewFlowLayout` controla la reorganización del contenido correctamente, como se muestra a continuación:

 [![](uicollectionview-images/05-layout-orientation.png "Example of the orientation changes")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />

#### <a name="section-inset"></a>Margen de la sección

Para proporcionar espacio alrededor del `UIContentView`, los diseños tienen una propiedad `SectionInset` de tipo `UIEdgeInsets`. Por ejemplo, el código siguiente proporciona un búfer de 50 píxeles alrededor de cada sección del `UIContentView` cuando lo dispone un `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Esto da lugar a un espaciado alrededor de la sección, como se muestra a continuación:

 [![](uicollectionview-images/06-sectioninset.png "Spacing around the section as shown here")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />

#### <a name="subclassing-uicollectionviewflowlayout"></a>Subclase de UICollectionViewFlowLayout

En la edición para usar `UICollectionViewFlowLayout` directamente, también se pueden crear subclases para personalizar aún más el diseño del contenido a lo largo de una línea. Por ejemplo, se puede usar para crear un diseño que no ajusta las celdas en una cuadrícula, sino que crea una sola fila con un efecto de desplazamiento horizontal, como se muestra a continuación:

 [![](uicollectionview-images/07-line-layout.png "A single row with a horizontal scrolling effect")](uicollectionview-images/07-line-layout.png#lightbox)

Para implementar esto mediante subclases `UICollectionViewFlowLayout` requiere:

- Inicializar las propiedades de diseño que se aplican al propio diseño o a todos los elementos del diseño en el constructor.
- Reemplazar `ShouldInvalidateLayoutForBoundsChange`, devolver true para que cuando cambien los límites del `UICollectionView`, se vuelva a calcular el diseño de las celdas. Esto se usa en este caso, asegúrese de que el código de transformación que se aplica a la celda centermost se aplicará durante el desplazamiento.
- Invalidar `TargetContentOffset` para hacer que la celda centermost se ajuste al centro de la `UICollectionView` como se detiene el desplazamiento.
- Reemplazar `LayoutAttributesForElementsInRect` para devolver una matriz de `UICollectionViewLayoutAttributes`. Cada `UICollectionViewLayoutAttribute` contiene información sobre cómo diseñar el elemento determinado, incluidas propiedades como su `Center`, `Size`, `ZIndex` y `Transform3D`.

En el código siguiente se muestra este tipo de implementación:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
  public class LineLayout : UICollectionViewFlowLayout
  {
    public const float ITEM_SIZE = 200.0f;
    public const int ACTIVE_DISTANCE = 200;
    public const float ZOOM_FACTOR = 0.3f;

    public LineLayout ()
    {
      ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
      ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
      MinimumLineSpacing = 50.0f;
    }

    public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
    {
      return true;
    }

    public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
    {
      var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

      foreach (var attributes in array) {
        if (attributes.Frame.IntersectsWith (rect)) {
          float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
          float normalizedDistance = distance / ACTIVE_DISTANCE;
          if (Math.Abs (distance) < ACTIVE_DISTANCE) {
            float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
            attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
            attributes.ZIndex = 1;
          }
        }
      }
      return array;
    }

    public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
    {
      float offSetAdjustment = float.MaxValue;
      float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
      CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
      var array = base.LayoutAttributesForElementsInRect (targetRect);
      foreach (var layoutAttributes in array) {
        float itemHorizontalCenter = (float)layoutAttributes.Center.X;
        if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
          offSetAdjustment = itemHorizontalCenter - horizontalCenter;
        }
      }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
    }

  }
}
```

 <a name="Custom_Layout" />

### <a name="custom-layout"></a>Diseño personalizado

Además de usar `UICollectionViewFlowLayout`, los diseños también se pueden personalizar completamente heredando directamente de `UICollectionViewLayout`.

Los métodos clave que se van a invalidar son:

- `PrepareLayout`: se usa para realizar cálculos geométricos iniciales que se usarán a lo largo del proceso de diseño.
- `CollectionViewContentSize`: devuelve el tamaño del área que se usa para mostrar el contenido.
- `LayoutAttributesForElementsInRect`: como en el ejemplo de UICollectionViewFlowLayout mostrado anteriormente, este método se utiliza para proporcionar información al `UICollectionView` sobre cómo diseñar cada elemento. Sin embargo, a diferencia del `UICollectionViewFlowLayout`, al crear un diseño personalizado, puede colocar los elementos de la forma que prefiera.

Por ejemplo, el mismo contenido podría aparecer en un diseño circular, tal como se muestra a continuación:

 [![](uicollectionview-images/08-circle-layout.png "A circular custom layout as shown here")](uicollectionview-images/08-circle-layout.png#lightbox)

Lo más eficaz de los diseños es que cambiar del diseño de la cuadrícula a un diseño de desplazamiento horizontal y, posteriormente, a este diseño circular solo requiere la clase de diseño proporcionada a la `UICollectionView` cambiar. Nada en el `UICollectionView`, su delegado o el código de origen de datos cambia en absoluto.

## <a name="changes-in-ios-9"></a>Cambios en iOS 9

En iOS 9, la vista de colección (`UICollectionView`) admite ahora arrastrar reordenación de elementos de forma predeterminada agregando un nuevo reconocedor de gestos predeterminado y varios métodos de compatibilidad nuevos.

Con estos nuevos métodos, puede implementar fácilmente arrastrar para reordenar en la vista de colección y tener la opción de personalizar la apariencia de los elementos durante cualquier fase del proceso de reordenación.

[![](uicollectionview-images/intro01.png "An example of the reordering process")](uicollectionview-images/intro01.png#lightbox)

En este artículo, echaremos un vistazo a la implementación de la función de arrastrar y volver a ordenar en una aplicación de Xamarin. iOS, así como algunos de los cambios que iOS 9 ha realizado en el control de vista de colección:

- [Reordenación sencilla de los elementos](#Easy-Reordering-of-Items)
  - [Ejemplo de reordenación simple](#Simple-Reordering-Example)
  - [Usar un reconocedor de gestos personalizado](#Using-a-Custom-Gesture-Recognizer)
  - [Diseños personalizados y reordenación](#Custom-Layouts-and-Reording)
- [Cambios en la vista de colección](#collection-view-changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Reordenación de elementos

Como se indicó anteriormente, uno de los cambios más importantes en la vista de recopilación en iOS 9 fue la adición de una funcionalidad sencilla de arrastrar a reordenar de forma rápida.

En iOS 9, la manera más rápida de agregar la reordenación a una vista de colección es usar una `UICollectionViewController`.
El controlador de vista de colección ahora tiene una propiedad `InstallsStandardGestureForInteractiveMovement`, que agrega un *reconocedor de gestos* estándar que admite arrastrar para reordenar los elementos de la colección.
Puesto que el valor predeterminado es `true`, solo tiene que implementar el método `MoveItem` de la clase `UICollectionViewDataSource` para admitir la función de arrastrar y volver a ordenar. Por ejemplo:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
  // Reorder our list of items
  ...
}
```

<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Ejemplo de reordenación simple

Como ejemplo rápido, inicie un nuevo proyecto de Xamarin. iOS y edite el archivo **Main. Storyboard** . Arrastre un `UICollectionViewController` hasta la superficie de diseño:

[![](uicollectionview-images/quick01.png "Adding a UICollectionViewController")](uicollectionview-images/quick01.png#lightbox)

Seleccione la vista de colección (puede que sea más fácil hacerlo desde el esquema del documento). En la pestaña diseño del Panel de propiedades, establezca los siguientes tamaños, tal como se muestra en la siguiente captura de pantalla:

- **Tamaño de celda**: ancho: 60 | Alto: 60
- **Tamaño del encabezado**: width – 0 | Alto: 0
- **Tamaño de pie de página**: ancho – 0 | Alto: 0
- **Espaciado mínimo**: para las celdas: 8 | Para las líneas: 8
- **Insets de sección**: Top-16 | Inferior: 16 | Izquierda: 16 | Derecho: 16

[![](uicollectionview-images/quick04.png "Set the Collection View sizes")](uicollectionview-images/quick04.png#lightbox)

A continuación, edite la celda predeterminada:

- Cambiar el color de fondo a azul
- Agregue una etiqueta para que actúe como título de la celda
- Establecimiento del identificador de reutilización en la **celda**

[![](uicollectionview-images/quick02.png "Edit the default Cell")](uicollectionview-images/quick02.png#lightbox)

Agregue restricciones para mantener la etiqueta centrada dentro de la celda a medida que cambia de tamaño:

En el **Panel de propiedades** de _CollectionViewCell_ y establezca la **clase** en `TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "Set the Class to TextCollectionViewCell")](uicollectionview-images/quick05.png#lightbox)

Establezca la **vista reutilizable** de la colección en `Cell`:

[![](uicollectionview-images/quick06.png "Set the Collection Reusable View to Cell")](uicollectionview-images/quick06.png#lightbox)

Por último, seleccione la etiqueta y asígnele el nombre `TextLabel`:

[![](uicollectionview-images/quick07.png "name label TextLabel")](uicollectionview-images/quick07.png#lightbox)

Edite la clase `TextCollectionViewCell` y agregue las siguientes propiedades:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
  public partial class TextCollectionViewCell : UICollectionViewCell
  {
    #region Computed Properties
    public string Title {
      get { return TextLabel.Text; }
      set { TextLabel.Text = value; }
    }
    #endregion

    #region Constructors
    public TextCollectionViewCell (IntPtr handle) : base (handle)
    {
    }
    #endregion
  }
}
```

Aquí, la propiedad `Text` de la etiqueta se expone como el título de la celda, por lo que se puede establecer desde el código.

Agregue una nueva C# clase al proyecto y llámela`WaterfallCollectionSource`. Edite el archivo y haga que tenga el aspecto siguiente:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
  public class WaterfallCollectionSource : UICollectionViewDataSource
  {
    #region Computed Properties
    public WaterfallCollectionView CollectionView { get; set;}
    public List<int> Numbers { get; set; } = new List<int> ();
    #endregion

    #region Constructors
    public WaterfallCollectionSource (WaterfallCollectionView collectionView)
    {
      // Initialize
      CollectionView = collectionView;

      // Init numbers collection
      for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
      }
    }
    #endregion

    #region Override Methods
    public override nint NumberOfSections (UICollectionView collectionView) {
      // We only have one section
      return 1;
    }

    public override nint GetItemsCount (UICollectionView collectionView, nint section) {
      // Return the number of items
      return Numbers.Count;
    }

    public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
    {
      // Get a reusable cell and set {~~it's~>its~~} title from the item
      var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
      cell.Title = Numbers [(int)indexPath.Item].ToString();

      return cell;
    }

    public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
      // We can always move items
      return true;
    }

    public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
    {
      // Reorder our list of items
      var item = Numbers [(int)sourceIndexPath.Item];
      Numbers.RemoveAt ((int)sourceIndexPath.Item);
      Numbers.Insert ((int)destinationIndexPath.Item, item);
    }
    #endregion
  }
}
```

Esta clase será el origen de datos para nuestra vista de colección y proporcionará la información para cada celda de la colección.
Observe que el método `MoveItem` se implementa para permitir que los elementos de la colección se arrastren en orden.

Agregue otra clase C# nueva al proyecto y llámela`WaterfallCollectionDelegate`. Edite este archivo y haga que tenga un aspecto similar al siguiente:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
  public class WaterfallCollectionDelegate : UICollectionViewDelegate
  {
    #region Computed Properties
    public WaterfallCollectionView CollectionView { get; set;}
    #endregion

    #region Constructors
    public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
    {

      // Initialize
      CollectionView = collectionView;

    }
    #endregion

    #region Overrides Methods
    public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
      // Always allow for highlighting
      return true;
    }

    public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
    {
      // Get cell and change to green background
      var cell = collectionView.CellForItem(indexPath);
      cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
    }

    public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
    {
      // Get cell and return to blue background
      var cell = collectionView.CellForItem(indexPath);
      cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
    }
    #endregion
  }
}
```

Esto actuará como el delegado para nuestra vista de colección. Los métodos se han invalidado para resaltar una celda a medida que el usuario interactúa con ella en la vista de colección.

Agregue una última C# clase al proyecto y llámela`WaterfallCollectionView`. Edite este archivo y haga que tenga un aspecto similar al siguiente:

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
  [Register("WaterfallCollectionView")]
  public class WaterfallCollectionView : UICollectionView
  {

    #region Constructors
    public WaterfallCollectionView (IntPtr handle) : base (handle)
    {
    }
    #endregion

    #region Override Methods
    public override void AwakeFromNib ()
    {
      base.AwakeFromNib ();

      // Initialize
      DataSource = new WaterfallCollectionSource(this);
      Delegate = new WaterfallCollectionDelegate(this);

    }
    #endregion
  }
}
```

Observe que `DataSource` y `Delegate` que creamos anteriormente se establecen cuando la vista de colección se construye a partir de su guion gráfico (o archivo **. Xib** ).

Edite el archivo **Main. Storyboard** de nuevo y seleccione la vista de colección y cambie a las **propiedades**. Establezca la **clase** en la clase de `WaterfallCollectionView` personalizada que definimos anteriormente:

Guarde los cambios realizados en la interfaz de usuario y ejecute la aplicación.
Si el usuario selecciona un elemento de la lista y lo arrastra a una nueva ubicación, los demás elementos se animarán automáticamente cuando salgan del modo del elemento.
Cuando el usuario coloca el elemento en una nueva ubicación, se adhiere a esa ubicación. Por ejemplo:

[![](uicollectionview-images/intro01.png "An example of dragging an item to a new location")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Usar un reconocedor de gestos personalizado

En los casos en los que no puede usar un `UICollectionViewController` y debe utilizar un `UIViewController`normal, o si desea tomar más control sobre el gesto de arrastrar y colocar, puede crear su propio reconocedor de gestos personalizado y agregarlo a la vista de colección cuando se cargue la vista. Por ejemplo:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  // Create a custom gesture recognizer
  var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

    // Take action based on state
    switch(gesture.State) {
    case UIGestureRecognizerState.Began:
      var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
      if (selectedIndexPath !=null) {
        CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
      }
      break;
    case UIGestureRecognizerState.Changed:
      CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
      break;
    case UIGestureRecognizerState.Ended:
      CollectionView.EndInteractiveMovement();
      break;
    default:
      CollectionView.CancelInteractiveMovement();
      break;
    }

  });

  // Add the custom recognizer to the collection view
  CollectionView.AddGestureRecognizer(longPressGesture);
}
```

Aquí se usan varios métodos nuevos que se agregan a la vista de colección para implementar y controlar la operación de arrastre:

- `BeginInteractiveMovementForItem`: marca el inicio de una operación de movimiento.
- `UpdateInteractiveMovementTargetPosition`: se envía a medida que se actualiza la ubicación del elemento.
- `EndInteractiveMovement`: marca el final de un movimiento del elemento.
- `CancelInteractiveMovement`: marca el usuario para cancelar la operación de movimiento.

Cuando se ejecuta la aplicación, la operación de arrastrar funciona exactamente igual que el reconocedor de gestos de arrastre predeterminado incluido en la vista de colección.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Diseños personalizados y reordenación

En iOS 9, se han agregado varios métodos nuevos para trabajar con los diseños de arrastrar y reordenar y personalizados en una vista de colección. Para explorar esta característica, vamos a agregar un diseño personalizado a la colección.

En primer lugar, agregue C# una nueva clase denominada`WaterfallCollectionLayout`al proyecto. Edítelo y haga que tenga un aspecto similar al siguiente:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
  [Register("WaterfallCollectionLayout")]
  public class WaterfallCollectionLayout : UICollectionViewLayout
  {
    #region Private Variables
    private int columnCount = 2;
    private nfloat minimumColumnSpacing = 10;
    private nfloat minimumInterItemSpacing = 10;
    private nfloat headerHeight = 0.0f;
    private nfloat footerHeight = 0.0f;
    private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
    private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
    private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
    private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
    private List<CGRect> unionRects = new List<CGRect>();
    private List<nfloat> columnHeights = new List<nfloat>();
    private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
    private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
    private nfloat unionSize = 20;
    #endregion

    #region Computed Properties
    [Export("ColumnCount")]
    public int ColumnCount {
      get { return columnCount; }
      set {
        WillChangeValue ("ColumnCount");
        columnCount = value;
        DidChangeValue ("ColumnCount");

        InvalidateLayout ();
      }
    }

    [Export("MinimumColumnSpacing")]
    public nfloat MinimumColumnSpacing {
      get { return minimumColumnSpacing; }
      set {
        WillChangeValue ("MinimumColumnSpacing");
        minimumColumnSpacing = value;
        DidChangeValue ("MinimumColumnSpacing");

        InvalidateLayout ();
      }
    }

    [Export("MinimumInterItemSpacing")]
    public nfloat MinimumInterItemSpacing {
      get { return minimumInterItemSpacing; }
      set {
        WillChangeValue ("MinimumInterItemSpacing");
        minimumInterItemSpacing = value;
        DidChangeValue ("MinimumInterItemSpacing");

        InvalidateLayout ();
      }
    }

    [Export("HeaderHeight")]
    public nfloat HeaderHeight {
      get { return headerHeight; }
      set {
        WillChangeValue ("HeaderHeight");
        headerHeight = value;
        DidChangeValue ("HeaderHeight");

        InvalidateLayout ();
      }
    }

    [Export("FooterHeight")]
    public nfloat FooterHeight {
      get { return footerHeight; }
      set {
        WillChangeValue ("FooterHeight");
        footerHeight = value;
        DidChangeValue ("FooterHeight");

        InvalidateLayout ();
      }
    }

    [Export("SectionInset")]
    public UIEdgeInsets SectionInset {
      get { return sectionInset; }
      set {
        WillChangeValue ("SectionInset");
        sectionInset = value;
        DidChangeValue ("SectionInset");

        InvalidateLayout ();
      }
    }

    [Export("ItemRenderDirection")]
    public WaterfallCollectionRenderDirection ItemRenderDirection {
      get { return itemRenderDirection; }
      set {
        WillChangeValue ("ItemRenderDirection");
        itemRenderDirection = value;
        DidChangeValue ("ItemRenderDirection");

        InvalidateLayout ();
      }
    }
    #endregion

    #region Constructors
    public WaterfallCollectionLayout ()
    {
    }

    public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

    }
    #endregion

    #region Public Methods
    public nfloat ItemWidthInSectionAtIndex(int section) {

      var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
      return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
    }
    #endregion

    #region Override Methods
    public override void PrepareLayout ()
    {
      base.PrepareLayout ();

      // Get the number of sections
      var numberofSections = CollectionView.NumberOfSections();
      if (numberofSections == 0)
        return;

      // Reset collections
      headersAttributes.Clear ();
      footersAttributes.Clear ();
      unionRects.Clear ();
      columnHeights.Clear ();
      allItemAttributes.Clear ();
      sectionItemAttributes.Clear ();

      // Initialize column heights
      for (int n = 0; n < ColumnCount; n++) {
        columnHeights.Add ((nfloat)0);
      }

      // Process all sections
      nfloat top = 0.0f;
      var attributes = new UICollectionViewLayoutAttributes ();
      var columnIndex = 0;
      for (nint section = 0; section < numberofSections; ++section) {
        // Calculate section specific metrics
        var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
          MinimumInterItemSpacingForSection (CollectionView, this, section);

        // Calculate widths
        var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
        var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

        // Calculate section header
        var heightHeader = (HeightForHeader == null) ? HeaderHeight :
          HeightForHeader (CollectionView, this, section);

        if (heightHeader > 0) {
          attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
          attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
          headersAttributes.Add (section, attributes);
          allItemAttributes.Add (attributes);

          top = attributes.Frame.GetMaxY ();
        }

        top += SectionInset.Top;
        for (int n = 0; n < ColumnCount; n++) {
          columnHeights [n] = top;
        }

        // Calculate Section Items
        var itemCount = CollectionView.NumberOfItemsInSection(section);
        List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

        for (nint n = 0; n < itemCount; n++) {
          var indexPath = NSIndexPath.FromRowSection (n, section);
          columnIndex = NextColumnIndexForItem (n);
          var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
          var yOffset = columnHeights [columnIndex];
          var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
          nfloat itemHeight = 0.0f;

          if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
            itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
          }

          attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
          attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
          itemAttributes.Add (attributes);
          allItemAttributes.Add (attributes);
          columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
        }
        sectionItemAttributes.Add (itemAttributes);

        // Calculate Section Footer
        nfloat footerHeight = 0.0f;
        columnIndex = LongestColumnIndex();
        top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
        footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

        if (footerHeight > 0) {
          attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
          attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
          footersAttributes.Add (section, attributes);
          allItemAttributes.Add (attributes);
          top = attributes.Frame.GetMaxY ();
        }

        for (int n = 0; n < ColumnCount; n++) {
          columnHeights [n] = top;
        }
      }

      var i =0;
      var attrs = allItemAttributes.Count;
      while(i < attrs) {
        var rect1 = allItemAttributes [i].Frame;
        i = (int)Math.Min (i + unionSize, attrs) - 1;
        var rect2 = allItemAttributes [i].Frame;
        unionRects.Add (CGRect.Union (rect1, rect2));
        i++;
      }

    }

    public override CGSize CollectionViewContentSize {
      get {
        if (CollectionView.NumberOfSections () == 0) {
          return new CGSize (0, 0);
        }

        var contentSize = CollectionView.Bounds.Size;
        contentSize.Height = columnHeights [0];
        return contentSize;
      }
    }

    public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
    {
      if (indexPath.Section >= sectionItemAttributes.Count) {
        return null;
      }

      if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
        return null;
      }

      var list = sectionItemAttributes [indexPath.Section];
      return list [(int)indexPath.Item];
    }

    public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
    {
      var attributes = new UICollectionViewLayoutAttributes ();

      switch (kind) {
      case "header":
        attributes = headersAttributes [indexPath.Section];
        break;
      case "footer":
        attributes = footersAttributes [indexPath.Section];
        break;
      }

      return attributes;
    }

    public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
    {
      var begin = 0;
      var end = unionRects.Count;
      List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();

      for (int i = 0; i < end; i++) {
        if (rect.IntersectsWith(unionRects[i])) {
          begin = i * (int)unionSize;
        }
      }

      for (int i = end - 1; i >= 0; i--) {
        if (rect.IntersectsWith (unionRects [i])) {
          end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
          break;
        }
      }

      for (int i = begin; i < end; i++) {
        var attr = allItemAttributes [i];
        if (rect.IntersectsWith (attr.Frame)) {
          attrs.Add (attr);
        }
      }

      return attrs.ToArray();
    }

    public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
    {
      var oldBounds = CollectionView.Bounds;
      return (newBounds.Width != oldBounds.Width);
    }
    #endregion

    #region Private Methods
    private int ShortestColumnIndex() {
      var index = 0;
      var shortestHeight = nfloat.MaxValue;
      var n = 0;

      // Scan each column for the shortest height
      foreach (nfloat height in columnHeights) {
        if (height < shortestHeight) {
          shortestHeight = height;
          index = n;
        }
        ++n;
      }

      return index;
    }

    private int LongestColumnIndex() {
      var index = 0;
      var longestHeight = nfloat.MinValue;
      var n = 0;

      // Scan each column for the shortest height
      foreach (nfloat height in columnHeights) {
        if (height > longestHeight) {
          longestHeight = height;
          index = n;
        }
        ++n;
      }

      return index;
    }

    private int NextColumnIndexForItem(nint item) {
      var index = 0;

      switch (ItemRenderDirection) {
      case WaterfallCollectionRenderDirection.ShortestFirst:
        index = ShortestColumnIndex ();
        break;
      case WaterfallCollectionRenderDirection.LeftToRight:
        index = ColumnCount;
        break;
      case WaterfallCollectionRenderDirection.RightToLeft:
        index = (ColumnCount - 1) - ((int)item / ColumnCount);
        break;
      }

      return index;
    }
    #endregion

    #region Events
    public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
    public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
    public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

    public event WaterfallCollectionSizeDelegate SizeForItem;
    public event WaterfallCollectionFloatDelegate HeightForHeader;
    public event WaterfallCollectionFloatDelegate HeightForFooter;
    public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
    public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
    #endregion
  }
}
```

Esta clase se puede utilizar para proporcionar un diseño de tipo cascada personalizado de dos columnas a la vista de colección.
El código utiliza codificación de clave-valor (a través de los métodos `WillChangeValue` y `DidChangeValue`) para proporcionar el enlace de datos para nuestras propiedades calculadas en esta clase.

A continuación, edite el `WaterfallCollectionSource` y realice los siguientes cambios y adiciones:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
  // Initialize
  CollectionView = collectionView;

  // Init numbers collection
  for (int n = 0; n < 100; ++n) {
    Numbers.Add (n);
    Heights.Add (rnd.Next (0, 100) + 40.0f);
  }
}
```

Esto creará un alto aleatorio para cada uno de los elementos que se mostrarán en la lista.

A continuación, edite la clase `WaterfallCollectionView` y agregue la siguiente propiedad auxiliar:

```csharp
public WaterfallCollectionSource Source {
  get { return (WaterfallCollectionSource)DataSource; }
}
```

Esto hará que sea más fácil obtener el origen de datos (y el alto del elemento) del diseño personalizado.

Por último, edite el controlador de vista y agregue el código siguiente:

```csharp
public override void AwakeFromNib ()
{
  base.AwakeFromNib ();

  var waterfallLayout = new WaterfallCollectionLayout ();

  // Wireup events
  waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
    var collection = collectionView as WaterfallCollectionView;
    return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
  };

  // Attach the custom layout to the collection
  CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

Esto crea una instancia de nuestro diseño personalizado, establece el evento para proporcionar el tamaño de cada elemento y asocia el nuevo diseño a nuestra vista de colección.

Si ejecutamos de nuevo la aplicación de Xamarin. iOS, la vista de colección ahora tendrá el siguiente aspecto:

[![](uicollectionview-images/custom01.png "The collection view will now look like this")](uicollectionview-images/custom01.png#lightbox)

Todavía podemos arrastrar y reordenar los elementos como antes, pero ahora los elementos cambiarán de tamaño para ajustarse a su nueva ubicación cuando se quiten.

## <a name="collection-view-changes"></a>Cambios en la vista de colección

En las secciones siguientes, se detallan los cambios realizados en cada clase en la vista de recopilación mediante iOS 9.

### <a name="uicollectionview"></a>UICollectionView

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionView` para iOS 9:

- `BeginInteractiveMovementForItem`: marca el inicio de una operación de arrastre.
- `CancelInteractiveMovement`: informa a la vista de colección de que el usuario ha cancelado una operación de arrastre.
- `EndInteractiveMovement`: informa a la vista de colección de que el usuario ha finalizado una operación de arrastre.
- `GetIndexPathsForVisibleSupplementaryElements`: devuelve el `indexPath` de un encabezado o pie de página en una sección de vista de colección.
- `GetSupplementaryView`: devuelve el encabezado o el pie de página especificados.
- `GetVisibleSupplementaryViews`: devuelve una lista de todos los encabezados y pies de página visibles.
- `UpdateInteractiveMovementTargetPosition`: informa a la vista de colección de que el usuario ha movido o está moviendo un elemento durante una operación de arrastre.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewController` en iOS 9:

- `InstallsStandardGestureForInteractiveMovement`: si `true` se usará el nuevo reconocedor de gestos que admite automáticamente arrastrar a reorden.
- `CanMoveItem`: informa a la vista de colección si un elemento determinado se puede arrastrar por orden.
- `GetTargetContentOffset`: se usa para obtener el desplazamiento de un elemento de vista de colección determinado.
- `GetTargetIndexPathForMove`: obtiene el `indexPath` de un elemento determinado para una operación de arrastre.
- `MoveItem`: mueve el orden de un elemento determinado en la lista.

### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewDataSource` en iOS 9:

- `CanMoveItem`: informa a la vista de colección si un elemento determinado se puede arrastrar por orden.
- `MoveItem`: mueve el orden de un elemento determinado en la lista.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewDelegate` en iOS 9:

- `GetTargetContentOffset`: se usa para obtener el desplazamiento de un elemento de vista de colección determinado.
- `GetTargetIndexPathForMove`: obtiene el `indexPath` de un elemento determinado para una operación de arrastre.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewFlowLayout` en iOS 9:

- `SectionFootersPinToVisibleBounds`: se pegan los pies de página en los límites visibles de la vista de colección.
- `SectionHeadersPinToVisibleBounds`: se pegan los encabezados de la sección en los límites visibles de la vista de colección.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewLayout` en iOS 9:

- `GetInvalidationContextForEndingInteractiveMovementOfItems`: devuelve el contexto de invalidación al final de una operación de arrastre cuando el usuario termina de arrastrarlo o lo cancela.
- `GetInvalidationContextForInteractivelyMovingItems`: devuelve el contexto de invalidación en el inicio de una operación de arrastre.
- `GetLayoutAttributesForInteractivelyMovingItem`: obtiene los atributos de diseño de un elemento determinado mientras se arrastra un elemento.
- `GetTargetIndexPathForInteractivelyMovingItem`: devuelve la `indexPath` del elemento que se encuentra en el punto especificado al arrastrar un elemento.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewLayoutAttributes` en iOS 9:

- `CollisionBoundingPath`: devuelve la ruta de acceso de colisión de dos elementos durante una operación de arrastre.
- `CollisionBoundsType`: devuelve el tipo de colisión (como un `UIDynamicItemCollisionBoundsType`) que se ha producido durante una operación de arrastre.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewLayoutInvalidationContext` en iOS 9:

- `InteractiveMovementTarget`: devuelve el elemento de destino de una operación de arrastre.
- `PreviousIndexPathsForInteractivelyMovingItems`: devuelve el `indexPaths` de otros elementos implicados en una operación de arrastrar y reordenar.
- `TargetIndexPathsForInteractivelyMovingItems`: devuelve el `indexPaths` de elementos que se reordenarán como resultado de una operación de arrastrar y reordenar.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Se han realizado los siguientes cambios o adiciones en la clase `UICollectionViewSource` en iOS 9:

- `CanMoveItem`: informa a la vista de colección si un elemento determinado se puede arrastrar por orden.
- `GetTargetContentOffset`: devuelve los desplazamientos de los elementos que se moverán a través de una operación de arrastrar y reordenar.
- `GetTargetIndexPathForMove`: devuelve el `indexPath` de un elemento que se va a desplace durante una operación de arrastrar y reordenar.
- `MoveItem`: mueve el orden de un elemento determinado en la lista.

## <a name="summary"></a>Resumen

En este artículo se han explicado los cambios en las vistas de colección en iOS 9 y se ha descrito cómo implementarlas en Xamarin. iOS.
Se ha tratado la implementación de una sencilla acción de arrastrar a reorden en una vista de colección; usar un reconocedor de gestos personalizado con arrastrar y volver a ordenar; y el modo en que la función de arrastrar a reorden afecta a un diseño de vista de colección personalizado.

## <a name="related-links"></a>Vínculos relacionados

- [Ejemplos de iOS 9](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Ejemplo de vista de colección](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-collectionview)
- [SimpleCollectionView (ejemplo)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplecollectionview)
- [Eventos, protocolos y delegados](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Trabajar con tablas y celdas](~/ios/user-interface/controls/tables/index.md)
