<properties 
	pageTitle="Domande sui database DocumentDB - Domande frequenti | Microsoft Azure" 
	description="Risposte alle domande frequenti sul servizio DocumentDB di Azure, servizio di database di documenti NoSQL per JSON. Rispondere a domande sui database relative a capacità, livelli delle prestazioni e scalabilità." 
	keywords="Domande sui database - Domande frequenti, documentdb, azure, Microsoft azure"
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/30/2016" 
	ms.author="mimig"/>


#Domande frequenti su DocumentDB

## Domande sui database relative ai concetti di base di Microsoft Azure DocumentDB

### Che cos'è Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB è un database di documenti distribuito come servizio NoSQL con scalabilità elevata che offre funzionalità di query avanzate su dati privi di schema, che garantisce prestazioni configurabili e affidabili e che consente lo sviluppo rapido, tutto tramite una piattaforma gestita supportata dalla tecnologia e dalla copertura di Microsoft Azure. DocumentDB è la soluzione ideale per applicazioni Web, per dispositivi mobili, di gioco e IoT che necessitano di velocità effettiva prevedibile, bassa latenza e un modello di dati privo di schema. DocumentDB offre flessibilità dello schema e indicizzazione avanzata tramite un modello dati JSON nativo e include il supporto transazionale per più documenti con JavaScript integrato.
  
Per altre domande sui database, risposte e istruzioni su come distribuire e usare questo servizio, vedere la [pagina relativa alla documentazione di DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

### Qual è il tipo di database di DocumentDB?
DocumentDB è un database orientato ai documenti NoSQL che archivia i dati in formato JSON. DocumentDB supporta strutture dei dati complete nidificate su cui è possibile eseguire query tramite la [grammatica di query SQL](documentdb-sql-query.md) avanzata di DocumentDB. DocumentDB fornisce funzionalità di elaborazione transazionale con prestazioni elevate di JavaScript lato server tramite [stored procedure, trigger e funzioni definite dall'utente](documentdb-programming.md). Il database supporta inoltre livelli di coerenza ottimizzabili dagli sviluppatori, a cui sono associati [livelli di prestazioni](documentdb-performance-levels.md).
 
### I database DocumentDB contengono tabelle come i database relazionali (RDBMS)?
No, DocumentDB archivia i dati in raccolte di documenti JSON. Per altre informazioni sulle risorse di DocumentDB, vedere [Modello di risorse e concetti relativi a DocumentDB](documentdb-resources.md).

### Nei database DocumentDB sono supportati i dati privi di schema?
Sì, DocumentDB consente alle applicazioni di archiviare documenti JSON arbitrari senza hint o definizione dello schema. I dati sono immediatamente disponibili per l'esecuzione di Query tramite l'interfaccia di query del linguaggio SQL di DocumentDB.

### In DocumentDB sono supportate le transazioni ACID?
Sì, DocumentDB supporta le transazioni tra documenti espresse come trigger e stored procedure JavaScript. L'ambito delle transazioni viene impostato su una singola partizione in ogni raccolta e le transazioni vengono eseguite con semantica ACID isolate o non isolate da altro codice e altre richieste utente simultaneamente in esecuzione. Se vengono generate eccezioni tramite l'esecuzione sul lato server del codice dell'applicazione JavaScript, viene eseguito il rollback dell'intera transazione.

### Quali sono i casi di utilizzo tipici per DocumentDB?  
DocumentDB rappresenta la scelta ideale per le nuove applicazioni Web, per dispositivi mobili, di gioco e IoT in cui scalabilità automatica, prestazioni prevedibili, tempi di risposta rapidi nell'ordine di millisecondi e la possibilità di effettuare una query su dati privi di schema sono importanti. DocumentDB si presta allo sviluppo rapido e al supporto dell'iterazione continua dei modelli di dati delle applicazioni. Le applicazioni che gestiscono contenuto e dati generati dall'utente rappresentano i [casi di utilizzo comuni per DocumentDB](documentdb-use-cases.md).

### Come fa DocumentDB a offrire prestazioni prevedibili?
Un'unità richiesta è l'unità di misura della velocità effettiva in DocumentDB. Un'unità richiesta corrisponde alla velocità effettiva dell'operazione GET di un documento da 1 KB. Ogni operazione in DocumentDB, tra cui letture, scritture, query SQL ed esecuzioni di stored procedure, ha un valore di unità richiesta deterministico basato sulla velocità effettiva necessaria per completare l'operazione. Invece di considerare CPU, I/O e memoria e il modo in cui ogni elemento influisce sulla velocità effettiva dell'applicazione, è possibile ragionare in termini di singola misura di unità richiesta.

Ogni raccolta di DocumentDB può essere riservata con la velocità effettiva minima di provisioning in termini di unità richiesta di velocità effettiva al secondo. Per applicazioni di qualsiasi dimensione, è possibile effettuare un benchmark delle singole richieste per misurare i valori delle unità richiesta ed effettuare il provisioning delle raccolte per gestire la somma totale delle unità richiesta in tutte le richieste. È anche possibile aumentare o ridurre la velocità effettiva della raccolta con il variare delle esigenze dell'applicazione. Per altre informazioni sulle unità richiesta e su come determinare il numero di raccolte necessarie, vedere [Gestire la capacità e le prestazioni](documentdb-manage.md).

### DocumentDB dispone della conformità HIPAA?
Sì, DocumentDB è conforme alla normativa HIPAA. La normativa HIPAA stabilisce i requisiti per l'uso, la divulgazione e la protezione delle informazioni sanitarie personali sensibili. Per altre informazioni, visitare il sito [Microsoft Trust Center](https://www.microsoft.com/it-IT/TrustCenter/Compliance/HIPAA).

### Quali sono i limiti di archiviazione di DocumentDB? 
Non esiste in teoria alcun limite alla quantità totale di dati che una raccolta può archiviare in DocumentDB. Per archiviare più di 250 GB di dati in una singola raccolta, [contattare il supporto](documentdb-increase-limits.md) per chiedere un aumento della quota dell'account.

### Quali sono i limiti di velocità effettiva di DocumentDB? 
Non esiste in teoria alcun limite alla quantità totale di velocità effettiva che una raccolta può supportare in DocumentDB, se il carico di lavoro può essere distribuito in modo abbastanza uniforme tra un numero sufficientemente elevato di chiavi di partizioni. Per superare le 250.000 unità richiesta/secondo per ogni raccolta o account, [contattare il supporto](documentdb-increase-limits.md) per chiedere un aumento della quota dell'account.

### Quando costa Microsoft Azure DocumentDB?
Per informazioni dettagliate sui prezzi, vedere [Prezzi di DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/). Gli addebiti per l'uso di DocumentDB sono determinati dal numero di raccolte in uso, dal numero di ore in cui le raccolte sono state online, dalla quantità di archivio utilizzata e dalla velocità effettiva di cui è stato effettuato il provisioning per ogni raccolta.

### È disponibile un account gratuito?
I nuovi utenti possono iscriversi per ottenere un [account gratuito di Azure](https://azure.microsoft.com/pricing/free-trial/) che è valido 30 giorni e include un credito di 200 dollari statunitensi per provare tutti i servizi di Azure. Se si possiede una sottoscrizione a Visual Studio si ha invece diritto a [150 dollari statunitensi di crediti Azure gratuiti al mese](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) da usare per qualsiasi servizio di Azure.

### Dove è possibile reperire ulteriori informazioni e supporto su DocumentDB?
Per ottenere informazioni o supporto, è possibile visitare il sito [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb), i [forum per sviluppatori MSDN relativi ad Azure DocumentDB](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB) o pianificare una [chat 1:1 con il team di progettazione di DocumentDB](http://www.askdocdb.com/). Per rimanere sempre aggiornati sulle novità e sulle funzionalità più recenti di DocumentDB, è possibile seguirci su [Twitter](https://twitter.com/DocumentDB).

## Configurare Microsoft Azure DocumentDB

### In che modo è possibile iscriversi a Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB è disponibile nel [portale di Azure][azure-portal]. È innanzitutto necessario iscriversi a una sottoscrizione di Microsoft Azure. Una volta creata la sottoscrizione di Microsoft Azure, sarà possibile aggiungere un account DocumentDB alla sottoscrizione di Azure. Per istruzioni su come aggiungere un account DocumentDB, vedere [Creare un account di database di DocumentDB](documentdb-create-account.md).

### Che cos'è una chiave master?
Una chiave master è un token di sicurezza per l'accesso a tutte le risorse di un account. Gli utenti con la chiave hanno l'accesso in lettura e scrittura a tutte le risorse nell'account del database. È quindi consigliabile prestare attenzione quando si distribuiscono le chiavi master. La chiave master primaria e la chiave master secondaria sono disponibili nel pannello **Chiavi** del [portale di Azure][azure-portal]. Per altre informazioni sulle chiavi, vedere [Visualizzare, copiare e rigenerare le chiavi di accesso](documentdb-manage-account.md#keys).

### Come si crea un database?
Per creare i database, è possibile usare il [portale di Azure](), come descritto in [Creare un database DocumentDB](documentdb-create-database.md), nonché uno degli [SDK di DocumentDB](documentdb-sdk-dotnet.md) o le [API REST](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### Che cos'è una raccolta?
Una raccolta è un contenitore di documenti JSON e di logica dell'applicazione JavaScript associata. Una raccolta è un'entità fatturabile, in cui il [costo](documentdb-performance-levels.md) è determinato dal livello di prestazioni associato alla raccolta. Le raccolte possono estendersi su più partizioni o server e possono essere ridimensionate per gestire volumi praticamente illimitati di archiviazione o di velocità effettiva.

Le raccolte costituiscono anche le entità di fatturazione per DocumentDB. Ogni raccolta viene fatturata su base oraria a seconda della velocità effettiva di cui viene effettuato il provisioning e dello spazio di archiviazione usato. Per altre informazioni, vedere [DocumentDB - Prezzi](https://azure.microsoft.com/pricing/details/documentdb/).

### Come è possibile configurare gli utenti e le autorizzazioni?
È possibile creare gli utenti e le autorizzazioni usando uno degli [SDK di DocumentDB](documentdb-sdk-dotnet.md) o le [API REST](https://msdn.microsoft.com/library/azure/dn781481.aspx).

## Domande sui database relative allo sviluppo con Microsoft Azure DocumentDB

### Come è possibile iniziare a sviluppare per DocumentDB?
Sono disponibili [SDK](documentdb-sdk-dotnet.md) per .NET, Python, Node.js, JavaScript e Java. Gli sviluppatori possono anche usare le [API HTTP RESTful](https://msdn.microsoft.com/library/azure/dn781481.aspx) per interagire con le risorse di DocumentDB da un'ampia gamma di piattaforme e linguaggi.

Nel sito Web GitHub sono disponibili esempi per gli SDK di DocumentDB per [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md) e [Python](documentdb-python-samples.md).

### In DocumentDB è supportato SQL?
Il linguaggio di query SQL di DocumentDB è un sottoinsieme ottimizzato della funzionalità di query supportata da SQL. Il linguaggio di query SQL di DocumentDB offre operatori avanzati gerarchici e relazionali ed estendibilità tramite funzioni definite dall'utente basate su JavaScript. La grammatica JSON consente la creazione di modelli di documenti JSON come alberi con etichette come nodi dell'albero. Questa struttura è usata dalle tecniche di indicizzazione automatica di DocumentDB e dal dialetto di query SQL di DocumentDB. Per informazioni dettagliate su come usare la grammatica SQL, vedere [Esecuzione di query tramite DocumentDB][query].

### Quali sono i tipi di dati supportati da DocumentDB?
I tipi di dati primitivi supportati in DocumentDB sono gli stessi supportati in JSON. JSON è caratterizzato da un sistema di tipo semplice costituito da stringhe, numeri (IEEE754 precisione doppia), valori booleani (True e False) e valori Null. È possibile rappresentare tipi di dati più complessi, ad esempio DateTime, Guid, Int64 e Geometry, sia in JSON che in DocumentDB tramite la creazione di oggetti annidati usando l'operatore { } e di matrici usando l'operatore [ ].

### In che modo viene fornita la concorrenza DocumentDB?
DocumentDB supporta il controllo della concorrenza ottimistica tramite tag di entità HTTP o Etag. Ogni risorsa di DocumentDB ha un ETag e i client DocumentDB includono la relativa versione di lettura più recente nelle richieste di scrittura. Se l'ETag è aggiornato, viene eseguito il commit della modifica. Se il valore è stato modificato esternamente, il server rifiuta la scrittura con un codice di risposta "HTTP 412 - Errore della condizione preliminare". I client devono leggere la versione più recente della risorsa e ripetere la richiesta.

### Come si eseguono le transazioni in DocumentDB?
DocumentDB supporta le transazioni Language Integrated tramite trigger e stored procedure JavaScript. Tutte le operazioni di database negli script vengono eseguite con l'isolamento dello snapshot limitato alla raccolta se si tratta di una raccolta a partizione singola o ai documenti con lo stesso valore della chiave di partizione in una raccolta se la raccolta è partizionata. Viene acquisito uno snapshot delle versioni (ETag) del documento all'inizio della transazione e il commit della transazione viene eseguito solo se lo script ha esito positivo. Se JavaScript genera un errore, viene eseguito il rollback della transazione. Per informazioni dettagliate, vedere [Programmazione sul lato server di DocumentDB](documentdb-programming.md).

### Come è possibile eseguire inserimenti bulk in DocumentDB? 
Sono disponibili tre modi per eseguire l'inserimento in blocco di documenti in DocumentDB:

- Lo strumento per la migrazione dei dati, come descritto in [Importare dati in DocumentDB](documentdb-import-data.md).
- La funzionalità Esplora documenti nel portale di Azure, come descritto in [Aggiungere documenti in blocco con Esplora documenti](documentdb-view-json-document-explorer.md#BulkAdd).
- Le stored procedure, come descritto in [Programmazione sul lato server di DocumentDB](documentdb-programming.md).

### In DocumentDB è supportata la memorizzazione nella cache dei collegamenti alle risorse?
Sì. Dal momento che DocumentDB è un servizio RESTful, i collegamenti alle risorse sono immutabili e possono essere memorizzati nella cache. I client DocumentDB possono specificare un'intestazione "If-None-Match" per le letture in ogni risorsa come documento o raccolta e aggiornare le copie locali solo quando viene modificata la versione del server.




[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 

<!---HONumber=AcomDC_0615_2016-->