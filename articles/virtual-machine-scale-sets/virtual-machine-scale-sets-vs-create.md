---
title: Implementación de un conjunto de escalado de máquinas virtuales mediante Visual Studio | Microsoft Docs
description: Implementación de un conjunto de escalado de máquinas virtuales mediante Visual Studio y una plantilla de Resource Manager
services: virtual-machine-scale-sets
documentationcenter: ''
author: gbowerman
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: guybo

---
# Implementación de un conjunto de escalado de máquinas virtuales mediante Visual Studio
En este artículo se muestra cómo implementar un conjunto de escalado de máquinas virtuales de Azure mediante la utilización de una implementación de grupo de recursos de Visual Studio.

Los [conjuntos de escalado de máquinas virtuales de Azure](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) son un recurso de proceso de Azure para implementar y administrar un conjunto de máquinas virtuales similares con opciones integradas fácilmente para equilibrio de carga y escalado automático. Puede aprovisionar e implementar conjuntos de escalado de máquinas virtuales mediante [plantillas del Administrador de recursos de Azure (ARM)](https://github.com/Azure/azure-quickstart-templates). Las plantillas ARM se pueden implementar mediante CLI de Azure, PowerShell, REST y también directamente desde Visual Studio. Visual Studio proporciona un conjunto de plantillas que se pueden implementar como parte de un proyecto de implementación de grupo de recursos de Azure.

Las implementaciones de grupo de recursos de Azure son una forma de agrupar y publicar un conjunto de recursos de Azure relacionados en una única operación de implementación. Puede obtener más información aquí: [Creación e implementación de grupos de recursos de Azure mediante Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## Requisitos previos
Para empezar a implementar conjuntos de escalado de máquinas virtuales en Visual Studio necesita lo siguiente:

* Visual Studio 2013 o 2015
* Azure SDK 2.7, 2.8 o 2.9

Nota: en estas instrucciones se asume que utiliza Visual Studio 2015 con el [SDK de Azure 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## Creación de un proyecto
1. Cree un proyecto nuevo en Visual Studio 2015; para ello, seleccione **Archivo | Nuevo | Proyecto**.
   
    ![Archivo nuevo][file_new]
2. En **Visual C# | Nube**, seleccione **Administrador de recursos de Azure** para crear un proyecto para implementar una plantilla ARM.
   
    ![Crear proyecto][create_project]
3. En la lista de plantillas, seleccione la plantilla del conjunto de escalado de máquinas virtuales de Linux o Windows.
   
   ![Seleccionar plantilla][select_Template]
4. Una vez creado el proyecto, verá los scripts de implementación de PowerShell, una plantilla del Administrador de recursos de Azure y un archivo de parámetros para el conjunto de escalado de máquinas virtuales.
   
    ![Explorador de soluciones][solution_explorer]

## Personalización del proyecto
Ahora puede editar la plantilla para personalizarla en función de las necesidades de su aplicación, como agregar propiedades de extensión de máquinas virtuales o editar reglas de equilibrio de carga. De forma predeterminada, las plantillas de conjunto de escalado de máquinas virtuales se configuran para implementar la extensión AzureDiagnostics que facilita la operación de agregar reglas de escalado automático. También implementa un equilibrador de carga con una dirección IP pública, configurado con reglas NAT entrantes que permiten conectarse a las instancias de máquina virtual con SSH (Linux) o RDP (Windows): el intervalo de puertos front-end comienza en 50000, que significa que en el caso de Linux, si el SSH se remite al puerto 50000 de la dirección IP pública (o el nombre de dominio), se le enrutará al puerto 22 de la primera máquina virtual del conjunto de escalado. La conexión al puerto 50001 lo enrutará al puerto 22 de la segunda máquina virtual, y así sucesivamente.

 Una buena forma de editar las plantillas con Visual Studio es utilizar el esquema JSON para organizar los parámetros, las variables y los recursos. Para comprender el esquema, Visual Studio puede señalar errores en la plantilla antes de implementarla.

![Explorador de JSON][json_explorer]

## Implementación del proyecto
1. Implemente la plantilla ARM en Azure para crear el recurso del conjunto de escalado de máquinas virtuales. Haga clic con el botón secundario en el nodo de proyecto y seleccione **Implementar | Nueva implementación**.
   
    ![Implementar plantilla][5deploy_Template]
2. Seleccione la suscripción en el cuadro de diálogo "Implementar en grupo de recursos".
   
    ![Implementar plantilla][6deploy_Template]
3. Desde aquí también puede crear un nuevo grupo de recursos de Azure donde implementar la plantilla.
   
    ![Nuevo grupo de recursos][new_resource]
4. A continuación, seleccione el botón **Editar parámetros** para escribir parámetros que se pasarán a la plantilla, ciertos valores como el nombre de usuario y la contraseña para el sistema operativo son necesarios para crear la implementación. Si no tiene instaladas las herramientas de PowerShell para Visual Studio, se recomienda marcar la casilla Save passwords (Guardar contraseñas) con el fin de evitar que no se muestre una línea de comandos de PowerShell oculto, o bien utilizar la compatibilidad con [Almacén de claves](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Editar parámetros][edit_parameters]
5. Ahora haga clic en **Implementar**. En la ventana **Salida** se mostrará el progreso de la implementación. Tenga en cuenta que la acción ejecuta el script **Deploy-AzureResourceGroup.ps1**.
   
   ![Ventana de salida][output_window]

## Exploración del conjunto de escalado de máquinas virtuales
Una vez finalizada la implementación, puede ver el nuevo conjunto de escalado de máquinas virtuales en Visual Studio **Cloud Explorer** (actualización de la lista). Cloud Explorer le permite administrar recursos de Azure en Visual Studio mientras se desarrollan aplicaciones. También puede ver el conjunto de escalas de máquina virtual en el [Portal de Azure](https://portal.azure.com) y en el [Explorador de recursos de Azure](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 El portal es la mejor opción para administrar visualmente la infraestructura de Azure con un explorador web, mientras que el Explorador de recursos de Azure ofrece una forma sencilla de explorar y depurar los recursos de Azure, proporcionando una ventana en la “vista de instancias” y mostrar también los comandos de PowerShell para los recursos que busca. Aunque los conjuntos de escalado de máquinas virtuales se encuentran disponibles en vista previa, en el Explorador de recursos se mostrarán todos los detalles de estos conjuntos.

## Pasos siguientes
Después de implementar satisfactoriamente los conjuntos de escalado de máquinas virtuales en Visual Studio, puede personalizar aún más el proyecto para satisfacer las necesidades de la aplicación. Por ejemplo, configurar el escalado automático agregando un recurso de Insights, agregar infraestructura a la plantilla como máquinas virtuales independientes o implementar aplicaciones con la extensión del script personalizado. Se puede encontrar una buena serie de plantillas de ejemplo en el repositorio de GitHub de [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates) (buscar "vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png

<!---HONumber=AcomDC_0615_2016-->