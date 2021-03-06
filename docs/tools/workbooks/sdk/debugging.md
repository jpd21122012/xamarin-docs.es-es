---
title: Depuración de integraciones
description: En este documento se describe cómo depurar integraciones de Xamarin Workbooks, tanto de agente como de cliente en Windows y Mac.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 3030bf907d1ddbb02884f997f178b55950b442d4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018815"
---
# <a name="debugging-integrations"></a>Depuración de integraciones

## <a name="debugging-agent-side-integrations"></a>Depurar integraciones del agente

La depuración de integraciones del agente se realiza mejor mediante los métodos de registro proporcionados por la clase `Log` en `Xamarin.Interactive.Logging`.

En macOS, los mensajes de registro aparecen en el menú del visor de registros (**Window > log Viewer**) y en el registro de cliente. En Windows, los mensajes solo aparecen en el registro de cliente, ya que no hay ningún visor de registros.

El registro de cliente se encuentra en las siguientes ubicaciones de macOS y Windows:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Es importante tener en cuenta que, al cargar integraciones a través del mecanismo de `#r` habitual durante el desarrollo, el ensamblado de integración se recogerá como una _dependencia_ del libro y se empaquetará con él si no se usa una ruta de acceso absoluta. Esto puede hacer que parezca que los cambios no se propaguen, como si volvera a generar la integración no hiciera nada.

## <a name="debugging-client-side-integrations"></a>Depurar integraciones del lado cliente

Como las integraciones del lado cliente se escriben en JavaScript y se cargan en nuestra superficie del explorador Web (consulte la documentación de la [arquitectura](~/tools/workbooks/sdk/architecture.md) ), la mejor manera de depurarlos es usar las herramientas de desarrollo de WebKit en Mac o usar el selector F12 en Windows.

Ambos conjuntos de herramientas permiten ver el origen de JavaScript o TypeScript, establecer puntos de interrupción, ver la salida de la consola e inspeccionar y modificar el DOM.

### <a name="mac"></a>Mac

Para habilitar las herramientas de desarrollo para Xamarin Workbooks en Mac, ejecute el siguiente comando en el terminal:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

y, a continuación, reinicie Xamarin Workbooks. Una vez hecho esto, debería ver que el **elemento inspeccionar** aparece en el menú contextual del clic con el botón secundario y un nuevo panel para **desarrolladores** estará disponible en las preferencias de los libros. Esta opción le permite elegir si desea abrir las herramientas de desarrollo en el inicio:

[panel para desarrolladores de![](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Esta preferencia es solo restart: debe reiniciar el cliente de los libros para que surta efecto en los nuevos libros. Al activar las herramientas de desarrollo mediante el menú contextual o las preferencias, se mostrará la conocida interfaz de usuario de Safari:

[herramientas de desarrollo de![Safari](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Para obtener información sobre el uso de las herramientas de desarrollo de Safari, consulte la [documentación del inspector de WebKit][webkit-docs].

### <a name="windows"></a>Windows

En Windows, el equipo de IE proporciona una herramienta conocida como "F12 selector", que es un depurador remoto para las instancias de Internet Explorer incrustadas. La herramienta se puede encontrar en la siguiente ubicación:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Ejecute el selector F12 y debería ver la instancia incrustada que alimenta la superficie del cliente libros en la lista. Elíjalo y las conocidas herramientas de depuración F12 de Internet Explorer aparecerán adjuntas al cliente:

[herramientas de![F12](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
