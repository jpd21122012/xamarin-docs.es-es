---
title: Calendario de Xamarin. Android
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: d9c655a503ebdd9ebc08646fb385cdd8f4aa5753
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725172"
---
# <a name="xamarinandroid-calendar"></a>Calendario de Xamarin. Android

## <a name="calendar-api"></a>API de calendario

Un nuevo conjunto de API de calendario introducido en Android 4 es compatible con aplicaciones diseñadas para leer o escribir datos en el proveedor de calendario. Estas API admiten una gran cantidad de opciones de interacción con datos de calendario, incluida la capacidad de leer y escribir eventos, asistentes y recordatorios. Al usar el proveedor de calendario en la aplicación, los datos que agregue a través de la API aparecerán en la aplicación de calendario integrada que se incluye en Android 4.

## <a name="adding-permissions"></a>Agregar permisos

Al trabajar con las nuevas API de calendario en la aplicación, lo primero que debe hacer es agregar los permisos adecuados al manifiesto de Android. Los permisos que necesita agregar son `android.permisson.READ_CALENDAR` y `android.permission.WRITE_CALENDAR`, en función de si está leyendo o escribiendo datos de calendario.

## <a name="using-the-calendar-contract"></a>Usar el contrato de calendario

Una vez establecidos los permisos, puede interactuar con los datos del calendario mediante la clase `CalendarContract`. Esta clase proporciona un modelo de datos que las aplicaciones pueden usar cuando interactúan con el proveedor de calendario. El `CalendarContract` permite a las aplicaciones resolver los URI para entidades de calendario, como calendarios y eventos. También proporciona una manera de interactuar con varios campos de cada entidad, como el nombre y el identificador de un calendario, o la fecha de inicio y finalización de un evento.

Echemos un vistazo a un ejemplo que usa la API Calendar. En este ejemplo, examinaremos cómo enumerar los calendarios y sus eventos, además de cómo agregar un nuevo evento a un calendario.

## <a name="listing-calendars"></a>Enumerar calendarios

En primer lugar, vamos a examinar cómo enumerar los calendarios que se han registrado en la aplicación de calendario. Para ello, podemos crear una instancia de un `CursorLoader`. Presentado en Android 3,0 (API 11), `CursorLoader` es la mejor manera de consumir un `ContentProvider`. Como mínimo, es necesario especificar el URI de contenido para los calendarios y las columnas que se van a devolver. Esta especificación de columna se conoce como _proyección_.

La llamada al método `CursorLoader.LoadInBackground` nos permite consultar los datos de un proveedor de contenido, como el proveedor de calendario.
`LoadInBackground` realiza la operación de carga real y devuelve una `Cursor` con los resultados de la consulta.

El `CalendarContract` nos ayuda a especificar tanto el `Uri` de contenido como la proyección. Para obtener el contenido `Uri` para consultar calendarios, podemos usar simplemente la propiedad `CalendarContract.Calendars.ContentUri` como esta:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

El uso del `CalendarContract` para especificar qué columnas de calendario queremos es igualmente simple. Solo se agregan campos de la clase `CalendarContract.Calendars.InterfaceConsts` a una matriz. Por ejemplo, el código siguiente incluye el identificador, el nombre para mostrar y el nombre de cuenta del calendario:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

La `Id` es importante incluir si usa un `SimpleCursorAdapter` para enlazar los datos a la interfaz de usuario, como veremos en breve. Con el URI de contenido y la proyección en su lugar, creamos una instancia de la `CursorLoader` y llamaremos al método `CursorLoader.LoadInBackground` para devolver un cursor con los datos del calendario como se muestra a continuación:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

La interfaz de usuario de este ejemplo contiene un `ListView`, con cada elemento de la lista que representa un solo calendario. En el código XML siguiente se muestra el marcado que incluye el `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

Además, es necesario especificar la interfaz de usuario para cada elemento de lista, que se coloca en un archivo XML independiente de la siguiente manera:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

A partir de este punto, es solo código de Android normal para enlazar los datos del cursor a la interfaz de usuario. Usaremos una `SimpleCursorAdapter` como se indica a continuación:

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

En el código anterior, el adaptador toma las columnas especificadas en la matriz de `sourceColumns` y las escribe en los elementos de la interfaz de usuario de la matriz de `targetResources` para cada entrada de calendario del cursor. La actividad utilizada aquí es una subclase de `ListActivity`; incluye la propiedad `ListAdapter` en la que se establece el adaptador.

Esta es una captura de pantalla que muestra el resultado final, con la información del calendario que se muestra en el `ListView`:

[![CalendarDemo que se ejecuta en el emulador, mostrando dos entradas de calendario](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)

## <a name="listing-calendar-events"></a>Enumerar eventos de calendario

A continuación, veamos cómo enumerar los eventos de un calendario determinado.
Basándose en el ejemplo anterior, se presentará una lista de eventos cuando el usuario seleccione uno de los calendarios. Por lo tanto, es necesario controlar la selección del elemento en el código anterior:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

En este código, vamos a crear una intención de abrir una actividad de tipo `EventListActivity`, pasando el identificador del calendario en el intento. Necesitaremos el identificador para saber en qué calendario se deben consultar los eventos. En el método de `OnCreate` del `EventListActivity`, podemos recuperar el identificador de la `Intent` como se muestra a continuación:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Ahora, vamos a consultar eventos para este identificador de calendario. El proceso de consulta de eventos es similar a la forma en que hemos consultado una lista de calendarios anteriores, pero esta vez trabajaremos con la clase `CalendarContract.Events`. En el código siguiente se crea una consulta para recuperar eventos:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

En este código, primero obtenemos el contenido `Uri` para los eventos de la propiedad `CalendarContract.Events.ContentUri`. A continuación, se especifican las columnas de evento que se desean recuperar en la matriz eventsProjection.
Por último, se crea una instancia de un `CursorLoader` con esta información y se llama al método `LoadInBackground` del cargador para devolver un `Cursor` con los datos del evento.

Para mostrar los datos de evento en la interfaz de usuario, podemos usar el marcado y el código tal como hicimos antes de mostrar la lista de calendarios. Una vez más, usamos `SimpleCursorAdapter` para enlazar los datos a una `ListView` como se muestra en el código siguiente:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

La diferencia principal entre este código y el código que se usó anteriormente para mostrar la lista de calendarios es el uso de un `ViewBinder`, que se establece en la línea:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

La clase `ViewBinder` nos permite controlar mejor cómo se enlazan los valores a las vistas. En este caso, se usa para convertir la hora de inicio del evento de milisegundos a una cadena de fecha, como se muestra en la siguiente implementación:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Esto muestra una lista de eventos, como se muestra a continuación:

[![captura de pantalla de la aplicación de ejemplo que muestra tres eventos de calendario](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)

## <a name="adding-a-calendar-event"></a>Agregar un evento de calendario

Hemos visto cómo leer los datos de calendario. Ahora veamos cómo agregar un evento a un calendario. Para que esto funcione, asegúrese de incluir el `android.permission.WRITE_CALENDAR` permiso que hemos mencionado anteriormente. Para agregar un evento a un calendario, haremos lo siguiente:

1. Cree una instancia de `ContentValues`.
1. Use las claves de la clase `CalendarContract.Events.InterfaceConsts` para rellenar la instancia de `ContentValues`.
1. Establezca las zonas horarias para las horas de inicio y finalización del evento.
1. Use un `ContentResolver` para insertar los datos de evento en el calendario.

En el código siguiente se muestran estos pasos:

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Tenga en cuenta que si no se establece la zona horaria, se producirá una excepción de tipo `Java.Lang.IllegalArgumentException`. Dado que los valores de tiempo de evento deben expresarse en milisegundos desde la época, creamos un método `GetDateTimeMS` (en `EventListActivity`) para convertir nuestras especificaciones de fecha en formato de milisegundos:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Si agregamos un botón a la interfaz de usuario de la lista de eventos y ejecutamos el código anterior en el controlador de eventos Click del botón, el evento se agrega al calendario y se actualiza en la lista, como se muestra a continuación:

[![captura de pantalla de la aplicación de ejemplo con eventos de calendario seguidos por el botón Agregar evento de ejemplo](calendar-images/13.png)](calendar-images/13.png#lightbox)

Si se abre la aplicación de calendario, veremos que el evento también se escribe allí:

[![captura de pantalla de la aplicación de calendario que muestra el evento de calendario seleccionado](calendar-images/14.png)](calendar-images/14.png#lightbox)

Como puede ver, Android permite un acceso eficaz y sencillo para recuperar y conservar datos de calendario, lo que permite a las aplicaciones integrar sin problemas las capacidades del calendario.

## <a name="related-links"></a>Vínculos relacionados

- [Demo de calendario (ejemplo)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
