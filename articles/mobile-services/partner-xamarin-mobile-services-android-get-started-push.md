---
title: Incorporación de notificaciones push a la aplicación Xamarin Android | Microsoft Docs
description: Obtenga información acerca de cómo configurar las notificaciones push con el Servicio de mensajería en la nube de Google para sus aplicaciones de Xamarin.Android con los Servicios móviles de Azure y los Centros de notificaciones de Azure.
documentationcenter: xamarin
author: ggailey777
manager: dwrede
services: mobile-services
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/21/2016
ms.author: glenga

---
# Incorporación de notificaciones de inserción a la aplicación de Servicios móviles
[!INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Para más información sobre la versión de Aplicaciones móviles equivalente de este tema, consulte [Agregar notificaciones push a la aplicación de Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started-push.md).
> 
> 

## Información general
Este tema muestra cómo puede usar Servicios móviles de Azure para enviar notificaciones de inserción a una aplicación de Xamarin.Android. En este tutorial aprenderá a agregar notificaciones de inserción con el servicio de mensajería en la nube de Google (GCM) al proyecto [Introducción a Servicios móviles]. Cuando haya finalizado, el servicio móvil le enviará una notificación de inserción cada vez que se inserte un registro.

Este tutorial requiere lo siguiente:

* Una cuenta de Google activa.
* [Componente cliente Servicio de mensajería en la nube de Google] Agregará este componente durante este tutorial.

Ya debe tener instalados el componente [Servicios móviles de Azure] y Xamarin en el proyecto desde el momento en que completó [Introducción a Servicios móviles].

## <a id="register"></a>Habilitación del servicio de mensajería en la nube de Google
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a id="configure"></a>Configuración del servicio móvil para enviar solicitudes de inserción
[!INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

## <a id="update-scripts"></a>Actualización del script de inserción registrado para enviar notificaciones
> [!TIP]
> En los pasos siguientes se muestra cómo actualizar el script registrado para la operación de inserción en la tabla TodoItem del Portal de Azure clásico. También puede acceder a este script de servicios móviles y editarlo directamente en Visual Studio, en el nodo de Azure del Explorador de servidores.
> 
> 

[!INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../../includes/mobile-services-javascript-backend-android-push-insert-script.md)]

## <a id="configure-app"></a>Configuración del proyecto existente para notificaciones de inserción
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Incorporación de código de notificaciones de inserción a la aplicación
[!INCLUDE [mobile-services-xamarin-android-push-add-to-app](../../includes/mobile-services-xamarin-android-push-add-to-app.md)]

## <a id="test"></a>Prueba de las notificaciones de inserción en su aplicación
Puede probar la aplicación conectando directamente un teléfono Android con un cable USB o utilizando un dispositivo virtual en el emulador.

[!INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

Ha completado correctamente este tutorial.

## <a name="next-steps"></a>Pasos siguientes
Puede obtener más información acerca de los Servicios móviles y los Centros de notificaciones en los siguientes temas:

* [Introducción a la autenticación](mobile-services-android-get-started-users.md) <br/>Aprenda a autenticar a los usuarios de su aplicación con distintos tipos de cuentas con los servicios móviles.
* [¿Qué son los Centros de notificaciones?](../notification-hubs/notification-hubs-push-notification-overview.md) <br/>Obtenga más información sobre el funcionamiento de los Centros de notificaciones para entregar notificaciones a sus aplicaciones en todas las principales plataformas de cliente.
* [Depuración de aplicaciones de los Centros de notificaciones](http://go.microsoft.com/fwlink/p/?linkid=386630) </br>Obtenga orientación sobre la solución de problemas y la depuración de soluciones de los Centros de notificaciones.
* [Uso de la biblioteca de cliente .NET para Servicios móviles](mobile-services-dotnet-how-to-use-client-library.md) <br/>Obtenga más información sobre cómo usar Servicios móviles con código C# para Xamarin.
* [Referencia de scripts de servidor de Servicios móviles](mobile-services-how-to-use-server-scripts.md) <br/>Obtenga más información sobre cómo implementar lógica empresarial al servicio móvil.

<!-- URLs. -->
[Introducción a Servicios móviles]: mobile-services-ios-get-started.md

[Componente cliente Servicio de mensajería en la nube de Google]: http://components.xamarin.com/view/GCMClient/
[Servicios móviles de Azure]: http://components.xamarin.com/view/azure-mobile-services/

<!---HONumber=AcomDC_0727_2016-->