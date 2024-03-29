<properties
   pageTitle="Creare un'istanza di SQL Data Warehouse con TSQL | Microsoft Azure"
   description="Informazioni su come creare un'istanza di Azure SQL Data Warehouse con TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Creare un database di SQL Data Warehouse usando Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Portale di Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Questo articolo descrive come creare un database di SQL Data Warehouse con Transact-SQL (T-SQL).

## Prerequisiti
Prima di iniziare la distribuzione, verificare che siano soddisfatti i prerequisiti seguenti.

- **Account Azure**: vedere [Versione di valutazione gratuita][] o [Credito Azure mensile per sottoscrittori di Visual Studio][] per creare un account.
- **Server SQL Azure V12**: vedere [Creare un server logico del database SQL di Azure con il portale di Azure][] o [Creare un server logico del database SQL di Azure con PowerShell][].
- **Nome gruppo di risorse**: usare lo stesso gruppo di risorse del server SQL Azure versione 12 o vedere [Gruppi di risorse][] per creare un nuovo gruppo di risorse.
- **Visual Studio con SQL Server Data Tools**: per istruzioni di installazione, vedere [Installare Visual Studio 2015 e SSDT per SQL Data Warehouse][].

> [AZURE.NOTE] La creazione di un nuovo database di SQL Data Warehouse può avere come risultato un nuovo servizio fatturabile. Per informazioni dettagliate sui prezzi, vedere [Prezzi di SQL Data Warehouse][].

## Creare un database con Visual Studio

Se non si ha familiarità con Visual Studio, vedere l'articolo [Connettersi a SQL Data Warehouse con Visual Studio][]. Per iniziare, aprire Esplora oggetti di SQL Server in Visual Studio e connettersi al server che ospiterà il database di SQL Data Warehouse. Dopo aver stabilito la connessione, è possibile creare un database di SQL Data Warehouse eseguendo il comando SQL seguente sul database **master**. Questo comando crea il database MySqlDwDb con un obiettivo di servizio DW400 e permette un aumento delle dimensioni del database fino al limite massimo di 10 TB.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Creare un database con sqlcmd

In alternativa, è possibile usare lo stesso comando con sqlcmd eseguendo quanto segue al prompt dei comandi.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Il parametro `MAXSIZE` può essere compreso tra 250 GB e 240 TB. `SERVICE_OBJECTIVE` può essere compreso tra DW100 e DW2000 [DWU][]. Per un elenco di tutti i valori validi, vedere la documentazione in MSDN relativa [CREATE DATABASE][]. MAXSIZE e SERVICE\_OBJECTIVE possono entrambi essere modificati anche con un comando T-SQL [ALTER DATABASE][]. Prestare attenzione quando si modifica il parametro SERVICE\_OBJECTIVE, perché questa operazione provoca un riavvio dei servizi e il conseguente annullamento delle query in corso. La modifica di MAXSIZE non riavvia i servizi, perché si tratta di una semplice operazione sui metadati.

## Passaggi successivi
Al termine del provisioning di SQL Data Warehouse, è possibile [caricare dati di esempio][] o vedere come eseguire le attività di [sviluppo][], [caricamento][] o [migrazione][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: ./sql-data-warehouse-get-started-provision.md
[Connettersi a SQL Data Warehouse con Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[migrazione]: ./sql-data-warehouse-overview-migrate.md
[sviluppo]: ./sql-data-warehouse-overview-develop.md
[caricamento]: ./sql-data-warehouse-overview-load.md
[caricare dati di esempio]: ./sql-data-warehouse-get-started-load-sample-databases.md
[Creare un server logico del database SQL di Azure con il portale di Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Creare un server logico del database SQL di Azure con PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Gruppi di risorse]: ../azure-portal/resource-group-portal.md
[Installare Visual Studio 2015 e SSDT per SQL Data Warehouse]: ./sql-data-warehouse-install-visual-studio.md


<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Prezzi di SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Versione di valutazione gratuita]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Credito Azure mensile per sottoscrittori di Visual Studio]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

<!---HONumber=AcomDC_0706_2016-->