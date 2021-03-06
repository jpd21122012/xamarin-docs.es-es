---
title: Recarga activa de XAML para Xamarin. Forms
description: Vuelva a cargar los cambios en el archivo XAML al instante en la aplicación en ejecución, por lo que no tiene que compilar el proyecto de Xamarin. Forms después de cada cambio de XAML.
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 08/13/2019
ms.openlocfilehash: d94f18d00ebf6eeec5f33343b5c0f985ba2a6ea8
ms.sourcegitcommit: 9ab907e053c57fc96419149f83187bc3e8983a6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2020
ms.locfileid: "75655412"
---
# <a name="xaml-hot-reload-for-xamarinforms-preview"></a>Recarga activa de XAML para Xamarin. Forms (versión preliminar)

La recarga activa de XAML se conecta al flujo de trabajo existente para aumentar su productividad y ahorrar tiempo. Sin la recarga activa de XAML, debe compilar e implementar la aplicación cada vez que quiera ver un cambio de XAML. Con la recarga activa, al guardar el archivo XAML, los cambios se reflejan en directo en la aplicación en ejecución. Además, se mantendrán el estado y los datos de navegación, lo que le permitirá iterar rápidamente en la interfaz de usuario sin perder su lugar en la aplicación. Por lo tanto, con la recarga activa de XAML, pasará menos tiempo recompilando e implementando sus aplicaciones para validar los cambios de la interfaz de usuario.

> [!NOTE]
> Si está escribiendo una aplicación de WPF o de UWP, consulte [recarga en caliente de XAML para UWP y WPF](/visualstudio/debugger/xaml-hot-reload).
>
> La recarga activa de XAML para Xamarin. Forms _no_ funciona actualmente para los proyectos de UWP. Forms de Xamarin.

## <a name="system-requirements"></a>Requisitos del sistema

| IDE/marco de trabajo | Versión necesaria |
|------|------------------|
|Visual Studio 2019 | 16,4 o superior
Visual Studio 2019 para Mac | 8,4 o superior
Xamarin.Forms | 4,1 o superior

## <a name="use-xaml-hot-reload-for-xamarinforms"></a>Usar la recarga activa de XAML para Xamarin. Forms

No se requiere ninguna instalación o configuración adicional para usar la recarga activa de XAML. Está integrado en Visual Studio y puede habilitarse en la configuración del IDE. Una vez habilitada, puede empezar a usar la recarga activa de XAML mediante la depuración de la aplicación en un emulador, un simulador o un dispositivo físico. Actualmente, la recarga activa de XAML solo funciona durante la depuración en iOS o Android.

En Windows, la recarga activa de XAML se puede habilitar activando la casilla **Habilitar recarga activa de Xamarin** en **herramientas** > **Opciones** > **Xamarin** > la **recarga activa**.

En un equipo Mac, la recarga activa de XAML se puede habilitar activando la casilla **Habilitar la recarga activa de Xamarin** en **Visual Studio** > **preferencias** > **proyectos** > la **recarga activa de Xamarin**.

## <a name="resilient-reloading"></a>Recarga resistente

Si realiza un cambio que no se puede volver a cargar la recarga activa de XAML, se mostrará un mensaje de error con IntelliSense. Estos cambios, conocidos como ediciones forzadas, incluyen escribir indirectamente el código XAML o conectar un control a un controlador de eventos que no existe. Incluso con una edición forzada, puede seguir recargando sin reiniciar la aplicación: haga otro cambio en otra parte del archivo XAML y presione guardar. La edición forzada no se recargará, pero se seguirán aplicando los demás cambios.

## <a name="known-limitations"></a>Limitaciones conocidas

- No se pueden agregar, quitar ni cambiar el nombre de archivos o paquetes NuGet durante una sesión de recarga activa de XAML. Si agrega o quita un archivo o un paquete de NuGet, vuelva a compilar y volver a implementar la aplicación para seguir usando la recarga activa de XAML.
- Establezca el enlazador en **no vincular** para obtener la mejor experiencia. La configuración de **Link SDK solo** funciona la mayor parte del tiempo, pero puede producir un error en ciertos casos.
- La depuración en un iPhone físico requiere que el intérprete use la recarga activa de XAML. Para ello, abra la configuración del proyecto, seleccione la pestaña compilación de iOS y asegúrese **de que la opción Habilitar el intérprete mono** está habilitada. Es posible que tenga que cambiar la opción de **plataforma** en la parte superior de la página de propiedades a **iPhone**.
- Las referencias creadas mediante la asignación de un control a otro campo o propiedad mediante su valor `x:Name` no se volverán a cargar.
- La actualización de la jerarquía visual de la aplicación de Shell en **AppShell. Xaml** puede producir problemas al mantener el estado de la aplicación. Vuelva a compilar la aplicación para continuar con la recarga.
- La recarga activa de XAML no puede C# volver a cargar el código, incluidos los controladores de eventos, los controles personalizados, el código subyacente de la página y las clases adicionales.
- No _funciona en_ otras plataformas compatibles con Xamarin. Forms (como Mac OS o UWP).

## <a name="migrate-from-the-private-preview"></a>Migración desde la versión preliminar privada

Si forma parte de la versión preliminar privada, la extensión de recarga activa de XAML se actualizará automáticamente cuando se actualice Visual Studio. Si decide no actualizar Visual Studio, puede seguir usando la versión actual de la recarga activa de XAML, pero no recibirá ninguna actualización adicional a través de la fuente de extensión de la versión preliminar privada.

## <a name="troubleshooting"></a>Solucionar problemas

- Si no se puede inicializar la recarga activa de XAML:
  - Actualice la versión de Xamarin. Forms.
  - Asegúrese de que se encuentre en la versión más reciente del IDE.
  - Establezca la configuración del enlazador de Android o iOS en **no vincular** en la configuración de compilación del proyecto.
- Si no ocurre nada al guardar el archivo XAML, asegúrese de que la recarga activa está habilitada en el IDE.
- Si realiza la depuración en un iPhone físico y la aplicación deja de responder, compruebe que el intérprete está habilitado. Para activarla, abra la configuración del proyecto, seleccione la pestaña compilación de iOS y active la opción **Habilitar el intérprete mono** .

Para notificar un error, use la herramienta de comentarios en el menú **ayuda** > **Enviar comentarios** > **informar de un problema** en Windows y el menú **ayuda** > **informar de un problema** en un equipo Mac.
