<properties 
	pageTitle="Usare Gestione risorse per allocare le risorse nel cluster Apache Spark in HDInsight | Microsoft Azure" 
	description="Imparare a usare il gestore di risorse per i cluster di Spark su HDInsight per ottenere prestazioni migliori." 
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


# Gestire le risorse del cluster Apache Spark in HDInsight Linux

Questo articolo descrive come accedere a interfacce associate al cluster Spark, come l'interfaccia utente di Ambari, l'interfaccia utente di YARN e Server cronologia Spark. Si apprenderà anche come ottimizzare la configurazione del cluster per ottenere prestazioni ottimali.

**Prerequisiti:**

È necessario disporre di quanto segue:

- Una sottoscrizione di Azure. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Un cluster Apache Spark in HDInsight Linux. Per istruzioni, vedere l'articolo relativo alla [creazione di cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## Come è possibile avviare l'interfaccia utente Web di Ambari?

1. Dalla Schermata iniziale del [portale di Azure](https://portal.azure.com/) fare clic sul riquadro del cluster Spark (se è stato aggiunto sulla Schermata iniziale). È inoltre possibile passare al cluster in **Esplora tutto** > **cluster HDInsight**. 
 
2. Dal pannello del cluster Spark fare clic su **Dashboard**. Quando richiesto, immettere le credenziali di amministratore per il cluster di Spark.

	![Avviare Ambari](./media/hdinsight-apache-spark-resource-manager/hdispark.cluster.launch.dashboard.png "Avviare gestione risorse")

3. Questa operazione consente di avviare l'interfaccia utente Web di Ambari, come illustrato di seguito.

	![Interfaccia utente Web Ambari](./media/hdinsight-apache-spark-resource-manager/ambari-web-ui.png "Interfaccia utente Web Ambari")

## Come è possibile avviare il Server cronologia Spark?

1. Dal [portale di Azure](https://portal.azure.com/), dalla schermata iniziale, fare clic sul riquadro per il cluster Spark (se lo si è bloccato alla schermata iniziale).

2. Dal pannello del cluster in **Collegamenti rapidi** fare clic su **Dashboard cluster**. Nel pannello **Dashboard cluster** fare clic su **Server cronologia Spark**.

	![Server cronologia Spark](./media/hdinsight-apache-spark-resource-manager/launch-history-server.png "Server cronologia Spark")

	Quando richiesto, immettere le credenziali di amministratore per il cluster di Spark.


## Come è possibile avviare l'interfaccia utente di Yarn?

È possibile usare l'interfaccia utente di YARN per il monitoraggio delle applicazioni attualmente in esecuzione nel cluster Spark.

1. Nel pannello del cluster fare clic su **Dashboard cluster** e quindi su **YARN**.

	![Avviare l'interfaccia utente di YARN](./media/hdinsight-apache-spark-resource-manager/launch-yarn-ui.png)

	>[AZURE.TIP] In alternativa, è anche possibile avviare l'interfaccia utente di YARN dall'interfaccia utente di Ambari. Per avviare l'interfaccia utente di Ambari, nel pannello del cluster fare clic su **Dashboard cluster** e quindi su **Dashboard cluster HDInsight**. Nell'interfaccia utente di Ambari fare clic su **YARN**, fare clic su **Collegamenti rapidi**, sulla funzionalità di gestione risorse attiva e quindi fare clic su **ResourceManager UI**.

## Qual è la configurazione cluster ottimale per eseguire le applicazioni Spark?

I tre parametri principali che possono essere usati per la configurazione di Spark, a seconda dei requisiti dell'applicazione, sono `spark.executor.instances`, `spark.executor.cores` e `spark.executor.memory`. Un Executor è un processo avviato per un'applicazione Spark. Viene eseguito sul nodo di lavoro e svolge le attività per l'applicazione. Il numero predefinito di executor e le relative dimensioni per ogni cluster vengono calcolati in base al numero di nodi di lavoro e alle relative dimensioni. Questi dati vengono archiviati in `spark-defaults.conf` nei nodi head del cluster.

I tre parametri di configurazione possono essere configurati a livello di cluster, per tutte le applicazioni in esecuzione nel cluster, o possono anche essere specificati per ogni singola applicazione.

### Modificare i parametri con l'interfaccia utente di Ambari

1. Nell'interfaccia utente di Ambari fare clic su **Spark**, fare clic su **Configs** e quindi espandere **Custom spark-defaults**.

	![Impostare parametri con Ambari](./media/hdinsight-apache-spark-resource-manager/set-parameters-using-ambari.png)

2. I valori predefiniti sono appropriati per 4 applicazioni Spark in esecuzione contemporaneamente nel cluster. È possibile modificare questi valori nell'interfaccia utente, come illustrato di seguito.

	![Impostare parametri con Ambari](./media/hdinsight-apache-spark-resource-manager/set-executor-parameters.png)

3. Per salvare la configurazione, fare clic su **Save**. Nella parte superiore della pagina verrà richiesto di riavviare tutti i servizi interessati. Fare clic su **Restart**.

	![Riavviare i servizi](./media/hdinsight-apache-spark-resource-manager/restart-services.png)


### Modificare i parametri per un'applicazione in esecuzione in Jupyter Notebook

Per le applicazioni in esecuzione in Jupyter Notebook è possibile usare `%%configure` magic per apportare le modifiche di configurazione. Idealmente, è necessario apportare le modifiche all'inizio dell'applicazione, prima di eseguire la prima cella di codice. Ciò garantisce che la configurazione venga applicata alla sessione Livy quando viene creata. Se si vuole modificare la configurazione in una fase successiva nell'applicazione, è necessario usare il parametro `-f`. Tuttavia, in questo modo tutte le operazioni eseguite nell'applicazione andranno perse.

Il frammento di codice riportato di seguito mostra come modificare la configurazione per un'applicazione in esecuzione in Jupyter.

	%%configure 
	{"executorMemory": "3072M", "executorCores": 4, “numExecutors”:10}

I parametri di configurazione devono essere passati come una stringa JSON e devono trovarsi nella riga successiva a magic, come illustrato nella colonna di esempio.

### Modificare i parametri per un'applicazione inviata tramite spark-submit

Il comando seguente è un esempio di come modificare i parametri di configurazione per un'applicazione batch inviata tramite `spark-submit`.

	spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### Modificare i parametri per un'applicazione inviata tramite cURL

Il comando seguente è un esempio di come modificare i parametri di configurazione per un'applicazione batch inviata tramite cURL.

	curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### Come è possibile modificare questi parametri nel server Spark Thrift?

Il server Spark Thrift fornisce l'accesso JDBC/ODBC a un cluster Spark e viene usato per rispondere alle query di Spark SQL. Strumenti come Power BI, Tableau e così via usano il protocollo ODBC per comunicare con il server Spark Thrift per eseguire query di Spark SQL come un'applicazione Spark. Quando si crea un cluster Spark, vengono avviate due istanze del server Spark Thrift, una in ogni nodo head. Ogni Thrift Spark Server è visibile come un'applicazione Spark nell'interfaccia utente di YARN.

Il server Spark Thrift usa l'allocazione di executor dinamica di Spark e quindi non viene usato `spark.executor.instances`. Il server Spark Thrift usa invece `spark.dynamicAllocation.minExecutors` e `spark.dynamicAllocation.maxExecutors` per specificare il numero di executor. Per modificare le dimensioni degli executor, si usano i parametri di configurazione `spark.executor.cores` e `spark.executor.memory`. È possibile modificare questi parametri, come illustrato di seguito.

* Espandere la categoria **Advanced spark-thrift-sparkconf** per aggiornare i parametri `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors` e `spark.executor.memory`.

	![Configurare il server Spark Thrift](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-1.png)

* Espandere la categoria **Custom spark-thrift-sparkconf** per aggiornare il parametro `spark.executor.cores`.

	![Configurare il server Spark Thrift](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-2.png)

### Come è possibile modificare la memoria del driver del server Spark Thrift?

La memoria del driver del server Spark Thrift è configurata al 25% delle dimensioni della RAM nodo head, a condizione che le dimensioni totali della RAM del nodo head siano maggiori di 14 GB. Per modificare la configurazione della memoria del driver, è possibile usare l'interfaccia utente di Ambari, come illustrato di seguito.

* Nell'interfaccia utente di Ambari fare clic su **Spark**, fare clic su **Configs**, espandere **Advanced spark-env**, quindi fornire il valore per **spark\_thrift\_cmd\_opts**.

	![Configurare la RAM del server Spark Thrift](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-ram.png)

## La funzionalità di Business Intelligence non è in uso con il cluster Spark. Come è possibile ripristinare le risorse?

Poiché si usa l'assegnazione dinamica di Spark, le uniche risorse utilizzate dal server Thrift sono quelle per i due master applicazioni. Per recuperare tali risorse, è necessario arrestare i servizi del server Thrift in esecuzione nel cluster.

1. Nel riquadro sinistro dell'interfaccia utente di Ambari fare clic su **Spark**.

2. Nella pagina successiva fare clic su **Server Spark Thrift**.

	![Riavviare il server Thrift](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-1.png)

3. Verranno visualizzati due nodi head in cui è in esecuzione il server Spark Thrift. Fare clic su uno dei nodi head.

	![Riavviare il server Thrift](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-2.png)

4. Nella pagina successiva sono elencati tutti i servizi in esecuzione in quel nodo head. Nell'elenco fare clic sul pulsante dell'elenco a discesa accanto al server Spark Thrift e quindi fare clic su **Stop**.

	![Riavviare il server Thrift](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-3.png)

5. Ripetere questi passaggi anche per l'altro nodo head.


## I notebook Jupyter non vengono eseguiti come previsto. Come è possibile riavviare il servizio?

1. Avviare l'interfaccia utente Web di Ambari, come illustrato in precedenza. Dal riquadro di spostamento sinistro fare clic su **Jupyter**, **Service Actions** e quindi su **Restart All**. Verrà avviato il servizio Jupyter su tutti i nodi head.

	![Riavviare Jupyter](./media/hdinsight-apache-spark-resource-manager/restart-jupyter.png "Riavviare Jupyter")

	


## <a name="seealso"></a>Vedere anche


* [Panoramica: Apache Spark su Azure HDInsight](hdinsight-apache-spark-overview.md)

### Scenari

* [Spark con Business Intelligence: eseguire l'analisi interattiva dei dati con strumenti di Business Intelligence mediante Spark in HDInsight](hdinsight-apache-spark-use-bi-tools.md)

* [Spark con Machine Learning: utilizzare Spark in HDInsight per l'analisi della temperatura di compilazione utilizzando dati HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark con Machine Learning: usare Spark in HDInsight per prevedere i risultati del controllo degli alimenti](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

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

* [Tenere traccia ed eseguire il debug dei processi in esecuzione nel cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-job-debugging.md)



[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

<!---HONumber=AcomDC_0615_2016-->