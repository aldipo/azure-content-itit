<properties 
	pageTitle="Attività di stored procedure di SQL Server" 
	description="Informazioni sull'uso dell'attività di stored procedure di SQL Server da una pipeline di Data factory per richiamare una stored procedure in un database SQL di Azure o in Azure SQL Data Warehouse." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/27/2016" 
	ms.author="spelluru"/>

# Attività di stored procedure di SQL Server

È possibile usare l'attività di stored procedure di SQL Server in una [pipeline](data-factory-create-pipelines.md) di Data factory per richiamare una stored procedure in uno degli archivi dati seguenti.


- Database SQL di Azure
- Azure SQL Data Warehouse
- Database SQL Server in una VM di Azure o dell'azienda. È necessario installare Gateway di gestione dati nello stesso computer che ospita il database o in un computer separato per evitare che competa per le risorse con il database. Gateway di gestione dati è un software che connette le origini dati locali o ospitate in macchine virtuali di Azure ai servizi cloud, in modo sicuro e gestito. Vedere l’articolo [Spostare dati tra cloud e locale](data-factory-move-data-between-onprem-and-cloud.md) per informazioni dettagliate sui Gateway di Gestione dati.

Questo articolo si basa sull'articolo relativo alle [attività di trasformazione dei dati](data-factory-data-transformation-activities.md) che presenta una panoramica generale della trasformazione dei dati e le attività di trasformazione supportate.

## Sintassi
	{
    	"name": "SQLSPROCActivity",
    	"description": "description", 
    	"type": "SqlServerStoredProcedure",
    	"inputs":  [ { "name": "inputtable"  } ],
    	"outputs":  [ { "name": "outputtable" } ],
    	"typeProperties":
    	{
        	"storedProcedureName": "<name of the stored procedure>",
        	"storedProcedureParameters":  
        	{
				"param1": "param1Value"
				…
        	}
    	}
	}

## Dettagli sintassi

Proprietà | Descrizione | Obbligatorio
-------- | ----------- | --------
name | Nome dell'attività | Sì
description | Testo descrittivo per lo scopo dell'attività | No
type | SqlServerStoredProcedure | Sì
inputs | Set di dati di input che devono essere disponibili (in stato 'Ready') per l'esecuzione dell'attività di stored procedure da eseguire. Gli input per l'attività di stored procedure vengono usati solo per la gestione delle dipendenze nel concatenamento di questa attività con le altre. I set di dati di input non possono essere utilizzati nella stored procedure come parametro | No
outputs | Set di dati di output generati dall'attività di stored procedure. Assicurarsi che la tabella di output usi un servizio collegato che collega un database SQL di Azure, un Azure SQL Data Warehouse o un database SQL Server alla data factory. Gli output nell'attività di stored procedure possono essere utilizzati come un metodo per trasferire il risultato dell'attività di stored procedure per la successiva elaborazione e/o possono essere utilizzati per la gestione delle dipendenze quando si concatena questa attività con le altre | Sì
storedProcedureName | Specificare il nome della stored procedure nel database SQL di Azure o Azure SQL Data Warehouse rappresentato dal servizio collegato che usa la tabella di output. | Sì
storedProcedureParameters | Specificare i valori dei parametri della stored procedure | No

## Procedura dettagliata di esempio

### Tabella di esempio e stored procedure
> [AZURE.NOTE] Questo esempio usa il database SQL di Azure, ma funziona nello stesso modo per Azure SQL Data Warehouse e il database di SQL Server.

1. Creare la seguente **tabella** nel database SQL di Azure usando SQL Server Management Studio o qualsiasi altro strumento conosciuto. La colonna datetimestamp riporta la data e l'ora in cui viene generato l'ID corrispondente.

		CREATE TABLE dbo.sampletable
		(
			Id uniqueidentifier,
			datetimestamp nvarchar(127)
		)
		GO

		CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
		GO

	La colonna ID riporta l'identificatore univoco, mentre la colonna datetimestamp riporta la data e l'ora in cui viene generato l'ID corrispondente. ![Dati di esempio](./media/data-factory-stored-proc-activity/sample-data.png)

2. Creare la seguente **stored procedure** che inserisce dati nel **sampletable**.

		CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
		AS
		
		BEGIN
		    INSERT INTO [sampletable]
		    VALUES (newid(), @DateTime)
		END

	> [AZURE.IMPORTANT] Il **nome** e la **combinazione di maiuscole e minuscole** per il parametro (DateTime in questo esempio) devono corrispondere a quelli del parametro specificato nel codice JSON per pipeline/attività. Nella definizione della stored procedure assicurarsi che **@** sia usato come prefisso per il parametro.
	
### Creare un'istanza di Data factory  
4. Dopo l'accesso al [portale di Azure](https://portal.azure.com/), seguire questa procedura:
	1.	Fare clic su **NUOVO** nel menu a sinistra.
	2.	Fare clic su **Analisi dei dati** nel pannello **Crea**.
	3.	Fare clic su **Data factory** nel pannello **Analisi dei dati**.
4.	Nel pannello **Nuova data factory** immettere **SProcDF** come nome. I nomi di Data factory di Azure sono univoci. È necessario anteporre al nome della data factory il proprio nome, per consentire la corretta creazione della factory.
3.	Se non è stato creato un gruppo di risorse, sarà necessario crearne uno. A tale scopo, seguire questa procedura:
	1.	Fare clic su **NOME DEL GRUPPO DI RISORSE**.
	2.	Selezionare **Crea un nuovo gruppo di risorse** nel pannello **Gruppo di risorse**.
	3.	Immettere **ADFTutorialResourceGroup** nel campo **Nome** nel pannello **Crea gruppo di risorse**.
	4.	Fare clic su **OK**.
4.	Dopo aver selezionato il gruppo di risorse, verificare se la sottoscrizione in cui si vuole creare la data factory è quella corretta.
5.	Fare clic su **Crea** nel pannello **Nuova data factory**.
6.	Nella **Schermata iniziale** del portale di Azure verrà visualizzata la data factory in fase di creazione. Dopo aver creato la data factory, verrà visualizzata la pagina corrispondente con elencato il contenuto della data factory.

### Creare un servizio collegato SQL di Azure  
Dopo aver creato la data factory, si crea un servizio collegato SQL di Azure che collega il database SQL di Azure alla data factory. Si tratta del database che contiene la tabella sampletable e la stored procedure sp\_sample.

7.	Fare clic su **Crea e distribuisci** nel pannello **DATA FACTORY** per **SProcDF**. Verrà avviato l'editor di Data factory.
2.	Fare clic su **Nuovo archivio dati** sul barra dei comandi e scegliere **Azure SQL**. Nell'editor verrà visualizzato lo script JSON per la creazione di un servizio collegato SQL di Azure.
4. Sostituire **servername** con il nome del server di database SQL di Azure, **databasename** con il database in cui sono state create la tabella e la stored procedure, **username@servername** con l'account utente che ha accesso al database e **password** con la password per l'account utente.
5. Fare clic su **Distribuisci** sulla barra dei comandi per distribuire il servizio collegato.

### Creare un set di dati di output
6. Fare clic su **Nuovo set di dati** sulla barra dei comandi e selezionare **Azure SQL**.
7. Copiare e incollare il seguente script JSON nell'editor JSON.

		{			    
			"name": "sprocsampleout",
			"properties": {
				"type": "AzureSqlTable",
				"linkedServiceName": "AzureSqlLinkedService",
				"typeProperties": {
					"tableName": "sampletable"
				},
				"availability": {
					"frequency": "Hour",
					"interval": 1
				}
			}
		}
7. Fare clic su **Distribuisci** sulla barra dei comandi per distribuire il set di dati.

### Creare una pipeline con l'attività SqlServerStoredProcedure
Si crea ora una pipeline con un'attività SqlServerStoredProcedure.
 
9. Fare clic su **... (puntini di sospensione)** sulla barra dei comandi e quindi su **Nuova pipeline**.
9. Copiare e incollare il frammento JSON seguente. Impostare **storedProcedureName** su **sp\_sample**. Il nome e la combinazione di maiuscole e minuscole del parametro **DateTime** deve corrispondere al nome e alla combinazione di maiuscole e minuscole del parametro nella definizione della stored procedure.

		{
		    "name": "SprocActivitySamplePipeline",
		    "properties": {
		        "activities": [
		            {
		                "type": "SqlServerStoredProcedure",
		                "typeProperties": {
		                    "storedProcedureName": "sp_sample",
		                    "storedProcedureParameters": {
		                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
		                    }
		                },
		                "outputs": [
		                    {
		                        "name": "sprocsampleout"
		                    }
		                ],
		                "scheduler": {
		                    "frequency": "Hour",
		                    "interval": 1
		                },
		                "name": "SprocActivitySample"
		            }
		        ],
		        "start": "2015-01-02T00:00:00Z",
		        "end": "2015-01-03T00:00:00Z",
		        "isPaused": false
		    }
		}
9. Fare clic su **Distribuisci** sulla barra degli strumenti per distribuire la pipeline.

### Monitorare la pipeline

6. Fare clic su **X** per chiudere i pannelli dell'editor di Data factory e per tornare al pannello Data factory e quindi fare clic su **Diagramma**.
7. In Vista diagramma saranno visualizzati una panoramica della pipeline e i set di dati usati in questa esercitazione.
8. In Vista diagramma fare doppio clic sul set di dati **sprocsampleout**. Verranno visualizzate le sezioni con stato pronto. Dovrebbe essere presenti 24 sezioni perché viene generata una sezione ogni ora tra 2015/01/02 e 2015/01/03.
10. Quando lo stato di una sezione è **Pronto**, eseguire una query *select * from sampledata* sul database SQL di Azure per verificare che la stored procedure abbia inserito i dati nella tabella.

	![Dati di output](./media/data-factory-stored-proc-activity/output.png)

	Vedere [Monitorare la pipeline](data-factory-monitor-manage-pipelines.md) per informazioni dettagliate sul monitoraggio delle pipeline di Data Factory di Azure.

> [AZURE.NOTE] Nell'esempio precedente, l'attività SprocActivitySample non contiene dati di input. Se si desidera concatenarla con un'attività upstream (ad esempio elaborazione precedente), è possibile usare gli output dell'attività upstream come input di questa attività. In questo caso, l'attività non verrà eseguita finché l'attività upstream non sarà completata e gli output delle attività upstream non saranno disponibili (in stato Ready). Non è possibile usare gli input direttamente come parametri dell'attività di stored procedure.

## Passaggio di un valore statico 
A questo punto, si consideri l'aggiunta di un'altra colonna denominata 'Scenario' nella tabella contenente un valore statico denominato 'Document sample'.

![Dati di esempio 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

	CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
	
	AS
	
	BEGIN
	    INSERT INTO [sampletable]
	    VALUES (newid(), @DateTime, @Scenario)
	END

A tale scopo, passare il parametro di Scenario e il valore dall'attività di stored procedure. La sezione typeProperties nell'esempio precedente è simile alla seguente:

	"typeProperties":
	{
		"storedProcedureName": "sp_sample",
	    "storedProcedureParameters": 
	    {
	    	"DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
			"Scenario": "Document sample"
		}
	}

<!---HONumber=AcomDC_0629_2016-->