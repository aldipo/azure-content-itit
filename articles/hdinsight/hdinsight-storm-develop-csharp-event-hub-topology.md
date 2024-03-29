<properties
   pageTitle="Elaborare eventi dell'hub eventi con Storm in HDInsight | Microsoft Azure"
   description="Informazioni su come elaborare i dati dell'hub eventi con una topologia Storm C# creata in Visual Studio tramite HDInsight Tools per Visual Studio."
   services="hdinsight,notification hubs"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/17/2016"
   ms.author="larryfr"/>

# Elaborare eventi dell'hub eventi di Azure con Storm in HDInsight (C#)

L'hub eventi di Azure consente di elaborare grandi quantità di dati da siti Web, app e dispositivi. Lo spout dell'hub eventi semplifica l'uso di Apache Storm in HDInsight per l'analisi di questi dati in tempo reale. È anche possibile scrivere dati nell'hub eventi da Storm usando il relativo bolt.

Questa esercitazione spiega come usare i modelli di Visual Studio installati con Strumenti HDInsight per Visual Studio per creare due topologie che funzionano con gli hub eventi di Azure.

* **EventHubWriter**: genera dati casualmente e li scrive nell'hub eventi.

* **EventHubReader**: legge dati dall'hub eventi e li archivia nell'archivio tabelle di Azure.

> [AZURE.NOTE] I passaggi descritti in questo documento funzionano solo con un cluster HDInsight basati su Windows. Per una versione Java di questo progetto, che funzionerà con un cluster basato su Windows o Linux, vedere [Elaborare gli eventi da hub eventi di Azure con Storm in HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).

## Prerequisiti

* Un [cluster Apache Storm in HDInsight](hdinsight-apache-storm-tutorial-get-started.md)

* Un [Hub eventi di Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

* L’[Azure .NET SDK](http://azure.microsoft.com/downloads/)

* L’[HDInsight Tools per Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

## Progetto completato

È possibile scaricare la versione completa del progetto creato in questa esercitazione all'indirizzo GitHub: [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub). Sarà tuttavia necessario fornire le impostazioni di configurazione seguendo la procedura riportata in questa esercitazione.

> [AZURE.NOTE] Quando si usa il progetto completato, è necessario eseguire **Gestione pacchetti NuGet** per ripristinare i pacchetti richiesti da questa soluzione.

## Spout e bolt dell'hub eventi

Lo spout e il bolt dell'hub eventi sono componenti Java che facilitano l'uso dell'hub da Apache Storm. Anche se questi componenti sono scritti in Java, con HDInsight Tools per Visual Studio è possibile creare topologie ibride che combinano componenti C# e Java.

Lo spout e il bolt vengono distribuiti come un singolo file di archivio Java (con estensione jar) denominato **eventhubs-storm-spout-0.9-jar-with-dependencies.jar**.

### Scaricare il file con estensione jar

La versione più recente del file **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** è disponibile nella cartella **lib** del progetto [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples). Per scaricare il file, usare uno dei metodi seguenti.

> [AZURE.NOTE] Per lo spout e il bolt è stata inviata la richiesta di inclusione nel progetto Apache Storm. Per altre informazioni, vedere l'articolo relativo a [STORM-583: archiviazione iniziale per hub eventi basati su Storm](https://github.com/apache/storm/pull/336/files) in GitHub.

* **Scaricare un file ZIP**: nella pagina [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples) selezionare **Download ZIP** nel riquadro destro e scaricare un file con estensione zip contenente il progetto.

	![pulsante download zip](./media/hdinsight-storm-develop-csharp-event-hub-topology/download.png)

	Dopo aver completato il download, è possibile estrarre l'archivio. Il file sarà disponibile nella directory **lib**.

* **Clonare il progetto**: se [Git](http://git-scm.com/) è installato, usare il comando seguente per clonare il repository in locale, quindi trovare il file nella directory **lib**.

		git clone https://github.com/hdinsight/hdinsight-storm-examples

## Configurare gli hub eventi

L'hub eventi è l'origine dati per questo esempio. Per creare un nuovo hub eventi, seguire questa procedura.

1. Nel [portale di Azure classico](https://manage.windowsazure.com) selezionare **NUOVO** > __SERVIZI APP__ > **BUS DI SERVIZIO** > **HUB EVENTI** > **CREAZIONE PERSONALIZZATA**.

2. Nella schermata **Aggiungi hub eventi** immettere un nome in **Nome hub eventi**, selezionare l'area in cui creare l'hub in **Area geografica** e creare un nuovo spazio dei nomi o selezionarne uno esistente. Fare clic sulla **freccia** per continuare.

	![procedura guidata - pagina 1](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

	> [AZURE.NOTE] È consigliabile selezionare lo stesso **Percorso** di Storm nel server HDInsight per ridurre la latenza e i costi.

2. Nella schermata **Configura hub eventi** immettere i valori per **Conteggio partizioni** e **Conservazione messaggi**. Per questo esempio usare un numero di partizioni pari a 8 e un valore di conservazione dei messaggi pari a 1. Prendere nota del numero di partizioni, perché questo valore sarà necessario in seguito.

3. Dopo aver creato l'hub eventi, selezionare lo spazio dei nomi, selezionare **Hub eventi**, quindi selezionare l'hub eventi creato in precedenza.

4. Selezionare **Configura** e quindi creare due nuovi criteri di accesso usando le informazioni seguenti.

	| Name | Autorizzazioni |
    | ----- | ----- |
	| Writer | Invio |
	| Reader | Attesa |

	Dopo avere creato le autorizzazioni, selezionare l'icona **Salva** nella parte inferiore della pagina. Verranno creati i criteri di accesso condivisi che saranno usati per inviare (writer) e rimanere in ascolto (reader) in questo hub eventi.

	![criteri](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

5. Dopo avere salvato i criteri, usare **Generatore di chiavi di accesso condivise** nella parte inferiore della pagina per recuperare la chiave per i criteri **writer** e **reader**. Salvare queste informazioni perché verranno usate in seguito.

## Configurare Archiviazione tabelle

L'archivio tabelle verrà usato per memorizzare i valori letti dall'hub eventi, in quanto è facilmente visibile da Visual Studio tramite **Esplora server**. Per creare un nuovo archivio tabelle, seguire questa procedura.

1. Nel [Portale di Azure classico](https://manage.windowsazure.com) selezionare **NUOVO** > **Servizi dati** > **Archiviazione** > **Creazione rapida**.

	![creazione rapida archiviazione](./media/hdinsight-storm-develop-csharp-event-hub-topology/storagecreate.png)

2. Immettere un **Nome** per l'account di archiviazione, selezionare un **Percorso** e quindi fare clic sul **segno di spunta** per creare l'account di archiviazione.

	> [AZURE.NOTE] È consigliabile selezionare lo stesso **Percorso** dell'hub eventi e di Storm nel server HDInsight per ridurre la latenza e i costi.

3. Dopo aver creato il nuovo account di archiviazione, selezionare l'account e quindi usare il collegamento **Gestisci chiavi di accesso** nella parte inferiore della pagina per recuperare i valori di **Nome account di archiviazione** e **Chiave di accesso primaria**. Salvare queste informazioni perché verranno usate in seguito.

	![chiavi di accesso](./media/hdinsight-storm-develop-csharp-event-hub-topology/managekeys.png)

4. Aprire Visual Studio. Scegliere __Cloud Explorer__ dal menu __Visualizza__. In __Cloud Explorer__ espandere __Account di archiviazione__ e quindi l'account di archiviazione creato in precedenza.

    ![Cloud Explorer](./media/hdinsight-storm-develop-csharp-event-hub-topology/createtablestorage.png)

5. Fare clic con il pulsante destro del mouse su __Tabelle__ per l'account di archiviazione e quindi scegliere __Crea tabella__. Quando richiesto, immettere **events** come nome della tabella. Salvare il nome perché sarà necessario nei passaggi successivi.

## Creare EventHubWriter

In questa sezione si creerà una topologia che scrive dati nell'hub eventi usando il relativo bolt.

1. Se la versione più recente di HDInsight Tools per Visual Studio non è ancora installata, vedere [Introduzione all'uso di HDInsight Tools per Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Aprire Visual Studio, selezionare **File** > **Nuovo** e quindi fare clic su **Progetto**.

3. Nella schermata **Nuovo progetto** espandere **Installato** > **Modelli** e quindi selezionare **HDInsight**. Dall'elenco dei modelli selezionare **Applicazione Storm**. Nella parte inferiore della schermata immettere **EventHubWriter** come nome dell'applicazione.

	![immagine](./media/hdinsight-storm-develop-csharp-event-hub-topology/newproject.png)

4. Dopo aver creato il progetto, è necessario disporre dei seguenti file:

	* **Program.cs**: definisce la topologia per il progetto. Per impostazione predefinita viene creata una topologia predefinita costituita da uno spout e da un bolt.

	* **Spout.cs**: spout di esempio.

	* **Bolt.cs**: bolt di esempio. Questo verrà eliminato perché per la scrittura nell'hub eventi si userà il bolt dell'hub eventi stesso.

### Configurazione

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **EventHubWriter** e quindi selezionare **Proprietà**.

2. Nelle proprietà del progetto selezionare **Impostazioni** e quindi **Il progetto non contiene un file di impostazioni predefinito. Fare clic qui per crearne uno.**

3. Immettere le impostazioni seguenti. Usare le informazioni per l'hub eventi create in precedenza nella colonna **Valore**.

	| Nome | Tipo | Scope |
    | ----- | ----- | ----- |
	| EventHubPolicyName | stringa | Applicazione |
	| EventHubPolicyKey | stringa | Applicazione |
	| EventHubNamespace | stringa | Applicazione |
	| EventHubName | stringa | Applicazione |
	| EventHubPartitionCount | int | Applicazione |

4. Salvare e chiudere la pagina **Proprietà**.

### Definire la topologia

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **Bolt.cs** e scegliere **Elimina**. Poiché si usa il bolt dell'hub eventi Java, questo file non è necessario.

2. Aprire il file **Program.cs** e aggiungere il codice seguente immediatamente dopo la riga `TopologyBuilder topologyBuilder = new TopologyBuilder("EventHubWriter" + DateTime.Now.ToString("yyyyMMddHHmmss"));`.

		int partitionCount = Properties.Settings.Default.EventHubPartitionCount;
		List<string> javaDeserializerInfo =
            new List<string>() { "microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer", "java.lang.String" };

	La prima riga legge il numero di partizioni dalle proprietà definite in precedenza. La seconda riga definisce un deserializzatore che viene usato per deserializzare i dati JSON generati dallo spout in `java.lang.String`, in modo che il bolt dell'hub eventi sia in grado di utilizzarli.

4. Trovare il codice seguente:

		topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"count"}}
            },
            1);

	Sostituirlo con quanto riportato di seguito:

		topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"Event"}}
            },
            partitionCount).
            DeclareCustomizedJavaDeserializer(javaDeserializerInfo);

	Viene creato uno spout e il numero di partizioni dell'hub eventi viene usato come hint di parallelismo per questo componente. In questo modo viene creata un'istanza dello spout per ogni partizione.

	In questo modo viene associato anche il de-serializzatore creato in precedenza con il flusso di output da questo componente. Questo consente al componente downstream EventHubSpout di utilizzare i dati prodotti dallo spout C#.

5. Immediatamente dopo il codice precedente aggiungere quanto riportato di seguito:

		JavaComponentConstructor constructor =
            JavaComponentConstructor.CreateFromClojureExpr(
            String.Format(@"(com.microsoft.eventhubs.bolt.EventHubBolt. (com.microsoft.eventhubs.bolt.EventHubBoltConfig. " +
            @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
            Properties.Settings.Default.EventHubPolicyName,
            Properties.Settings.Default.EventHubPolicyKey,
            Properties.Settings.Default.EventHubNamespace,
            "servicebus.windows.net", //suffix for servicebus fqdn
            Properties.Settings.Default.EventHubName,
			"true"));

	Viene creato un nuovo costruttore per il bolt Java, che sarà usato in fase di esecuzione per configurare una nuova istanza del bolt. In questo caso, si userà <a href="http://storm.apache.org/documentation/Clojure-DSL.html" target="_blank">Apache Storm Clojure DSL</a> per configurare lo spout con le informazioni di configurazione dell'hub eventi aggiunte in precedenza. In particolare, questo codice viene usato da HDInsight in fase di esecuzione per eseguire le operazioni seguenti:

	* Creare una nuova istanza di **com.microsoft.eventhubs.bolt.EventHubBoltConfig** usando le informazioni dell'hub eventi fornite.
	* Creare una nuova istanza di **com.microsoft.eventhubs.bolt.EventHubBolt** passando l'istanza di **EventHubBoltConfig**.

6. Trovare il codice seguente:

		topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            1).shuffleGrouping("Spout");

	Sostituirlo con quanto riportato di seguito:

		topologyBuilder.SetJavaBolt(
            "EventHubBolt",
            constructor,
            partitionCount).
            shuffleGrouping("Spout");

	Indica alla topologia di usare come bolt **JavaComponentConstructor** dal passaggio precedente. In questa topologia è possibile fare riferimento al componente con il nome descrittivo "EventHubBolt". L'hint di parallelismo viene impostato sul numero di partizioni per l'hub eventi e sottoscrive i dati prodotti dallo spout ("Spout").

A questo punto, la procedura per **Program.cs** è completata. La topologia è stata definita, ma è ora necessario modificare **Spout.cs** in modo da generare dati che l'hub eventi sia in grado di usare.

> [AZURE.NOTE] Per impostazione predefinita questa topologia crea un solo processo di lavoro, sufficiente per scopi di esempio. Se si adatta la topologia per un cluster di produzione, è necessario aggiungere quanto riportato di seguito per modificare il numero di ruoli di lavoro:

    StormConfig config = new StormConfig();
    config.setNumWorkers(1);
    topologyBuilder.SetTopologyConfig(config);


### Modificare lo spout

Il bolt dell'hub eventi prevede una singola stringa che verrà indirizzata all'hub eventi. Nell'esempio seguente si modificherà il file predefinito **Spout.cs** per produrre una stringa JSON.

1. In **Esplora Soluzioni**, aprire **Spout.cs** e aggiungere quanto segue all'inizio del file:

		using Newtonsoft.Json;
		using Newtonsoft.Json.Linq;

	In questo modo sarà più facile usare i dati JSON.
    
    > [AZURE.NOTE] Il pacchetto JSON.NET deve essere già installato, come richiesto dal framework SCP.NET utilizzato per le topologie di Storm in c#.

3. Trovare il codice seguente:

		Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        outputSchema.Add("default", new List<Type>() { typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));

	Sostituirlo con quanto riportato di seguito:

		Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer());

	La definizione dei dati creati dallo spout verrà modificata per l'uso dei dati **stringa** e del serializzatore **CustomizedInteropJSONSerializer** dichiarato in precedenza nella topologia, nel file program.cs.

2. Sostituire il metodo **NextTuple** con quanto riportato di seguito:

		public void NextTuple(Dictionary<string, Object> parms)
        {
            JObject eventData = new JObject();
            eventData.Add("deviceId", r.Next(10));
            eventData.Add("deviceValue", r.Next());
            ctx.Emit(new Values(eventData.ToString(Formatting.None)));
        }

	Verranno generati casualmente un ID dispositivo e un valore, quindi verrà usato Json.NET per generare un oggetto JSON con questi valori.

3. Salvare il file **Spout.cs**.

A questo punto si dispone di una topologia di base che genera dati casuali e li archivia nell'hub eventi tramite il bolt dell'hub eventi stesso. Si procederà ora alla creazione del lettore (reader).

## Creare EventHubReader

In questa sezione si creerà una topologia che legge i dati dall'hub eventi tramite lo spout dell'hub eventi stesso.

2. Aprire Visual Studio e selezionare **File** > **Nuovo** > **Progetto**.

3. Nella schermata **Nuovo progetto** espandere **Installato** > **Modelli** e quindi selezionare **HDInsight**. Dall'elenco dei modelli selezionare **Applicazione Storm**. Nella parte inferiore della schermata immettere **EventHubReader** come nome dell'applicazione.

### Configurazione

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **EventHubReader** e quindi scegliere **Proprietà**.

2. Scegliere **Strumenti**, **Gestione pacchetti NuGet** e quindi **Console di Gestione pacchetti**. Quando viene visualizzata la console, usare il comando seguente per installare i pacchetti di archiviazione di Azure.

        NuGet install WindowsAzure.Storage

2. Nelle proprietà del progetto selezionare **Impostazioni** e quindi **Il progetto non contiene un file di impostazioni predefinito. Fare clic qui per crearne uno.**

3. Immettere le impostazioni seguenti. Usare le informazioni relative all'hub eventi e all'account di archiviazione create in precedenza nella colonna **Valore**.

	| Nome | Tipo | Scope |
    | ----- | ----- | ----- |
	| EventHubPolicyName | stringa | Applicazione |
	| EventHubPolicyKey | stringa | Applicazione |
	| EventHubNamespace | stringa | Applicazione |
	| EventHubName | stringa | Applicazione |
	| EventHubPartitionCount | int | Applicazione |
	| StorageConnection | (Stringa di connessione) | Applicazione |
	| TableName | stringa | Applicazione |

	Per **TableName** immettere il nome della tabella da usare per archiviare gli eventi.

    Per **StorageConnection** immettere un valore di `DefaultEndpointsProtocol=https;AccountName=myAccount;AccountKey=myKey;`. Sostituire **myAccount** e **myKey** con la chiave e il nome dell'account di archiviazione ottenuti in precedenza.

	Questi valori verranno usati dalla topologia per comunicare con gli hub eventi e l'archivio tabelle.

4. Salvare e chiudere la pagina **Proprietà**.

### Definire la topologia

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **Spout.cs** e selezionare **Delete**. Poiché si usa lo spout dell'hub eventi Java, questo file non è necessario.

2. Aprire il file **Program.cs** e aggiungere il codice seguente immediatamente dopo la riga `TopologyBuilder topologyBuilder = new TopologyBuilder("EventHubReader" + DateTime.Now.ToString("yyyyMMddHHmmss"));`:

		int partitionCount = Properties.Settings.Default.EventHubPartitionCount;
		EventHubSpoutConfig ehConfig = new EventHubSpoutConfig(
                Properties.Settings.Default.EventHubPolicyName,
                Properties.Settings.Default.EventHubPolicyKey,
                Properties.Settings.Default.EventHubNamespace,
                Properties.Settings.Default.EventHubName,
                partitionCount);

	Il conteggio delle partizioni viene letto e assegnato a una variabile locale. Verrà usato più volte.

	Il `EventHubSpoutConfig` definisce la configurazione dello spout dell'hub eventi. In questo caso, le informazioni di configurazione degli hub eventi aggiunte in precedenza. In modo invisibile viene utilizzato lo spot dell'hub eventi Java e viene creata una nuova istanza di **com.microsoft.eventhubs.spout.EventHubSpoutConfig** mediante le informazioni dell'hub eventi.

5. Trovare il codice seguente:

		topologyBuilder.SetSpout(
            "Spout",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"count"}}
            },
            1);

	Sostituirlo con quanto riportato di seguito:

        topologyBuilder.SetEventHubSpout(
            "EventHubSpout", 
            ehConfig, 
            partitionCount); 

	Ciò indica alla topologia di creare un nuovo spout dell'hub eventi e di utilizzare il `EventHubSpoutConfig` utilizzato nel passaggio precedente come configurazione. "EventHubSpout" imposta il nome descrittivo dello spout e `partitionCount` viene utilizzato per impostare l'hint di parallelismo. In modo invisibile verrà creata una nuova istanza del componente Java **com.microsoft.eventhubs.spout.EventHubSpout** utilizzando le informazioni di configurazione fornite.

2. Aggiungere quanto riportato di seguito immediatamente dopo il codice precedente:

         List<string> javaSerializerInfo = new List<string>() { "microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer" };

	Viene creato un serializzatore personalizzato che sarà usato per serializzare le informazioni prodotte dai componenti Java, ad esempio EventHubSpout, in un formato JSON utilizzabile dai componenti downstream C#.

3. Trovare il codice seguente:

		topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            1).shuffleGrouping("Spout");

	Sostituirlo con quanto riportato di seguito:

		topologyBuilder.SetBolt(
            "Bolt",
            Bolt.Get,
            new Dictionary<string, List<string>>(),
            partitionCount,
            true).
            DeclareCustomizedJavaSerializer(javaSerializerInfo).
            shuffleGrouping("EventHubSpout");

	Questo codice indica alla topologia di usare un bolt (definito in Bolt.cs). Il serializzatore personalizzato definito in precedenza è qui utilizzato in modo che il bolt possa utilizzare i dati generati dai componenti upstream Java, in questo caso EventHubSpout.

    > [AZURE.IMPORTANT] L'ultimo parametro per SetBolt, (con un valore di `true`) abilita la funzionalità ACK per questo bolt. Questo parametro è obbligatorio perché il componente EventHubSpout si aspetta un ACK per i dati che genera. Se i componenti downstream non restituiscono ACK, lo spout arresterà la ricezione dopo aver elaborato circa 1000 messaggi.

A questo punto, la procedura per **Program.cs** è completata. La topologia è stata definita, ma è ora necessario creare una classe helper per scrivere dati nell'archivio tabelle. Occorre quindi modificare il file **Bolt.cs** affinché sia in grado di riconoscere i dati prodotti dallo spout.

> [AZURE.NOTE] Per impostazione predefinita questa topologia crea un solo processo di lavoro, sufficiente per scopi di esempio. Se si adatta la topologia a un cluster di produzione, è necessario aggiungere quanto riportato di seguito per modificare il numero di processi di lavoro:

    StormConfig config = new StormConfig();
    config.setNumWorkers(1);
    topologyBuilder.SetTopologyConfig(config);


### Creare una classe helper

Quando si scrivono dati nell'archivio tabelle, è necessario creare una classe che descriva tali dati.

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto **EventHubReader**, scegliere **Aggiungi** e quindi **Classe**. Assegnare il nome **Device.cs** alla nuova classe.

2. Aprire **Device.cs** e sostituire il codice predefinito con il seguente:

		using System;
		using System.Collections.Generic;
		using System.Linq;
		using System.Text;
		using System.Threading.Tasks;
		using Microsoft.WindowsAzure.Storage.Table;

		namespace EventHubReader
		{
		    class Device : TableEntity
		    {
		        public int value { get; set; }

		        public Device() { }
		        public Device(int id)
		        {
		            this.PartitionKey = id.ToString();
		            this.RowKey = System.Guid.NewGuid().ToString();
		        }
		    }
		}

	Nell'archivio tabelle verranno create entità che consistono in una chiave di partizione (impostata sull'ID dispositivo letto dall'hub eventi), una chiave di riga univoca e un valore letto dall'hub eventi. A ogni entità sarà associato anche un timestamp, che verrà creato automaticamente all'inserimento dell'entità nella tabella.

### Modificare il bolt

1. In **Esplora Soluzione**, espandere il progetto **EventHubReader** e aprire il file **Bolt.cs**. Aggiungere quanto segue all'inizio del file:

		using Newtonsoft.Json.Linq;
		using Microsoft.WindowsAzure.Storage;
		using Microsoft.WindowsAzure.Storage.Table;

	In questo modo sarà possibile usare più facilmente i dati JSON dal bolt e scriverli nell'archivio tabelle.

2. Trovare l'istruzione `private int count;` e sostituirla con quanto riportato di seguito.

        private CloudTable table;

	Verrà usata durante la connessione alla tabella.

4. Trovare il codice seguente:

		Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        inputSchema.Add("default", new List<Type>() { typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));

	Sostituirlo con quanto riportato di seguito:

		Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
        this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());

	Questo indicherà al bolt che riceverà un valore **stringa** anziché un **int** e che i dati dovranno essere deserializzati mediante **CustomizedInteropJSONDeserialzer**, precedentemente dichiarato nella topologia, nel file program.cs file.

3. Immediatamente dopo il codice precedente aggiungere quanto riportato di seguito:

		CloudStorageAccount storageAccount = CloudStorageAccount.Parse(Properties.Settings.Default.StorageConnection);
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
        table = tableClient.GetTableReference(Properties.Settings.Default.TableName);
        table.CreateIfNotExists();

	Verrà effettuata la connessione alla tabella di archiviazione di Azure creata in precedenza con la stringa di connessione archiviata in `TableName`.

2. Trovare il metodo **Execute** e sostituirlo con quanto riportato di seguito:

		public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Processing events");
            string eventValue = (string)tuple.GetValue(0);
            if (eventValue != null)
            {
                JObject eventData = JObject.Parse(eventValue);

                Device device = new Device((int)eventData["deviceId"]);
                device.value = (int)eventData["deviceValue"];

                TableOperation insertOperation = TableOperation.Insert(device);

                table.Execute(insertOperation);
                this.ctx.Ack(tuple);
            }
        }

	Questo codice usa Json.NET per analizzare i dati JSON dallo spout e quindi rileva i valori dei campi **deviceId** e **deviceValue**. Viene quindi creato un nuovo oggetto **Device** usando **deviceId** durante l'inizializzazione per impostare la chiave di partizione per la tabella. Il valore viene impostato su **deviceValue** e infine l'entità viene inserita nella tabella.

    Dopo che l'entità è stata inserita nella tabella, `Ack()` viene chiamato per la tupla, per comunicare allo spout che i dati sono stati elaborati correttamente.

    > [AZURE.IMPORTANT] Il componente EventHubSpout attente un ACK per ogni tupla da componenti downstream come questo bolt. Se non vengono ricevuti ACK, EventHubSpout presupporrà che l'elaborazione per la tupla ha avuto esito negativo.

A questo punto si ha una topologia completa che leggerà i dati dall'hub eventi e li inserirà nell'archivio tabelle della tabella creata in precedenza.

## Distribuire le topologie

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto **EventHubReader** e quindi selezionare **Submit to Storm on HDInsight**.

	![invio a storm](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. Nella schermata **Submit Topology** selezionare una voce in **Storm Cluster**. Espandere **Additional Configurations**, selezionare **Java File Paths**, fare clic su **...** e quindi selezionare la directory contenente il file **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** scaricato in precedenza. Infine fare clic su **Submit**.

	![immagine della finestra di dialogo di invio](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. Dopo l'invio della topologia, verrà aperto **Storm Topologies Viewer**. Selezionare la topologia **EventHubReader** a sinistra della finestra di dialogo per visualizzare le relative statistiche. Non dovrebbe essere in corso alcuna operazione, perché nell'hub eventi non sono ancora stati scritti eventi.

	![visualizzazione di archiviazione di esempio](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto **EventHubWriter** e selezionare **Submit to Storm on HDInsight**.

2. Nella schermata **Submit Topology** selezionare una voce in **Storm Cluster**. Espandere **Additional Configurations**, selezionare **Java File Paths**, fare clic su **...** e selezionare la directory contenente il file **eventhubs-storm-spout-0.9-jar-with-dependencies.jar** scaricato in precedenza. Infine fare clic su **Submit**.

5. Dopo l'invio della topologia, aggiornare l'elenco delle topologie in **Storm Topologies Viewer** per verificare che siano entrambe in esecuzione nel cluster.

6. In caso affermativo, selezionare **Esplora server**, espandere **Azure** > **Archiviazione** e quindi selezionare l'account di archiviazione creato in precedenza. Nell'account di archiviazione espandere **Tabelle**. Infine fare doppio clic sulla tabella **events** per aprirla. Dovrebbero essere visibili i dati archiviati nella tabella dalla topologia **EventHubReader**.

	* Gli eventi vengono generati dalla topologia **EventHubWriter** e scritti nell'hub eventi.

	* **EventHubReader** legge quindi gli eventi dall'hub eventi e li archivia nella tabella **events** dell'archivio tabelle.

## Arrestare le topologie

Per arrestare le topologie, selezionarle singolarmente in **Storm Topology Viewer** e quindi fare clic su **Kill**.

![immagine dell'arresto di una topologia](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

##Eliminare il cluster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Note

### Checkpoint

EventHubSpout controlla periodicamente il proprio stato nel nodo Zookeeper, operazione che salva l'offset corrente per i messaggi letti dalla coda. Questo consente al componente di avviare la ricezione di messaggi in corrispondenza dell'offset salvato negli scenari seguenti:

* L'istanza del componente ha esito negativo e viene riavviata.

* Il cluster viene aumentato o ridotto mediante l'aggiunta o la rimozione di nodi.

* La topologia viene arrestata e riavviata **con lo stesso nome**.

È inoltre possibile esportare e importare i checkpoint persistenti in WASB (servizio Archiviazione di Azure usato dal cluster HDInsight.) Gli script per eseguire questa operazione si trovano in Storm, nel cluster HDInsight, in **c:\\apps\\dist\\storm-0.9.3.2.2.1.0-2340\\zkdatatool-1.0\\bin**.

>[AZURE.NOTE] Il numero di versione indicato nel percorso può essere diverso, poiché la versione di Storm installata nel cluster può cambiare.

Gli script presenti in questa directory sono:

* **stormmeta\_import.cmd**: importa tutti i metadati Storm dal contenitore di archiviazione predefinito del cluster in Zookeeper.

* **stormmeta\_export.cmd**: esporta tutti i metadati Storm da Zookeeper al contenitore di archiviazione predefinito del cluster.

* **stormmeta\_delete.cmd**: elimina tutti i metadati Storm da Zookeeper.

L'esportazione di un'importazione consente di rendere persistenti i dati del punto di controllo quando è necessario eliminare il cluster ma si desidera riprendere l'elaborazione dall'offset corrente nell'hub quando si riporta un nuovo cluster online.

> [AZURE.NOTE] Poiché i dati vengono mantenuti nel contenitore di archiviazione predefinito, il nuovo cluster **deve** usare gli stessi account e contenitore di archiviazione del cluster precedente.

## Passaggi successivi

In questo documento si è appreso come usare lo spout e il bolt dell'hub eventi Java da una topologia C# per utilizzare i dati nell'hub eventi di Azure. Per altre informazioni sulla creazione di topologie C#, vedere gli articoli seguenti.

* [Sviluppare topologie C# per Apache Storm in HDInsight tramite Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Guida alla programmazione SCP](hdinsight-storm-scp-programming-guide.md)

* [Topologie di esempio per Storm in HDInsight](hdinsight-storm-example-topology.md)
 

<!---HONumber=AcomDC_0518_2016-->