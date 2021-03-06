---
title: Personalización de enlaces
description: Puede personalizar un enlace de Xamarin. Android editando los metadatos que controlan el proceso de enlace. Estas modificaciones manuales suelen ser necesarias para resolver errores de compilación y para dar forma a la API resultante para que sea más coherente con C#/.net. En estas guías se explica la estructura de estos metadatos, cómo modificar los metadatos y cómo usar JavaDoc para recuperar los nombres de los parámetros de método.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 04f3720d8684129476c955819390e91330a7800a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020653"
---
# <a name="customizing-bindings"></a>Personalización de enlaces

_Puede personalizar un enlace de Xamarin. Android editando los metadatos que controlan el proceso de enlace. Estas modificaciones manuales suelen ser necesarias para resolver errores de compilación y para dar forma a la API resultante para que sea más coherente con C#/.net. En estas guías se explica la estructura de estos metadatos, cómo modificar los metadatos y cómo usar JavaDoc para recuperar los nombres de los parámetros de método._

## <a name="overview"></a>Información general

Xamarin. Android automatiza gran parte del proceso de enlace. sin embargo, en algunos casos, se requiere una modificación manual para solucionar los problemas siguientes:

- Resolver errores de compilación causados por tipos que faltan, tipos ofuscados, nombres duplicados, problemas de visibilidad de clases y otras situaciones que las herramientas de Xamarin. Android no pueden resolver. 

- Cambio de la asignación que usa Xamarin. Android para enlazar la API de Android a C# diferentes tipos en (por ejemplo, muchos desarrolladores prefieren asignar constantes de Java C# `int`a constantes de`enum`).

- Quitar los tipos no usados que no es necesario enlazar. 

- Agregar tipos que no tienen homólogos en la API de Java subyacente. 

Puede realizar algunos o todos estos cambios modificando los metadatos que controlan el proceso de enlace.

## <a name="guides"></a>Guías

En las guías siguientes se describen los metadatos que controlan el proceso de enlace y se explica cómo modificarlos para solucionar estos problemas:

- Los [metadatos de enlaces de Java](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) proporcionan información general de los metadatos que entran en un enlace de Java.
    Describe los distintos pasos manuales que a veces se requieren para completar una biblioteca de enlace de Java y explica cómo dar forma a una API expuesta por un enlace para seguir las instrucciones de diseño de .NET.

- [El nombre de los parámetros con javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) explica cómo recuperar los nombres de los parámetros en un proyecto de enlace de Java mediante el uso de Javadoc generado a partir del proyecto de Java enlazado.
