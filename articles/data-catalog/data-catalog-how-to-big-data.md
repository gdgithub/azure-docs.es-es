---
title: "Trabajo con orígenes de macrodatos | Microsoft Docs"
description: "Artículo de procedimientos que resalta los patrones necesarios para usar Azure Data Catalog con orígenes de &quot;macrodatos&quot;, incluidos Azure Blob Storage, Azure Data Lake y Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/04/2016
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8cf7ea7ca69753c12469dff9dc352d616c276ddd


---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Trabajo con orígenes de macrodatos en el Catálogo de datos de Azure
## <a name="introduction"></a>Introducción
**Catálogo de datos de Microsoft Azure** es un servicio en la nube totalmente administrado que actúa como sistema de registro y de detección de orígenes de datos empresariales. En otras palabras, el **Catálogo de datos de Azure** sirve para ayudar a las personas a detectar, comprender y usar orígenes de datos, y para ayudar a las organizaciones a obtener más valor de sus orígenes de datos existentes, incluyendo macrodatos.

**Catálogo de datos de azure** admite el registro de blobs y directorios de Almacenamiento de blobs de Azure, así como directorios y archivos de Hadoop HDFS. La naturaleza semiestructurada de estos orígenes de datos proporciona gran flexibilidad, pero también significa que los usuarios deben considerar cómo se organizan los orígenes de datos para sacar el máximo valor de su registro en **Catálogo de datos de Azure**.

## <a name="directories-as-logical-data-sets"></a>Directorios como conjuntos de datos lógicos
Un patrón común para organizar los orígenes de macrodatos es tratar a los directorios como conjuntos de datos lógicos. Los directorios de nivel superior se usan para definir un conjunto de datos, mientras que las subcarpetas para definir las particiones y los archivos que contienen almacenarán los propios datos.

Un ejemplo de este patrón puede ser:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

En este ejemplo, vehicle_maintenance_events y location_tracking_events representan conjuntos de datos lógicos. Cada una de estas carpetas contiene archivos de datos que se organizan por año y mes en las subcarpetas. Potencialmente, cada una de estas carpetas puede contener cientos o miles de archivos.

En este patrón, el registro de archivos individuales en **Catálogo de datos de Azure** probablemente no tenga sentido. En su lugar, registre los directorios que representan los conjuntos de datos que sean significativos para los usuarios que trabajan con los datos.

## <a name="reference-data-files"></a>Archivos de datos de referencia
Un patrón complementario es almacenar conjuntos de datos de referencia como archivos individuales. Estos conjuntos de datos puede considerarse como el lado "pequeño" de los macrodatos y a menudo son similares a las dimensiones de un modelo de datos analítico. Los archivos de datos de referencia contienen los registros que se usan para proporcionar contexto para la mayoría de los archivos de datos almacenados en otro lugar en el almacén de macrodatos.

Un ejemplo de este patrón puede ser:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Cuando un analista o un científico de datos trabaja con los datos que se encuentran en estructuras de directorio grandes, los datos de estos archivos de referencia se pueden usar para proporcionar información más detallada a las entidades a las que se hace referencia solo por nombre o Id. en el conjunto de datos mayor.

En este patrón, tendrá sentido registrar los archivos de datos de referencia individuales en **Catálogo de datos de Azure**. Cada archivo representa un conjunto de datos y cada uno de ellos se puede anotar y detectar individualmente.

## <a name="alternate-patterns"></a>Patrones alternativos
Los patrones descritos anteriormente son solo dos formas posibles en que un almacén de macrodatos puede organizarse, pero cada implementación será diferente. Independientemente de cómo se estructuren los orígenes de datos, al registrar los orígenes de macrodatos en **Catálogo de datos de Azure**, es preciso centrarse en el registro de los archivos y directorios que representen los conjuntos de datos que sean útiles a otras personas de la organización. Si se registran todos los archivos y directorios, se puede provocar un desorden en el catálogo, lo que dificulta la búsqueda a los usuarios.

## <a name="summary"></a>Resumen
El registro de orígenes de datos en **Catálogo de datos de Azure** facilita su detección y comprensión. Mediante el registro y anotación de los archivos y directorios de macrodatos que representan conjuntos de datos lógicos, puede facilitar a los usuarios la búsqueda y el uso de los orígenes de macrodatos que necesitan.




<!--HONumber=Nov16_HO3-->


