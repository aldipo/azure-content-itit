<properties 
	pageTitle="Come implementare il partizionamento lato client con SDK | Microsoft Azure" 
	description="Informazioni su come usare gli SDK di Azure DocumentDB per partizionare (condividere) i dati e instradare le richieste in più raccolte" 
	services="documentdb" 
	authors="arramac" 
	manager="jhubbard" 
	editor="cgronlun" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/10/2016" 
	ms.author="arramac"/>

# Come partizionare i dati in DocumentDB con .NET SDK

Azure DocumentDB supporta le raccolte che è possono aumentare fino a raggiungere [grandi volumi di archiviazione e velocità effettiva](documentdb-partition-data.md). Tuttavia, esistono casi di utilizzo in cui risulta utile mantenere un controllo con granularità fine sul comportamento di partizionamento. Per ridurre il codice boilerplate per il partizionamento delle attività, negli SDK per .NET, Node.js e Java è stata aggiunta una funzionalità che semplifica la compilazione di applicazioni per cui è stato aumentato il numero delle istanze in più raccolte.

In questo articolo, verranno esaminate le classi e le interfacce in .NET SDK e verrà spiegato come usarle per sviluppare le applicazioni partizionate. Altri SDK come Java, Node. js e Python supportano interfacce e metodi simili per il partizionamento lato client.

## Partizionamento con l'SDK di DocumentDB

Prima di esaminare più in dettaglio il partizionamento, è opportuno riepilogare alcuni concetti di base di DocumentDB correlati al partizionamento. Ogni account di database di DocumentDB è costituito da un insieme di database, ognuno dei quali include più raccolte, che possono contenere stored procedure, trigger, UDF, documenti e allegati correlati. Le raccolte possono essere partizionate singolarmente o autopartizionate con le seguenti proprietà:

- Le raccolte offrono l’isolamento delle prestazioni. La collazione di documenti simili all'interno della stessa raccolta comporta quindi un miglioramento delle prestazioni. Ad esempio, per i dati relativi alle serie temporali, è possibile inserire i dati per l’ultimo mese, per cui viene spesso eseguita una query, all'interno di una raccolta con una velocità effettiva di provisioning più elevata, mentre i dati meno recenti vengono collocati all'interno di raccolte con bassa velocità effettiva di provisioning.
- Le transazioni ACID, ovvero stored procedure e trigger, non possono estendersi a una raccolta. L’ambito delle transazioni è impostato all'interno di un valore della chiave di partizione singola in una raccolta.
- Le raccolte non applicano uno schema e quindi possono essere usate per documenti JSON dello stesso tipo o di tipi diversi.

A partire dalla versione [1\.5.x degli SDK di Azure DocumentDB](documentdb-sdk-dotnet.md), è possibile eseguire operazioni sui documenti direttamente in un database. Internamente [DocumentClient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) usa la classe PartitionResolver specificata per il database per instradare le richieste alla raccolta appropriata.

>[AZURE.NOTE] [Server-side partitioning](documentdb-partition-data.md) introdotto in API REST 2015-12-16 e SDK 1.6.0+ depreca l'approccio del resolver della partizione lato client per casi d'uso semplici. Il partizionamento lato client, tuttavia, è più flessibile e consente di controllare l'isolamento delle prestazioni tra le chiavi di partizione, controllare il livello di parallelismo durante la lettura dei risultati di più partizioni e usare approcci di partizionamento basati su intervallo/spazio e hash.

Ad esempio, in .NET, ogni classe PartitionResolver è un'implementazione concreta di un'interfaccia [IPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.aspx) che presenta tre metodi: [GetPartitionKey](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.getpartitionkey.aspx), [ResolveForCreate](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.resolveforcreate.aspx) e [ResolveForRead](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.ipartitionresolver.resolveforread.aspx). Le query LINQ e gli iteratori ReadFeed usano il metodo ResolveForRead internamente per scorrere tutte le raccolte che corrispondono alla chiave di partizione per la richiesta. Similmente le operazioni di creazione usano il metodo ResolveForCreate per instradare le creazioni alla partizione giusta. Non sono necessarie modifiche per Replace, Delete e Read perché usano documenti che contengono già il riferimento alla raccolta corrispondente.

Gli SDK includono anche due classi che supportano le due tecniche di partizionamento canoniche, l'hashing e le ricerche basate su intervalli, tramite [HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) e [RangePartitionResolver](https://msdn.microsoft.com/library/azure/mt126047.aspx). È possibile usare queste classi per aggiungere facilmente la logica di partizionamento all'applicazione.

## Aggiungere la logica di partizionamento e registrare PartitionResolver 

Ecco un frammento di codice che mostra come creare [HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) e registrarlo con DocumentClient per un database.

```cs
// Create some collections to partition data.
DocumentCollection collection1 = await client.CreateDocumentCollectionAsync(...);
DocumentCollection collection2 = await client.CreateDocumentCollectionAsync(...);

// Initialize a HashPartitionResolver using the "UserId" property and the two collection self-links.
HashPartitionResolver hashResolver = new HashPartitionResolver(
    u => ((UserProfile)u).UserId, 
    new string[] { collection1.SelfLink, collection2.SelfLink });

// Register the PartitionResolver with the database.
this.client.PartitionResolvers[database.SelfLink] = hashResolver;

```

## Creare documenti in una partizione  

Una volta registrato PartitionResolver, è possibile eseguire creazioni e query direttamente nel database, come illustrato di seguito. In questo esempio, l'SDK usa PartitionResolver per estrarre l'ID utente, generarne l'hash e quindi usare tale valore per instradare l'operazione di creazione alla raccolta corretta.

```cs
Document johnDocument = await this.client.CreateDocumentAsync(
    database.SelfLink, new UserProfile("J1", "@John", Region.UnitedStatesEast));
Document ryanDocument = await this.client.CreateDocumentAsync(
    database.SelfLink, new UserProfile("U4", "@Ryan", Region.AsiaPacific, UserStatus.AppearAway));
```

## Creare query per le partizioni  

È possibile eseguire una query con il metodo [CreateDocumentQuery](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createdocumentquery.aspx) passando il database e una chiave di partizione. La query restituisce un solo set di risultati per tutte le raccolte del database che eseguono il mapping alla chiave di partizione.

```cs
// Query for John's document by ID - uses PartitionResolver to restrict the query to the partitions 
// containing @John. Again the query uses the database self link, and relies on the hash resolver 
// to route the appropriate collection.
var query = this.client.CreateDocumentQuery<UserProfile>(
    database.SelfLink, null, partitionResolver.GetPartitionKey(johnProfile))
    .Where(u => u.UserName == "@John");
johnProfile = query.AsEnumerable().FirstOrDefault();
```

## Creare query per tutte le raccolte del database 

È anche possibile eseguire una query di tutte le raccolte del database ed enumerare i risultati come illustrato di seguito, ignorando l'argomento della chiave di partizione.

```cs
// Query for all "Available" users. Here since there is no partition key, the query is serially executed 
// across each partition/collection and returns a single result-set. 
query = this.client.CreateDocumentQuery<UserProfile>(database.SelfLink)
    .Where(u => u.Status == UserStatus.Available);
foreach (UserProfile activeUser in query)
{
    Console.WriteLine(activeUser);
}
```

## Resolver della partizione hash
Con il partizionamento hash le partizioni vengono assegnate in base al valore di una funzione hash, permettendo di distribuire uniformemente richieste e dati tra diverse partizioni. Questo approccio viene generalmente usato per il partizionamento dei dati prodotti o usati da un numero elevato di client distinti e risulta utile per l'archiviazione di profili utente, elementi del catalogo e dati di telemetria IoT ("Internet of Things"). Il partizionamento hash viene anche usato dal supporto di partizionamento lato server di DocumentDB all'interno di una raccolta.

**Partizionamento hash:** ![Diagramma che illustra come il partizionamento hash distribuisca in modo uniforme le richieste tra le partizioni](media/documentdb-sharding/partition-hash.png)

Un semplice schema di partizionamento hash in *N* raccolte consiste nel prendere un documento e nel calcolare *hash(d) mod N* per determinare in quale raccolta si trova. Questa semplice tecnica però non funziona bene quando si aggiungono nuove raccolte o si rimuovono le raccolte perché in questo caso sarebbe necessario riprodurre con sequenza casuale quasi tutti i dati. L'[hashing coerente](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.23.3738) è un noto algoritmo che risolve questo problema implementando uno schema di hashing che riduce al minimo la quantità di dati da spostare durante l'aggiunta o la rimozione di raccolte.

La classe [HashPartitionResolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) implementa la logica per compilare una sequenza di hash coerenti sulla funzione hash specificata nell'interfaccia [IHashGenerator](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.ihashgenerator.aspx). Per impostazione predefinita, HashPartitionResolver usa una funzione hash MD5, ma è possibile sostituirla con la propria implementazione di hashing. HashPartitionResolver crea internamente 16 hash o "nodi virtuali" nella sequenza di hash per ogni raccolta per ottenere una distribuzione più uniforme dei documenti nelle raccolte, ma è possibile variare questo numero in modo compensare l'asimmetria dei dati con la quantità di calcoli lato client.

**Hashing coerente con HashPartitionResolver:** ![Diagramma che illustra come HashPartitionResolver crei una sequenza di hash](media/documentdb-sharding/HashPartitionResolver.JPG)

## Resolver della partizione a intervalli

Nel partizionamento per intervalli le partizioni vengono assegnate in base alla presenza della chiave di partizione in un determinato intervallo. Questo approccio viene in genere usato per il partizionamento con proprietà di tipo timestamp, (ad esempio, eventTime tra 1 aprile 2015 e 14 aprile 2015). La classe [RangePartitionResolver](https://msdn.microsoft.com/library/azure/mt126047.aspx) aiuta a mantenere un mapping tra Range<T> e il collegamento automatico della raccolta.

[Range<T>](https://msdn.microsoft.com/library/azure/mt126048.aspx) è una semplice classe che gestisce gli intervalli dei tipi che implementano stringhe o numeri di tipo IComparable<T> e IEquatable<T>. Per le operazioni di lettura e di creazione, è possibile passare qualsiasi intervallo arbitrario e il resolver identifica tutte le raccolte candidate identificando gli intervalli delle partizioni che intersecano l'intervallo richiesto. Questa funzionalità può essere utile quando si eseguono query di intervallo sui dati di una serie temporale.

**Partizionamento per intervalli:**

![Diagramma che illustra come il partizionamento per intervalli distribuisca in modo uniforme le richieste tra le partizioni](media/documentdb-sharding/partition-range.png)

Un caso speciale di partizionamento per intervalli si ha quando l'intervallo è un solo valore discreto, denominato a volte "partizionamento basato su ricerca". Questo approccio viene in genere usato per il partizionamento in base all'area (ad esempio, la partizione per la Scandinavia contiene Norvegia, Danimarca e Svezia) o per il partizionamento di tenant in un'applicazione multi-tenant.

## Esempi 

Esaminare il [progetto Github degli esempi di partizionamento di DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning) contenente frammenti di codice per usare le classi PartitionResolver ed estenderle per implementare i propri resolver per casi d'uso specifici, come i seguenti:

* Come specificare un'espressione lambda arbitraria per GetPartitionKey e usarla per implementare chiavi di partizionamento composte o per partizionare in modo diverso ogni tipo di oggetti.
* Come creare un semplice [LookupPartitionResolver](https://github.com/Azure/azure-documentdb-dotnet/blob/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning/Partitioners/LookupPartitionResolver.cs) che usa una tabella di ricerca manuale per eseguire il partizionamento. Questo modello di solito viene usato per il partizionamento basato su valori discreti, come area, ID tenant o nome applicazione.
* Come creare un oggetto [ManagedPartitionResolver](https://github.com/Azure/azure-documentdb-dotnet/blob/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning/Partitioners/ManagedHashPartitionResolver.cs) che crea automaticamente raccolte basate su un modello che definisce uno schema di denominazione, IndexingPolicy e le stored procedure da registrare nelle nuove raccolte.
* Come creare un oggetto [SpilloverPartitionResolver](https://github.com/Azure/azure-documentdb-dotnet/blob/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning/Partitioners/SpilloverPartitionResolver.cs) senza schema che crea semplicemente nuove raccolte quando le vecchie raccolte sono piene.
* Come serializzare e deserializzare lo stato di PartitionResolver come JSON, in modo da poter eseguire una condivisione tra processi e negli arresti. È possibile renderli persistenti nei file config o anche in una raccolta DocumentDB.
* Una classe [DocumentClientHashPartitioningManager](https://github.com/Azure/azure-documentdb-dotnet/blob/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning/Util/DocumentClientHashPartitioningManager.cs) per l'aggiunta e la rimozione dinamiche di partizioni in un database partizionato in base all'hashing coerente. Usa internamente [TransitionHashPartitionResolver](https://github.com/Azure/azure-documentdb-dotnet/blob/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning/Partitioners/TransitionHashPartitionResolver.cs) per instradare le operazioni di lettura e di scrittura durante la migrazione usando una di queste quattro modalità: lettura dal vecchio schema di partizionamento (ReadCurrent), da quello nuovo (ReadNext), unione dei risultati da entrambi (ReadBoth) o mancata disponibilità durante la migrazione (None).

Gli esempi sono open source e si consiglia di inviare richieste pull con contributi che potrebbero essere utili ad altri sviluppatori DocumentDB. Per informazioni su come contribuire, fare riferimento alle [linee guida specifiche](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md).

>[AZURE.NOTE] Le creazioni di raccolte sono soggette a limitazioni di frequenza da parte di DocumentDB e quindi il completamento di alcuni dei metodi di esempio mostrati qui potrebbe richiedere alcuni minuti.

##Domande frequenti
**DocumentDB supporta il partizionamento lato server?**

Sì, DocumentDB supporta il [partizionamento lato server](documentdb-partition-data.md). DocumentDB supporta anche il partizionamento lato client tramite resolver della partizione lato client per i casi di utilizzo più avanzati.

* * Come distinguere quando utilizzare il partizionamento lato server o lato client? * * Per la maggior parte dei casi di utilizzo, è consigliabile l'utilizzo del partizionamento lato server, perché gestisce le attività amministrative di partizionamento dei dati e routing delle richieste. Tuttavia, se è necessario il partizionamento per intervalli o in un caso di utilizzo specializzato per l'isolamento delle prestazioni tra i diversi valori delle chiavi di partizione, il partizionamento lato client potrebbe essere l'approccio migliore.

**Come aggiungere o rimuovere una raccolta nel proprio schema di partizionamento?**

Per un esempio di come implementare il ripartizionamento, esaminare l'implementazione di DocumentClientHashPartitioningManager nel progetto degli esempi.

**Come rendere persistente o condividere la propria configurazione di partizionamento con altri client?**

È possibile serializzare lo stato del partitioner come JSON e archiviarlo in file di configurazione o anche nelle raccolte DocumentDB. Per un esempio, esaminare il metodo RunSerializeDeserializeSample nel progetto degli esempi.

**Come concatenare diverse tecniche di partizionamento?**

È possibile concatenare classi PartitionResolver implementando la propria interfaccia IPartitionResolver che usa internamente uno o più sistemi resolver esistenti. Per un esempio, esaminare TransitionHashPartitionResolver nel progetto degli esempi.

##Riferimenti
* [Partizionamento dei dati in DocumentDB lato server](documentdb-partition-data.md)
* [Raccolte e livelli di prestazioni in DocumentDB](documentdb-performance-levels.md)
* [Esempi di codici di partizionamento su Github](https://github.com/Azure/azure-documentdb-dotnet/tree/287acafef76ad223577759b0170c8f08adb45755/samples/code-samples/Partitioning)
* [Documentazione di DocumentDB .NET SDK in MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Esempi di .NET in DocumentDB](https://github.com/Azure/azure-documentdb-net)
* [Limiti di DocumentDB](documentdb-limits.md)
* [Blog di DocumentDB sui suggerimenti per le prestazioni](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
 

<!---HONumber=AcomDC_0413_2016-->