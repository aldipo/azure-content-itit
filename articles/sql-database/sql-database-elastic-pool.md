<properties
	pageTitle="Che cos'è un pool di database elastici di Azure? | Microsoft Azure"
	description="È possibile gestire centinaia o migliaia di database con un pool. Un unico prezzo per un set di unità di prestazioni può essere distribuito nel pool. I database possono essere inseriti o rimossi in base alle esigenze."
	keywords="database elastici, database SQL"
	services="sql-database"
	documentationCenter=""
	authors="srinia"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="05/27/2016"
	ms.author="srinia"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Che cos'è un pool di database elastici di Azure?

I pool elastici offrono una soluzione semplice e conveniente per gestire gli obiettivi di prestazioni per più database con modelli di utilizzo estremamente mutevoli e imprevedibili.

> [AZURE.NOTE] I pool elastici sono disponibili a livello generale in tutte le aree di Azure ad eccezione di Stati Uniti centro-settentrionali, Brasile meridionale, India occidentale e Cina settentrionale, dove sono attualmente in anteprima. La disponibilità generale dei pool elastici in queste aree verrà offerta al più presto.

## Funzionamento

Un modello di applicazione SaaS molto diffuso è il modello di database a singolo tenant, in cui a ogni cliente viene assegnato un database. Ciascun cliente (database) ha requisiti di risorse imprevedibili per memoria, IO e CPU. Questi picchi e stagnazioni della domanda rendono difficile l'allocazione delle risorse. In genere, è possibile procedere in due modi: (1) effettuare il provisioning eccessivo delle risorse in base all'utilizzo massimo e pagare di più oppure (2) effettuare un provisioning insufficiente per risparmiare sui costi, a scapito delle prestazioni e del livello di soddisfazione del cliente nei momenti di picco. I pool di database elastici risolvono il problema assicurando ai database prestazioni adeguate in ogni momento e situazione, fornendo al tempo stesso un semplice meccanismo di allocazione delle risorse con un budget prevedibile. Per altre informazioni sui modelli di progettazione per le applicazioni SaaS mediante pool elastici, vedere [Modelli di progettazione per applicazioni SaaS multi-tenant con database SQL di Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [AZURE.VIDEO elastic-databases-helps-saas-developers-tame-explosive-growth]

Nel database SQL, la misura relativa della capacità del database di gestire le richieste delle risorse è espressa in unità di transazione del database (DTU) per i singoli database e in DTU elastiche (eDTU) per i pool di database elastici. Per altre informazioni su DTU ed eDTU, vedere [Introduzione al database SQL](sql-database-technical-overview.md#understand-dtus).

A un pool viene assegnato un numero definito di eDTU per un prezzo prestabilito. All'interno del pool i singoli database sono sufficientemente flessibili da assicurare una scalabilità automatica nell'ambito di parametri prefissati. Se il carico di lavoro è importante, un database può utilizzare più eDTU per soddisfare la domanda. Se invece il carico di lavoro è più leggero, i database utilizzano meno eDTU e in assenza di carico non ne utilizzano affatto. La possibilità di effettuare il provisioning delle risorse per l'intero pool e non per i singoli database semplifica le attività di gestione. Il budget del pool, inoltre, è facilmente prevedibile.

È possibile aggiungere ulteriori eDTU a un pool esistente senza tempi di inattività del database o impatti negativi sui database. Analogamente, se gli eDTU aggiuntivi non sono più necessari, è possibile rimuoverli da un pool esistente in qualsiasi momento.

È possibile aggiungere e rimuovere database dal pool. Se si prevede che un database sottoutilizzerà le proprie risorse, è possibile rimuoverlo.

## Scelta dei database da inserire in un pool

![Database SQL che condividono eDTU in un pool di database elastici.][1]

I database che sono ottimi candidati per i pool di database elastici sono in genere caratterizzati da periodi di attività e da periodi di inattività. Nell'esempio sopra riportato è possibile visualizzare l'attività di un singolo database, di 4 database e infine di un pool di database elastici con 20 database. I database con livelli di attività variabili nel corso del tempo sono ottimi candidati per i pool elastici in quanto non sono tutti attivi nello stesso momento e possono condividere gli eDTU. Non tutti i database seguono questo modello. I database che hanno una richiesta di risorse più costante sono più indicati per i livelli di servizio Basic, Standard e Premium, nei quali le risorse vengono assegnate singolarmente.

[Considerazioni di prezzo e prestazioni per un pool di database elastici](sql-database-elastic-pool-guidance.md).


> [AZURE.NOTE] I pool di database elastici sono attualmente in anteprima e sono disponibili unicamente con i server di Database SQL V12.

## Limiti di archiviazione e di eDTU dei pool elastici e dei database elastici

[AZURE.INCLUDE [Tabella livelli di servizio di database SQL per database elastici](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

## Proprietà pool di database elastico

### Limiti per i pool elastici

| Proprietà | Descrizione |
| :-- | :-- |
| Livello di servizio | Basic, Standard o Premium. Il livello di servizio determina l'intervallo relativo ai limiti di archiviazione e di prestazioni configurabili nonché le opzioni di continuità aziendale. Ogni database all'interno di un pool ha lo stesso livello di servizio del pool. Il "livello di servizio" è detto anche "edizione". |
| eDTU per pool | Numero massimo di eDTU condivisibile dai database nel pool. In numero totale di eDTU usate dai database nel pool nello stesso momento non può superare questo limite. |
| Spazio di archiviazione massimo per pool (GB) | Quantità massima di spazio di archiviazione in GB condivisibile dai database nel pool. La quantità totale di spazio di archiviazione usato da tutti i database nel pool non può superare questo limite, che è determinato dal numero di eDTU per pool. Se il limite viene superato, tutti i database diventano di sola lettura. |
| Numero massimo di database per pool | Il numero massimo di database consentiti per pool. |
| Numero massimo di ruoli di lavoro simultanei per pool | Il numero massimo di ruoli di lavoro simultanei (richieste) disponibile per tutti i database nel pool. |
| Numero massimo di accessi simultanei per pool | Il numero massimo di accessi simultanei per tutti i database nel pool. |
| Numero massimo di sessioni simultanee per pool | Il numero massimo di sessioni disponibili per tutti i database nel pool. |


### Limiti per i database elastici

| Proprietà | Descrizione |
| :-- | :-- |
| Numero massimo di eDTU per database | Il numero massimo di eDTU di cui un database può usufruire nel pool se disponibili sulla base dell'uso da parte di altri database nel pool. Il numero massimo di eDTU per database non è una garanzia di risorse per un database. Si tratta di un'impostazione globale che si applica a tutti i database nel pool. Impostare il numero massimo di eDTU per database sufficiente per gestire i picchi di utilizzo dei database. È previsto un certo grado di overcommit perché il pool in genere presuppone modelli di utilizzo dei database a freddo e a caldo in cui i database non raggiungono il picco contemporaneamente. Si pensi al caso in cui il picco di utilizzo per ogni database sia di 20 eDTU e solo il 20% dei 100 database nel pool raggiunga il picco nello stesso momento. Se il numero massimo di eDTU per ogni database è impostato su 20 eDTU, è ragionevole eseguire l'overcommit del pool moltiplicando per 5 e impostare il numero di eDTU su 400. |
| Numero minimo di eDTU per database | Il numero minimo di eDTU garantito a ogni database nel pool. Si tratta di un'impostazione globale che si applica a tutti i database nel pool. Il numero minimo di eDTU per database può essere impostato su 0, che corrisponde anche al valore predefinito. Questa proprietà è generalmente impostata su un valore compreso tra 0 e l'utilizzo medio di eDTU per ogni database. Notare che il prodotto tra il numero di database nel pool e il numero minimo di eDTU per database non può superare il numero di eDTU per pool. Ad esempio, se un pool dispone di 20 database e di un numero minimo di eDTU per database impostato su 10 eDTU, il numero di eDTU per pool deve essere almeno pari a 200. |
| Spazio di archiviazione massimo per database (GB) | Lo spazio di archiviazione massimo per un database in un pool. I database elastici condividono lo spazio di archiviazione del pool, pertanto lo spazio di archiviazione del database è limitato allo spazio di archiviazione del pool rimanente e allo spazio massimo per database.|


## Processi di database elastici

L'uso di un pool permette di semplificare le attività di gestione con l'esecuzione di script in **[processi elastici](sql-database-elastic-jobs-overview.md)**. Un processo di database elastico elimina la maggior parte delle attività ripetitive associate a un elevato numero di database. Per iniziare, vedere [Introduzione ai processi di database elastici](sql-database-elastic-jobs-getting-started.md).

Per informazioni sugli altri strumenti, vedere [Database SQL - Funzionalità e strumenti di database elastico](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/).

## Funzionalità di continuità aziendale per i database in un pool

In genere, i database elastici supportano le stesse [funzionalità di continuità aziendale](sql-database-business-continuity.md) disponibili per i singoli database nei server V12.


### Ripristino temporizzato

Il ripristino temporizzato usa i backup automatici del database per ripristinare un database in un pool a un punto specifico nel tempo. Vedere [Ripristinare un database SQL di Azure a seguito di un errore causato dall'utente](sql-database-user-error-recovery.md)

### Ripristino geografico

Il ripristino geografico fornisce un'opzione predefinita di ripristino quando un database non è disponibile a causa di un evento imprevisto nell'area in cui è ospitato. Vedere [Ripristinare un database SQL di Azure in seguito a un'interruzione del servizio](sql-database-disaster-recovery.md)

### Replica geografica attiva

Per le applicazioni che hanno requisiti di ripristino più rigorosi rispetto alle possibilità del ripristino geografico, configurare la replica geografica attiva usando il [portale di Azure](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md) o [Transact-SQL](sql-database-geo-replication-transact-sql.md).


<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png

<!---HONumber=AcomDC_0601_2016-->