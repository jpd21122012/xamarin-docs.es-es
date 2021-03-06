---
title: Introducción a DataSourceControl
description: En este artículo se explica cómo empezar a crear una página sencilla controlada por datos mediante Xamarin.Forms DataSourceControl.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1f7917784ea66c31979b87f43639a7d03756692c
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725600"
---
# <a name="getting-started-with-datapages"></a>Introducción a DataSourceControl

[![Descargar ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> Las páginas de formularios requieren una referencia de tema de Xamarin. Forms para representar. Esto implica la instalación del paquete de Nuget [Xamarin. Forms. theme. base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) en el proyecto, seguido de los paquetes de Nuget [Xamarin. Forms. theme. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) o [Xamarin. Forms. theme. Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) .

Para empezar a crear una página sencilla controlada por datos mediante la versión preliminar de DataSourceControl, siga estos pasos. Este usa demostración crea un estilo codificado de forma rígida ("eventos") en la vista previa que sólo funciona con el formato específico de JSON en el código.

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. agregar paquetes NuGet

Agregue estos paquetes NuGet a los proyectos de aplicación y biblioteca de Xamarin. Forms .NET Standard:

- Xamarin.Forms.Pages
- Xamarin.Forms.Theme.Base
- Un NuGet de implementación de tema (p. ej., Xamarin.Forms.Theme.Light)

## <a name="2-add-theme-reference"></a>2. Agregar referencia de tema

En el archivo **app. Xaml** , agregue un `xmlns:mytheme` personalizado para el tema y asegúrese de que el tema se combina en el Diccionario de recursos de la aplicación:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

> [!IMPORTANT]
> También debe seguir los pasos para [cargar ensamblados de tema (a continuación)](#loadtheme) agregando código reutilizable a las `MainActivity`de iOS `AppDelegate` y Android. Se mejorará en versión preliminar futura.

## <a name="3-add-a-xaml-page"></a>3. agregar una página XAML

Agregue una nueva página XAML a la aplicación de Xamarin. Forms y *cambie la clase base* de `ContentPage` a `Xamarin.Forms.Pages.ListDataPage`. Esto debe hacerse en C# y el XAML:

**C#filesystem**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**Archivo XAML**

Además de cambiar el elemento raíz a `<p:ListDataPage>` el espacio de nombres personalizado para `xmlns:p` también debe agregarse:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Subclase de aplicación**

Cambie el constructor de clase `App` para que el `MainPage` se establezca en un `NavigationPage` que contenga la nueva `SessionDataPage`. Se *debe* usar una página de navegación.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. agregar el origen de los

Elimine el elemento `Content` y reemplácelo por un `p:ListDataPage.DataSource` para rellenar la página con datos. En el ejemplo siguiente Json remoto se está cargando el archivo de datos desde una dirección URL.

> [!NOTE]
> La vista previa *requiere* un atributo `StyleClass` para proporcionar sugerencias de representación para el origen de datos. El `StyleClass="Events"` hace referencia a un diseño que está predefinido en la vista previa y contiene estilos *codificados* para coincidir con el origen de datos JSON que se está usando.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**Datos de JSON**

A continuación se muestra un ejemplo de los datos JSON del origen de la demostración:

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. ejecutar

Deben dar como resultado de los pasos anteriores en una página de datos de trabajo:

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

Esto funciona porque el estilo predefinido **"eventos"** existe en el paquete NuGet de tema claro y tiene estilos definidos que coinciden con el origen de datos (por ejemplo, "title", "image", "presentador").

El `StyleClass` "eventos" se crea para mostrar el control de `ListDataPage` con un control de `CardView` personalizado que se define en Xamarin. Forms. pages. El control `CardView` tiene tres propiedades: `ImageSource`, `Text`y `Detail`. El tema está codificado para enlazar tres campos del origen de datos (desde el archivo JSON) a estas propiedades para mostrarlas.

## <a name="5-customize"></a>5. personalizar

El estilo heredado puede invalidarse especificando una plantilla y usar enlaces de orígenes de datos. El código XAML siguiente declara una plantilla personalizada para cada fila con la nueva sintaxis de `ListItemControl` y `{p:DataSourceBinding}` que se incluye en el NuGet de **Xamarin. Forms. pages** :

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Al proporcionar un `DataTemplate` este código invalida el `StyleClass` y, en su lugar, utiliza el diseño predeterminado para un `ListItemControl`.

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

Los desarrolladores que C# prefieren XAML también pueden crear enlaces de origen de datos (Recuerde incluir una instrucción `using Xamarin.Forms.Pages;`):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

Es un poco más trabajo crear temas desde cero, pero las versiones preliminares futuras harán que esto sea más fácil.

## <a name="troubleshooting"></a>Solución de problemas

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>No se pudo cargar el archivo o ensamblado 'Xamarin.Forms.Theme.Light' o uno de sus dependencias

En la versión preliminar, los temas no pueda cargar en tiempo de ejecución. Agregue el código que se muestra a continuación, en los proyectos correspondientes para corregir este error.

**iOS**

En **AppDelegate.CS** , agregue las siguientes líneas después de `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

En **MainActivity.CS** , agregue las siguientes líneas después de `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>Vínculos relacionados

- [Ejemplo de DataPagesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
