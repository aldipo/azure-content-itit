<properties
   pageTitle="Risolvere i problemi di compatibilità del server SQL, utilizzando SQL Server Managment Studio prima di effettuare la migrazione al database SQL | Microsoft Azure"
   description="Database SQL di Microsoft Azure, migrazione del database, compatibilità, migrazione guidata di SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="06/07/2016"
   ms.author="carlrab"/>

# Risolvere i problemi di compatibilità del database del server SQL, usando SQL Server Management Studio, prima di effettuare la migrazione al database SQL

> [AZURE.SELECTOR]
- Usare [Migrazione guidata SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Usare [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Usare [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Gli utenti esperti possono risolvere i problemi di compatibilità del database di SQL Server usando SQL Server Management Studio prima di eseguire la migrazione al database SQL di Azure.

## Utilizzare SQL Server Management Studio

Utilizzare SQL Server Management Studio per risolvere i problemi di compatibilità usando alcuni comandi Transact-SQL, ad esempio **ALTER DATABASE**. Questo metodo è destinato principalmente a utenti esperti che non hanno difficoltà a utilizzare Transact-SQL in un database attivo. In caso contrario, è consigliabile usare SSDT.



## Passaggi successivi

- [Scaricare la versione più recente di SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx)
- [Scaricare SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
- [Eseguire la migrazione di un database di SQL Server compatibile nel database SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## Risorse aggiuntive

- [Novità della versione 12 del database SQL](sql-database-v12-whats-new.md)
- [Transact-SQL partially or unsupported functions](sql-database-transact-sql-information.md) (Funzionalità di Transact-SQL parzialmente supportate o non supportate)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/) (Eseguire la migrazione di database non SQL Server mediante SQL Server Migration Assistant)

<!---HONumber=AcomDC_0615_2016-->