---
title: El archivo IPA es de 0 bytes
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 376BBA27-8694-4E63-9976-BF60349D42D8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 052df9676e4e6e105b5345d6120b884a3dead11f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031008"
---
# <a name="ipa-file-is-0-bytes"></a>El archivo IPA es de 0 bytes

> [!IMPORTANT]
> Este problema se ha resuelto en las versiones recientes de Xamarin. Sin embargo, si el problema se produce en la versión más reciente del software, registre un [nuevo error](~/cross-platform/troubleshooting/questions/howto-file-bug.md) con la información de control de versiones completa y el resultado del registro de compilación completo.

Hubo algunos problemas conocidos en las versiones anteriores de Xamarin que podrían provocar que el archivo IPA en Windows fuese 0 bytes. 

### <a name="fixed-in-xamarin-for-visual-studio-311584"></a>Corregido en Xamarin para Visual Studio 3.11.584 

- [Error 24416: la compilación de la configuración "ad-hoc" desde la línea de comandos no copia el archivo IPA en Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24416)
- [Error 24417-cambio de "propiedades del proyecto-> Opciones de IPA de iOS-> nombre del paquete" evita que se vuelva a copiar el IPA en Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=24417)
- [El error 29822-[XVS. iOS 3,11] si se establece el número de "compilación" distinto del número de "versión", no se puede copiar IPA en Windows](https://bugzilla.xamarin.com/show_bug.cgi?id=29822)

### <a name="fixed-in-xamarin-for-visual-studio-410496"></a>Corregido en Xamarin para Visual Studio 4.1.0.496

- [Error 27989: se produce un error al mostrar el archivo IPA en el servidor de compilación si el nombre del ensamblado no coincide con el nombre del proyecto](https://bugzilla.xamarin.com/show_bug.cgi?id=27989)
