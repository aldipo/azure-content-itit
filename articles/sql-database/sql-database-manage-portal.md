<properties
	pageTitle="Gestire database SQL di Azure mediante il portale di Azure"
	description="Informazioni su come usare il portale di Azure per gestire un database relazionale nel cloud tramite il portale di Azure."
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"
	ms.date="05/20/2016"
	ms.author="sstein"/>


# Gestione dei database SQL di Azure tramite il portale di Azure


> [AZURE.SELECTOR]
- [Portale di Azure](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-command-line-tools.md)

Il [portale di Azure](https://portal.azure.com/) consente di creare, monitorare e gestire server logici e database SQL di Azure. Questo articolo evidenzia alcune delle attività più comuni.

![Panoramica dei database](./media/sql-database-manage-portal/sqldatabase_annotated.png)

## 1\. Azioni di gestione del database

![Azioni di gestione del database](./media/sql-database-manage-portal/sqldatabase_actions.png)

Il portale di Azure offre un set di azioni di database comuni accessibile nella parte superiore del pannello di un database. È possibile ripristinare un database a un momento precedente, aprire un database in Visual Studio, copiare un database in un nuovo server ed esportare il database a un account di archiviazione di Azure.

- [Ripristino di un database SQL](sql-database-user-error-recovery.md)
- [Aprire un database SQL in Visual Studio](sql-database-connect-query.md)
- [Esportare un database SQL](sql-database-export.md)

## 2\. Monitoraggio del database

![Monitoraggio del database](./media/sql-database-manage-portal/sqldatabase_monitoring.png)

Per impostazione predefinita, i database SQL di Azure dispongono di grafici di monitoraggio per unità di transazione database (DTU), dimensioni del database e stato della connessione. Questi grafici di monitoraggio possono essere personalizzati ed estesi per indicare la percentuale di CPU, la percentuale di IO dei dati, i deadlock, la percentuale di IO del log o persino la percentuale di richieste bloccate dal firewall.

Inoltre, è possibile configurare regole di avviso per monitorare una metrica specificata e avvisare un amministratore designato e un co-amministratore quando vengono raggiunte le soglie predefinite.

## 3\. Protezione del database e controllo

![Protezione del database](./media/sql-database-manage-portal/sqldatabase_security.png)

I database SQL di Azure possono essere configurati per tenere traccia di tutti gli eventi di database e scriverli in un log di controllo nell'account di archiviazione di Azure. Tale funzionalità consente di mantenere la conformità alle normative, comprendere l’attività del database e ottenere informazioni su eventuali discrepanze che potrebbero indicare problemi aziendali o sospette violazioni della sicurezza.

- [Controllo del Database SQL](sql-database-auditing-get-started.md)

I database SQL di Azure possono essere configurati anche per mascherare i dati sensibili agli utenti che non hanno privilegi.

- [Maschera dati dinamica](sql-database-dynamic-data-masking-get-started.md)


## 4\. Replica geografica

![Replica geografica](./media/sql-database-manage-portal/sqldatabase_georeplication.png)

I database SQL di Azure possono essere configurati per replicare in modo asincrono le transazioni sottoposte a commit in un database secondario. La parte della replica geografica nel portale consente di selezionare l'area di Azure in cui si vuole far risiedere il database secondario.

- [Replica geografica](sql-database-geo-replication-overview.md)



## Risorse aggiuntive

- [Database SQL](sql-database-technical-overview.md)

<!---HONumber=AcomDC_0608_2016-->