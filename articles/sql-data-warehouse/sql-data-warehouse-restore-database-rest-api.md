<properties
   pageTitle="Ripristinare un'istanza di Azure SQL Data Warehouse (API REST) | Microsoft Azure"
   description="Attività dell'API REST per il ripristino di un'istanza di Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyama"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/28/2016"
   ms.author="sonyama;barbkess"/>

# Ripristinare un'istanza di Azure SQL Data Warehouse (API REST)

> [AZURE.SELECTOR]
- [Panoramica][]
- [Portale][]
- [PowerShell][]
- [REST][]

Questo articolo illustra come ripristinare un'istanza di Azure SQL Data Warehouse usando l'API REST.

## Prima di iniziare

**Verificare la capacità in DTU.** Ogni istanza di SQL Data Warehouse è ospitata da un server logico SQL, il cui limite di capacità è misurato in DTU. Prima di ripristinare un'istanza di SQL Data Warehouse, è importante assicurarsi che il server logico SQL che ospita il database abbia una capacità in DTU sufficiente per il database da ripristinare. Per altre informazioni su [come visualizzare e aumentare la quota DTU][], vedere il relativo post di blog.

## Ripristinare un database attivo o sospeso

Per ripristinare un database:

1. Ottenere l'elenco dei punti di ripristino del database utilizzando l'operazione Get Database Restore Points.
2. Iniziare il ripristino utilizzando l'operazione [Create database restore request][]
3. Monitorare lo stato del ripristino mediante l'operazione [Database Operation Status][].

>[AZURE.NOTE] Al termine del ripristino sarà possibile configurare il database ripristinato seguendo la guida disponibile in [Finalizzare il database SQL di Azure ripristinato][].

## Ripristino di un database eliminato

Per ripristinare un database eliminato:

1.	Elencare tutti i database eliminati ripristinabili mediante l'operazione [List Restorable Dropped Databases][].
2.	Ottenere i dettagli del database eliminato che si desidera ripristinare mediante l'operazione [Get Restorable Dropped Database][].
3.	Iniziare il ripristino utilizzando l'operazione [Create database restore request][].
4.	Monitorare lo stato del ripristino mediante l'operazione [Database Operation Status][].

>[AZURE.NOTE] Al termine del ripristino sarà possibile configurare il database ripristinato seguendo la guida disponibile in [Finalizzare il database SQL di Azure ripristinato][].


## Passaggi successivi
Per altre informazioni sulle funzionalità di continuità aziendale delle edizioni del database SQL di Azure, vedere [Panoramica: Continuità aziendale del cloud e ripristino di emergenza del database con database SQL][].

<!--Image references-->

<!--Article references-->
[Panoramica: Continuità aziendale del cloud e ripristino di emergenza del database con database SQL]: ./sql-database-business-continuity.md
[Finalizzare il database SQL di Azure ripristinato]: ./sql-database-recovered-finalize.md
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Panoramica]: ./sql-data-warehouse-restore-database-overview.md
[Portale]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx


<!--Blog references-->
[come visualizzare e aumentare la quota DTU]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

<!---HONumber=AcomDC_0629_2016-->