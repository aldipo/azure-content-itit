<properties
   pageTitle="Continuità aziendale cloud - Ripristino di un database eliminato - Database SQL | Microsoft Azure"
   description="Informazioni sul ripristino temporizzato, che consente di ripristinare un database SQL di Azure a un momento precedente (fino a 35 giorni)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-bcdr"
   ms.date="06/09/2016"
   ms.author="sstein"/>

# Ripristinare un database SQL di Azure mediante i backup automatici del database

Il database SQL offre tre opzioni di ripristino dei database con i [backup automatici del database SQL](sql-database-automated-backups.md). È possibile ripristinare un database dai backup avviati dal servizio durante il relativo [periodo di conservazione](sql-database-service-tiers.md) in:

- Un nuovo database nello stesso server logico ripristinato in una temporizzazione specificata entro il periodo di conservazione.
- Un database nello stesso server logico ripristinato all'ora di eliminazione per un database eliminato.
- Un nuovo database in qualsiasi server logico in qualsiasi area ripristinato ai backup giornalieri più recenti nell'archiviazione BLOB con replica geografica (RA-GRS).

È inoltre possibile usare [backup automatici del database SQL](sql-database-automated-backups.md) per creare una [copia del database](sql-database-copy.md) in qualsiasi server logico in qualsiasi area coerente da un punto di vista transazionale con il database SQL corrente. È possibile usare la copia del database ed [esportare in un file BACPAC](sql-database-export.md) per archiviare una copia di un database coerente da un punto di vista transazionale per l'archiviazione a lungo termine oltre il periodo di conservazione o trasferire una copia del database in un'istanza locale o un'istanza VM di Azure di SQL Server.

## Tempo di ripristino

Il tempo di ripristino di un database tramite i backup automatici del database dipende da numerosi fattori:
 - Le dimensioni del database
 - Il livello di prestazioni del database
 - Il numero dei log delle transazioni interessate
 - La quantità di attività che deve essere ripetuta per il ripristino al punto di ripristino
 - La larghezza di banda di rete se il ripristino avviene in un'area diversa
 - Il numero di richieste simultanee di ripristino in corso di elaborazione nell'area di destinazione.
 
 Per un database molto grande e/o attivo il ripristino potrebbe richiedere diverse ore. Nel caso di un'interruzione prolungata in un'area, è possibile che in altre aree vengano elaborate molte richieste di ripristino geografico. In presenza di un numero elevato di richieste, i tempi di ripristino dei database in tale area potrebbero aumentare. Per la maggior parte dei database, il ripristino richiede al massimo 12 ore.

 Non è disponibile una funzionalità incorporata per il ripristino in blocco. Lo script [Database SQL di Azure: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) è un esempio di un modo per eseguire questa operazione.

## Ripristino temporizzato

La funzionalità di ripristino temporizzato consente di ripristinare un database esistente come database nuovo a un momento precedente nel tempo sullo stesso server logico, usando i [backup automatici del database SQL](sql-database-automated-backups.md). Non è possibile sovrascrivere il database esistente. È possibile eseguire il ripristino a un punto precedente nel tempo usando il [portale di Azure](sql-database-point-in-time-restore-portal.md), [PowerShell](sql-database-point-in-time-restore-powershell.md) o l'[API REST](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Ripristino temporizzato: portale di Azure](sql-database-point-in-time-restore-portal.md)
- [Ripristino temporizzato: PowerShell](sql-database-point-in-time-restore-powershell.md)

Il database può essere ripristinato a qualsiasi livello di prestazioni o pool elastico. È necessario avere a disposizione una quota DTU sufficiente per il server logico o il pool elastico. Tenere presente che il ripristino crea un nuovo database e che il livello di prestazioni e il livello di servizio del database ripristinato potrebbero essere diversi dallo stato corrente del database attivo. Al termine, il database ripristinato è un normale database online completamente accessibile alle tariffe normali in base al livello di prestazioni e al livello di servizio. Non è previsto alcun addebito fino al completamento del ripristino del database.

In genere si ripristina un database a un punto di precedenza per scopi di ripristino. Quando si esegue questa operazione, è possibile considerare il database ripristinato come una sostituzione per il database originale o usarlo per recuperare i dati e quindi aggiornare il database originale.

- ***Sostituzione del database:***se il database ripristinato è da intendersi come una sostituzione per il database originale, è necessario verificare che il livello di prestazioni e/o livello di servizio sia appropriato e ridimensionare il database se necessario. È possibile rinominare il database originale e quindi assegnare al database ripristinato il nome originale usando il comando ALTER DATABASE in T-SQL.
- ***Ripristino dei dati:*** se si prevede di recuperare dati dal database ripristinato in caso di errore dell'applicazione o dell'utente, sarà necessario scrivere ed eseguire separatamente qualsiasi script di ripristino dei dati sia necessario per estrarre i dati dal database ripristinato nel database originale. Anche se il completamento dell'operazione di ripristino può richiedere molto tempo, il database in fase di ripristino sarà visibile nell'elenco dei database per tutto il tempo. Se si elimina il database durante il ripristino l'operazione verrà annullata e non verrà addebitata per il database il cui ripristino non è stato completato.

Per informazioni dettagliate sull'uso del ripristino temporizzato in caso di errori dell'utente e dell'applicazione, vedere [Ripristino da un errore utente](sql-database-user-error-recovery.md)

## Ripristino di un database eliminato

Il ripristino di un database eliminato consente di ripristinare un database all'ora di eliminazione per un database eliminato sullo stesso server logico, usando i [backup automatici del database SQL](sql-database-automated-backups.md).

> [AZURE.IMPORTANT] Se si elimina un'istanza del server di database SQL di Azure, vengono eliminati anche tutti i relativi database e non possono essere recuperati. Non è attualmente disponibile alcun supporto per il ripristino di un server eliminato.

È possibile usare lo stesso nome o un nuovo nome di database per il database ripristinato. È possibile usare il [portale di Azure](sql-database-restore-deleted-database-portal.md), [PowerShell](sql-database-restore-deleted-database-powershell.md) o l'[REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Ripristino di un database eliminato: portale di Azure](sql-database-restore-deleted-database-portal.md)
- [Ripristino di un database eliminato: PowerShell](sql-database-restore-deleted-database-powershell.md)

## Ripristino geografico

Il ripristino geografico consente di ripristinare un database SQL presente su un server in qualsiasi area di Azure usando l'ultimo [backup giornaliero automatico](sql-database-automated-backups.md) con replica geografica. Il ripristino geografico usa un backup con ridondanza geografica come origine e può essere usato per ripristinare un database anche se il database o il data center è inaccessibile a causa di un guasto. È possibile usare il [portale di Azure](sql-database-geo-restore-portal.md), [PowerShell](sql-database-geo-restore-powershell.md) o il [REST (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)

> [AZURE.SELECTOR]
- [Ripristino geografico: portale di Azure](sql-database-geo-restore-portal.md)
- [Ripristino geografico: PowerShell](sql-database-geo-restore-powershell.md)

Il ripristino geografico è l'opzione di ripristino predefinita quando il database non è disponibile a causa di un evento imprevisto nell'area in cui è ospitato. Se si verifica un evento imprevisto su larga scala che determina la mancata disponibilità dell'applicazione di database, è possibile usare il ripristino geografico per ripristinare un database dal backup più recente in un server in un'altra area. Tutti i backup sono con replica geografica ed è possibile si verifichi un ritardo che tra l'acquisizione del backup e la replica geografica nel BLOB di Azure in un'area diversa. Questo ritardo può essere al massimo di un'ora, quindi in caso di emergenza può verificarsi una perdita massima di 1 ora di dati, ovvero un valore RPO fino a 1 ora. Di seguito è mostrato il ripristino del database dall'ultimo backup giornaliero.

![ripristino geografico](./media/sql-database-geo-restore/geo-restore-2.png)

Per informazioni dettagliate sull'uso del ripristino geografico in caso di un'interruzione del servizio, vedere [Ripristino dopo un'interruzione del servizio](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Il ripristino geografico, disponibile con tutti i livelli di servizio, è la più semplice delle soluzioni di ripristino di emergenza disponibili nel database SQL con il massimo valore per RPO e tempo di recupero stimato. Per i database di base con una dimensione massima di 2 GB, il ripristino geografico rappresenta una soluzione ragionevole per il ripristino di emergenza con un tempo di recupero stimato di 12 ore. Per i database Standard o Premium di dimensioni maggiori, se è auspicabile una riduzione significativa dei tempi di ripristino o per limitare le probabilità di perdita di dati, è consigliabile usare la replica geografica attiva. La replica geografica attiva offre un obiettivo del punto di ripristino e un tempo di recupero stimato decisamente inferiori perché richiede solo l'avvio di un failover in un database secondario con replica continua. Per informazioni dettagliate, vedere [Replica geografica attiva](sql-database-geo-replication-overview.md).

## Esecuzione a livello di codice del ripristino tramite backup automatici

Come indicato in precedenza, oltre al portale di Azure, il ripristino di database può essere eseguito a livello di codice usando Azure PowerShell e l'API REST. Nelle tabelle seguenti viene descritto il set di comandi disponibili.

### PowerShell

|Cmdlet|Descrizione|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/it-IT/library/azure/mt603648.aspx)|Ottiene uno o più database.|
|[Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/it-IT/library/azure/mt693387.aspx)|Ottiene un database eliminato che è possibile ripristinare.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Ottiene una copia di backup con ridondanza geografica di un database.|
|[Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Ripristina un database SQL.|
||||

### API REST

|API|Descrizione|
|---|-----------|
|[REST (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Ripristina un database|
|[Get Create or Update Database Status](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Restituisce lo stato durante un'operazione di ripristino|
||||



## Riepilogo

I backup automatici proteggono i database da errori dell'utente e delle applicazioni, dall'eliminazione accidentale e da interruzioni prolungate. Questa soluzione a costo zero e senza l'intervento dell'amministratore è disponibile con tutti i database SQL.

## Passaggi successivi

- Per una panoramica sulla continuità aziendale, vedere [Panoramica sulla continuità aziendale](sql-database-business-continuity.md)
- Per informazioni sui backup automatici del database SQL di Azure, vedere [Backup automatici del database SQL](sql-database-automated-backups.md)
- Per informazioni sugli scenari di progettazione e ripristino della continuità aziendale, vedere [Scenari di continuità aziendale](sql-database-business-continuity-scenarios.md)
- Per altre informazioni sulle opzioni di ripristino più veloci, vedere [Replica geografica attiva](sql-database-geo-replication-overview.md)
- Per altre informazioni sull'uso di backup automatici per l'archiviazione, vedere [Copia del database](sql-database-copy.md)

<!---HONumber=AcomDC_0629_2016-->