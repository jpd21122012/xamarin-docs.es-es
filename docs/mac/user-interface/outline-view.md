---
title: Esquematizar vistas en Xamarin. Mac
description: En este artículo se explica cómo trabajar con vistas de esquema en una aplicación de Xamarin. Mac. Describe cómo crear y mantener vistas de esquema en Xcode y Interface Builder y cómo trabajar con ellas mediante programación.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: dd710f13d9324ce31e08641f214241e0433fe809
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004341"
---
# <a name="outline-views-in-xamarinmac"></a>Esquematizar vistas en Xamarin. Mac

_En este artículo se explica cómo trabajar con vistas de esquema en una aplicación de Xamarin. Mac. Describe cómo crear y mantener vistas de esquema en Xcode y Interface Builder y cómo trabajar con ellas mediante programación._

Al trabajar con C# y .net en una aplicación de Xamarin. Mac, tiene acceso a las mismas vistas de esquema que un desarrollador que trabaja en *Objective-C* y *Xcode* . Como Xamarin. Mac se integra directamente con Xcode, puede usar los _Interface Builder_ de Xcode para crear y mantener las vistas de esquema (o, opcionalmente, C# crearlas directamente en el código).

Una vista de esquema es un tipo de tabla que permite al usuario expandir o contraer filas de datos jerárquicos. Al igual que una vista de tabla, una vista de esquema muestra los datos de un conjunto de elementos relacionados, con filas que representan elementos individuales y columnas que representan los atributos de esos elementos. A diferencia de una vista de tabla, los elementos de una vista de esquema no están en una lista plana, sino que se organizan en una jerarquía, como los archivos y las carpetas de una unidad de disco duro.

[![](outline-view-images/populate03.png "An example app run")](outline-view-images/populate03.png#lightbox)

En este artículo, trataremos los aspectos básicos del trabajo con las vistas de esquema en una aplicación de Xamarin. Mac. Se recomienda encarecidamente que trabaje primero en el artículo [Hello, Mac](~/mac/get-started/hello-mac.md) , específicamente en las secciones [Introducción a Xcode y Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) y [salidas y acciones](~/mac/get-started/hello-mac.md#outlets-and-actions) , ya que trata conceptos clave y técnicas que usaremos en Este artículo.

Es posible que desee echar un vistazo a la sección [exponer C# clases y métodos a Objective-C](~/mac/internals/how-it-works.md) del documento [interno de Xamarin. Mac](~/mac/internals/how-it-works.md) , donde se explican los comandos`Register`y`Export`que se usan para conectar las C# clases a Objective-C. objetos y elementos de la interfaz de usuario.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Introducción a las vistas de esquema

Una vista de esquema es un tipo de tabla que permite al usuario expandir o contraer filas de datos jerárquicos. Al igual que una vista de tabla, una vista de esquema muestra los datos de un conjunto de elementos relacionados, con filas que representan elementos individuales y columnas que representan los atributos de esos elementos. A diferencia de una vista de tabla, los elementos de una vista de esquema no están en una lista plana, sino que se organizan en una jerarquía, como los archivos y las carpetas de una unidad de disco duro.

Si un elemento de una vista de esquema contiene otros elementos, el usuario puede expandirlo o contraerlo. Un elemento expansible muestra un triángulo de divulgación, que apunta a la derecha cuando el elemento está contraído y apunta hacia abajo cuando se expande el elemento. Al hacer clic en el triángulo de divulgación, el elemento se expande o se contrae.

La vista de esquema (`NSOutlineView`) es una subclase de la vista de tabla (`NSTableView`) y, como tal, hereda gran parte de su comportamiento de la clase primaria. Como resultado, muchas de las operaciones admitidas por una vista de tabla, como la selección de filas o columnas, la reposición de las columnas arrastrando los encabezados de columna, etc., también se admiten en una vista de esquema. Una aplicación de Xamarin. Mac tiene el control de estas características y puede configurar los parámetros de la vista de esquema (ya sea en código o Interface Builder) para permitir o no determinadas operaciones.

Una vista de esquema no almacena sus propios datos, sino que se basa en un origen de datos (`NSOutlineViewDataSource`) para proporcionar las filas y las columnas requeridas, según sea necesario.

El comportamiento de una vista de esquema se puede personalizar proporcionando una subclase del delegado de la vista esquema (`NSOutlineViewDelegate`) para admitir la administración de columnas de esquema, el tipo para seleccionar la funcionalidad, la selección de filas y la edición, el seguimiento personalizado y las vistas personalizadas de columnas individuales. y filas.

Dado que una vista de esquema comparte gran parte del comportamiento y la funcionalidad de la vista de tabla, es posible que desee consultar la documentación de las [vistas de tabla](~/mac/user-interface/table-view.md) antes de continuar con este artículo.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Crear y mantener vistas de esquema en Xcode

Cuando se crea una nueva aplicación de Cocoa de Xamarin. Mac, se obtiene una ventana en blanco de forma predeterminada. Esta ventana se define en un archivo `.storyboard` incluido automáticamente en el proyecto. Para editar el diseño de Windows, en el **Explorador de soluciones**, haga doble clic en el archivo de `Main.storyboard`:

[![](outline-view-images/edit01.png "Selecting the main storyboard")](outline-view-images/edit01.png#lightbox)

Se abrirá el diseño de ventana en la Interface Builder de Xcode:

[![](outline-view-images/edit02.png "Editing the UI in Xcode")](outline-view-images/edit02.png#lightbox)

Escriba `outline` en el cuadro **de búsqueda del inspector** de la biblioteca para que sea más fácil encontrar los controles de la vista de esquema:

[![](outline-view-images/edit03.png "Selecting an Outline View from the Library")](outline-view-images/edit03.png#lightbox)

Arrastre una vista de esquema en el controlador de vista en el editor de la **interfaz**, haga que rellene el área de contenido del controlador de vista y establézcalo en donde se reduzca y crezca con la ventana del **Editor de restricciones**:

[![](outline-view-images/edit04.png "Editing the constraints")](outline-view-images/edit04.png#lightbox)

Seleccione la vista esquema en la **jerarquía** de la interfaz y las siguientes propiedades están disponibles en el **Inspector de atributos**:

[![](outline-view-images/edit05.png "The Attribute Inspector")](outline-view-images/edit05.png#lightbox)

- **Columna de esquema** : columna de la tabla en la que se muestran los datos jerárquicos.
- **Columna de esquema autoguardado** : si `true`, la columna de esquema se guardará y restaurará automáticamente entre las ejecuciones de la aplicación.
- **Indentación** : la cantidad para aplicar sangría a las columnas bajo un elemento expandido.
- La **sangría sigue a las celdas** : si `true`, se aplicará sangría a la marca de sangría junto con las celdas.
- **Autoguardar elementos expandidos** : si `true`, el estado expandido o contraído de los elementos se guardará y restaurará automáticamente entre las ejecuciones de la aplicación.
- **Modo de contenido** : permite usar vistas (`NSView`) o celdas (`NSCell`) para mostrar los datos en las filas y columnas. A partir de macOS 10,7, debe utilizar las vistas.
- **Floating Group Rows** : si `true`, la vista de tabla dibujará celdas agrupadas como si fueran flotantes.
- **Columnas** : define el número de columnas que se muestran.
- **Encabezados** : si `true`, las columnas tendrán encabezados.
- **Reordenación** : si `true`, el usuario podrá arrastrar de nuevo el orden de las columnas de la tabla.
- **Cambio de tamaño** : si `true`, el usuario podrá arrastrar encabezados de columna para cambiar el tamaño de las columnas.
- **Tamaño de columna** : controla el modo en que la tabla ajustará automáticamente el tamaño de las columnas.
- **Resaltar** : controla el tipo de resaltado que usa la tabla cuando se selecciona una celda.
- **Filas alternativas** : si `true`, otra fila tendrá un color de fondo diferente.
- **Cuadrícula horizontal** : selecciona el tipo de borde dibujado entre las celdas horizontalmente.
- **Cuadrícula vertical** : selecciona el tipo de borde que se dibuja entre las celdas verticalmente.
- **Color** de la cuadrícula: establece el color del borde de la celda.
- **Background** : establece el color de fondo de la celda.
- **Selección** : permite controlar el modo en que el usuario puede seleccionar las celdas de la tabla como:
  - **Multiple** : si `true`, el usuario puede seleccionar varias filas y columnas.
  - **Columna** : si `true`, el usuario puede seleccionar columnas.
  - **Escriba Select** : si `true`, el usuario puede escribir un carácter para seleccionar una fila.
  - **Vacío** : si `true`, no es necesario que el usuario seleccione una fila o columna; la tabla no permite ninguna selección.
- **Autosave** : nombre en el que se guarda automáticamente el formato de las tablas.
- **Información de columna** : si `true`, el orden y el ancho de las columnas se guardarán automáticamente.
- **Saltos de línea** : Seleccione cómo controla la celda los saltos de línea.
- **Trunca la última línea visible** : si `true`, la celda se truncará en los datos no caben dentro de sus límites.

> [!IMPORTANT]
> A menos que esté manteniendo una aplicación de Xamarin. Mac heredada, las vistas de esquema basadas en `NSView` deben usarse en vistas de tabla basadas en `NSCell`. `NSCell` se considera heredada y puede que no se admita en el futuro.

Seleccione una columna de la tabla en la jerarquía de la **interfaz** y las siguientes propiedades estarán disponibles en el **Inspector de atributos**:

[![](outline-view-images/edit06.png "The Attribute Inspector")](outline-view-images/edit06.png#lightbox)

- **Título** : establece el título de la columna.
- **Alineación** : establece la alineación del texto dentro de las celdas.
- **Fuente de título** : selecciona la fuente del texto de encabezado de la celda.
- **Criterio de ordenación** : es la clave utilizada para ordenar los datos de la columna. Déjelo en blanco si el usuario no puede ordenar esta columna.
- **Selector** : es la **acción** que se usa para realizar la ordenación. Déjelo en blanco si el usuario no puede ordenar esta columna.
- **Order** : es el criterio de ordenación de los datos de las columnas.
- **Cambio de tamaño** : selecciona el tipo de cambio de tamaño de la columna.
- **Editable** : si `true`, el usuario puede editar las celdas de una tabla basada en celdas.
- **Hidden** : si `true`, la columna está oculta.

También puede cambiar el tamaño de la columna arrastrando su identificador (verticalmente centrado en el lado derecho de la columna) hacia la izquierda o la derecha.

Vamos a seleccionar cada columna en la vista de tabla y asignaremos a la primera columna un **título** de `Product` y la segunda `Details`.

Seleccione una vista de celda de tabla (`NSTableViewCell`) en la jerarquía de la **interfaz** y las siguientes propiedades están disponibles en el **Inspector de atributos**:

[![](outline-view-images/edit07.png "The Attribute Inspector")](outline-view-images/edit07.png#lightbox)

Estas son todas las propiedades de una vista estándar. También tiene la opción de cambiar el tamaño de las filas de esta columna aquí.

Seleccione una celda de vista de tabla (de forma predeterminada, se trata de una `NSTextField`) en la jerarquía de la **interfaz** y las siguientes propiedades están disponibles en el **Inspector de atributos**:

[![](outline-view-images/edit08.png "The Attribute Inspector")](outline-view-images/edit08.png#lightbox)

Tendrá todas las propiedades de un campo de texto estándar para establecer aquí. De forma predeterminada, se usa un campo de texto estándar para mostrar los datos de una celda de una columna.

Seleccione una vista de celda de tabla (`NSTableFieldCell`) en la jerarquía de la **interfaz** y las siguientes propiedades están disponibles en el **Inspector de atributos**:

[![](outline-view-images/edit09.png "The Attribute Inspector")](outline-view-images/edit09.png#lightbox)

La configuración más importante aquí es:

- **Diseño** : Seleccione cómo se colocan las celdas de esta columna.
- **Usa el modo de una sola línea** : si `true`, la celda se limita a una sola línea.
- **Ancho del primer diseño en tiempo de ejecución** : si `true`, la celda preferirá el ancho establecido para ella (ya sea de forma manual o automática) cuando se muestre la primera vez que se ejecute la aplicación.
- **Acción** : controla cuándo se envía la **acción** de edición para la celda.
- **Comportamiento** : define si una celda es seleccionable o editable.
- **Texto enriquecido** : si `true`, la celda puede mostrar texto con formato y con estilo.
- **Deshacer** : si `true`, la celda asume la responsabilidad del comportamiento de deshacer.

Seleccione la vista celda de tabla (`NSTableFieldCell`) en la parte inferior de una columna de la tabla en la jerarquía de la **interfaz**:

[![](outline-view-images/edit11.png "Selecting the table cell view")](outline-view-images/edit10.png#lightbox)

Esto le permite editar la vista de celda de tabla utilizada como _patrón_ base para todas las celdas creadas para la columna especificada.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Agregar acciones y salidas

Al igual que cualquier otro control de la interfaz de usuario de coco, es necesario exponer nuestra vista de esquema y C# sus columnas y celdas al código mediante **acciones** y **salidas** (según la funcionalidad requerida).

El proceso es el mismo para cualquier elemento de la vista de esquema que desee exponer:

1. Cambie al **Editor de asistentes** y asegúrese de que está seleccionado el archivo de `ViewController.h`:

    [![](outline-view-images/edit11.png "Selecting the correct .h file")](outline-view-images/edit11.png#lightbox)
2. Seleccione la vista de esquema en la jerarquía de la **interfaz**, haga clic en el control y arrástrela hasta el archivo de `ViewController.h`.
3. Cree una **salida** para la vista de esquema denominada `ProductOutline`:

    [![](outline-view-images/edit13.png "Configuring an Outlet")](outline-view-images/edit13.png#lightbox)
4. Cree **salidas** para las columnas de las tablas, también denominadas `ProductColumn` y `DetailsColumn`:

    [![](outline-view-images/edit14.png "Configuring an Outlet")](outline-view-images/edit14.png#lightbox)
5. Guarde los cambios y vuelva a Visual Studio para Mac sincronizar con Xcode.

A continuación, escribiremos el código para mostrar algunos datos del esquema cuando se ejecute la aplicación.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Rellenar la vista de esquema

Con nuestra vista de esquema diseñada en Interface Builder y expuesta a través de una **salida**, es necesario crear C# el código para rellenarlo.

En primer lugar, vamos a crear una nueva clase de `Product` para almacenar la información de las filas y grupos de subproductos individuales. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo archivo...** Seleccione **General** > **clase vacía**, escriba `Product` para el **nombre** y haga clic en el botón **nuevo** :

[![](outline-view-images/populate01.png "Creating an empty class")](outline-view-images/populate01.png#lightbox)

Haga que el archivo de `Product.cs` tenga el aspecto siguiente:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}
```

A continuación, es necesario crear una subclase de `NSOutlineDataSource` para proporcionar los datos para el esquema que se solicitan. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo archivo...** Seleccione **General** > **clase vacía**, escriba `ProductOutlineDataSource` para el **nombre** y haga clic en el botón **nuevo** .

Edite el archivo de `ProductTableDataSource.cs` y haga que tenga un aspecto similar al siguiente:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }

        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }

        }
        #endregion
    }
}
```

Esta clase tiene almacenamiento para los elementos de la vista de esquema e invalida el `GetChildrenCount` para devolver el número de filas de la tabla. El `GetChild` devuelve un elemento primario o secundario específico (solicitado por la vista de esquema) y el `ItemExpandable` define el elemento especificado como primario o secundario.

Por último, es necesario crear una subclase de `NSOutlineDelegate` para proporcionar el comportamiento de nuestro esquema. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo archivo...** Seleccione **General** > **clase vacía**, escriba `ProductOutlineDelegate` para el **nombre** y haga clic en el botón **nuevo** .

Edite el archivo de `ProductOutlineDelegate.cs` y haga que tenga un aspecto similar al siguiente:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Cuando se crea una instancia de la `ProductOutlineDelegate`, también se pasa una instancia del `ProductOutlineDataSource` que proporciona los datos para el esquema. El método `GetView` es responsable de devolver una vista (datos) para mostrar la celda para una columna y una fila. Si es posible, se volverá a usar una vista existente para mostrar la celda, en caso de que no se deba crear una nueva vista.

Para rellenar el esquema, vamos a editar el archivo de `MainWindow.cs` y hacer que el método `AwakeFromNib` sea similar al siguiente:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Si ejecutamos la aplicación, se muestra lo siguiente:

[![](outline-view-images/populate02.png "The collapsed view")](outline-view-images/populate02.png#lightbox)

Si expandimos un nodo en la vista de esquema, tendrá un aspecto similar al siguiente:

[![](outline-view-images/populate03.png "The expanded view")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Ordenar por columna

Vamos a permitir que el usuario ordene los datos en el esquema haciendo clic en un encabezado de columna. En primer lugar, haga doble clic en el archivo `Main.storyboard` para abrirlo y editarlo en Interface Builder. Seleccione la columna `Product`, escriba `Title` para la **clave de ordenación**, `compare:` para el **Selector** y seleccione `Ascending` para el **pedido**:

[![](outline-view-images/sort01.png "Setting the sort key order")](outline-view-images/sort01.png#lightbox)

Guarde los cambios y vuelva a Visual Studio para Mac para sincronizarlos con Xcode.

Ahora vamos a editar el archivo de `ProductOutlineDataSource.cs` y agregar los siguientes métodos:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

El método `Sort` nos permite ordenar los datos en el origen de datos basándose en un campo de clase `Product` determinado en orden ascendente o descendente. Se llamará al método `SortDescriptorsChanged` invalidado cada vez que se haga clic en un encabezado de columna. Se le pasará el valor de **clave** que se establece en Interface Builder y el criterio de ordenación de esa columna.

Si ejecutamos la aplicación y hacemos clic en los encabezados de columna, las filas se ordenarán por esa columna:

[![](outline-view-images/sort02.png "Example of sorted output")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Selección de filas

Si desea permitir que el usuario seleccione una sola fila, haga doble clic en el archivo `Main.storyboard` para abrirlo y editarlo en Interface Builder. Seleccione la vista esquema en la **jerarquía** de la interfaz y desactive la casilla **múltiple** en el **Inspector de atributos**:

[![](outline-view-images/select01.png "The Attribute Inspector")](outline-view-images/select01.png#lightbox)

Guarde los cambios y vuelva a Visual Studio para Mac para sincronizarlos con Xcode.

Después, edite el archivo de `ProductOutlineDelegate.cs` y agregue el siguiente método:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Esto permitirá al usuario seleccionar cualquier fila única en la vista de esquema. Devuelve `false` para el `ShouldSelectItem` de cualquier elemento que no desea que el usuario pueda seleccionar o `false` para cada elemento si no desea que el usuario pueda seleccionar ningún elemento.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Selección de varias filas

Si desea permitir que el usuario seleccione varias filas, haga doble clic en el archivo `Main.storyboard` para abrirlo y editarlo en Interface Builder. Seleccione la vista esquema en la **jerarquía** de la interfaz y active la casilla **múltiple** en el **Inspector de atributos**:

[![](outline-view-images/select02.png "The Attribute Inspector")](outline-view-images/select02.png#lightbox)

Guarde los cambios y vuelva a Visual Studio para Mac para sincronizarlos con Xcode.

Después, edite el archivo de `ProductOutlineDelegate.cs` y agregue el siguiente método:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Esto permitirá al usuario seleccionar cualquier fila única en la vista de esquema. Devuelve `false` para el `ShouldSelectRow` de cualquier elemento que no desea que el usuario pueda seleccionar o `false` para cada elemento si no desea que el usuario pueda seleccionar ningún elemento.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Tipo para seleccionar fila

Si desea permitir que el usuario escriba un carácter con la vista de esquema seleccionada y seleccionar la primera fila que contiene ese carácter, haga doble clic en el archivo de `Main.storyboard` para abrirlo y editarlo en Interface Builder. Seleccione la vista esquema en la **jerarquía** de la interfaz y active la casilla **tipo Select** en el **Inspector de atributos**:

[![](outline-view-images/type01.png "Editing the row type")](outline-view-images/type01.png#lightbox)

Guarde los cambios y vuelva a Visual Studio para Mac para sincronizarlos con Xcode.

Ahora vamos a editar el archivo de `ProductOutlineDelegate.cs` y agregaremos el método siguiente:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

El método `GetNextTypeSelectMatch` toma el `searchString` dado y devuelve el elemento de la primera `Product` que tiene esa cadena en su `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Reordenación de columnas

Si desea permitir que el usuario arrastre columnas de reordenación en la vista de esquema, haga doble clic en el archivo de `Main.storyboard` para abrirlo y editarlo en Interface Builder. Seleccione la vista esquema en la **jerarquía** de la interfaz y active la casilla **reordenación** en el **Inspector de atributos**:

[![](outline-view-images/reorder01.png "The Attribute Inspector")](outline-view-images/reorder01.png#lightbox)

Si asignamos un valor a la propiedad **autosave** y comprobamos el campo de **información de columna** , los cambios que se realicen en el diseño de la tabla se guardarán automáticamente y se restaurarán la próxima vez que se ejecute la aplicación.

Guarde los cambios y vuelva a Visual Studio para Mac para sincronizarlos con Xcode.

Ahora vamos a editar el archivo de `ProductOutlineDelegate.cs` y agregaremos el método siguiente:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

El método `ShouldReorder` debe devolver `true` para cualquier columna que desee permitir que se vuelva a ordenar en el `newColumnIndex`; de lo contrario, devolverá `false`;

Si ejecutamos la aplicación, podemos arrastrar encabezados de columna para reordenar las columnas:

[![](outline-view-images/reorder02.png "Example of reordering columns")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Editar celdas

Si desea permitir que el usuario edite los valores de una celda determinada, edite el archivo de `ProductOutlineDelegate.cs` y cambie el método `GetViewForItem` de la siguiente manera:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Ahora, Si ejecutamos la aplicación, el usuario puede editar las celdas en la vista de tabla:

[![](outline-view-images/editing01.png "An example of editing cells")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Usar imágenes en las vistas de esquema

Para incluir una imagen como parte de la celda en una `NSOutlineView`, deberá cambiar la forma en que el método `NSTableViewDelegate's` `GetView` de la vista de esquema devuelve los datos para utilizar una `NSTableCellView` en lugar de la `NSTextField` típica. Por ejemplo:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Para obtener más información, consulte la sección [uso de imágenes con las vistas de esquema](~/mac/app-fundamentals/image.md) de nuestra documentación sobre [Cómo trabajar con imágenes](~/mac/app-fundamentals/image.md) .

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Enlace de datos, vistas de esquema

Mediante el uso de la codificación de valores clave y técnicas de enlace de datos en la aplicación de Xamarin. Mac, puede reducir considerablemente la cantidad de código que debe escribir y mantener para rellenar y trabajar con los elementos de la interfaz de usuario. También tiene la ventaja de desacoplar aún más los datos de copia de seguridad (_modelo de datos_) de la interfaz de usuario de front-end (_Model-View-Controller_), lo que permite un diseño de aplicaciones más sencillo y flexible.

La codificación de valores clave (KVC) es un mecanismo para tener acceso indirectamente a las propiedades de un objeto, mediante claves (cadenas con formato especial) para identificar propiedades en lugar de tener acceso a ellas a través de variables de instancia o métodos de descriptor de acceso (`get/set`). Mediante la implementación de los descriptores de acceso compatibles con el código clave-valor en la aplicación de Xamarin. Mac, obtiene acceso a otras características de macOS como la observación de clave-valor (KVO), el enlace de datos, los enlaces de cacao y la creación de scripts.

Para obtener más información, vea la sección sobre el enlace de datos de la [vista de esquema](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) de nuestra documentación sobre el enlace de [datos y el código de valor de clave](~/mac/app-fundamentals/databinding.md) .

<a name="Summary" />

## <a name="summary"></a>Resumen

En este artículo se ha tomado una visión detallada del trabajo con las vistas de esquema en una aplicación de Xamarin. Mac. Vimos los distintos tipos y usos de las vistas de esquema, cómo crear y mantener las vistas de esquema en la Interface Builder de Xcode y cómo trabajar con las C# vistas de esquema en el código.

## <a name="related-links"></a>Vínculos relacionados

- [MacOutlines (ejemplo)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [MacImages (ejemplo)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vistas de tabla](~/mac/user-interface/table-view.md)
- [Listas de origen](~/mac/user-interface/source-list.md)
- [Enlace de datos y codificación de clave-valor](~/mac/app-fundamentals/databinding.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) (Directrices de interfaz humana de OS X)
- [Introducción a las vistas de esquema](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
