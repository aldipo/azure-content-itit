<properties
   pageTitle="Creare SQL Data Warehouse mediante PowerShell | Microsoft Azure"
   description="Creare SQL Data Warehouse mediante PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Creare SQL Data Warehouse con PowerShell

> [AZURE.SELECTOR]
- [Portale di Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

## Prerequisiti
Prima di iniziare la distribuzione, verificare che siano soddisfatti i prerequisiti seguenti.

- **Account Azure**: vedere [Versione di valutazione gratuita][] o [Credito Azure mensile per sottoscrittori di Visual Studio][] per creare un account.
- **Server SQL Azure V12**: vedere [Creare un server logico del database SQL di Azure con il portale di Azure][] o [Creare un server logico del database SQL di Azure con PowerShell][].
- **Nome gruppo di risorse**: usare lo stesso gruppo di risorse del server SQL Azure versione 12 o vedere [Gruppi di risorse][] per creare un nuovo gruppo di risorse.
- **PowerShell versione 1.0.3 o successiva**: è possibile controllare la versione in uso eseguendo **Get-Module -ListAvailable -Name Azure**. È possibile installare la versione più recente usando [Installazione guidata piattaforma Web Microsoft][]. Per altre informazioni sull'installazione della versione più recente, vedere [Come installare e configurare Azure PowerShell][].

> [AZURE.NOTE] La creazione di un nuovo database di SQL Data Warehouse può avere come risultato un nuovo servizio fatturabile. Per informazioni dettagliate sui prezzi, vedere [Prezzi di SQL Data Warehouse][].

## Creare un database di SQL Data Warehouse
1. Aprire Windows PowerShell.
2. Eseguire questo cmdlet per accedere a Gestione risorse di Azure.

	```Powershell
	Login-AzureRmAccount
	```
	
3. Selezionare la sottoscrizione da usare per la sessione corrente.

	```Powershell
	Get-AzureRmSubscription	-SubscriptionName "MySubscription" | Select-AzureRmSubscription
	```

4.  Creare il database. Questo esempio crea un nuovo database denominato "mynewsqldw", con un livello di obiettivo di servizio "DW400" nel server denominato "sqldwserver1" incluso nel gruppo di risorse "mywesteuroperesgp1".

	```Powershell
	New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse"
	```

I parametri necessari per il cmdlet sono i seguenti:

- **RequestedServiceObjectiveName**: quantità di [DWU][] richiesta. I valori supportati sono: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500 e DW2000.
- **DatabaseName**: il nome dell'istanza di SQL Data Warehouse che si sta creando.
- **ServerName**: il nome del server che si sta usando per la creazione (deve essere V12).
- **ResourceGroupName**: il gruppo di risorse in uso. Per ottenere i gruppi di risorse disponibili nella sottoscrizione, usare Get-AzureResource.
- **Edition**: per creare un'istanza di SQL Data Warehouse, è necessario impostare l'edizione su "DataWarehouse".

Per informazioni dettagliate sulle opzioni dei parametri, vedere [CREATE DATABASE (Azure SQL Data Warehouse)][]. Per informazioni di riferimento sui comandi, vedere [New-AzureRmSqlDatabase][]

## Passaggi successivi
Al termine del provisioning di SQL Data Warehouse, è possibile provare a [caricare dati di esempio][] o scoprire come eseguire le attività di [sviluppo][], [caricamento][] o [migrazione][].

Per altre informazioni su come gestire SQL Data Warehouse a livello di codice, vedere l'articolo su come [Usare i cmdlet di PowerShell e le API REST con SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migrazione]: ./sql-data-warehouse-overview-migrate.md
[sviluppo]: ./sql-data-warehouse-overview-develop.md
[caricamento]: ./sql-data-warehouse-load-with-bcp.md
[caricare dati di esempio]: ./sql-data-warehouse-get-started-load-sample-databases.md
[Usare i cmdlet di PowerShell e le API REST con SQL Data Warehouse]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Come installare e configurare Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Creare un server logico del database SQL di Azure con il portale di Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Creare un server logico del database SQL di Azure con PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Gruppi di risorse]: ../azure-portal/resource-group-portal.md

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[CREATE DATABASE (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Installazione guidata piattaforma Web Microsoft]: https://aka.ms/webpi-azps
[Prezzi di SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Versione di valutazione gratuita]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Credito Azure mensile per sottoscrittori di Visual Studio]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

<!---HONumber=AcomDC_0706_2016-->