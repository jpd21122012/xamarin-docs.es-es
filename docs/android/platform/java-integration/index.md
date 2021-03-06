---
title: Integración de Java con Xamarin. Android
description: El ecosistema de Java incluye una colección de componentes diversa y inmensa. Muchos de estos componentes se pueden usar para reducir el tiempo necesario para desarrollar una aplicación de Android. En este documento se proporciona información general de alto nivel de algunas de las formas en que los desarrolladores pueden usar estos componentes de Java existentes para mejorar la experiencia de desarrollo de aplicaciones de Xamarin. Android.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: ecaa02e036c74074b4fa922ea079355b72ff02e2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020102"
---
# <a name="java-integration-with-xamarinandroid"></a>Integración de Java con Xamarin. Android

_El ecosistema de Java incluye una colección de componentes diversa y inmensa. Muchos de estos componentes se pueden usar para reducir el tiempo necesario para desarrollar una aplicación de Android. En este documento se proporciona información general de alto nivel de algunas de las formas en que los desarrolladores pueden usar estos componentes de Java existentes para mejorar la experiencia de desarrollo de aplicaciones de Xamarin. Android._

## <a name="overview"></a>Información general

Dada la extensión del ecosistema Java, es muy probable que cualquier funcionalidad determinada necesaria para una aplicación de Xamarin. Android ya se haya codificado en Java. Por este motivo, es atractivo probar y volver a usar estas bibliotecas existentes al crear una aplicación de Xamarin. Android.

Hay tres formas posibles de volver a usar las bibliotecas de Java en una aplicación de Xamarin. Android: 

- **Cree una biblioteca de enlaces de Java** &ndash; con esta técnica, se usa un proyecto de Xamarin. Android C# para crear contenedores alrededor de los tipos de Java. Una aplicación de Xamarin. Android puede hacer referencia C# a los contenedores creados por este proyecto y, a continuación, usar el archivo de`.jar`. 

- **Java Native interface** &ndash; la *interfaz* nativa de Java (JNI) es un marco que permite que el código que no es C++ Java C#(como o) llame a o sea llamado por el código de Java que se ejecuta dentro de una JVM. 

- **Porte el código** &ndash; este método implica tomar el código fuente Java y, a continuación, C#convertirlo en. Esto se puede hacer manualmente o mediante una herramienta automatizada como el enfoque. 

En el núcleo de las dos primeras técnicas se encuentra *Java Native Interface* (JNI). JNI es un marco que permite a las aplicaciones no escritas en Java interactuar con el código Java que se ejecuta en un Máquina virtual Java. Xamarin. Android usa JNI para crear *enlaces* para C# el código. 

La primera técnica es un enfoque más automatizado y declarativo para enlazar bibliotecas de Java. Implica el uso de Visual Studio para Mac o de un tipo de proyecto de Visual Studio proporcionado por Xamarin. Android &ndash; la biblioteca de enlaces de Java. Para crear correctamente estos enlaces, es posible que una biblioteca de enlaces de Java siga requiriendo algunas modificaciones manuales, pero no tantas como sea un enfoque de JNI puro. Vea [enlazar una biblioteca de Java](~/android/platform/binding-java-library/index.md) para obtener más información sobre las bibliotecas de enlace de Java. 

La segunda técnica, con JNI, funciona en un nivel mucho menor, pero puede proporcionar un mayor control y acceso a los métodos de Java que normalmente no serían accesibles a través de una biblioteca de enlace de Java. 

La tercera técnica es radicalmente diferente de las dos anteriores: trasladar el código de Java a C#. Trasladar código de un idioma a otro puede ser un proceso muy laborioso, pero es posible reducir ese esfuerzo con la ayuda de una herramienta denominada *nitidez*. Sharp es una herramienta de código abierto que es una herramienta de JavaC# a convertidor. 

## <a name="summary"></a>Resumen

En este documento se proporciona información general de alto nivel sobre algunas de las distintas formas en que las bibliotecas de Java se pueden volver a usar en una aplicación de Xamarin. Android. Se introdujeron los conceptos de los enlaces y los contenedores a los que se puede llamar administrados, y se explican las opciones para migrar código Java a C#. 

## <a name="related-links"></a>Vínculos relacionados

- [Arquitectura](~/android/internals/architecture.md)
- [Enlace de una biblioteca Java](~/android/platform/binding-java-library/index.md)
- [Trabajar con JNI](~/android/platform/java-integration/working-with-jni.md)
- [Dar nitidez](https://github.com/slluis/sharpen)
- [Interfaz nativa de Java](https://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
