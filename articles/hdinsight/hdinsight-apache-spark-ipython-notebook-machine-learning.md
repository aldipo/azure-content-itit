<properties 
	pageTitle="Usare Apache Spark per creare applicazioni di machine learning su HDInsight | Microsoft Azure" 
	description="Istruzioni dettagliate su come usare notebook con Apache Spark per compilare applicazioni di machine learning" 
	services="hdinsight" 
	documentationCenter="" 
	authors="nitinme" 
	manager="paulettm" 
	editor="cgronlun"
	tags="azure-portal"/>

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/06/2016" 
	ms.author="nitinme"/>


# Compilare le applicazioni di Machine Learning da eseguire su cluster Apache Spark in HDInsight Linux

Informazioni su come creare un'applicazione che usa un cluster Apache Spark in HDInsight di apprendimento automatico. Questo articolo illustra come usare il notebook Jupyter disponibile con il cluster per compilare e testare l'applicazione. L'applicazione utilizza i dati di HVAC.csv di esempio sono disponibili in tutti i cluster per impostazione predefinita.

**Prerequisiti:**

È necessario disporre di quanto segue:

- Una sottoscrizione di Azure. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Un cluster Apache Spark in HDInsight Linux. Per istruzioni, vedere [Creazione dei cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Mostra i dati

Prima di iniziare la compilazione dell'applicazione, possiamo comprendere la struttura dei dati e il tipo di analisi che facciamo sui dati.

In questo articolo, si utilizza l'esempio file dati **HVAC.csv** disponibile in tutti i cluster HDInsight per impostazione predefinita in **\\HdiSamples\\HdiSamples\\SensorSampleData\\hvac**. Scaricare e aprire il file con estensione csv per ottenere uno snapshot dei dati.

![Snapshot dati HVAC](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Snapshot dei dati HVAC")

I dati illustrano la temperatura di destinazione e la temperatura effettiva di un edificio con sistemi HVAC installati. Si supponga che la colonna **sistema** rappresenti l'ID del sistema e la colonna **SystemAge** rappresenti il numero di anni in cui il sistema HVAC è stato in luogo presso l’edificio.

Questi dati sono usati per stimare se un edificio è caldo o freddo in base alla temperatura di destinazione, in base a un ID di sistema e all'età di sistema.

##<a name="app"></a>Scrivere un'applicazione di formazione machine utilizzando MLlib Spark

In questa applicazione viene usata una pipeline di Spark ML per eseguire una classificazione di documento. Nella pipeline si è suddiviso il documento in parole, convertite le parole in un vettore di funzionalità numerico e infine creato un modello di stima usando i vettori di funzionalità e le etichette. Procedere come descritto di seguito per creare l'applicazione.

1. Dalla Schermata iniziale del [portale di Azure](https://portal.azure.com/) fare clic sul riquadro del cluster Spark (se è stato aggiunto sulla Schermata iniziale). È anche possibile passare al cluster in **Sfoglia tutto** > **Cluster HDInsight**.   

2. Dal pannello del cluster Spark fare clic su **Collegamenti rapidi** e dal pannello **Dashboard cluster** fare clic su **Notebook di Jupyter**. Se richiesto, immettere le credenziali per il cluster.

	> [AZURE.NOTE] È anche possibile raggiungere il notebook di Jupyter per il cluster aprendo l'URL seguente nel browser. Sostituire __CLUSTERNAME__ con il nome del cluster:
	>
	> `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Creare un nuovo notebook. Fare clic su **Nuovo** e quindi su **PySpark**.

	![Creare un nuovo notebook Jupyter](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Creare un nuovo notebook Jupyter")

3. Un nuovo notebook verrà creato e aperto con il nome Untitled.pynb. Fare clic sul nome del notebook nella parte superiore e immettere un nome descrittivo.

	![Specificare un nome per il notebook](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Specificare un nome per il notebook")

3. Poiché il notebook è stato creato tramite il kernel PySpark, non è necessario creare contesti in modo esplicito. I contesti Spark e Hive vengono creati automaticamente quando si esegue la prima cella di codice. È possibile iniziare con l'importazione dei tipi necessari per questo scenario. Incollare il frammento di codice seguente in una cella vuota e quindi premere **MAIUSC+INVIO**.

		from pyspark.ml import Pipeline
		from pyspark.ml.classification import LogisticRegression
		from pyspark.ml.feature import HashingTF, Tokenizer
		from pyspark.sql import Row
		
		import os
		import sys
		from pyspark.sql.types import *
		
		from pyspark.mllib.classification import LogisticRegressionWithSGD
		from pyspark.mllib.regression import LabeledPoint
		from numpy import array
		
		
	 
4. È ora necessario caricare i dati (hvac.csv), da analizzare e usare per eseguire il training del modello. A tale scopo, è possibile definire una funzione che controlla se la temperatura effettiva dell'edificio è maggiore della temperatura di destinazione. Se la temperatura effettiva è maggiore, l’edificio è caldo ed è contrassegnato dal valore **1.0**. Se la temperatura effettiva è inferiore, l’edificio è freddo ed è contrassegnato dal valore **0.0**.

	Incollare il seguente frammento di codice in una cella vuota e premere **MAIUSC + INVIO**.

		
		# List the structure of data for better understanding. Becuase the data will be
		# loaded as an array, this structure makes it easy to understand what each element
		# in the array corresponds to

		# 0 Date
		# 1 Time
		# 2 TargetTemp
		# 3 ActualTemp
		# 4 System
		# 5 SystemAge
		# 6 BuildingID

		LabeledDocument = Row("BuildingID", "SystemInfo", "label")

		# Define a function that parses the raw CSV file and returns an object of type LabeledDocument
		
		def parseDocument(line):
    		values = [str(x) for x in line.split(',')]
    		if (values[3] > values[2]):
        		hot = 1.0
    		else:
        		hot = 0.0        
    
    		textValue = str(values[4]) + " " + str(values[5])
    
    		return LabeledDocument((values[6]), textValue, hot)

		# Load the raw HVAC.csv file, parse it using the function
		data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

		documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
		training = documents.toDF()


5. Configurare la pipeline di machine learning Spark che è costituita da tre fasi: tokenizer, hashingTF, e lr. Per altre informazioni su una pipeline e funzionamento vedere <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">pipeline machine learning di Spark</a>.

	Incollare il seguente frammento di codice in una cella vuota e premere **MAIUSC + INVIO**.

		tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
		hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
		lr = LogisticRegression(maxIter=10, regParam=0.01)
		pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Adattare la pipeline al documento di formazione. Incollare il seguente frammento di codice in una cella vuota e premere **MAIUSC + INVIO**.

		model = pipeline.fit(training)

7. Verificare il documento di formazione per controllare lo stato di avanzamento con l'applicazione. Incollare il seguente frammento di codice in una cella vuota e premere **MAIUSC + INVIO**.

		training.show()

	Questo dovrebbe fornire un output simile al seguente:

		+----------+----------+-----+
		|BuildingID|SystemInfo|label|
		+----------+----------+-----+
		|         4|     13 20|  0.0|
		|        17|      3 20|  0.0|
		|        18|     17 20|  1.0|
		|        15|      2 23|  0.0|
		|         3|      16 9|  1.0|
		|         4|     13 28|  0.0|
		|         2|     12 24|  0.0|
		|        16|     20 26|  1.0|
		|         9|      16 9|  1.0|
		|        12|       6 5|  0.0|
		|        15|     10 17|  1.0|
		|         7|      2 11|  0.0|
		|        15|      14 2|  1.0|
		|         6|       3 2|  0.0|
		|        20|     19 22|  0.0|
		|         8|     19 11|  0.0|
		|         6|      15 7|  0.0|
		|        13|      12 5|  0.0|
		|         4|      8 22|  0.0|
		|         7|      17 5|  0.0|
		+----------+----------+-----+


	Tornare indietro e verificare l'output nel file con estensione csv non elaborato. Ad esempio, la prima riga del file con estensione csv ha dati:

	![Snapshot dati HVAC](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Snapshot dei dati HVAC")

	Si noti come la temperatura effettiva è inferiore alla temperatura di destinazione. Questo dato indica che l'edificio è freddo. Pertanto nella formazione di output, il valore per **etichetta** nella prima riga è **0.0**, che indica l’edificio non è caldo.

8.  Preparazione per l'esecuzione del training modello rispetto a un set di dati. A tale scopo, si passa a un ID di sistema e all'età di sistema (indicati come **SystemInfo** nell'output di formazione), e il modello potrebbe prevedere se la compilazione con tale ID di sistema ed età di sistema sarebbe più calda (contrassegnata da 1.0) o fredda (contrassegnata da 0.0).

	Incollare il seguente frammento di codice in una cella vuota e premere **MAIUSC + INVIO**.
		
		# SystemInfo here is a combination of system ID followed by system age
		Document = Row("id", "SystemInfo")
		test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
    		.map(lambda x: Document(*x)).toDF() 

9. Infine, eseguire stime sui dati di test. Incollare il seguente frammento di codice in una cella vuota e premere **MAIUSC + INVIO**.

		# Make predictions on test documents and print columns of interest
		prediction = model.transform(test)
		selected = prediction.select("SystemInfo", "prediction", "probability")
		for row in selected.collect():
		    print row

10. L'output dovrebbe essere simile al seguente:

		Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
		Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
		Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
		Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
		Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
		Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

	Nella prima riga di stima, si noterà che per un sistema HVAC con ID 20 e validità del sistema di 25 anni, l’edificio sarà caldo (**stima = 1.0**). Il primo valore per DenseVector (0.49999) corrisponde alla stima 0,0 e il secondo valore (0.5001) corrisponde alla stima 1.0. Nell'output, anche se il secondo valore è solo leggermente superiore, viene illustrato il modello **stima = 1.0**.

11. Al termine dell'esecuzione dell'applicazione, è necessario arrestare il notebook per rilasciare le risorse. A tale scopo, dal menu **File** del notebook fare clic su **Close and Halt**. Questo arresterà e chiuderà il notebook.
	  	   

##<a name="anaconda"></a>Usare la libreria Anaconda scikit-informazioni per machine learning

I cluster Apache Spark in HDInsight includono librerie Anaconda. Include inoltre la libreria **scikit-informazioni** per machine learning. La libreria include inoltre diversi set di dati che è possibile usare per compilare applicazioni di esempio direttamente da un notebook Jupyter. Per esempi sull'utilizzo della libreria scikit-informazioni, vedere [http://scikit-learn.org/stable/auto\_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Vedere anche

* [Panoramica: Apache Spark su Azure HDInsight](hdinsight-apache-spark-overview.md)

### Scenari

* [Spark con Business Intelligence: eseguire l'analisi interattiva dei dati con strumenti di Business Intelligence mediante Spark in HDInsight](hdinsight-apache-spark-use-bi-tools.md)

* [Spark con Machine Learning: utilizzare Spark in HDInsight per stimare i risultati dell'ispezione cibo](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Streaming Spark: usare Spark in HDInsight per la creazione di applicazioni di streaming in tempo reale](hdinsight-apache-spark-eventhub-streaming.md)

* [Analisi dei log del sito Web mediante Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Creare ed eseguire applicazioni

* [Creare un'applicazione autonoma con Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Eseguire processi in modalità remota in un cluster Spark usando Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Strumenti ed estensioni

* [Usare il plug-in degli strumenti HDInsight per IntelliJ IDEA per creare e inviare applicazioni Spark in Scala](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Utilizzare il plug-in Strumenti HDInsight per IntelliJ IDEA per eseguire il debug di applicazioni Spark in remoto](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Usare i notebook di Zeppelin con un cluster Spark in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernel disponibili per notebook di Jupyter nel cluster Spark per HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Usare pacchetti esterni con i notebook Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Installare Jupyter nel computer e connetterlo a un cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Gestire risorse

* [Gestire le risorse del cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Tenere traccia ed eseguire il debug di processi in esecuzione nel cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

<!---HONumber=AcomDC_0608_2016-->