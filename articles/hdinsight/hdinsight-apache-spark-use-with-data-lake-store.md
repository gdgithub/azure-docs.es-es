---
title: "Ejecución de trabajos de Spark en datos almacenados en Azure Data Lake Store | Microsoft Docs"
description: Ejecutar trabajos de Spark en datos almacenados en Azure Data Lake Store
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/18/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 85752f9286fc7fe8d9cc3acf6be66cca623934f5
ms.openlocfilehash: b65ad7ed4e975bafb9a578d89fa5eb32c3066ee8


---

# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-store"></a>Uso de un clúster de HDInsight Spark para analizar los datos en Data Lake Store

En este tutorial usará Jupyter Notebook, disponible con los clústeres de HDInsight Spark, para ejecutar un trabajo que lee datos de una cuenta de Data Lake Store.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Vea [Obtener evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

* Cuenta de Azure Data Lake Store. Siga las instrucciones que se describen en [Introducción al Almacén de Azure Data Lake mediante el Portal de Azure](../data-lake-store/data-lake-store-get-started-portal.md).

* Clúster de Azure HDInsight Spark con Data Lake Store como almacenamiento. Consulte las instrucciones que se indican en [Creación de un clúster de HDInsight con Data Lake Store mediante Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prepare-the-data"></a>Preparación de los datos

Si ha creado el clúster de HDInsight con Data Lake Store como almacenamiento predeterminado, debe realizar este paso porque los procesos de creación del clúster agregan algunos datos de ejemplo a la cuenta de Data Lake Store que se especifica al crea el clúster.

Si ha creado un clúster de HDInsight con Data Lake Store como almacenamiento adicional y Azure Storage Blob como almacenamiento predeterminado, primero debe copiar algunos datos de ejemplo en la cuenta de Data Lake Store. Puede usar los datos de ejemplo de la instancia de Azure Storage Blob asociada al clúster de HDInsight. Puede usar la [herramienta ADLCopy](http://aka.ms/downloadadlcopy) para hacerlo. Descargue e instale la herramienta desde el vínculo.

1. Abra un símbolo del sistema y vaya al directorio donde está instalada la herramienta AdlCopy, normalmente `%HOMEPATH%\Documents\adlcopy`.

2. Ejecute el siguiente comando para copiar un blob específico desde el contenedor de origen a un Almacén de Data Lake:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Copie el archivo de datos de ejemplo **HVAC.csv** situado en **/HdiSamples/HdiSamples/SensorSampleData/hvac/** en la cuenta de Azure Data Lake Store. El fragmento de código debería tener este aspecto:

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > Asegúrese de que coincida el uso de mayúsculas y minúsculas en los nombres de archivo y ruta de acceso.
   >
   >
3. Se le pedirá que escriba las credenciales de la suscripción a Azure en la que tiene la cuenta de Azure Data Lake Store. Verá un resultado similar al siguiente:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    El archivo de datos (**HVAC.csv**) se copiará en una carpeta **/hvac** en la cuenta de Azure Data Lake Store.

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a>Uso de un clúster de HDInsight Spark con Data Lake Store

1. Desde el [Portal de Azure](https://portal.azure.com/), en el panel de inicio, haga clic en el icono del clúster Spark (si lo ancló al panel de inicio). También puede navegar hasta el clúster en **Examinar todo** > **Clústeres de HDInsight**.

2. En la hoja del clúster Spark, haga clic en **Vínculos rápidos** y, luego, en la hoja **Panel de clúster**, haga clic en **Jupyter Notebook**. Cuando se le pida, escriba las credenciales del clúster.

   > [!NOTE]
   > También puede comunicarse con el equipo Jupyter Notebook en el clúster si abre la siguiente dirección URL en el explorador. Reemplace **CLUSTERNAME** por el nombre del clúster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Cree un nuevo notebook. Haga clic en **Nuevo** y, luego, en **PySpark**.

    ![Crear un nuevo cuaderno de Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdispark.note.jupyter.createnotebook.png "Create a new Jupyter notebook")

4. Dado que creó un cuaderno con el kernel PySpark, no necesitará crear ningún contexto explícitamente. Los contextos Spark y Hive se crearán automáticamente al ejecutar la primera celda de código. Puede empezar por importar los tipos necesarios para este escenario. Para ello, pegue el siguiente fragmento de código en una celda y presione **MAYÚS + ENTRAR**.

        from pyspark.sql.types import *

    Cada vez que se ejecuta un trabajo en Jupyter, el título de la ventana del explorador web mostrará el estado **(Busy)** (Ocupado) junto con el título del cuaderno. También verá un círculo sólido junto al texto **PySpark** en la esquina superior derecha. Una vez completado el trabajo, cambiará a un círculo hueco.

     ![Estado de un trabajo de cuaderno de Jupyter](./media/hdinsight-apache-spark-use-with-data-lake-store/hdispark.jupyter.job.status.png "Status of a Jupyter notebook job")

5. Cargue datos de ejemplo en una tabla temporal mediante el archivo **HVAC.csv** que copió a la cuenta de Azure Data Lake Store. Ahora puede acceder a los datos de la cuenta del Almacén de Data Lake con el siguiente patrón de dirección URL.

    Si dispone de Data Lake Store como almacenamiento predeterminado, HVAC.csv estará en la ruta de acceso similar a la siguiente dirección URL:

         adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    Si cuenta con Data Lake Store como almacenamiento adicional, HVAC.csv estará en la ubicación donde lo copió, por ejemplo:

        adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     En una celda vacía, pegue el siguiente ejemplo de código, reemplace **MYDATALAKESTORE** por el nombre de su cuenta de Azure Data Lake Store y presione **MAYÚS+ENTRAR**. Este ejemplo de código registra los datos en una tabla temporal llamada **hvac**.

         # Load the data. The path below assumes Data Lake Store is default storage for the Spark cluster
         hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

         # Create the schema
         hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

         # Parse the data in hvacText
         hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

         # Create a data frame
         hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

         # Register the data fram as a table to run queries against
         hvacdf.registerTempTable("hvac")

6. Al usar un kernel de PySpark, puede ejecutar directamente una consulta SQL en la tabla temporal **hvac** que acaba de crear con la instrucción mágica `%%sql`. Para obtener más información sobre la función mágica `%%sql` , así como otras función mágicas disponibles con el kernel de PySpark, vea [Kernels disponibles para cuadernos de Jupyter con clústeres Spark en HDInsight (Linux)](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels).

         %%sql
         SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. Una vez que el trabajo se completa correctamente, se muestra de forma predeterminada el resultado tabular siguiente.

      ![Salida de tabla del resultado de la consulta](./media/hdinsight-apache-spark-use-with-data-lake-store/tabular.output.png "Table output of query result")

     También puede ver la salida en otras visualizaciones. Por ejemplo, un gráfico de área con la misma salida tendría el siguiente aspecto.

     ![Gráfico de área del resultado de la consulta](./media/hdinsight-apache-spark-use-with-data-lake-store/area.output.png "Area graph of query result")

8. Cuando haya terminado de ejecutar la aplicación, debe cerrar el cuaderno para liberar los recursos. Para ello, en el menú **Archivo** del cuaderno, haga clic en **Cerrar y detener**. De esta manera se apagará y se cerrará el cuaderno.


## <a name="next-steps"></a>Pasos siguientes

* [Creación de una aplicación independiente con Scala para ejecutarla en un clúster de Apache Spark](hdinsight-apache-spark-create-standalone-application.md)
* [Uso de las herramientas de HDInsight del kit de herramientas de Azure para IntelliJ con el fin de crear aplicaciones Spark destinadas al clúster Spark en HDInsight (Linux)](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Uso de las herramientas de HDInsight del kit de herramientas de Azure para Eclipse con el fin de crear aplicaciones Spark destinadas al clúster Spark en HDInsight (Linux)](hdinsight-apache-spark-eclipse-tool-plugin.md)


<!--HONumber=Nov16_HO4-->


