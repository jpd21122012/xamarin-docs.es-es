---
title: "Android.Support.v7.AppCompat: no se encontró ningún recurso que coincida con el nombre especificado: attr 'android: actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: e688bd27d1116b2a77a12ccd6da29ea582053581
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728114"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat: no se encontró ningún recurso que coincida con el nombre especificado: attr 'android: actionModeShareDrawable'

1. Asegúrese de descargar los extras más recientes, así como el SDK de Android 5,0 (API 21) a través del administrador de Android SDK.

2. Asegúrese de que está compilando la aplicación con compileSdkVersion establecido en 21. También puede establecer el valor de targetSdkVersion en 21.

3. Si necesita una versión anterior, como la API 19, descargue la versión correspondiente que se encuentra en la página de NuGet:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

> [!NOTE]
> Si lo instala manualmente a través de la consola del administrador de paquetes, asegúrese de instalar también la misma versión de Xamarin. Android. support. V4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Referencia de Stack Overflow: [https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Vea también

- [¿Qué paquetes de Android SDK debo instalar?](~/android/troubleshooting/questions/install-android-sdk-packages.md)
