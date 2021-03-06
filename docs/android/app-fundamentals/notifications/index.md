---
title: Notificaciones en Xamarin. Android
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: e0035414cb8a24e5571b2972ec638eec550c593c
ms.sourcegitcommit: 191f1f3b13a14e2afadcb95126c5f653722f126f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/30/2019
ms.locfileid: "75545561"
---
# <a name="notifications-in-xamarinandroid"></a>Notificaciones en Xamarin. Android

En esta sección se explica cómo implementar notificaciones en Xamarin. Android. Describe los distintos elementos de la interfaz de usuario de una notificación de Android y describe las API implicadas en la creación y visualización de una notificación.

## <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Notificaciones locales en Android](local-notifications.md)

En esta sección se explica cómo implementar notificaciones locales en Xamarin. Android. Se describen los distintos elementos de la interfaz de usuario de una notificación de Android y se describen las API relacionadas con la creación y visualización de una notificación.

## <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Tutorial: uso de notificaciones locales en Xamarin. Android](local-notifications-walkthrough.md)  

En este tutorial se explica cómo usar las notificaciones locales en una aplicación de Xamarin. Android. Muestra los aspectos básicos de la creación y publicación de una notificación. Cuando el usuario hace clic en la notificación en el cajón de notificaciones, inicia una segunda actividad. 

## <a name="further-reading"></a>Información adicional

[Firebase Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) Messaging &ndash; Firebase Cloud Messaging (FCM) es un servicio que facilita la mensajería entre aplicaciones móviles y aplicaciones de servidor. La mensajería en la nube Firebase se puede usar para implementar notificaciones remotas (también denominadas notificaciones de envío) en aplicaciones de Xamarin. Android.

Las [notificaciones](https://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; este tema para desarrolladores de Android es la guía definitiva para las notificaciones de Android. Incluye una sección de consideraciones de diseño que le ayuda a diseñar las notificaciones para que cumplan las directrices de la interfaz de usuario de Android. Proporciona más información sobre cómo conservar la navegación al iniciar una actividad y explica cómo mostrar el progreso en una notificación y controlar la reproducción multimedia en la pantalla de bloqueo.

[NotificationListenerService](xref:Android.Service.Notification.NotificationListenerService) &ndash; este servicio Android permite que la aplicación escuche (e interactúe con) todas las notificaciones publicadas en el dispositivo Android, no solo las notificaciones que la aplicación está registrada para recibir.
Tenga en cuenta que el usuario debe conceder explícitamente permiso a la aplicación para que pueda escuchar notificaciones en el dispositivo.

## <a name="related-links"></a>Vínculos relacionados

- [Notificaciones locales (ejemplo)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/localnotifications)
