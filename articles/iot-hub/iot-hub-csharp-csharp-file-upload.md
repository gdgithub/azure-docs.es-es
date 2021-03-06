---
title: Carga de archivos desde dispositivos mediante IoT Hub | Microsoft Docs
description: Siga este tutorial para aprender a cargar archivos desde dispositivos con el centro de IoT de Azure con C#.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: ce514e19370d2b42fb16b4e96b66f212d5fa999c
ms.openlocfilehash: 2310e7eab7d2649f91264798ad59cd4cfe92fa96


---
# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Tutorial: cómo cargar archivos desde dispositivos a la nube con un centro de IoT
## <a name="introduction"></a>Introducción

Azure IoT Hub es un servicio totalmente administrado que permite la comunicación bidireccional de confianza y segura entre millones de dispositivos y un back-end de aplicación. En tutoriales anteriores ([Introducción a Iot Hub] y [Envío de mensajes de nube a dispositivo con IoT Hub]) se muestra cómo usar la funcionalidad básica de mensajería de dispositivo a nube y de nube a dispositivo de IoT Hub. En el [tutorial de procesamiento de mensajes de dispositivo a nube] se describe una forma de almacenar de manera confiable los mensajes enviados del dispositivo a la nube en Azure Blob Storage. Sin embargo, en algunos casos no se pueden asignar fácilmente los datos de que los dispositivos envían en los mensajes de dispositivo a nube con un tamaño relativamente reducido que acepta el Centro de IoT. Algunos ejemplos son los archivos de gran tamaño que contienen imágenes, vídeos, muestras de datos de vibración a alta frecuencia, o bien archivos que contienen algún tipo de datos preprocesados. Dichos archivos se suelen procesar por lotes en la nube mediante herramientas como [Azure Data Factory] o la pila [Hadoop]. Cuando se prefiera cargar un archivo desde un dispositivo en lugar de enviar eventos, se puede utilizar la funcionalidad de fiabilidad y seguridad del Centro de IoT.

Este tutorial se basa en el código de [Envío de mensajes de nube a dispositivo con IoT Hub] para mostrarle cómo usar las funcionalidades de carga de archivos del Centro de IoT. En él se muestra cómo realizar las siguientes acciones:

- Proporcionar un dispositivo de forma segura con un identificador URI de blob de Azure para cargar un archivo.
- Usar las notificaciones de carga de archivo de IoT Hub para desencadenar el procesamiento del archivo en el back-end de aplicación.

Al final de este tutorial, ejecutará dos aplicaciones de consola de Windows:

* **SimulatedDevice**, una versión modificada de la aplicación que creó en el tutorial [Envío de mensajes de nube a dispositivo con IoT Hub]. Esta aplicación carga un archivo en el almacenamiento mediante un identificador URI de SAS proporcionado por el centro de IoT.
* **ReadFileUploadNotification**, que recibe notificaciones de carga de archivos del Centro de IoT.

> [!NOTE]
> El Centro de IoT admite muchas plataformas de dispositivos y lenguajes (incluido C, Java y JavaScript), mediante los SDK de dispositivo IoT de Azure. Consulte el [Centro para desarrolladores de IoT de Azure] para obtener instrucciones paso a paso sobre cómo conectar el dispositivo al Centro de IoT de Azure.
> 
> 

Para completar este tutorial, necesitará lo siguiente:

* Microsoft Visual Studio 2015
* Una cuenta de Azure activa. (En caso de no tenerla, puede crear una [cuenta gratuita][lnk-free-trial] en solo unos minutos).

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Asociación de una cuenta de Almacenamiento de Azure al Centro de IoT
Como la aplicación de dispositivo simulado carga un archivo en un blob, debe tener una cuenta de [Azure Storage] asociada a IoT Hub. Cuando se asocia una cuenta de Azure Storage con un centro de IoT, el centro puede generar un identificador URI de SAS que un dispositivo puede usar para cargar un archivo de manera segura en un contenedor de blobs. El servicio Centro de IoT y los SDK de dispositivo coordinan el proceso que genera el URI de SAS y permite que esté disponible para un dispositivo para cargar un archivo.

Siga las instrucciones de [Configuración de cargas de archivos mediante Azure Portal][lnk-configure-upload] para asociar una cuenta de Azure Storage a su centro de IoT.

## <a name="upload-a-file-from-a-simulated-device-app"></a>Carga de un archivo desde una aplicación de dispositivo simulado
En esta sección, modificará la aplicación de dispositivo simulado que creó en [Envío de mensajes de nube a dispositivo con IoT Hub] para recibir mensajes de nube a dispositivo de IoT Hub.

1. En Visual Studio, haga clic con el botón derecho en el proyecto **SimulatedDevice**, haga clic en **Agregar** y luego haga clic en **Elemento existente**. Vaya a un archivo de imagen e inclúyalo en su proyecto. En este tutorial se supone que la imagen se llama `image.jpg`.
2. Haga doble clic en ella y luego haga clic en **Propiedades**. Asegúrese de que **Copiar en el directorio de salida** está establecido en **Copiar siempre**.
   
    ![][1]
3. En el archivo **Program.cs** , agregue las siguientes instrucciones al principio del archivo:
   
        using System.IO;
4. Agregue el método siguiente a la clase **Program** :
   
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();
   
            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }
   
            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }
   
    El método `UploadToBlobAsync` toma el nombre de archivo y el origen de streaming del archivo que se va a cargar y administra la carga en el almacenamiento. La aplicación de consola muestra el tiempo que se tarda en cargar el archivo.
5. Agregue el siguiente método en el método **Main**, inmediatamente delante de la línea `Console.ReadLine()`:
   
        SendToBlobAsync();

> [!NOTE]
> Por simplificar, este tutorial no implementa ninguna directiva de reintentos. En el código de producción, deberá implementar directivas de reintentos (por ejemplo, retroceso exponencial), tal y como se sugiere en el artículo de MSDN [Transient Fault Handling](Control de errores transitorios).
> 
> 

## <a name="receive-a-file-upload-notification"></a>Recepción de una notificación de carga de archivos
En esta sección, se escribe una aplicación de consola de Windows que recibe mensajes de notificación de carga de archivos del Centro de IoT.

1. En la solución actual de Visual Studio, cree un proyecto de aplicación de Visual C# para Windows con la plantilla de proyecto de **Aplicación de consola** . Denomine al proyecto **ReadFileUploadNotification**.
   
    ![Nuevo proyecto en Visual Studio][2]
2. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **ReadFileUploadNotification** y luego haga clic en **Administrar paquetes NuGet**.
   
    Esta acción muestra la ventana Administrar paquetes NuGet.
3. Busque `Microsoft.Azure.Devices`, haga clic en **Instalar**y acepte las condiciones de uso. 
   
    Esta acción descarga, instala y agrega una referencia al [paquete NuGet del SDK de servicio de IoT de Azure] en el proyecto **ReadFileUploadNotification**.
4. En el archivo **Program.cs** , agregue las siguientes instrucciones al principio del archivo:
   
        using Microsoft.Azure.Devices;
5. Agregue los campos siguientes a la clase **Program** . Sustituya el valor de marcador de posición por la cadena de conexión de IoT Hub de [Introducción a Iot Hub]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Agregue el método siguiente a la clase **Program** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();
   
            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();
   
                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }
   
    Tenga en cuenta que el patrón de recepción es el mismo que se usa para recibir mensajes de nube a dispositivo desde la aplicación de dispositivo.
7. Por último, agregue las líneas siguientes al método **Main** :
   
        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Ejecución de las aplicaciones
Ahora está preparado para ejecutar las aplicaciones.

1. Desde Visual Studio, haga clic con el botón derecho en la solución y seleccione **Establecer proyectos de inicio**. Seleccione **Proyectos de inicio múltiples**, después, elija la acción **Iniciar** para **ReadFileUploadNotification** y **SimulatedDevice**.
2. Presione **F5**. Deberían iniciarse las dos aplicaciones. Debería ver que se ha completado la carga en una aplicación de consola y que la otra aplicación de consola ha recibido el mensaje de notificación de carga. Puede usar [Azure Portal] o el Explorador de servidores de Visual Studio para comprobar que el archivo cargado está en la cuenta de Azure Storage.
   
   ![][50]

## <a name="next-steps"></a>Pasos siguientes
En este tutorial ha aprendido a usar la funcionalidad de carga de archivos del Centro de IoT para simplificar la carga de archivos desde los dispositivos. Puede continuar explorando las características y los escenarios del centro de IoT con los siguientes artículos:

* [Creación de un centro de IoT mediante programación][lnk-create-hub]
* [Introducción al SDK de C][lnk-c-sdk]
* [SDK de IoT de Azure][lnk-sdks]

Para explorar aún más las funcionalidades de Centro de IoT, consulte:

* [Simulación de un dispositivo con el SDK de puerta de enlace de IoT][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure Portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Envío de mensajes de nube a dispositivo con IoT Hub]: iot-hub-csharp-csharp-c2d.md
[tutorial de procesamiento de mensajes de dispositivo a nube]: iot-hub-csharp-csharp-process-d2c.md
[Introducción a Iot Hub]: iot-hub-csharp-csharp-getstarted.md
[Centro para desarrolladores de IoT de Azure]: http://www.azure.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[paquete NuGet del SDK de servicio de IoT de Azure]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md





<!--HONumber=Nov16_HO5-->


