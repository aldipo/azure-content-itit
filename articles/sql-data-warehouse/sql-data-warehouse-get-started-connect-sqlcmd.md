<properties
   pageTitle="Eseguire query in Azure SQL Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Eseguire query in Azure SQL Data Warehouse con l'utilità della riga di comando sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Eseguire query in Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Questa procedura dettagliata usa l'utilità della riga di comando sqlcmd per eseguire query in un'istanza di Azure SQL Data Warehouse.

## Prerequisiti

Per eseguire questa esercitazione, è necessario:

-  [sqlcmd.exe][]. Per scaricarlo, vedere [Microsoft Command Line Utilities 11 for SQL Server][].

## 1\. Connetti

Per iniziare a usare sqlcmd, aprire il prompt dei comandi e immettere **sqlcmd** seguito dalla stringa di connessione per il database di SQL Data Warehouse. La stringa di connessione richiede i parametri obbligatori seguenti:

+ **Server (-S):** server nel formato `<`Server Name`>`.database.windows.net
+ **Database (-d):** nome del database.
+ **Utente (-U):** utente del server nel formato `<`User`>`
+ **Password (-P):** password associata all'utente.
+ **Abilita identificatori delimitati (-I):** gli identificatori delimitati devono essere abilitati per consentire la connessione a un'istanza di SQL Data Warehouse.

Ad esempio, la stringa di connessione sarà simile alla seguente:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

> [AZURE.NOTE] L'opzione -I, che abilita gli identificatori delimitati, attualmente è obbligatoria per connettersi a SQL Data Warehouse.

## 2\. Query

Dopo la connessione sarà possibile eseguire qualsiasi istruzione Transact-SQL supportata nell'istanza. In questo esempio le query vengono inviate in modalità interattiva.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Questi esempi successivi illustrano come è possibile eseguire le query in modalità batch usando l'opzione -Q o inviando pipe di SQL a sqlcmd.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
C:\>"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## Passaggi successivi

Per informazioni su tutte le opzioni sqlcmd, vedere la [documentazione su sqlcmd][sqlcmd.exe].

<!--Articles-->
[connecting with PowerBI]: sql-data-warehouse-integrate-power-bi.md


<!--Other-->
[sqlcmd.exe]: https://msdn.microsoft.com/library/ms162773.aspx
[Microsoft Command Line Utilities 11 for SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[Azure portal]: https://portal.azure.com

<!--Image references-->

<!---HONumber=AcomDC_0622_2016-->