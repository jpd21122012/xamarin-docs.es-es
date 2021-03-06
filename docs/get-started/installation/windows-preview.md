---
title: Instalación de la versión preliminar de Xamarin en Windows
description: En este documento se describe cómo instalar una versión preliminar de Xamarin en Visual Studio 2019 mediante el canal de versión preliminar.
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: conceptdev
ms.author: crdun
ms.date: 03/20/2018
ms.openlocfilehash: 805edf0aba0e45a631bb9e3f7536da3f18dc789d
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "70277654"
---
# <a name="installing-xamarin-preview-on-windows"></a>Instalación de la versión preliminar de Xamarin en Windows

En Visual Studio 2019 y Visual Studio 2017 no se admiten los canales alfa, beta ni estable de la misma manera que en versiones anteriores. En su lugar, hay solo dos opciones:

- **Lanzamiento**: equivalente al canal _estable_ en Visual Studio para Mac
- **Versión preliminar**: equivalente a los canales _alfa_ y _beta_ en Visual Studio para Mac

> [!TIP]
> Para probar las características de versión preliminar, debe [descargar el instalador de Visual Studio Preview](https://visualstudio.microsoft.com/vs/preview/), que le ofrece la opción de instalar **versiones preliminares** de Visual Studio en paralelo con la versión estable (versión de lanzamiento). En las [notas de la versión](https://docs.microsoft.com/visualstudio/releases/2019/release-notes) puede encontrar más información sobre las novedades de Visual Studio 2019.

La versión preliminar de Visual Studio puede contener las versiones preliminares correspondientes de la funcionalidad de Xamarin, incluidas:

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Generador de perfiles de Xamarin
- Xamarin Inspector
- Simulador iOS remoto de Xamarin

La captura de pantalla del **instalador de versión preliminar** siguiente muestra las opciones de versión preliminar y lanzamiento (tenga en cuenta los números de versión en gris: la versión 15.0 es de lanzamiento y la versión 15.1 es una versión preliminar):

![instalador que muestra las opciones de versión preliminar](windows-images/vs2017-installer.jpg)

Durante el proceso de instalación, se puede aplicar un **alias de instalación** a la instalación en paralelo (por lo que pueden distinguirse en el menú Inicio), tal y como se muestra a continuación:

[![edición del alias antes de la instalación](windows-images/vs2017-nickname-sml.png "edición del alias antes de la instalación")](windows-images/vs2017-nickname.png#lightbox)

### <a name="uninstalling-visual-studio-2019-preview"></a>Desinstalación de Visual Studio 2019 Preview

El **Instalador de Visual Studio** también se debe usar para desinstalar las versiones preliminares de Visual Studio 2019. Lea la [guía de desinstalación de Xamarin](uninstalling-xamarin.md#uninstallvs2017) para más información.
