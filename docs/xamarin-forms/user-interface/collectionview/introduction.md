---
title: Introducción de Xamarin.Forms CollectionView
description: El elemento CollectionView es una vista flexible y eficaz para presentar las listas de datos con las especificaciones de diseño diferente.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 23de838a395b68f656f95c5665dc97a1d928bece
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054655"
---
# <a name="xamarinforms-collectionview-introduction"></a>Introducción de Xamarin.Forms CollectionView

![](~/media/shared/preview.png "Esta API es actualmente una versión preliminar")

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` es una vista para presentar las listas de datos mediante las especificaciones de diseño diferente. Su objetivo es proporcionar una forma más flexible y una alternativa de alto rendimiento [ `ListView` ](xref:Xamarin.Forms.ListView). Por ejemplo, las capturas de pantalla siguientes muestran un `CollectionView` que usa una cuadrícula vertical de dos columnas, y que permite la selección múltiple:

[![Captura de pantalla de un diseño de cuadrícula vertical CollectionView, en iOS y Android](introduction-images/verticalgrid-multipleselection.png "CollectionView diseño de cuadrícula vertical con selección múltiple")](introduction-images/verticalgrid-multipleselection-large.png#lightbox "CollectionView diseño de cuadrícula vertical con selección múltiple")

`CollectionView` está disponible en las versiones preliminares de Xamarin.Forms 4.0. Sin embargo, es experimental actualmente y solo se puede usar agregando la siguiente línea de código para su `AppDelegate` clase en iOS, así como su `MainActivity` clase en Android, antes de llamar a `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> `CollectionView` solo está disponible en iOS y Android.

## <a name="collectionview-and-listview-differences"></a>Diferencias CollectionView y ListView

Mientras el `CollectionView` y `ListView` API son similares, existen algunas diferencias importantes:

- `CollectionView` tiene un modelo de diseño flexible que permite que los datos se presentan vertical u horizontalmente, en una lista o una cuadrícula.
- `CollectionView` solo admite y la selección múltiple.
- `CollectionView` no tiene ningún concepto de celdas. En su lugar, se usa una plantilla de datos para definir la apariencia de cada elemento de datos en la lista.
- `CollectionView` usa automáticamente la virtualización proporcionada por los controles nativos subyacentes.
- `CollectionView` reduce la superficie de API de [ `ListView` ](xref:Xamarin.Forms.ListView). Muchas propiedades y eventos de `ListView` no están presentes en `CollectionView`.
- `CollectionView` no incluye separadores integrados.

## <a name="move-from-listview-to-collectionview"></a>Mover de ListView a CollectionView

[`ListView`](xref:Xamarin.Forms.ListView) las implementaciones en las implementaciones existentes de Xamarin.Forms se pueden migrar a `CollectionView` implementaciones con la Ayuda de la tabla siguiente:

| Concepto | API de ListView | CollectionView |
|---|---|---|
| Datos | `ItemsSource` | Un `CollectionView` se rellena con datos estableciendo sus `ItemsSource` propiedad. Para obtener más información, consulte [rellenar una colección mediante CollectionView con datos](populate-data.md#populate-a-collectionview-with-data). |
| Apariencia del elemento | `ItemTemplate` | La apariencia de cada elemento en un `CollectionView` pueden definirse mediante el establecimiento del `ItemTemplate` propiedad a un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate). Para obtener más información, consulte [definen la apariencia del elemento](populate-data.md#define-item-appearance). |
| Celdas | `TextCell`, `ImageCell`, `ViewCell` | `CollectionView` no tiene ningún concepto de celdas. En su lugar, se usa una plantilla de datos para definir la apariencia de cada elemento de datos en la lista. |
| Separadores de fila | `SeparatorColor`, `SeparatorVisibility` | `CollectionView` no incluye separadores integrados. Estos se pueden proporcionar, si lo desea, en la plantilla de elemento. |
| Selección | `SelectionMode`, `SelectedItem` | `CollectionView` solo admite y la selección múltiple. Para obtener más información, consulte [Xamarin.Forms CollectionView selección](selection.md). |
| Alto de fila | `HasUnevenRows`, `RowHeight` | En un `CollectionView`, el alto de fila de cada elemento viene determinada por la `ItemSizingStrategy` propiedad. Para obtener más información, consulte [ajuste de tamaño del elemento](layout.md#item-sizing).|
| Almacenamiento en memoria caché | `CachingStrategy` | `CollectionView` usa automáticamente la virtualización proporcionada por los controles nativos subyacentes. |
| Encabezados y pies de página | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Encabezados y pies de página no son actualmente compatibles `CollectionView`, pero se agregará en una versión futura.|
| Agrupar | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Agrupación no se admite actualmente en `CollectionView`, pero se agregará en una versión futura. |
| Deslizar para actualizar | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Deslizar para actualizar no se admite actualmente en `CollectionView`, pero se agregará en una versión futura. |
| Acciones de contexto | `ContextActions` | Acciones de contexto no se admiten actualmente en `CollectionView`, pero se agregará en una versión futura. |
| Desplazarse | `ScrollTo()` | `CollectionView` define `ScrollTo` métodos, que desplazar los elementos en la vista. Para obtener más información, consulte [desplazamiento](scrolling.md). |

## <a name="related-links"></a>Vínculos relacionados

- [CollectionView (ejemplo)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)