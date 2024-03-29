<properties 
	pageTitle="Gestire un Database SQL con SQL Server Management Studio | Microsoft Azure" 
	description="Informazioni su come usare SQL Server Management Studio per gestire database e server di database SQL." 
	services="sql-database" 
	documentationCenter=".net" 
	authors="stevestein" 
	manager="jhubbard" 
	editor="tysonn"/>

<tags 
	ms.service="sql-database" 
	ms.workload="data-management" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/09/2016" 
	ms.author="sstein"/>


# Gestione di database SQL di Azure tramite SQL Server Management Studio 


> [AZURE.SELECTOR]
- [Portale di Azure](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-command-line-tools.md)

È possibile utilizzare SQL Server Management Studio (SSMS) per amministrare i server logici e i database del database SQL di Azure. In questo argomento vengono illustrate le attività comuni con SSMS. Prima di iniziare, è necessario disporre già di un server logico e di un database creati nel database SQL di Azure. Per informazioni su come connettersi ed eseguire una query SELECT semplice, vedere gli articoli relativi alla [creazione del primo database SQL Azure](sql-database-get-started.md) e alla [connessione ed esecuzione di query con SSMS](sql-database-connect-query-ssms.md).

Quando si lavora con il database SQL di Azure, si consiglia di utilizzare la versione più recente di SSMS.

> [AZURE.IMPORTANT] Usare sempre la versione più aggiornata di SQL Server Management Studio (SSMS) per restare sincronizzati con gli aggiornamenti di Microsoft Azure e del database SQL. Le versioni precedenti di SQL Server Management Studio non funzionano correttamente con il database SQL. Visitare [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (Scaricare SQL Server Management Studio) per ottenere una nuova versione.

## Creazione e gestione dei database SQL di Azure

Durante la connessione al database **master**, è possibile creare nuovi database nel server e modificare o eliminare database esistenti. Nei seguenti passaggi viene descritto come completare diverse attività comuni di gestione del database tramite Management Studio. Per eseguire queste attività, assicurarsi di essere connessi al database **master** con l'account di accesso dell'entità di livello server creato al momento della configurazione del server.

Per aprire una finestra Query in Management Studio, aprire la cartella Database, espandere la cartella **Database di sistema** fare clic con il pulsante destro del mouse su **master**, e quindi fare clic su **Nuova query**.

-   Per creare un nuovo database, utilizzare l'istruzione **CREATE DATABASE**. Per altre informazioni, vedere [CREATE DATABASE (database SQL)](https://msdn.microsoft.com/library/dn268335.aspx). La seguente istruzione consente di creare un nuovo database denominato **myTestDB** specificando che si tratta di un database Standard S0 Edition con una dimensione massima di 250 GB.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Fare clic su **Execute** per eseguire la query.

-   Usare l'istruzione **ALTER DATABASE** per modificare un database esistente, ad esempio se si desidera modificarne il nome e l'edizione. Per altre informazioni, vedere [ALTER DATABASE (database SQL)](https://msdn.microsoft.com/library/ms174269.aspx). La seguente istruzione consente di modificare il database creato nel passaggio precedente per modificare l’edizione in Standard S1.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Per eliminare un database esistente, utilizzare l'istruzione **DROP DATABASE**. Per altre informazioni, vedere [DROP DATABASE (database SQL)](https://msdn.microsoft.com/library/ms178613.aspx). L'istruzione seguente consente di eliminare il database **myTestDB**, ma non eliminarlo ora perché sarà necessario utilizzarlo per creare account di accesso nel prossimo passaggio.

        DROP DATABASE myTestBase;

-   Il database master presenta la vista **sys.databases** che può essere utilizzata per visualizzare i dettagli di tutti i database. Per visualizzare tutti i database esistenti, eseguire la seguente istruzione:

        SELECT * FROM sys.databases;

-   Nel database SQL l'istruzione **USE** non è supportata per passare da un database a un altro. È necessario stabilire invece una connessione diretta al database di destinazione.

>[AZURE.NOTE] Molte delle istruzioni Transact-SQL che consentono di creare o modificare un database devono essere eseguite nell'ambito del proprio batch e non possono essere raggruppate insieme ad altre istruzioni Transact-SQL. Per altre informazioni, vedere i dettagli specifici di ogni istruzione forniti nei collegamenti elencati sopra.

## Creare e gestire gli account di accesso

Il database **master** tiene traccia degli account di accesso riconoscendo quelli che dispongono delle autorizzazioni per creare database o ulteriori account di accesso. Per gestire gli account di accesso, connettersi al database **master** con l'account di accesso dell'entità di livello server creato al momento della configurazione del server. È possibile utilizzare le istruzioni **CREATE LOGIN**, **ALTER LOGIN** o **DROP LOGIN** per eseguire query sul database master che gestirà gli account di accesso per l'intero server. Per altre informazioni, vedere [Gestione di database e account di accesso in database SQL](http://msdn.microsoft.com/library/azure/ee336235.aspx).


-   Utilizzare l'istruzione **CREATE LOGIN** per creare un nuovo account di accesso di livello server. Per altre informazioni, vedere [CREATE LOGIN (database SQL)](https://msdn.microsoft.com/library/ms189751.aspx). L'istruzione seguente consente di creare un nuovo account di accesso denominato **login1**. Sostituire **password1** con la password desiderata.

        CREATE LOGIN login1 WITH password='password1';

-   Per concedere autorizzazioni di livello database, utilizzare l'istruzione **CREATE USER**. Tutti gli account di accesso devono essere creati nel database **master** ma per connettere un account di accesso a un database diverso, è necessario concedere a tale account autorizzazioni di livello database utilizzando l'istruzione **CREATE USER** nel database a cui si desidera connetterlo. Per altre informazioni, vedere [CREATE USER (database SQL)](https://msdn.microsoft.com/library/ms173463.aspx).

-   Per concedere a login1 le autorizzazioni per un database denominato **myTestDB**, eseguire i passaggi seguenti:

 1.  Per aggiornare Esplora oggetti in modo da visualizzare il database **myTestDB** creato, fare clic con il pulsante destro del mouse sul nome del server in Esplora oggetti e quindi scegliere **Aggiorna**.  

     Se si è chiusa la connessione, è possibile ristabilirla selezionando **Connect Object Explorer** dal menu File.

 2. Fare clic con il pulsante destro del mouse sul database **myTestDB** e selezionare **New Query**.

    3.  Eseguire l'istruzione seguente sul database myTestDB per creare un utente database denominato **login1User** che corrisponde all'account di accesso a livello di server **login1**.

            CREATE USER login1User FROM LOGIN login1;

-   Utilizzare la stored procedure **sp\_addrolemember** per concedere all'account utente il livello appropriato di autorizzazioni per il database. Per altre informazioni, vedere [sp\_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx). L'istruzione seguente concede a **login1User** autorizzazioni di sola lettura del database mediante l'aggiunta di **login1User** al ruolo **db\_datareader**.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Per modificare un account di accesso esistente, ad esempio se si desidera modificarne la password, utilizzare l'istruzione **ALTER LOGIN**. Per altre informazioni, vedere [ALTER LOGIN (database SQL)](https://msdn.microsoft.com/library/ms189828.aspx). L'istruzione **ALTER LOGIN** deve essere eseguita sul database **master**. Tornare alla finestra Query relativa a tale database.

    L'istruzione seguente consente di modificare l'account di accesso **login1** per reimpostare la password. Sostituire **newPassword** con la password desiderata e **oldPassword** con la password attuale dell'account.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Per eliminare un account di accesso esistente, utilizzare l'istruzione **DROP LOGIN**. L'eliminazione di un account di accesso a livello del server comporta anche l'eliminazione di eventuali account utente database associati. Per altre informazioni, vedere [DROP DATABASE (database SQL)](https://msdn.microsoft.com/library/ms178613.aspx). L'istruzione **DROP LOGIN** deve essere eseguita sul database **master**. L'istruzione seguente consente di eliminare l'account di accesso **login1**.

        DROP LOGIN login1;

-   Il database master presenta la vista **sys.sql\_logins** che può essere utilizzata per visualizzare gli account di accesso. Per visualizzare tutti gli account di accesso esistenti, eseguire la seguente istruzione:

        SELECT * FROM sys.sql_logins;

## Monitorare il database SQL usando viste a gestione dinamica</h2>

Il database SQL supporta diverse viste a gestione dinamica che possono essere usate per il monitoraggio di database specifici. Di seguito vengono forniti alcuni esempi del tipo di dati di monitoraggio che possono essere recuperati tramite tali viste. Per informazioni dettagliate e altri esempi d'uso, vedere [Monitoraggio di database SQL di Azure mediante le viste a gestione dinamica](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   L'esecuzione di query in una vista a gestione dinamica richiede autorizzazioni **VIEW DATABASE STATE**. Per concedere le autorizzazioni **VIEW DATABASE STATE** a un utente database specifico, connettersi al database che si desidera gestire utilizzando l'account di accesso dell'entità di livello server ed eseguire l'istruzione seguente sul database:

        GRANT VIEW DATABASE STATE TO login1User;

-   Utilizzare la vista **sys.dm\_db\_partition\_stats** per calcolare la dimensione del database. La vista **sys.dm\_db\_partition\_stats** restituisce informazioni sulle pagine e sul numero di righe per ogni partizione del database, che possono essere utilizzate per calcolare la dimensione del database. La seguente query restituisce la dimensione del database in megabyte:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Utilizzare le viste **sys.dm\_exec\_connections** e **sys.dm\_exec\_sessions** per recuperare informazioni sulle attuali connessioni degli utenti e sulle attività interne relative al database. La seguente query restituisce informazioni relative alla connessione corrente:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Utilizzare la vista **sys.dm\_exec\_query\_stats** per recuperare statistiche aggregate sulle prestazioni per piani di query nella cache. La seguente query restituisce informazioni sulle cinque principali query classificate per tempo medio CPU.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 

<!---HONumber=AcomDC_0518_2016-->