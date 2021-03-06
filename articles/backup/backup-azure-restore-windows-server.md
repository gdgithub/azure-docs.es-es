---
title: "Restauración de datos en Windows Server o un cliente de Windows desde Azure mediante el modelo de implementación de Resource Manager | Microsoft Docs"
description: Aprenda a restaurar desde Windows Server o desde el Cliente de Windows.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2016
ms.author: trinadhk; jimpark; markgal;
translationtype: Human Translation
ms.sourcegitcommit: 9cf1faabe3ea12af0ee5fd8a825975e30947b03a
ms.openlocfilehash: 1ace5aa33201d9730b0708c3918597358f4dbd91


---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Restauración de archivos en un equipo de Windows Server o cliente de Windows mediante el modelo de implementación de Resource Manager
> [!div class="op_single_selector"]
> * [Portal de Azure](backup-azure-restore-windows-server.md)
> * [Portal clásico](backup-azure-restore-windows-server-classic.md)
> 
> 

En este artículo se describen los pasos necesarios para realizar dos tipos de operaciones de restauración:

* La restauración de datos en la misma máquina desde la cual se realizaron las copias de seguridad.
* La restauración de datos en cualquier otra máquina.

En ambos casos, los datos se recuperan del almacén de Servicios de recuperación de Azure.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="recover-data-to-the-same-machine"></a>Recuperar los datos en la misma máquina
Si ha eliminado accidentalmente un archivo y desea restaurarlo en la misma máquina (desde la que se realizó la copia de seguridad), los pasos siguientes le ayudarán a recuperar esos datos.

1. Abra el complemento **Copia de seguridad de Microsoft Azure** .
2. Haga clic en **Recuperar datos** para iniciar el flujo de trabajo.
   
    ![Recuperar datos](./media/backup-azure-restore-windows-server/recover.png)
3. Seleccione la opción **Este servidor (*nombreDeLaMáquina*)**, para restaurar en la misma máquina.el archivo del que ha creado una copia de seguridad.
   
    ![Misma máquina](./media/backup-azure-restore-windows-server/samemachine.png)
4. Elija entre **Examinar archivos** o **Buscar archivos**.
   
    Deje la opción predeterminada si va a restaurar uno o varios archivos cuya ruta de acceso se conoce. Si no está seguro de la estructura de carpetas pero desea buscar un archivo, escoja la opción **Buscar archivos** . En esta sección, se continuará con la opción predeterminada.
   
    ![Examinar archivos](./media/backup-azure-restore-windows-server/browseandsearch.png)
5. Seleccione el volumen desde el que desea restaurar el archivo.
   
    Puede realizar la restauración a un momento dado. Las fechas que aparecen en **negrita** en el control del calendario indican la disponibilidad de un punto de restauración. Una vez seleccionada una fecha, según su programación de copia de seguridad (y el éxito que haya tenido al realizar una operación de copia de seguridad), puede seleccionar un momento dato en el menú desplegable **Tiempo** .
   
    ![Fecha y volumen](./media/backup-azure-restore-windows-server/volanddate.png)
6. Seleccione los elementos que desea recuperar. Puede seleccionar varias carpetas y archivos para restaurar.
   
    ![Seleccionar archivos](./media/backup-azure-restore-windows-server/selectfiles.png)
7. Especifique los parámetros de recuperación.
   
    ![Opciones de recuperación](./media/backup-azure-restore-windows-server/recoveroptions.png)
   
   * Tiene la opción de restaurar en la ubicación original (en la que la carpeta o el archivos se sobrescribirán) o en otra ubicación en la misma máquina.
   * Si el archivo o carpeta que desea restaurar existe en la ubicación de destino, tiene la opción de crear copias (dos versiones del mismo archivo), sobrescribir los archivos en la ubicación de destino u omitir la recuperación de los archivos que existen en el destino.
   * Se recomienda que deje la opción predeterminada de la restauración de las ACL en los archivos que se está recuperando.
8. Una vez proporcionadas estas entradas, haga clic en **Siguiente**. El flujo de trabajo de recuperación que se encarga de restaurar los archivos de la máquina, se iniciará.

## <a name="recover-to-an-alternate-machine"></a>Recuperar en una máquina alternativa
Si ha perdido todo el servidor, todavía puede recuperar los datos de la Copia de seguridad de Azure en una máquina diferente. Los pasos siguientes muestran el flujo de trabajo.  

La terminología usada en estos pasos incluye:

* *Máquina de origen* : es la máquina original desde la que se realizó la copia de seguridad y que no está disponible actualmente.
* *Máquina de destino* : es la máquina en la que se recuperan los datos.
* *Almacén de ejemplo*: el almacén de Recovery Services en el que se registran la*máquina de origen* y la *máquina de destino*. <br/>

> [!NOTE]
> No se pueden restaurar copias de seguridad realizadas desde una máquina en una máquina que está ejecutando una versión anterior del sistema operativo. Por ejemplo, si se realizan copias de seguridad de una máquina con Windows 7, esta puede restaurarse en un Windows 8 o una máquina con una versión superior. Sin embargo, la acción a la inversa no está asegurada.
> 
> 

1. Abra el complemento **Copia de seguridad de Microsoft Azure** en la *Máquina de destino*.
2. Asegúrese de que tanto la *máquina de destino* como la *máquina de origen* están registradas en el mismo almacén de Recovery Services.
3. Haga clic en **Recuperar datos** para iniciar el flujo de trabajo.
   
    ![Recuperar datos](./media/backup-azure-restore-windows-server/recover.png)
4. Seleccione **Otro servidor**
   
    ![Otro servidor](./media/backup-azure-restore-windows-server/anotherserver.png)
5. Proporcione el archivo de credenciales de almacén que se corresponde con el *Almacén de ejemplo*. Si el archivo de credenciales de almacén no es válido (o ha expirado), descargue un nuevo archivo de credenciales de almacén desde el *Almacén de ejemplo* en el portal de Azure. Después de que se proporciona el archivo de credenciales de almacén, se muestra el almacén de Servicios de recuperación en el archivo de almacén de credenciales.
6. Seleccione la *Máquina de origen* en la lista de máquinas mostradas.
   
    ![Lista de máquinas](./media/backup-azure-restore-windows-server/machinelist.png)
7. Seleccione la opción **Buscar archivos** o **Examinar archivos**. En esta sección, usaremos la opción **Buscar archivos** .
   
    ![Search](./media/backup-azure-restore-windows-server/search.png)
8. Seleccione el volumen y la fecha en la pantalla siguiente. Busque el nombre de la carpeta o el archivo que desea restaurar.
   
    ![Buscar artículos](./media/backup-azure-restore-windows-server/searchitems.png)
9. Seleccione la ubicación en la que se deben restaurar los archivos.
   
    ![Restaurar ubicación](./media/backup-azure-restore-windows-server/restorelocation.png)
10. Proporcione la frase de contraseña de cifrado que se proporcionó durante el registro de la *Máquina de origen* en el *Almacén de ejemplo*.
    
    ![Cifrado](./media/backup-azure-restore-windows-server/encryption.png)
11. Una vez especificada la entrada, haga clic en **Recuperar**, para desencadenar la restauración de los archivos con copia de seguridad en el destino proporcionado.

## <a name="next-steps"></a>Pasos siguientes
* Ahora que ha recuperado los archivos y las carpetas, puede [administrar las copias de seguridad](backup-azure-manage-windows-server.md).




<!--HONumber=Nov16_HO3-->


