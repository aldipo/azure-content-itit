<properties 
	pageTitle="Introduzione a DocumentDB, un database JSON | Microsoft Azure" 
	description="Informazioni su Azure DocumentDB, un database NoSQL JSON. Questo database di documenti viene creato per Big Data, scalabilità elastica e disponibilità elevata." 
	keywords="database JSON, database di documenti"
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
	ms.topic="get-started-article" 
	ms.date="07/01/2016" 
	ms.author="mimig"/>

# Introduzione a DocumentDB: un Database NoSQL JSON

Azure DocumentDB è un servizio database NoSQL completamente gestito ideato per garantire prestazioni veloci e prevedibili, disponibilità elevata, ridimensionamento automatico, distribuzione globale e facilità di sviluppo. Il modello di dati flessibile, le latenze sempre basse e le funzionalità di query avanzate lo rendono particolarmente adatto per il Web, i dispositivi mobili, i giochi, l'IoT e molte altre applicazioni che richiedono scalabilità e semplicità.

Un modo rapido per ottenere informazioni su questo database JSON e vederlo in azione consiste nel seguire questi tre passaggi:

1. Guardare il video di due minuti [Che cos’è DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/), che presenta i vantaggi dell'utilizzo di DocumentDB.
2. Guardare il video di tre minuti [Creare DocumentDB in Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/), che evidenzia come iniziare a utilizzare DocumentDB tramite il portale di Azure.
3. Visitare [Query Playground](http://www.documentdb.com/sql/demo), dove è possibile eseguire diverse attività per conoscere le ricche funzionalità di query disponibili in DocumentDB. Quindi, passare alla scheda Sandbox ed eseguire le proprie query SQL personalizzate e sperimentare con DocumentDB.

Tornare quindi a questo articolo, che fornisce risposte alle domande seguenti:

-	[Che cos'è DocumentDB e che valore fornisce alle applicazioni moderne?](#what-is-azure-documentdb)
-	[Come vengono gestiti i dati in DocumentDB e come è possibile accedervi?](#data-management)
-	[Come si sviluppano le applicazioni usando DocumentDB?](#develop)
-	[Quali altri passaggi è necessario seguire per creare un'applicazione DocumentDB?](#next-steps)

## Che cos'è Azure DocumentDB?  

Le applicazioni attuali producono, usano e rispondono rapidamente a volumi di dati molto elevati. Queste applicazioni si evolvono molto rapidamente, insieme allo schema di dati sottostante. Gli sviluppatori scelgono quindi con maggiore frequenza database di documenti NoSQL privi di schema come soluzioni semplici, veloci e scalabili per archiviare ed elaborare i dati, mantenendo al tempo stesso la capacità di iterazione rapida su modelli di dati e di feed di dati non strutturati. Molti database privi di schema, tuttavia, non permettono query complesse ed elaborazione transazionale, complicando la gestione avanzata dei dati. Per risolvere questo problema, è possibile usare DocumentDB. Microsoft ha sviluppato DocumentDB per soddisfare questi requisiti quando si gestiscono dati per le applicazioni odierne.

DocumentDB è un servizio di database NoSQL effettivamente privo di schema progettato per applicazioni mobili, Web, di gioco e IoT moderne. DocumentDB assicura che il 99% delle operazioni di lettura siano gestite in 10 millisecondi e il 99% delle scritture in 15 millisecondi. Offre inoltre flessibilità dello schema e possibilità di aumentare o ridurre facilmente le dimensioni di un database su richiesta. Non presuppone né richiede schemi per i documenti JSON che indicizza. Per impostazione predefinita, indicizza automaticamente tutti i documenti nel database e non prevede né richiede schemi o la creazione di indici secondari. DocumentDB permette l'esecuzione di query ad hoc complesse tramite un linguaggio SQL, supporta livelli di coerenza ben definiti e offre l'elaborazione di transazioni su più documenti integrata con il linguaggio JavaScript tramite il familiare modello di programmazione di stored procedure, trigger e funzioni definite dall'utente (UDF, User Defined Functions).

DocumentDB è un database JSON e supporta in modo nativo i documenti JSON, permette di eseguire la semplice iterazione dello schema dell'applicazione e supporta applicazioni che richiedono modelli di dati chiave-valore, documento o tabulari. DocumentDB è compatibile con l'ubiquità di JSON e JavaScript, eliminando i problemi di mancata corrispondenza tra oggetti definiti dall'applicazione e schema del database. La profonda integrazione di JavaScript permette anche agli sviluppatori di eseguire la logica dell'applicazione in modo efficiente, direttamente nel motore di database in una transazione di database.

Azure DocumentDB offre le funzionalità chiave e i vantaggi seguenti:

-	**Velocità effettiva e risorse di archiviazione a scalabilità flessibile:** è possibile aumentare o ridurre le dimensioni del database JSON DocumentDB in base alle esigenze dell'applicazione. I dati sono archiviati in unità SSD (Solid State Drive) per garantire livelli di latenza bassi e prevedibili. DocumentDB supporta contenitori di archiviazione dei dati JSON, chiamati raccolte, che possono raggiungere dimensioni di archiviazione e velocità effettiva con provisioning quasi illimitate. È possibile ridimensionare DocumentDB in modo semplice e flessibile con prestazioni prevedibili in base alla crescita dell'applicazione.

-	**Replica multiarea:** DocumentDB replica in modo trasparente i dati in tutte le aree associate al proprio account DocumentDB, consentendo lo sviluppo di applicazioni che richiedono l'accesso globale ai dati, con il necessario compromesso tra coerenza, disponibilità e prestazioni, tutto con le garanzie corrispondenti. DocumentDB fornisce il failover trasparente a livello di area con le API multihosting e la possibilità di aumentare o ridurre la velocità effettiva e le risorse di archiviazione in tutto il mondo. Altre informazioni sono disponibili in [Distribuire i dati a livello globale con DocumentDB](documentdb-distribute-data-globally.md).

-	**Query ad hoc con sintassi SQL familiare:** è possibile archiviare documenti JSON eterogenei in DocumentDB ed eseguire query su questi documenti tramite una sintassi SQL familiare. DocumentDB usa una tecnologia di indicizzazione a simultaneità elevata, priva di blocchi, strutturata con log per l'indicizzazione automatica dell'intero contenuto dei documenti. Ciò permette l'esecuzione di query avanzate in tempo reale, senza dovere specificare hint di schema, indici secondari o visualizzazioni. Ulteriori informazioni sono disponibili in [Query DocumentDB](documentdb-sql-query.md).

-	**Esecuzione di JavaScript nel database:** è possibile esprimere la logica dell'applicazione sotto forma di stored procedure, trigger e funzioni definite dall'utente (UDF, User Defined Functions) tramite JavaScript standard. Ciò permette alla logica dell'applicazione di operare sui dati, senza preoccuparsi della mancata corrispondenza tra l'applicazione e lo schema di database. DocumentDB offre l'esecuzione transazionale completa della logica dell'applicazione JavaScript direttamente nel motore di database. La profonda integrazione di JavaScript permette l'esecuzione di operazioni INSERT, REPLACE, DELETE e SELECT da un programma JavaScript come transazione isolata. Ulteriori informazioni sono disponibili in [Programmazione sul lato server di DocumentDB](documentdb-programming.md).

-	**Livelli di coerenza perfezionabili:** è possibile scegliere tra quattro livelli di coerenza ben definiti per ottenere il compromesso ottimale tra coerenza e prestazioni. Per query e operazioni di lettura, DocumentDB offre quattro livelli di coerenza distinti, ovvero avanzata, con obsolescenza associata, sessione e futura. Questi livelli di coerenza granulari e ben definiti permettono di ottenere compromessi efficaci tra coerenza, disponibilità e latenza. Ulteriori informazioni sono disponibili in [Utilizzo dei livelli di coerenza per ottimizzare la disponibilità e le prestazioni di DocumentDB](documentdb-consistency-levels.md).

-	**Completamente gestito:** è possibile eliminare la necessità di gestire risorse di database e computer. Questo servizio Microsoft Azure completamente gestito non richiede la gestione di macchine virtuali, la distribuzione e la configurazione di software, la gestione del ridimensionamento o complessi aggiornamenti di livello dati. Il backup di ogni database è eseguito automaticamente e ogni database è protetto da errori nell'area geografica specifica. La possibilità di aggiungere con facilità account e capacità di provisioning di DocumentDB in base alla necessità permette di concentrarsi sull'applicazione invece che sul funzionamento e sulla gestione del database.

-	**Progettazione aperta:** è possibile iniziare subito a lavorare usando le competenze e gli strumenti esistenti. La programmazione per DocumentDB è semplice, intuitiva e non richiede l'adozione di nuovi strumenti o l'adesione a estensioni personalizzate per JSON o JavaScript. È possibile accedere a tutte le funzionalità del database, incluso CRUD, l'elaborazione di JavaScript e query in una semplice interfaccia HTTP RESTful. DocumentDB accetta i formati, i linguaggi e gli standard esistenti, offrendo al tempo stesso capacità di database di livello elevato.

È possibile usare DocumentDB per archiviare set di dati flessibili, che richiedono il recupero di query e l'elaborazione transazionale. Gli scenari relativi alle applicazioni possono includere i dati utente per applicazioni Web, mobili e di gioco interattive, oltre ad archiviazione, recupero ed elaborazione di dati JSON generati da dispositivi IoT. Un database può archiviare un volume qualsiasi di documenti JSON, dato che DocumentDB è particolarmente adatto ad applicazioni eseguite su larga scala in Internet.

##<a name="data-management"></a>Risorse di Azure DocumentDB
Azure DocumentDB gestisce i dati tramite risorse di database ben definite. Queste risorse sono replicate per ottenere disponibilità elevata e sono indirizzabili in modo univoco da parte dei rispettivi URI logici. DocumentDB offre un semplice modello di programmazione RESTful basato su HTTP per tutte le risorse.

L'account di database DocumentDB è uno spazio dei nomi univoco che offre l'accesso ad Azure DocumentDB. Prima di creare un account di database, è necessario avere una sottoscrizione di Azure, che consente di accedere ai diversi servizi di Azure.

Tutte le risorse disponibili in DocumentDB sono modellate e archiviate come documenti JSON. Le risorse sono gestite come elementi, ovvero documenti JSON contenenti metadati, e come feed, ovvero raccolte di elementi. Gli insiemi di elementi sono inclusi nei rispettivi feed.

La figura seguente mostra le relazioni tra le risorse di DocumentDB:

![La relazione gerarchica tra le risorse in DocumentDB, un database NoSQL JSON][1]

Un account di database è costituito da un insieme di database, ognuno dei quali include più raccolte, che possono contenere stored procedure, trigger, UDF, documenti e allegati correlati. Un database include anche utenti associati, ognuno dei quali ha un insieme di autorizzazioni per accedere alle diverse raccolte, stored procedure, trigger, UDF, documenti o allegati disponibili. Mentre i database, gli utenti, le autorizzazioni e le raccolte sono risorse definite dal sistema con schemi noti, i documenti, le stored procedure, i trigger, le funzioni UDF e gli allegati includono contenuto JSON arbitrario definito dagli utenti.

##<a name="develop"></a> Sviluppo con Azure DocumentDB
Azure DocumentDB espone risorse tramite un'API REST che può essere chiamata da qualsiasi linguaggio in grado di effettuare richieste HTTP/HTTPS. In DocumentDB sono inoltre disponibili librerie di programmazione per diversi linguaggi comuni. Queste librerie semplificano molti aspetti dell'uso di Azure DocumentDB tramite la gestione di dettagli quali la memorizzazione nella cache, la gestione delle eccezioni, l'esecuzione automatica di nuovi tentativi e così via. Le librerie attualmente disponibili riguardano i linguaggi e le piattaforme seguenti:

Scaricare | Documentazione
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [Libreria .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Libreria Node.js](http://azure.github.io/azure-documentdb-node/)
[SDK per Java](http://go.microsoft.com/fwlink/?LinkID=402380) | [Libreria Java](http://azure.github.io/azure-documentdb-java/)
[JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [Libreria JavaScript](http://azure.github.io/azure-documentdb-js/)
N/D | [JavaScript SDK lato server](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Libreria di Python](http://azure.github.io/azure-documentdb-python/)

Oltre alle operazioni di base di creazione, lettura, aggiornamento ed eliminazione, DocumentDB offre un'interfaccia di query SQL query avanzata per il recupero di documenti JSON e supporto lato server per l'esecuzione transazionale di logica dell'applicazione JavaScript. Le interfacce di esecuzione di query e script sono disponibili tramite tutte le librerie delle piattaforme, oltre alle API REST.

### Query SQL
Azure DocumentDB supporta l'esecuzione di query sui documenti tramite un linguaggio SQL, basato sul sistema di tipi JavaScript, ed espressioni con supporto per query relazionali, gerarchiche e spaziali. Il linguaggio di query di DocumentDB è un'interfaccia semplice ma avanzata per l'esecuzione di query su documenti JSON. Il linguaggio supporta un sottoinsieme della grammatica SQL ANSI e aggiunge un'integrazione profonda di oggetti JavaScript, matrici, costruzione di oggetti e chiamata di funzioni. DocumentDB offre il modello di query senza schema esplicito o hint di indicizzazioni dallo sviluppatore.

Le funzioni UDF possono essere registrate con DocumentDB ed è possibile farvi riferimento come parte di una query SQL, estendendo così la grammatica in modo da supportare la logica dell'applicazione. Queste funzioni UDF sono scritte come programmi JavaScript e sono eseguite nel database.

Per gli sviluppatori .NET, DocumentDB offre anche un provider di query LINQ come parte di [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx).

### Transazioni ed esecuzione di JavaScript
DocumentDB permette di scrivere logica dell'applicazione sotto forma di programmi con nome, scritti interamente in JavaScript. Questi programmi sono registrati per una raccolta e possono eseguire di operazioni di database nei documenti all'interno di una raccolta specifica. Il codice JavaScript può essere registrato per l'esecuzione come trigger, stored procedure o funzione UDF. I trigger e le stored procedure possono creare, leggere, aggiornare ed eliminare documenti, mentre le funzioni UDF sono eseguite come parte della logica di esecuzione delle query, senza accesso in scrittura alla raccolta.

L'esecuzione di JavaScript in DocumentDB è modellata sui concetti supportati dai sistemi di database relazionali, in cui JavaScript rappresenta un sostituto attuale di Transact-SQL. Tutta la logica JavaScript è eseguita in una transazione di ambiente ACID con isolamento dello snapshot. Se JavaScript genera un'eccezione durante l'esecuzione, l'intera transazione sarà interrotta.

## Passaggi successivi
Se si dispone già di un account Azure, è possibile iniziare con DocumentDB nel [portale di Azure](https://portal.azure.com/#gallery/Microsoft.DocumentDB) [creando un account di database DocumentDB](documentdb-create-account.md).

Se non si dispone di un account Azure, è possibile:

- Effettuare l'iscrizione alla [versione di valutazione gratuita di Azure](https://azure.microsoft.com/free/), che è valida 30 giorni e include un credito di 200 dollari statunitensi per provare tutti i servizi di Azure.
- Se si dispone di una sottoscrizione a MSDN, si ha invece diritto a [150 dollari statunitensi di crediti Azure gratuiti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) da usare per qualsiasi servizio di Azure.

Quindi, quando si è pronti per ulteriori informazioni, visitare il [percorso di apprendimento](https://azure.microsoft.com/documentation/learning-paths/documentdb/) per esplorare tutte le risorse di formazione disponibili.


[1]: ./media/documentdb-introduction/json-database-resources1.png
 

<!---HONumber=AcomDC_0706_2016-->