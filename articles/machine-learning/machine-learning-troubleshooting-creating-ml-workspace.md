---
title: "Solución de problemas: Creación de un área de trabajo de Machine Learning y conexión a la misma | Microsoft Docs"
description: "Soluciones para problemas comunes al crear y conectarse a un área de trabajo de aprendizaje automático de Azure"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3b94e391730a69ed8cd07b2141d0b549482482b5


---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Guía de solución de problemas: Creación de un área de trabajo de Aprendizaje automático y conexión a la misma
Esta guía proporciona soluciones para algunos de los desafíos más comunes al configurar áreas de trabajo de Aprendizaje automático de Azure.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Propietario del espacio de trabajo
Cuando se crea un área de trabajo de Machine Learning nueva, el identificador que escribe en el campo PROPIETARIO DEL ÁREA DE TRABAJO debe ser una cuenta de Microsoft válida (anteriormente Windows Live ID), por ejemplo, john-contoso@live.com o john-contoso@hotmail.com.. No puede ser una cuenta que no sea de Microsoft, como su cuenta de correo electrónico corporativa. Para crear una cuenta gratuita de Microsoft, vaya a [www.live.com](http://www.live.com).

Observe que la cuenta que usó para iniciar sesión en el Portal de Azure y crear el área de trabajo no tiene automáticamente permiso para *abrir* esa área de trabajo, a menos que especifique esa cuenta como propietaria. Para abrir un área de trabajo en Estudio de aprendizaje automático, debe iniciar sesión en la cuenta de Microsoft que se definió como propietaria del área de trabajo, o bien, puede ser necesario que reciba una invitación del propietario para unirse al área de trabajo. Sin embargo, en el Portal de Azure clásico, puede *administrar* el área de trabajo, lo que incluye la capacidad de cambiar el propietario y configurar el acceso.

Para obtener más información sobre cómo administrar un área de trabajo, consulte [Administración de un área de trabajo de Aprendizaje automático de Azure].

[Administración de un área de trabajo de Aprendizaje automático de Azure]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Regiones permitidas
Aprendizaje automático se encuentra actualmente disponible en un número limitado de regiones. Si su suscripción no incluye una de estas regiones, es posible que vea el mensaje de error "No dispone de una suscripción en las regiones permitidas".

Para solicitar que una región se agregue a su suscripción, seleccione **Póngase en contacto con el servicio de soporte técnico de Microsoft** desde el Portal de Azure clásico, elija **Facturación** como el tipo de problema y siga las indicaciones para enviar su solicitud.

![Consultar al soporte técnico de Microsoft][screen1]

## <a name="storage-account"></a>Cuenta de almacenamiento
El servicio de Aprendizaje automático necesita una cuenta de almacenamiento para almacenar los datos. Puede utilizar una cuenta de almacenamiento existente o crear una nueva cuenta de almacenamiento al crear la nueva área de trabajo de Aprendizaje automático (si dispone de cuota para crear una nueva cuenta de almacenamiento).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Creación del espacio de trabajo][screen2]

Tras crear la nueva área de trabajo de Aprendizaje automático, puede iniciar sesión en Estudio de aprendizaje automático con la cuenta de Microsoft que especificó como propietaria del área de trabajo. Si encuentra el mensaje de error "No se encontró el área de trabajo" (similar a la captura de pantalla siguiente), use estos pasos para eliminar las cookies del explorador.

![Espacio de trabajo no encontrado][screen3]

**Para eliminar las cookies del explorador**

Si usa Internet Explorer, haga clic en el botón **Herramientas** en la esquina superior derecha y seleccione **Opciones de Internet**.  

![Opciones de Internet][screen4]

En la pestaña **General**, haga clic en **Eliminar…**

![Pestaña General][screen5]

En el cuadro de diálogo **Eliminar el historial de exploración**, asegúrese de que **Cookies y datos del sitio web** está seleccionado y haga clic en **Eliminar**.

![Eliminación de cookies][screen6]

Una vez eliminadas las cookies, reinicie el explorador y vaya a la página [Aprendizaje automático de Microsoft Azure](https://studio.azureml.net) . Cuando se le solicite un nombre de usuario y contraseña, especifique la misma cuenta de Microsoft que ha escrito como el propietario del espacio de trabajo.

Nuestro objeto es hacer que la experiencia de Aprendizaje automático sea lo más directa posible. Publique cualquier comentario o problema en el [foro de Aprendizaje automático de Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) para ayudarnos a ofrecerle un mejor servicio.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png



<!--HONumber=Nov16_HO3-->


