<properties 
	pageTitle="Come configurare Cache Redis di Azure | Microsoft Azure"
	description="Informazioni sulla configurazione di Redis predefinita per Cache Redis di Azure e su come configurare le istanze di Cache Redis di Azure"
	services="redis-cache"
	documentationCenter="na"
	authors="steved0x"
	manager="douge"
	editor="tysonn" />
<tags 
	ms.service="cache"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="06/29/2016"
	ms.author="sdanie" />

# Come configurare Cache Redis di Azure

In questo argomento viene descritto come esaminare e aggiornare la configurazione per le istanze di Cache Redis di Azure e viene illustrata la configurazione predefinita del server Redis per le istanze di Cache Redis di Azure.

>[AZURE.NOTE] Per ulteriori informazioni sulla configurazione e sull'utilizzo delle funzionalità della cache premium, vedere [Come configurare la persistenza per una Cache Redis di Azure Premium](cache-how-to-premium-persistence.md),e [Come configurare il clustering per una Cache Redis di Azure Premium](cache-how-to-premium-clustering.md), e [Come configurare il supporto di una rete virtuale per una Cache Redis di Azure Premium](cache-how-to-premium-vnet.md).

## Configurare le impostazioni di Cache Redis di Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Cache Redis di Azure fornisce le impostazioni seguenti nel pannello **Impostazioni**.

![Impostazioni di Cache Redis](./media/cache-configure/redis-cache-settings.png)



-	[Supporto e impostazioni di risoluzione dei problemi](#support-amp-troubleshooting-settings)
-	[Impostazioni generali](#general-settings)
	-	[Proprietà](#properties)
	-	[Chiavi di accesso](#access-keys)
	-	[Porte di accesso](#access-ports)
	-	[Criterio per la memoria massima](#maxmemory-policy-and-maxmemory-reserved)
	-	[Impostazioni avanzate (notifiche dello spazio delle chiavi)](#keyspace-notifications-advanced-settings)
	-	[Redis Cache Advisor](#redis-cache-advisor)
-	[Impostazioni di scalabilità](#scale-settings)
	-	[Pricing tier](#pricing-tier)
	-	[Dimensione del cluster Redis](#cluster-size)
-	[Impostazioni di gestione dati](#data-management-settings)
	-	[Persistenza dei dati Redis:](#redis-data-persistence)
	-	[Importazione/Esportazione](#importexport)
-	[Impostazioni di amministrazione](#administration-settings)
	-	[Reboot](#reboot)
	-	[Pianificare gli aggiornamenti](#schedule-updates)
-	[Impostazioni di diagnostica](#diagnostics-settings)
-	[Impostazioni di rete](#network-settings)
-	[Impostazioni di gestione delle risorse](#resource-management-settings)

## Supporto e impostazioni di risoluzione dei problemi

Le impostazioni nella sezione **Supporto e risoluzione dei problemi** forniscono le opzioni per risolvere i problemi della cache.

![Supporto e risoluzione dei problemi](./media/cache-configure/redis-cache-support-troubleshooting.png)

Fare clic su **Risoluzione dei problemi** per conoscere i problemi comuni e le strategie per risolverli.

Fare clic su **Log di controllo** per visualizzare le operazioni eseguite nella cache. È possibile inoltre utilizzare il filtro per espandere la visualizzazione in modo da includere altre risorse. Per altre informazioni sull'uso dei log di controllo, vedere [Visualizzare eventi e log di controllo](../azure-portal/insights-debugging-with-events.md) e [Operazioni di controllo con Resource Manager](../resource-group-audit.md). Per altre informazioni sul monitoraggio degli eventi di Cache Redis di Azure, vedere [Operazioni e avvisi](cache-how-to-monitor.md#operations-and-alerts).

**Integrità risorsa** esamina la risorsa e indica se viene eseguita nel modo previsto. Per altre informazioni sul servizio Integrità risorse di Azure, vedere l'articolo sulla [Panoramica su Integrità risorse di Azure](../resource-health/resource-health-overview.md).

Fare clic su **Nuova richiesta di supporto** per aprire una richiesta di supporto per la cache.

## Impostazioni generali

Le impostazioni nella sezione **Generale** consentono di visualizzare e configurare le impostazioni seguenti relative alla cache.

![Impostazioni generali](./media/cache-configure/redis-cache-general-settings.png)

-	[Proprietà](#properties)
-	[Chiavi di accesso](#access-keys)
-	[Porte di accesso](#access-ports)
-	[Criterio per la memoria massima](#maxmemory-policy-and-maxmemory-reserved)
-	[Impostazioni avanzate (notifiche dello spazio delle chiavi)](#keyspace-notifications-advanced-settings)
-	[Redis Cache Advisor](#redis-cache-advisor)

### Proprietà

Fare clic su **Proprietà** per visualizzare le informazioni sulla cache, incluse porte ed endpoint della cache.

![Proprietà di Cache Redis](./media/cache-configure/redis-cache-properties.png)

### Chiavi di accesso

Fare clic su **Chiavi di accesso** per visualizzare o rigenerare le chiavi di accesso per la cache. Tali chiavi vengono utilizzate insieme al nome host e alle porte indicate nel pannello **Proprietà** dai client che si connettono alla cache.

![Chiavi di accesso di Cache Redis](./media/cache-configure/redis-cache-manage-keys.png)

### Porte di accesso

Per le nuove cache la porta senza SSL è disabilitata per impostazione predefinita. Per abilitare la porta non SSL, fare clic sul pannello **Porte di accesso** e quindi fare clic su **No**.

![Porte di accesso di Cache Redis](./media/cache-configure/redis-cache-access-ports.png)

### Maxmemory-policy e maxmemory-reserved

Fare clic su **Criterio per la memoria massima** per configurare i criteri di memoria per la cache. L'impostazione **maxmemory-policy** consente di configurare i criteri di eliminazione per la cache mentre **maxmemory-reserved** consente di configurare la memoria riservata per i processi non appartenenti alla cache.

![Criterio maxmemory di Cache Redis](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Criterio per la memoria massima** consente di scegliere tra i criteri di eliminazione seguenti.

-	volatile-lru: impostazione predefinita.
-	allkeys-lru
-	volatile-random
-	allkeys-random
-	volatile-ttl
-	noeviction

Per altre informazioni sui criteri maxmemory, vedere [Criteri di eliminazione](http://redis.io/topics/lru-cache#eviction-policies).

L'impostazione **maxmemory-reserved** consente di configurare la quantità di memoria in MB riservata per le operazioni non appartenenti alla cache, ad esempio la replica durante il failover. Può essere utilizzata anche in caso di elevato rapporto di frammentazione. L’impostazione di questo valore consente di avere un'esperienza più coerente del server Redis quando il carico varia. Questo valore deve essere impostato per i carichi di lavoro ad intensa attività di scrittura. Quando la memoria è riservata per tali operazioni non è disponibile per l'archiviazione dei dati della cache.

>[AZURE.IMPORTANT] L'impostazione **maxmemory-reserved** è disponibile solo per le cache Premium e Standard.

### Notifiche di Keyspace (impostazioni avanzate)

Per configurare le notifiche di Keyspace Redis, fare clic su **Impostazioni avanzate**. Le notifiche di Keyspace consentono ai client di ricevere notifiche quando si verificano determinati eventi.

![Impostazioni avanzate di Cache Redis](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Le notifiche di Keyspace e l'impostazione **notify-keyspace-events** sono disponibili solo per le cache Premium e Standard.

Per altre informazioni, vedere [Notifiche di Keyspace Redis](http://redis.io/topics/notifications). Per un esempio di codice, vedere il file [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) nell'esempio di [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld).

<a name="recommendations"></a>
## Redis Cache Advisor

Il pannello **Raccomandazioni** mostra le raccomandazioni per la cache. Durante il normale funzionamento non viene visualizzata nessuna raccomandazione.

![Indicazioni](./media/cache-configure/redis-cache-no-recommendations.png)

Se durante l'uso della cache si verifica una determinata condizione, ad esempio uso della memoria, larghezza di banda di rete o carico del server elevato, nel pannello **Cache Redis** viene visualizzato un avviso.

![Indicazioni](./media/cache-configure/redis-cache-recommendations-alert.png)

Per altre informazioni, vedere il pannello **Raccomandazioni**.

![Consigli](./media/cache-configure/redis-cache-recommendations.png)

È possibile monitorare le metriche nelle sezioni [Grafici di monitoraggio](cache-how-to-monitor.md#monitoring-charts) e [Grafici di utilizzo](cache-how-to-monitor.md#usage-charts) del pannello **Cache Redis**.

Ogni piano tariffario presenta diversi limiti di connessioni client, memoria e larghezza di banda. Se la cache rasenta la capacità massima di queste metriche per un periodo prolungato, viene creata una raccomandazione. Per altre informazioni sulle metriche e sui limiti considerati dallo strumento **Raccomandazioni**, vedere la tabella seguente.

| Metrica della cache Redis | Per altre informazioni vedere |
|-------------------------|---------------------------------------------------------------------------|
| Uso della larghezza di banda di rete | [Prestazioni della cache - Larghezza di banda disponibile](cache-faq.md#cache-performance) |
| Client connessi | [Configurazione predefinita del server Redis - maxclients](#maxclients) |
| Carico del server | [Grafici di utilizzo - Carico server Redis](cache-how-to-monitor.md#usage-charts) |
| Utilizzo della memoria | [Prestazioni della cache - Dimensioni](cache-faq.md#cache-performance) |

Per aggiornare la cache, fare clic su **Aggiorna ora** per modificare il [piano tariffario](#pricing-tier) e ridimensionare la cache. Per altre informazioni su come scegliere un piano tariffario, vedere [Quali offerte e dimensioni della Cache Redis è consigliabile usare?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## Impostazioni di scalabilità

Le impostazioni nella sezione **Scalabilità** consentono di visualizzare e configurare le impostazioni seguenti relative alla cache.

![Rete](./media/cache-configure/redis-cache-scale.png)

-	[Pricing tier](#pricing-tier)
-	[Dimensione del cluster Redis](#cluster-size)

### Piano tariffario

Fare clic su **Livello di prezzo** per visualizzare o modificare il livello di prezzo della cache. Per altre informazioni sulla scalabilità, vedere [Come monitorare Cache Redis di Azure](cache-how-to-scale.md).

![Livello di prezzo di Cache Redis](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### Dimensione del Cluster Redis

Fare clic su **(ANTEPRIMA) dimensione del cluster Redis** per modificare la dimensione di un cluster per una cache premium in esecuzione con il clustering abilitato.

>[AZURE.NOTE] Si noti che mentre il livello Premium della Cache Redis di Azure è stato rilasciato pubblicamente, la funzionalità dimensione del Cluster Redis è attualmente in anteprima.

![Dimensione del cluster Redis](./media/cache-configure/redis-cache-redis-cluster-size.png)

Per modificare le dimensioni del cluster, usare il dispositivo di scorrimento oppure digitare un numero compreso tra 1 e 10 nella casella di testo **Numero di partizioni** e fare clic su **OK** per salvare.

>[AZURE.IMPORTANT] Il clustering Redis è disponibile esclusivamente per le cache Premium. Per altre informazioni, vedere [Come configurare il clustering per una Cache Redis di Azure Premium](cache-how-to-premium-clustering.md).


## Impostazioni di gestione dati

Le impostazioni nella sezione **Gestione dati** consentono di visualizzare e configurare le impostazioni seguenti relative alla cache.

![Gestione dati](./media/cache-configure/redis-cache-data-management.png)

-	[Persistenza dei dati Redis:](#redis-data-persistence)
-	[Importazione/Esportazione](#importexport)

### Persistenza dei dati Redis:

Fare clic su **Persistenza dati Redis** per abilitare, disabilitare o configurare la persistenza dei dati per la cache premium.

![Persistenza dei dati Redis:](./media/cache-configure/redis-cache-persistence-settings.png)

Per abilitare la persistenza Redis, fare clic su **Abilitata** per consentire il backup RDB (database Redis). Per disabilitare la persistenza Redis, fare clic su **Disabilitata**.

Per configurare l'intervallo di backup, selezionare una **Frequenza di Backup** dall'elenco a discesa. Le opzioni disponibili includono **15 minuti**, **30 minuti**, **60 minuti**, **6 ore**, **12 ore** e **24 ore**. Il conto alla rovescia per l'intervallo inizia dopo il corretto completamento dell'operazione di backup precedente e al termine viene avviato un nuovo backup.

Fare clic su **Account di archiviazione** per selezionare l'account di archiviazione da utilizzare e scegliere la **Chiave primaria** o la **Chiave secondaria** da utilizzare dall’elenco a discesa **Chiave di archiviazione**. È necessario scegliere un account di archiviazione nella stessa area della cache ed è consigliato un account **Archiviazione Premium**, poiché archiviazione premium ha una velocità effettiva maggiore. Ogni volta che la chiave di archiviazione per l'account di persistenza viene rigenerata, è necessario scegliere nuovamente la chiave desiderata dall’elenco a discesa **Chiave di archiviazione**.

Fare clic su **OK** per salvare la configurazione della persistenza.

>[AZURE.IMPORTANT] La persistenza dei dati Redis è disponibile solo per le cache Premium. Per altre informazioni, vedere [Come configurare la persistenza dei dati per una Cache Redis di Azure Premium](cache-how-to-premium-persistence.md).

### Importazione/Esportazione

L'importazione/esportazione è un'operazione di gestione dati di Cache Redis di Azure che consente di importare o esportare dati da Cache Redis di Azure, importando o esportando uno snapshot del database di Cache Redis (RDB) da una cache Premium a un BLOB di pagine in un account di archiviazione di Azure. Questa operazione consente di eseguire la migrazione tra diverse istanze di Cache Redis di Azure o di popolare la cache con i dati prima dell'uso.

L'importazione può essere usata per spostare i file RDB compatibili con Redis da qualsiasi server Redis in esecuzione su qualsiasi cloud o ambiente, compresi i server Redis in esecuzione su Linux, Windows o su qualsiasi provider di servizi cloud come Amazon Web Services e altri. L'importazione dei dati è un modo semplice per creare una cache con dati già popolati. Durante il processo di importazione Cache Redis di Azure carica i file RDB dall'archiviazione di Azure nella memoria e quindi inserisce le chiavi nella cache.

L'esportazione consente di esportare i dati memorizzati in Cache Redis di Azure su file RDB compatibili con Redis. È possibile usare questa funzionalità per spostare i dati da un'istanza di Cache Redis di Azure a un'altra o su un altro server Redis. Durante il processo di esportazione viene creato un file temporaneo nella VM che ospita l'istanza del server Cache Redis di Azure e il file viene caricato nell'account di archiviazione designato. Quando l'operazione di esportazione viene completata con esito positivo o negativo, il file temporaneo viene eliminato.

>[AZURE.IMPORTANT] La funzionalità Importazione/Esportazione è disponibile solo per le cache del piano Premium. Per altre informazioni e istruzioni, vedere [Importare ed esportare dati in Cache Redis di Azure](cache-how-to-import-export-data.md).


## Impostazioni di amministrazione

Le impostazioni della sezione **Amministrazione** consentono di eseguire le attività amministrative per la cache Premium seguenti.

![Amministrazione](./media/cache-configure/redis-cache-administration.png)

-	[Reboot](#reboot)
-	[Pianificare gli aggiornamenti](#schedule-updates)

>[AZURE.IMPORTANT] Le impostazioni descritte in questa sezione sono disponibili solo per le cache di livello Premium.

### Reboot

Il pannello **Reboot** (Riavvio) consente di riavviare uno o più nodi della cache. Ciò consente di verificare la resilienza dell'applicazione in caso di errore.

![Reboot](./media/cache-configure/redis-cache-reboot.png)

Se si dispone di una cache Premium con clustering abilitato, è possibile selezionare le partizioni della cache da riavviare.

![Reboot](./media/cache-configure/redis-cache-reboot-cluster.png)

Per riavviare uno o più nodi della cache, selezionare i nodi desiderati e fare clic su **Reboot** (Riavvia). Se si dispone di una cache Premium con clustering abilitato, selezionare le partizioni per riavviare il computer e quindi fare clic su **Reboot** (Riavvia). Dopo alcuni minuti, i nodi selezionati si riavviano e vengono ripristinati online pochi minuti dopo.

>[AZURE.IMPORTANT] La funzionalità di riavvio è disponibile solo per le cache del piano Premium. Per altre informazioni e istruzioni, vedere [Come amministrare Cache Redis di Azure - Riavvio](cache-administration.md#reboot).

### Pianificare gli aggiornamenti

Il pannello **Schedule updates** (Pianificazione aggiornamenti) consente di progettare un intervallo di manutenzione per gli aggiornamenti del server Redis della cache.

>[AZURE.IMPORTANT] Si noti che l'intervallo di manutenzione è applicabile solo agli aggiornamenti del server Redis e non a tutti gli aggiornamenti di Azure o del sistema operativo delle macchine virtuali che ospitano la cache.

![Pianificare gli aggiornamenti](./media/cache-configure/redis-schedule-updates.png)

Per specificare un intervallo di manutenzione, selezionare i giorni desiderati e specificare l'ora di inizio dell'intervallo per ogni giorno, quindi fare clic su **OK**. Si noti che l'orario dell'intervallo di manutenzione è in formato UTC.

>[AZURE.IMPORTANT] La pianificazione degli aggiornamenti è disponibile solo per le cache del piano Premium. Per altre informazioni e istruzioni, vedere [Come amministrare Cache Redis di Azure - Pianificare gli aggiornamenti](cache-administration.md#schedule-updates).

## Impostazioni di diagnostica

La sezione **Diagnostica** consente di configurare la diagnostica per la cache Redis.

![Diagnostica](./media/cache-configure/redis-cache-diagnostics.png)

Fare clic su **Diagnostica** per [configurare l'account di archiviazione](cache-how-to-monitor.md#enable-cache-diagnostics) usato per archiviare la diagnostica della cache.

![Diagnostica di Cache Redis](./media/cache-configure/redis-cache-diagnostics-settings.png)

Fare clic su **Metriche di Redis** per [visualizzare le metriche](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) per la cache e su **Regole di avviso** per [configurare le regole di avviso](cache-how-to-monitor.md#operations-and-alerts).

Per altre informazioni sulla diagnostica di Cache Redis di Azure, vedere [Come monitorare Cache Redis di Azure](cache-how-to-monitor.md).


## Impostazioni di rete

Le impostazioni nella sezione **Rete** consentono di visualizzare e configurare le impostazioni seguenti relative alla cache.

![Rete](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Le impostazioni della rete virtuale sono disponibili solo per le cache di livello Premium configurate con il supporto della rete virtuale durante la creazione della cache. Per informazioni sulla creazione di una cache di livello Premium che supporti la rete virtuale e sull'aggiornamento delle impostazioni, vedere [Come configurare il supporto di una rete virtuale per un'istanza Premium di Cache Redis di Azure](cache-how-to-premium-vnet.md).

## Impostazioni di gestione delle risorse

![Resource management](./media/cache-configure/redis-cache-resource-management.png)

La sezione **Tag** consente di organizzare le risorse. Per altre informazioni, vedere [Uso dei tag per organizzare le risorse di Azure](../resource-group-using-tags.md).

La sezione **Blocchi** consente di bloccare una sottoscrizione, una risorsa o un gruppo di risorse per impedire che altri utenti nell'organizzazione modifichino o eliminino accidentalmente risorse strategiche. Per altre informazioni, vedere [Bloccare le risorse con Gestione risorse di Azure](../resource-group-lock-resources.md).

La sezione **Utenti** fornisce il supporto del controllo di accesso in base al ruolo (RBAC) nel portale di Azure per aiutare le organizzazioni a soddisfare i requisiti di gestione degli accessi in maniera semplice e precisa. Per altre informazioni, vedere[Controllo di accesso in base al ruolo nel portale di Azure](../active-directory/role-based-access-control-configure.md).

Fare clic su **Esporta modello** per compilare ed esportare un modello delle risorse distribuite per le distribuzioni future. Per altre informazioni sull'uso dei modelli, vedere [Distribuire le risorse con i modelli di Azure Resource Manager](../resource-group-template-deploy.md).

## Configurazione predefinita del server Redis

Le nuove istanze di Cache Redis di Azure sono configurate con i seguenti valori predefiniti di configurazione di Redis.

>[AZURE.NOTE] Le impostazioni in questa sezione non possono essere modificate con il metodo `StackExchange.Redis.IServer.ConfigSet`. Se questo metodo viene chiamato con uno dei comandi indicati in questa sezione, viene generata un'eccezione simile alla seguente:
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>I valori configurabili, ad esempio **max-memory-policy**, possono essere configurati tramite il portale di Azure o strumenti di gestione della linea di comando, come Azure CLI o PowerShell.

|Impostazione|Valore predefinito|Descrizione|
|---|---|---|
|database|16|Il numero predefinito di database è 16, ma è possibile configurare un numero diverso in base al piano tariffario.<sup>1</sup> Il database predefinito è DB 0, ma è possibile selezionarne uno diverso in base alla connessione usando `connection.GetDatabase(dbid)` dove dbid è un numero compreso tra `0` e `databases - 1`.|
|maxclients|Dipende dal piano tariffario<sup>2</sup>|Questo è il numero massimo consentito di client connessi contemporaneamente. Una volta raggiunto il limite, Redis chiuderà tutte le nuove connessioni inviando un errore di "numero massimo di client raggiunto".|
|maxmemory-policy|volatile-lru|Il criterio maxmemory è l'impostazione che serve a stabilire il modo in cui Redis selezionerà gli elementi da rimuovere quando viene raggiunto il valore di maxmemory (la dimensione dell’offerta della cache selezionata in fase di creazione della cache). Con Cache Redis di Azure l'impostazione predefinita è volatile-lru, che rimuove le chiavi con una scadenza impostata usando l'algoritmo LRU. Questa impostazione può essere configurata nel portale di Azure. Per ulteriori informazioni, vedere [Maxmemory-policy e maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved).|
|maxmemory-samples|3|Gli algoritmi LRU e TTL non sono precisi ma approssimativi (per risparmiare spazio in memoria), pertanto è possibile selezionare anche la dimensione del campione da controllare. Ad esempio, per impostazione predefinita Redis controllerà tre chiavi e sceglierà quella usata meno di recente.|
|lua-time-limit|5\.000|Tempo massimo di esecuzione di uno script Lua in millisecondi. Se il tempo di esecuzione massimo viene raggiunto, Redis registrerà che uno script è ancora in esecuzione dopo il tempo massimo consentito e inizierà a rispondere alle query con un errore.|
|lua-event-limit|500|Questa è la dimensione massima della coda di eventi di script.|
|client-output-buffer-limit normalclient-output-buffer-limit pubsub|0 0 032mb 8mb 60|I limiti del buffer di output del client possono essere utilizzati per forzare la disconnessione dei client che per qualche motivo non leggono i dati dal server in modo sufficientemente rapido. Ad esempio, è frequente che un client di pubblicazione o sottoscrizione non possa utilizzare i messaggi con la stessa velocità con cui il server di pubblicazione li produce. Per ulteriori informazioni, vedere [http://redis.io/topics/clients](http://redis.io/topics/clients).|

<a name="databases"></a> <sup>1</sup>Il limite per `databases` è diverso per ogni piano tariffario di Cache Redis di Azure e può essere impostato durante la creazione della cache. Se durante la creazione della cache non viene specificata alcuna impostazione `databases`, il valore predefinito è 16.

-	Cache di base e Standard
	-	Cache C0 (250 MB): fino a 16 database
	-	Cache C1 (1 GB): fino a 16 database
	-	Cache C2 (2,5 GB): fino a 16 database
	-	Cache C3 (6 GB): fino a 16 database
	-	Cache C4 (13 GB): fino a 32 database
	-	Cache C5 (26 GB): fino a 48 database
	-	Cache C6 (53 GB): fino a 64 database
-	Cache Premium
	-	P1 (6 GB - 60 GB): fino a 16 database
	-	P2 (13 GB - 130 GB): fino a 32 database
	-	P3 (26 GB - 260 GB): fino a 48 database
	-	P4 (53 GB - 530 GB): fino a 64 database
	-   Tutte le cache Premium con il cluster Redis abilitato: il cluster Redis supporta solo l'uso del database 0, quindi il limite `databases` per le cache Premium con il cluster Redis abilitato è effettivamente 1 e il comando [Select](http://redis.io/commands/select) non è consentito. Per altre informazioni, vedere [È necessario apportare modifiche all'applicazione client per usare il clustering?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] L'impostazione `databases` può essere configurata solo durante la creazione della cache e solo usando PowerShell, l'interfaccia della riga di comando o altri client di gestione. Per un esempio di configurazione di `databases` durante la creazione della cache con PowerShell, vedere [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a> <sup>2</sup>Il valore di `maxclients` è diverso per ogni piano tariffario di Cache Redis di Azure.

-	Cache di base e Standard
	-	Cache C0 (250 MB): fino a 256 connessioni
	-	Cache C1 (1 GB): fino a 1000 connessioni
	-	Cache C2 (2,5 GB): fino a 2000 connessioni
	-	Cache C3 (6 GB): fino a 5000 connessioni
	-	Cache C4 (13 GB): fino a 10.000 connessioni
	-	Cache C5 (26 GB): fino a 15.000 connessioni
	-	Cache C6 (53 GB): fino a 20.000 connessioni
-	Cache Premium
	-	P1 (6 GB - 60 GB) - fino a 7.500 connessioni
	-	P2 (13 GB - 130 GB) - fino a 15.000 connessioni
	-	P3 (26 GB - 260 GB) - fino a 30.000 connessioni
	-	P4 (53 GB - 530 GB) - fino a 40.000 connessioni

## Comandi di Redis non supportati in Cache Redis di Azure

>[AZURE.IMPORTANT] Poiché la configurazione e gestione delle istanze di Cache Redis di Azure viene gestita da Microsoft, i comandi seguenti sono disabilitati. Se si prova a richiamarli, si riceverà un messaggio di errore simile a `"(error) ERR unknown command"`.
>
>-	BGREWRITEAOF
>-	BGSAVE
>-	CONFIG
>-	DEBUG
>-	MIGRATE
>-	SALVA
>-	SHUTDOWN
>-	SLAVEOF
>-	CLUSTER: i comandi di scrittura del cluster sono disabilitati, ma i comandi del cluster di sola lettura sono consentiti.

Per ulteriori informazioni sui comandi di Redis, vedere [http://redis.io/commands](http://redis.io/commands).

## Console Redis

È possibile eseguire comandi in modo sicuro per le istanze di Cache Redis di Azure usando la **Console di Redis** disponibile per le cache Premium e Standard.

>[AZURE.IMPORTANT] La Console di Redis non funziona con la rete virtuale o il servizio cluster.
>
>-	[VNET](cache-how-to-premium-vnet.md): quando la cache fa parte di una rete virtuale, solo i client nella rete virtuale possono accedere alla cache. Poiché la Console di Redis utilizza il client di redis cli.exe ospitato su macchine virtuali che non fanno parte di una rete virtuale, è impossibile connettersi alla cache.
>-	[Clustering](cache-how-to-premium-clustering.md): la Console di Redis utilizza il client redis-cli.exe che non supporta il clustering in questo momento. L'utilità del cli redis nel ramo [instabile](http://redis.io/download) del repository Redis in GitHub implementa il supporto di base quando avviato con il passaggio `-c`. Per ulteriori informazioni vedere [Giocare con cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) su [http://redis.io](http://redis.io) nell’[Esercitazione cluster Redis](http://redis.io/topics/cluster-tutorial).

Per accedere alla Console Redis, fare clic su **Console** dal pannello **Cache Redis**.

![Console Redis](./media/cache-configure/redis-console-menu.png)

Per eseguire comandi con l'istanza di cache, digitare semplicemente il comando desiderato nella console.

![Console Redis](./media/cache-configure/redis-console.png)

Per un elenco dei comandi di Redis disabilitati per Cache Redis di Azure, vedere la precedente sezione [Comandi di Redis non supportati in Cache Redis di Azure](#redis-commands-not-supported-in-azure-redis-cache). Per ulteriori informazioni sui comandi di Redis, vedere [http://redis.io/commands](http://redis.io/commands).

## Spostare la cache in una nuova sottoscrizione

È possibile spostare la cache in una nuova sottoscrizione facendo clic su **Sposta**.

![Spostare la cache Redis](./media/cache-configure/redis-cache-move.png)

Per informazioni sullo spostamento delle risorse da un gruppo di risorse all'altro e da una sottoscrizione all'altra, vedere [Spostare le risorse in un gruppo di risorse o una sottoscrizione nuovi](../resource-group-move-resources.md).

## Passaggi successivi
-	Per altre informazioni sull'uso dei comandi di Redis, vedere [Come è possibile eseguire i comandi di Redis?](cache-faq.md#how-can-i-run-redis-commands).

<!---HONumber=AcomDC_0629_2016-->