<properties
   pageTitle="Migrare un database SQL Server nel database SQL di Azure | Microsoft Azure"
   description="Database SQL di Microsoft Azure, distribuzione del database, migrazione del database, importazione del database, esportazione del database, migrazione guidata"
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

# Importare da BACPAC al database SQL tramite SSMS

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Portale di Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In questo articolo viene illustrato come importare da un file [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) a un database SQL mediante la procedura guidata di esportazione dell’applicazione livello dati in SQL Server Management Studio.

> [AZURE.NOTE] Il passaggio precedente presuppone che sia già stato effettuato il provisioning dell'istanza logica del database SQL di Azure e che le informazioni di connessione siano disponibili.

1. Verificare di essere in possesso della versione più recente di SQL Server Management Studio. Le nuove versioni di Management Studio vengono aggiornate ogni mese per rimanere sincronizzate con gli aggiornamenti apportati al portale di Azure.

	 > [AZURE.IMPORTANT] È consigliabile usare sempre la versione più aggiornata di Management Studio per restare sincronizzati con gli aggiornamenti di Microsoft Azure e del database SQL. [Aggiornare SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Connettersi al server del database SQL di Azure, fare clic con il pulsante destro del mouse sulla cartella **Database** e quindi scegliere **Importa applicazione livello dati...**

    ![Importare la voce di menu dell’applicazione livello dati](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.	Nell'importazione guidata, importare il file BACPAC dal disco locale o selezionare l'account di archiviazione di Azure e il contenitore in cui è stato caricato il file BACPAC per creare il nuovo database nel database SQL di Azure.

    ![Importare impostazioni](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

    ***Importante***: quando si importa un file BACPAC dall'archivio BLOB di Azure, usare l'archiviazione standard. L'importazione di un file BACPAC dall'archiviazione Premium non è supportata.

4.	Specificare il **Nome nuovo database** nel database SQL di Azure, impostare **Edizione del database SQL di Microsoft Azure** (livello del servizio), **Dimensioni massime database** e **Obiettivo servizio** (livello di prestazioni).

    ![Impostazioni database](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.	Fare clic su **Avanti** e quindi su **Fine** per importare il file BACPAC in un nuovo database nel server del database SQL di Azure.

6. Usando Esplora oggetti, connettersi al database sottoposto a migrazione nel server del database SQL di Azure.

6.	Usando il portale di Azure visualizzare il database e le relative proprietà.

## Passaggi successivi

- [Scaricare la versione più recente di SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx)
- [Scaricare SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

## Risorse aggiuntive

- [Novità della versione 12 del database SQL](sql-database-v12-whats-new.md)
- [Transact-SQL partially or unsupported functions](sql-database-transact-sql-information.md) (Funzionalità di Transact-SQL parzialmente supportate o non supportate)
- [Migrate non-SQL Server databases using SQL Server Migration Assistant (Eseguire la migrazione di database non SQL Server mediante SQL Server Migration Assistant)](http://blogs.msdn.com/b/ssma/)

<!---HONumber=AcomDC_0615_2016-->