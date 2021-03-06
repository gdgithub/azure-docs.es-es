---
title: "Protección de Virtual Machines en Azure Security Center | Microsoft Docs"
description: "En este documento se explica cómo las recomendaciones de Azure Security Center ayudan a proteger las máquinas virtuales y a cumplir las directivas de seguridad."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c048263ab1ae453a10f0471ae33a122e21151d94


---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Protección de las máquinas virtuales en Azure Security Center
El Centro de seguridad de Azure analiza el estado de seguridad de los recursos de Azure. Cuando Security Center identifica posibles vulnerabilidades de seguridad, crea recomendaciones que lo guiarán por el proceso de configuración de los controles necesarios.  Las recomendaciones se aplican a los tipos de recursos de Azure: máquinas virtuales, redes, SQL y aplicaciones.

Este artículo aborda las recomendaciones sobre máquinas virtuales.  Las recomendaciones de máquinas virtuales se centran en la recopilación de datos, la aplicación de actualizaciones del sistema, el aprovisionamiento antimalware, el cifrado de los discos de máquinas virtuales y mucho más.  Use la tabla siguiente como referencia para ayudarlo a entender las recomendaciones disponibles sobre máquinas virtuales y lo que harán cada una de ellas si se aplican.

## <a name="available-vm-recommendations"></a>Recomendaciones de máquinas virtuales disponibles
| Recomendación | Description |
| --- | --- |
| [Habilitar la colección de datos de las suscripciones](security-center-enable-data-collection.md) |Recomienda activar la recopilación de datos en la directiva de seguridad para cada una de las suscripciones y para todas las máquinas virtuales de la suscripción. |
| [Corrección de vulnerabilidades del SO](security-center-remediate-os-vulnerabilities.md) |Recomienda armonizar las configuraciones del SO con las reglas de configuración recomendadas; por ejemplo, no permitir guardar las contraseñas. |
| [Aplicar actualizaciones del sistema](security-center-apply-system-updates.md) |Recomienda implementar las actualizaciones críticas y de seguridad del sistema en las máquinas virtuales. |
| [Reiniciar tras actualizar el sistema](security-center-apply-system-updates.md#reboot-after-system-updates) |Se recomienda que reinicie una máquina virtual para completar el proceso de aplicación de actualizaciones del sistema. |
| [Instalación de Endpoint Protection](security-center-install-endpoint-protection.md) |Recomienda aprovisionar programas antimalware a las máquinas virtuales (solo máquinas virtuales de Windows). |
| [Resolver alertas de estado de Endpoint Protection](security-center-resolve-endpoint-protection-health-alerts.md) |Recomienda resolver los errores de Endpoint Protection. |
| [Habilitar el Agente de máquina virtual](security-center-enable-vm-agent.md) |Permite ver las VM que requieren el Agente de VM. El agente de máquina virtual debe estar instalado en las máquinas virtuales para aprovisionar la detección de revisiones, la detección de línea de base y los programas antimalware. De manera predeterminada, el agente de máquina virtual está instalado en las máquinas virtuales que se implementan desde Azure Marketplace. El artículo [VM Agent and Extensions – Part 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (Agente de VM y extensiones, parte 2) proporciona información sobre cómo instalar el Agente de VM. |
| [Aplicar cifrado de discos](security-center-apply-disk-encryption.md) |Se recomienda cifrar los discos de la máquina virtual mediante Cifrado de discos de Azure (máquinas virtuales Linux y Windows). Se recomienda cifrar tanto los volúmenes de datos como los del sistema operativo en la máquina virtual. |
| [Actualizar versión del sistema operativo](security-center-update-os-version.md) |Recomienda actualizar la versión del sistema operativo para el servicio en la nube a la versión más reciente disponible para la familia del sistema operativo.  Para obtener más información sobre los servicios en la nube, consulte la [información general sobre los servicios en la nube](../cloud-services/cloud-services-choose-me.md). |
| [Evaluación de vulnerabilidades no instalada](security-center-vulnerability-assessment-recommendations.md) |Se recomienda instalar una solución de evaluación de vulnerabilidades en la máquina virtual. |
| [Corrección de vulnerabilidades](security-center-vulnerability-assessment-recommendations.md#review-recommendation) |Permite ver las vulnerabilidades tanto del sistema como de las aplicaciones que ha detectado la solución de evaluación de vulnerabilidades instalada en la máquina virtual. |

## <a name="see-also"></a>Consulte también
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Protección de las aplicaciones en Azure Security Center](security-center-application-recommendations.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](security-center-policies.md) : aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.




<!--HONumber=Nov16_HO3-->


