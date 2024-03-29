<properties
	pageTitle="Esercitazione di HBase: Introduzione a HBase in Hadoop | Microsoft Azure"
	description="Seguire questa esercitazione di HBase per iniziare a usare Apache HBase con Hadoop in HDInsight. Creare tabelle dalla shell HBase e sottoporle a query tramite Hive."
	keywords="Apache hbase, hbase, shell di hbase, esercitazione hbase"
	services="hdinsight"
	documentationCenter=""
	authors="mumian"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/27/2016"
	ms.author="jgao"/>



# Esercitazione su HBase: Introduzione all'uso di Apache HBase con Hadoop basato su Windows in HDInsight

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informazioni su come creare cluster HBase in HDInsight, creare tabelle HBase ed eseguire query usando Apache Hive. Per informazioni generali su HBase, vedere [Panoramica di HDInsight HBase][hdinsight-hbase-overview].

Le informazioni contenute in questo documento sono specifiche per i cluster HDInsight basati su Windows. Per informazioni sui cluster basati su Windows, usare il selettore di schede in alto nella pagina per passare alla scheda specifica.

> [AZURE.NOTE] HBase (versione 0.98.0) in HDInsight basato su Windows è disponibile solo per l'uso con cluster HDInsight 3.1 (basati su Apache Hadoop e YARN 2.4.0). Per informazioni sulla versione, vedere [Novità delle versioni cluster di Hadoop incluse in HDInsight][hdinsight-versions]

###Prima di iniziare

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Prima di iniziare questa esercitazione di HBase, è necessario disporre di quanto segue:

- **Una sottoscrizione di Microsoft Azure**. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Una workstation** con Visual Studio 2013 o versioni successive: per le istruzioni, vedere [Installazione di Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

## Nome del cluster HBase

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Per creare un cluster HBase tramite il portale di Azure**

1. Accedere al [portale di Azure][azure-management-portal].
2. Fare clic su **Nuovo** o su **+** nell'angolo in alto a sinistra e quindi fare clic su **Dati e analisi**, **HDInsight**.
3. Immettere i valori seguenti:

	- **Nome del cluster**: immettere un nome per identificare questo cluster.
	- **Tipo di cluster**: selezionare **HBase**.
	- **Sistema operativo cluster**: selezionare **Windows**. Per la creazione di cluster HBase basati su Linux, vedere [Esercitazione di HBase: Introduzione all'uso di Apache HBase con Hadoop in HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
	- **Versione**: selezionare una versione di HBase.
	- **Sottoscrizione**: selezionare la sottoscrizione di Azure usata per creare il cluster.
	- **Gruppo di risorse**: creare un nuovo gruppo di risorse di Azure o selezionarne uno esistente. Per altre informazioni, vedere [Panoramica di Gestione risorse di Azure](resource-group-overview.md)
	- **Credenziali**: per i cluster basati su Windows, è possibile creare un utente del cluster (noto anche come utente HTTP, utente del servizio Web HTTP) e un utente di Desktop remoto. Fare clic su **Abilita Desktop remoto** per aggiungere le credenziali utente desktop remoto. La sezione successiva richiede il protocollo RDP.
	- **Origine dati**: creare un nuovo account di archiviazione di Azure o selezionarne uno esistente da usare come file system predefinito per il cluster. La posizione dell'account di archiviazione determina la posizione del cluster. Il cluster e l'account di archiviazione predefinito devono trovarsi nello stesso data center.
	- **Piani tariffari per il nodo**: selezionare il numero di server di area per il cluster HBase.

		> [AZURE.WARNING] Per garantire disponibilità elevata dei servizi di HBase, è necessario creare un cluster contenente almeno **tre** nodi. Questo assicura che, se un nodo viene disattivato, le aree dati HBase siano disponibili in altri nodi.

		> Se si è in fase di apprendimento di HBase, scegliere sempre 1 per la dimensione del cluster ed eliminare il cluster dopo ogni uso per ridurre il costo.

	- **Configurazione facoltativa**: configurare la rete virtuale di Azure, configurare le azioni di script e aggiungere altri account di archiviazione.

4. Fare clic su **Crea**.

>[AZURE.NOTE] Dopo l'eliminazione di un cluster HBase, è possibile creare un altro cluster HBase usando lo stesso account di archiviazione predefinito e il contenitore di BLOB predefinito. Il nuovo cluster selezionerà le tabelle HBase create nel cluster originale. Per evitare incoerenze, è consigliabile disabilitare le tabelle HBase prima di eliminare il cluster.

## Creare tabelle e inserire dati

Attualmente, esistono due modi per accedere a HBase. In questa sezione viene illustrato l'uso della shell HBase. Nella sezione successiva viene illustrato l'uso di .NET SDK.

Per la maggior parte delle persone, i dati vengono visualizzati in formato tabulare:

![dati tabulari hbase di hdinsight][img-hbase-sample-data-tabular]

In HBase, che rappresenta un'implementazione di BigTable, gli stessi dati sono simili a:

![dati bigtable hbase di hdinsight][img-hbase-sample-data-bigtable]

Ciò può essere più utile dopo avere completato la procedura successiva.

**Per usare la shell HBase**

1. Utilizzare RDP per connettersi al cluster HBase in HDInsight. Per istruzioni, vedere [Gestire i cluster Hadoop in HDInsight con il portale di Azure][hdinsight-manage-portal].
2. Nella sessione RDP fare clic sul collegamento **Hadoop Command Line** sul desktop.
3. Aprire la shell HBase:

		cd %HBASE_HOME%\bin
		hbase shell

4. Creare un HBase con due gruppi di colonne:

		create 'Contacts', 'Personal', 'Office'
		list
5. Inserire alcuni dati:

		put 'Contacts', '1000', 'Personal:Name', 'John Dole'
		put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
		put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
		put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
		scan 'Contacts'

	![shell hbase di hdinsight hadoop][img-hbase-shell]

6. Ottenere una singola riga

		get 'Contacts', '1000'

	Verranno visualizzati gli stessi risultati usando il comando di analisi, poiché esiste solo una riga.

	Per altre informazioni sullo schema di tabella Hbase, vedere l'[introduzione alla progettazione dello schema HBase][hbase-schema]. Per altri comandi HBase, vedere la [guida di riferimento di Apache HBase][hbase-quick-start].


6. Chiudere la shell

		exit

**Per il caricamento bulk dei dati nella tabella HBase dei contatti**

HBase include diversi metodi di caricamento dei dati nelle tabelle. Per altre informazioni, vedere [Caricamento bulk](http://hbase.apache.org/book.html#arch.bulk.load).


Un file di dati di esempio è stato caricato in un contenitore BLOB pubblico, wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Il contenuto del file di dati è il seguente:

	8396	Calvin Raji		230-555-0191	230-555-0191	5415 San Gabriel Dr.
	16600	Karen Wu		646-555-0113	230-555-0192	9265 La Paz
	4324	Karl Xie		508-555-0163	230-555-0193	4912 La Vuelta
	16891	Jonn Jackson	674-555-0110	230-555-0194	40 Ellis St.
	3273	Miguel Miller	397-555-0155	230-555-0195	6696 Anchor Drive
	3588	Osa Agbonile	592-555-0152	230-555-0196	1873 Lion Circle
	10272	Julia Lee		870-555-0110	230-555-0197	3148 Rose Street
	4868	Jose Hayes		599-555-0171	230-555-0198	793 Crawford Street
	4761	Caleb Alexander	670-555-0141	230-555-0199	4775 Kentucky Dr.
	16443	Terry Chander	998-555-0171	230-555-0200	771 Northridge Drive

È possibile creare un file di testo e caricare il file nel proprio account di archiviazione. Per le istruzioni, vedere [Caricare dati per processi Hadoop in HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] In questa procedura viene utilizzata la tabella HBase dei contatti creata nella procedura precedente.

1. Nella sessione RDP fare clic sul collegamento **Hadoop Command Line** sul desktop.
2. Cambiare la directory:

		cd %HBASE_HOME%\bin

3. Eseguire il comando seguente per trasformare il file di dati in StoreFiles e archiviarlo in un percorso relativo specificato da Dimporttsv.bulk.output:

		hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Eseguire questo comando per caricare i dati da /example/data/storeDataFileOutput nella tabella di HBase:

		hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. È possibile aprire la shell HBase e usare il comando di analisi per visualizzare il contenuto della tabella.



## Usare Hive per eseguire query sulle tabelle HBase

È possibile eseguire query sui dati archiviati in HBase tramite Hive. In questa sezione una tabella Hive mappata alla tabella HBase viene creata e usata per interrogare i dati nella tabella HBase.

**Per aprire il dashboard cluster**

1. Passare a **https://<Nome cluster HDInsight>.azurehdinsight.net/**.
5. Immettere l'account utente e la password per Hadoop. Il nome utente predefinito è **admin** e la password corrisponde a quella immessa durante il processo di creazione. Si apre una nuova scheda del browser.
6. Fare clic su **Editor Hive** nella parte superiore della pagina. L'editor Hive ha un aspetto simile a questo:

	![Dashboard cluster di HDInsight][img-hdinsight-hbase-hive-editor]

**Per eseguire query Hive**

1. Immettere lo script HiveQL seguente nell'editor Hive e fare clic su **Invia** per creare una tabella Hive mappata alla tabella HBase. Prima di eseguire questa istruzione, assicurarsi di aver creato la tabella di esempio usata precedentemente in questa esercitazione come riferimento con la shell HBase.

		CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
		STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
		WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
		TBLPROPERTIES ('hbase.table.name' = 'Contacts');

	Attendere fino a quando lo **Stato** non passa a **Completato**.

2. Immettere lo script HiveQL seguente nell'editor Hive e quindi fare clic su **Invia**. La query Hive interroga i dati nella tabella HBase:

     	SELECT count(*) FROM hbasecontacts;

4. Per recuperare i risultati della query Hive, fare clic sul collegamento **Visualizza dettagli** nella finestra **Sessione processo** al termine del processo. Sarà presente solo un file di output del processo perché si è inserito un solo record nella tabella HBase.




**Per passare al file di output**

1. In Console query fare clic su **Browser file**.
2. Specificare l'account di archiviazione di Azure che viene usato come file system predefinito per il cluster HBase.
3. Fare clic sul nome del cluster HBase. Il contenitore dell'account di archiviazione di Azure predefinito usa il nome del cluster.
4. Fare clic su **Utente** e quindi su **Amministratore**. Si tratta del nome utente Hadoop.
6. Fare clic sul nome del processo con la **Data ultima modifica** corrispondente a quella in cui è stata eseguita la query Hive SELECT.
4. Fare clic su **stdout**. Salvare il file e aprirlo con il Blocco note. Sarà presente un file di output.

	![Browser file dell'editor Hive HBase di HDInsight][img-hdinsight-hbase-file-browser]

## Usare la libreria client delle API REST di HBase per .NET

È necessario scaricare la libreria client delle API REST di HBase per .NET da GitHub e compilare il progetto in modo da potere usare .NET SDK per HBase. Nella procedura seguente sono incluse le istruzioni per l'attività.

1. Creare una nuova applicazione console C# Desktop di Windows in Visual Studio.
2. Aprire la console di Gestione pacchetti NuGet facendo clic sul menu **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.
3. Eseguire il comando NuGet seguente nella console:

		Install-Package Microsoft.HBase.Client

5. Aggiungere le istruzioni **using** seguenti all'inizio del file:

		using Microsoft.HBase.Client;
		using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Sostituire la funzione **Main** con il codice seguente:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

		    //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
		    Scanner scanSettings = new Scanner()
		    {
    		    batch = 10,
    		    startRow = BitConverter.GetBytes(25),
    		    endRow = BitConverter.GetBytes(35)
		    };

		    ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
		    CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
		    {
    		    foreach (CellSet.Row row in next.rows)
    		    {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
    		    }
		    }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Impostare le prime tre variabili nella funzione **Main**.
8. Premere **F5** per eseguire l'applicazione.

## Controllare lo stato del cluster

HBase in HDInsight viene fornito con un'interfaccia utente Web per il monitoraggio dei cluster. Usando l’interfaccia Web è possibile richiedere statistiche o informazioni sulle aree.

Per aprire l'interfaccia utente Web, è necessario effettuare una connessione RDP al cluster e quindi fare clic sul collegamento dell’interfaccia utente Web di informazioni HMaster sul desktop o usare l'URL seguente in un browser Web:

	http://zookeeper[0-2]:60010/master-status

In un cluster a disponibilità elevata, verrà visualizzato un collegamento al nodo master HBase attivo corrente che ospita l'interfaccia utente Web.

##Eliminazione del cluster
Per evitare incoerenze, è consigliabile disabilitare le tabelle HBase prima di eliminare il cluster. [AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## Passaggi successivi
In questa esercitazione di HBase per HDInsight si è appreso come creare tabelle e un cluster HBase e come visualizzare i dati delle tabelle dalla shell HBase. Si è inoltre appreso come usare una query Hive sui dati nelle tabelle HBase e come usare le API REST C# di HBase per creare una tabella HBase e recuperare i dati dalla tabella.

Per altre informazioni, vedere:

- [Informazioni su HBase in HDInsight][hdinsight-hbase-overview]. HBase è un database NoSQL open source Apache basato su Hadoop che fornisce accesso rapido e coerenza assoluta per quantità elevate di dati non strutturati e semi-strutturati.
- [Creare cluster HBase nella rete virtuale di Azure][hdinsight-hbase-provision-vnet]. Grazie all'integrazione con la rete virtuale, i cluster HBase possono essere distribuiti nella stessa rete virtuale delle applicazioni, consentendo così alle applicazioni di comunicare direttamente con HBase.
- [Configurare la replica geografica di HBase in HDInsight](hdinsight-hbase-geo-replication.md). Informazioni su come configurare la replica di HBase in due data center di Azure.
- [Analizzare i sentimenti Twitter con HBase in HDInsight][hbase-twitter-sentiment] Informazioni per l'esecuzione dell'[analisi dei sentimenti](http://en.wikipedia.org/wiki/Sentiment_analysis) dei Big Data in tempo reale usando HBase in un cluster Hadoop in HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png

<!---HONumber=AcomDC_0629_2016-->