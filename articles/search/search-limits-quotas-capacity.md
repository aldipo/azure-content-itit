<properties
	pageTitle="Limiti dei servizi in Ricerca di Azure | Microsoft Azure"
	description="Limiti del servizio usati per la pianificazione della capacità e limiti massimi per richieste e risposte in Ricerca di Azure."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="paulettm"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="06/03/2016"
	ms.author="heidist"/>

# Limiti dei servizi in Ricerca di Azure

I limiti massimi per archiviazione, carichi di lavoro e quantità di indici, documenti e altri oggetti dipendono dal piano tariffario scelto per Ricerca di Azure: **gratuito**, **Basic** o **Standard**.

- Il servizio **gratuito** è un servizio condiviso multi-tenant fornito con la sottoscrizione di Azure. È un'opzione senza costi aggiuntivi per i sottoscrittori che consente di provare il servizio prima di iscriversi per usare risorse dedicate. 
- Il piano **Basic** fornisce risorse di elaborazione dedicate per carichi di lavoro di produzione di dimensioni ridotte.
- Il piano **Standard** prevede computer dedicati con maggiore capacità di elaborazione e archiviazione a ogni livello, inclusa la configurazione minima. Il piano Standard è disponibile in quattro livelli: S1, S2, S3 e ad alta densità S3 (S3 HD). S3 e S3 HD sono attualmente in fase di anteprima e disponibili al 50% durante il periodo di anteprima.

Tutti i livelli possono [essere forniti nel portale](search-create-service-portal.md). A un servizio vengono allocate inizialmente una partizione e una replica, ma è possibile modificare l'allocazione delle risorse dopo aver creato il servizio. Per informazioni dettagliate, vedere [Ridimensionare i livelli di risorse per le query e l'indicizzazione dei carichi di lavoro](search-capacity-planning.md).

## Limiti per sottoscrizione

[AZURE.INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## Limiti per servizio ##

[AZURE.INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## Limiti per indice ##

Esiste una corrispondenza uno a uno tra i limiti sugli indici e sugli indicizzatori. Dato un limite di 200 indici per ogni servizio S2, anche il numero massimo di indicizzatori e di origini dati indicizzatori è 200 per lo stesso servizio.

Risorsa|Free|Basic |S1|S2|S3 (anteprima)|S3 HD (anteprima) 
---|---|---|---|---- |---|----
Indice: numero massimo di campi per indice|1000|100 <sup>1</sup>|1000|1000|1000|1000 
Numero massimo di profili di punteggio per indice|16|16|16|16|16|16 
Numero massimo di funzioni per profilo|8|8|8|8|8|8 
Indicizzatori: carico di indicizzazione massimo per chiamata|10\.000 documenti|Limitato solo da numero massimo di documenti|Limitato solo da numero massimo di documenti|Limitato solo da numero massimo di documenti|Limitato solo da numero massimo di documenti|N/D <sup>2</sup> 
Indicizzatori: tempo massimo di esecuzione|3 minuti|24 ore|24 ore|24 ore|24 ore|N/D <sup>2</sup> 
Indicizzatore BLOB: dimensioni massime per un BLOB, MB|16|16|128|256|256|N/D <sup>2</sup> 
Indicizzatore BLOB: numero massimo di caratteri di contenuto estratti da un BLOB|32\.000|64\.000|4 milioni|4 milioni|4 milioni|N/D <sup>2</sup> 

<sup>1</sup> Il livello Basic è l'unico SKU con un limite inferiore di 100 campi per indice.

<sup>2</sup> S3 HD attualmente non supporta gli indicizzatori e le origini dati indicizzatori. In caso di esigenze urgenti per questa funzionalità, contattare il supporto di Azure.

## Limiti per la dimensione dei documenti ##

Risorsa|Free|Basic |S1|S2|S3 (anteprima)|S3 HD (anteprima) 
---|---|---|---|---- |---|----
Dimensione individuale dei documenti per l'API di indice|<16 MB|<16 MB|<16 MB |<16 MB|<16 MB|<16 MB

Si riferisce alla dimensione massima del documento quando si chiama un'API di indice. La dimensione del documento costituisce effettivamente un limite alla dimensione del corpo della richiesta dell'API di indice. Poiché è possibile passare all'API di indice un batch costituito da più documenti in una sola volta, il limite della dimensione dipende in effetti dal numero di documenti presenti nel batch. Per un batch con un solo documento, la dimensione massima del documento sarà di 16 MB di JSON.

Per limitare la dimensione del documento, è necessario escludere dalla richiesta i dati non disponibili per query. Le immagini e altri dati binari non sono direttamente disponibili per query e non devono quindi essere archiviati nell'indice. Per integrare i dati non disponibili per query nei risultati della ricerca, definire un campo non ricercabile che consenta l'archiviazione di un riferimento URL nella risorsa.

## Limiti dei carichi di lavoro (query al secondo) ##

Risorsa|Free|Basic|S1|S2|S3 (anteprima)|S3 HD (anteprima)
---|---|---|---|----|---|----
QUERY AL SECONDO|N/D|~3 per replica|~15 per replica|~60 per replica|>60 per replica|>60 per replica

Query al secondo è un'approssimazione basata sull'euristica che usa carichi di lavoro simulati ed effettivi per calcolare i valori stimati. La velocità di query al secondo effettiva varia a seconda dei dati e della natura della query.

Anche se sopra sono indicate stime approssimative, è difficile determinare una velocità effettiva, soprattutto nel servizio condiviso Free in cui la velocità effettiva dipende dalla larghezza di banda disponibile e dalla concorrenza tra le risorse di sistema. Nel livello gratuito le risorse di calcolo e di archiviazione vengono condivise da più sottoscrittori, pertanto il numero di query al secondo per la soluzione scelta varia a seconda del numero degli altri carichi di lavoro in esecuzione nello stesso momento.

Al livello Standard è possibile stimare in modo più preciso il numero di query al secondo, perché si ha il controllo di un numero maggiore di parametri. Per indicazioni su come calcolare il numero di query al secondo per i carichi di lavoro, vedere la sezione relativa alle procedure consigliate in [Manage your search solution](search-manage.md) (Gestire la soluzione di ricerca).

## Limiti delle richieste API

- 16 MB al massimo per <sup>1</sup> richiesta
- 8 KB al massimo per la lunghezza dell'URL
- 1000 documenti al massimo per ogni batch di carichi, unioni o eliminazioni di indice
- 32 campi al massimo nella clausola $orderby
- 32766 byte (32 KB meno 2 byte) di testo codificato UTF-8 per la dimensione massima del termine di ricerca

<sup>1</sup> In Ricerca di Azure il corpo di una richiesta è soggetto a un limite massimo di 16 MB, che impone un limite pratico ai contenuti di singoli campi o raccolte non vincolati a limiti teorici. Per altre informazioni sulla composizione dei campi e sulle relative restrizioni, vedere [Tipi di dati supportati](https://msdn.microsoft.com/library/azure/dn798938.aspx).

## Limiti delle risposte API

- 1000 documenti al massimo restituiti per pagina di risultati della ricerca
- 100 suggerimenti al massimo restituiti per richiesta di API di suggerimento

## Limiti delle chiavi API

Le chiavi API vengono utilizzate per l'autenticazione del servizio. Sono disponibili due tipi. Le chiavi amministratore, specificate nell'intestazione della richiesta, consentono l'accesso completo in lettura e scrittura al servizio. Le chiavi di query sono di sola lettura, sono specificate nell'URL e in genere sono distribuite ad applicazioni client.

- 2 chiavi di amministrazione al massimo per ogni servizio
- 50 chiavi di query al massimo per ogni servizio

<!----HONumber=AcomDC_0608_2016-->