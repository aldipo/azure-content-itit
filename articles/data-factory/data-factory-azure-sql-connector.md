<properties 
	pageTitle="Spostare dati in/dal database SQL di Azure | Microsoft Azure" 
	description="Informazioni su come spostare i dati da e verso il database SQL di Azure mediante Data factory di Azure." 
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
	ms.date="04/18/2016" 
	ms.author="spelluru"/>

# Spostare dati da e nel database SQL di Azure con Data factory di Azure

Questo articolo illustra come usare l'attività di copia in una data factory di Azure per spostare dati in SQL Azure da un altro archivio dati e spostare dati da un altro archivio dati a SQL Azure. Questo articolo si basa sull'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md), che offre una panoramica generale dello spostamento dei dati con attività di copia e delle combinazioni di archivio dati supportate.

Gli esempi seguenti mostrano come copiare dati da e nel database SQL di Azure e nell'archivio BLOB di Azure. Tuttavia, i dati possono essere copiati **direttamente** da una delle origini in qualsiasi sink dichiarato [qui](data-factory-data-movement-activities.md#supported-data-stores) usando l'attività di copia in Data factory di Azure.


## Esempio: Copiare i dati dal database SQL di Azure SQL nel BLOB di Azure

L'esempio seguente mostra:

1. Un servizio collegato di tipo [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Un [set di dati](data-factory-create-datasets.md) di input di tipo [AzureSqlTable](#azure-sql-dataset-type-properties).
4. Un [set di dati](data-factory-create-datasets.md) di output di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [SqlSource](#azure-sql-copy-activity-type-properties) e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

L'esempio copia dati appartenenti a una serie temporale da una tabella nel database SQL di Azure a un BLOB ogni ora. Le proprietà JSON usate in questi esempi sono descritte nelle sezioni riportate dopo gli esempi.

**Servizio collegato SQL di Azure**

	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

Vedere la sezione sul [servizio collegato SQL di Azure](#azure-sql-linked-service-properties) per l'elenco delle proprietà supportate da questo servizio collegato.

**Servizio collegato di archiviazione BLOB di Azure**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

Vedere l'articolo del [blog di Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) per l'elenco delle proprietà supportate da questo servizio collegato.

**Set di dati di input di SQL Azure**

L'esempio presuppone che sia stata creata una tabella "MyTable" in SQL Azure e che contenga una colonna denominata "timestampcolumn" per i dati di una serie temporale.

Impostando "external" su "true" e specificando i criteri externalData, si comunica al servizio Data factory di Azure che la tabella è esterna alla data factory e non è prodotta da un'attività al suo interno.

	{
	  "name": "AzureSqlInput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyTable"
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

Vedere la sezione sulle [proprietà del tipo di set di dati SQL di Azure](#azure-sql-dataset-type-properties) per l'elenco delle proprietà supportate da questo tipo di set di dati.

**Set di dati di output del BLOB di Azure**

I dati vengono scritti in un nuovo BLOB ogni ora (frequenza: ora, intervallo: 1). Il percorso della cartella per il BLOB viene valutato dinamicamente in base all'ora di inizio della sezione in fase di elaborazione. Il percorso della cartella usa le parti anno, mese, giorno e ora dell'ora di inizio.

	{
	  "name": "AzureBlobOutput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": "\t",
	        "rowDelimiter": "\n"
	      }
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

Vedere la sezione sulle [proprietà del tipo di set di dati BLOB di Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) per l'elenco delle proprietà supportate da questo tipo di set di dati.

**Pipeline con attività di copia**

La pipeline contiene un'attività di copia configurata per usare i set di dati di input e output precedenti. È programmata per essere eseguita ogni ora. Nella definizione JSON della pipeline, il tipo di **origine** è impostato su **SqlSource** e il tipo di **sink** è impostato su **BlobSink**. La query SQL specificata per la proprietà **SqlReaderQuery** consente di selezionare i dati da copiare nell'ultima ora.

	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2014-06-01T18:00:00",
	    "end":"2014-06-01T19:00:00",
	    "description":"pipeline for copy activity",
	    "activities":[  
	      {
	        "name": "AzureSQLtoBlob",
	        "description": "copy activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureSQLInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureBlobOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "SqlSource",
	            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
	          },
	          "sink": {
	            "type": "BlobSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	     ]
	   }
	}

Nell'esempio precedente, la proprietà **sqlReaderQuery** è specificata per SqlSource. L'attività di copia esegue questa query nell'origine del database SQL di Azure per ottenere i dati. In alternativa, è possibile specificare una stored procedure specificando i parametri **sqlReaderStoredProcedureName** e **storedProcedureParameters** (se la stored procedure accetta parametri).

Se non si specifica il parametro sqlReaderQuery o sqlReaderStoredProcedureName, le colonne definite nella sezione della struttura del set di dati JSON vengono usate per compilare una query (selezionare column1, column2 da mytable) da eseguire nel database SQL di Azure. Se la definizione del set di dati non dispone della struttura, vengono selezionate tutte le colonne della tabella.


Vedere la sezione [SqlSource](#sqlsource) e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) per l'elenco delle proprietà supportate da SqlSource e BlobSink.


## Esempio: Copiare i dati dal BLOB di Azure nel database SQL di Azure

L'esempio seguente mostra:

1.	Un servizio collegato di tipo [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.	Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.	Un [set di dati](data-factory-create-datasets.md) di input di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.	Un [set di dati](data-factory-create-datasets.md) di output di tipo [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.	Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) e [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

L'esempio copia dati appartenenti a una serie temporale dal BLOB di Azure a una tabella nel database SQL di Azure ogni ora. Le proprietà JSON usate in questi esempi sono descritte nelle sezioni riportate dopo gli esempi.


**Servizio collegato SQL di Azure**
	
	{
	  "name": "AzureSqlLinkedService",
	  "properties": {
	    "type": "AzureSqlDatabase",
	    "typeProperties": {
	      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
	    }
	  }
	}

Vedere la sezione sul [servizio collegato SQL di Azure](#azure-sql-linked-service-properties) per l'elenco delle proprietà supportate da questo servizio collegato.

**Servizio collegato di archiviazione BLOB di Azure**

	{
	  "name": "StorageLinkedService",
	  "properties": {
	    "type": "AzureStorage",
	    "typeProperties": {
	      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
	    }
	  }
	}

Vedere l'articolo del [blog di Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) per l'elenco delle proprietà supportate da questo servizio collegato.

**Set di dati di input del BLOB di Azure**

I dati vengono prelevati da un nuovo BLOB ogni ora (frequenza: ora, intervallo: 1). Il percorso della cartella e il nome del file per il BLOB vengono valutati dinamicamente in base all'ora di inizio della sezione in fase di elaborazione. Il percorso della cartella usa le parti anno, mese, e giorno dell'ora di inizio e il nome del file usa la parte dell'ora di inizio relativa all'ora. L'impostazione di "external" su "true" comunica al servizio Data factory che la tabella è esterna alla data factory e non è prodotta da un'attività al suo interno.

	{
	  "name": "AzureBlobInput",
	  "properties": {
	    "type": "AzureBlob",
	    "linkedServiceName": "StorageLinkedService",
	    "typeProperties": {
	      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
	      "fileName": "{Hour}.csv",
	      "partitionedBy": [
	        {
	          "name": "Year",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "yyyy"
	          }
	        },
	        {
	          "name": "Month",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%M"
	          }
	        },
	        {
	          "name": "Day",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%d"
	          }
	        },
	        {
	          "name": "Hour",
	          "value": {
	            "type": "DateTime",
	            "date": "SliceStart",
	            "format": "%H"
	          }
	        }
	      ],
	      "format": {
	        "type": "TextFormat",
	        "columnDelimiter": ",",
	        "rowDelimiter": "\n"
	      }
	    },
	    "external": true,
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    },
	    "policy": {
	      "externalData": {
	        "retryInterval": "00:01:00",
	        "retryTimeout": "00:10:00",
	        "maximumRetry": 3
	      }
	    }
	  }
	}

Vedere la sezione sulle [proprietà del tipo di set di dati BLOB di Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) per l'elenco delle proprietà supportate da questo tipo di set di dati.

**Set di dati di output di SQL Azure**

L'esempio copia dati in una tabella denominata "MyTable" in SQL Azure. È necessario creare la tabella in SQL Azure con lo stesso numero di colonne di quelle contenute nel file con estensione csv del BLOB. Alla tabella vengono aggiunte nuove righe ogni ora.

	{
	  "name": "AzureSqlOutput",
	  "properties": {
	    "type": "AzureSqlTable",
	    "linkedServiceName": "AzureSqlLinkedService",
	    "typeProperties": {
	      "tableName": "MyOutputTable"
	    },
	    "availability": {
	      "frequency": "Hour",
	      "interval": 1
	    }
	  }
	}

Vedere la sezione sulle [proprietà del tipo di set di dati SQL di Azure](#azure-sql-dataset-type-properties) per l'elenco delle proprietà supportate da questo tipo di set di dati.

**Pipeline con attività di copia**

La pipeline contiene un'attività di copia configurata per usare i set di dati di input e output precedenti. È programmata per essere eseguita ogni ora. Nella definizione JSON della pipeline, il tipo di **origine** è impostato su **BlobSource** e il tipo di **sink** è impostato su **SqlSink**.

	{  
	    "name":"SamplePipeline",
	    "properties":{  
	    "start":"2014-06-01T18:00:00",
	    "end":"2014-06-01T19:00:00",
	    "description":"pipeline with copy activity",
	    "activities":[  
	      {
	        "name": "AzureBlobtoSQL",
	        "description": "Copy Activity",
	        "type": "Copy",
	        "inputs": [
	          {
	            "name": "AzureBlobInput"
	          }
	        ],
	        "outputs": [
	          {
	            "name": "AzureSqlOutput"
	          }
	        ],
	        "typeProperties": {
	          "source": {
	            "type": "BlobSource",
	            "blobColumnSeparators": ","
	          },
	          "sink": {
	            "type": "SqlSink"
	          }
	        },
	       "scheduler": {
	          "frequency": "Hour",
	          "interval": 1
	        },
	        "policy": {
	          "concurrency": 1,
	          "executionPriorityOrder": "OldestFirst",
	          "retry": 0,
	          "timeout": "01:00:00"
	        }
	      }
	      ]
	   }
	}

Vedere la sezione [SqlSink](#sqlsink) e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) per l'elenco delle proprietà supportate da SqlSink e BlobSink.


## Proprietà del servizio collegato Azure SQL

La tabella seguente fornisce la descrizione degli elementi JSON specifici del servizio collegato SQL di Azure.

| Proprietà | Descrizione | Obbligatorio |
| -------- | ----------- | -------- |
| type | La proprietà del tipo deve essere impostata su: AzureSqlDatabase | Sì |
| connectionString | Specificare le informazioni necessarie per connettersi all'istanza di database SQL di Azure per la proprietà connectionString. | Sì |

**Nota:** è necessario configurare il [firewall del database SQL di Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). È necessario configurare il server del database per [consentire ai servizi di Azure di accedere al server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Inoltre, se si copiano dati in SQL Azure dall'esterno di Azure e da origini dati locali con gateway di data factory, sarà necessario configurare un intervallo di indirizzi IP appropriato per la macchina che sta inviando dati a SQL Azure.

## Proprietà del tipo del set di dati di Azure SQL

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione di set di dati, fare riferimento all'articolo sulla [creazione di set di dati](data-factory-create-datasets.md). Le sezioni come struttura, disponibilità e criteri di un set di dati JSON sono simili per tutti i tipi di set di dati (SQL Azure, BLOB di Azure, tabelle di Azure e così via).

La sezione typeProperties è diversa per ogni tipo di set di dati e contiene informazioni sulla posizione dei dati nell'archivio dati. La sezione **typeProperties** per il set di dati di tipo **AzureSqlTable** presenta le proprietà seguenti.

| Proprietà | Descrizione | Obbligatorio |
| -------- | ----------- | -------- |
| tableName | Nome della tabella nell'istanza di database SQL di Azure a cui fa riferimento il servizio collegato. | Sì |

## Proprietà del tipo di attività di copia di Azure SQL

Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, fare riferimento all'articolo sulla [creazione di pipeline](data-factory-create-pipelines.md). Per tutti i tipi di attività sono disponibili proprietà come nome, descrizione, tabelle di input e output, diversi criteri e così via.

> [AZURE.NOTE] L'attività di copia accetta solo un input e produce solo un output.

Le proprietà disponibili nella sezione typeProperties dell'attività variano invece per ogni tipo di attività e in caso di attività di copia variano in base ai tipi di origini e ai sink.

### SqlSource

In caso di attività di copia con origine di tipo **SqlSource**, sono disponibili le proprietà seguenti nella sezione **typeProperties**:

| Proprietà | Descrizione | Valori consentiti | Obbligatorio |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Usare la query personalizzata per leggere i dati. | Stringa di query SQL. Ad esempio: selezionare * da MyTable. Se non specificato, l'istruzione SQL eseguita: selezionare da MyTable. | No |
| sqlReaderStoredProcedureName | Nome della stored procedure che legge i dati dalla tabella di origine. | Nome della stored procedure. | No |
| storedProcedureParameters | Parametri per la stored procedure. | Coppie nome/valore. I nomi e le maiuscole e minuscole dei parametri devono corrispondere ai nomi e alle maiuscole e minuscole dei parametri della stored procedure. | No |

Se la proprietà **sqlReaderQuery** è specificata per SqlSource, l'attività di copia esegue questa query nell'origine del database SQL di Azure per ottenere i dati. In alternativa, è possibile specificare una stored procedure specificando i parametri **sqlReaderStoredProcedureName** e **storedProcedureParameters** (se la stored procedure accetta parametri).

Se non si specifica il parametro sqlReaderQuery o sqlReaderStoredProcedureName, le colonne definite nella sezione della struttura del set di dati JSON vengono usate per compilare una query (selezionare column1, column2 da mytable) da eseguire nel database SQL di Azure. Se la definizione del set di dati non dispone della struttura, vengono selezionate tutte le colonne della tabella.

> [AZURE.NOTE] Quando si usa **sqlReaderStoredProcedureName**, è necessario specificare un valore per la proprietà **tableName** nel set di dati JSON. Si tratta di una limitazione attuale del prodotto. Non sono disponibili convalide eseguite su questa tabella.

### Esempio SqlSource

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Definizione della stored procedure:**

	CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
	(
		@stringData varchar(20),
		@id int
	)
	AS
	SET NOCOUNT ON;
	BEGIN
	     select *
	     from dbo.UnitTestSrcTable
	     where dbo.UnitTestSrcTable.stringData != stringData
	    and dbo.UnitTestSrcTable.id != id
	END
	GO


### SqlSink 

**SqlSink** supporta le proprietà seguenti:

| Proprietà | Descrizione | Valori consentiti | Obbligatorio |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Tempo di attesa per l'operazione di inserimento batch da completare prima del timeout. | Intervallo di tempo<br/><br/> Ad esempio: "00:30:00" (30 minuti). | No | 
| writeBatchSize | Inserisce dati nella tabella SQL quando la dimensione del buffer raggiunge writeBatchSize. | Integer | No (valore predefinito: 10000)
| sqlWriterCleanupScript | Query specificata dall'utente per l'attività di copia da eseguire in modo che i dati di una sezione specifica vengano eliminati. Vedere di seguito la sezione ripetibilità per ulteriori dettagli. | Istruzione di query. | No |
| sliceIdentifierColumnName | Nome di colonna specificato dall'utente per l'attività di copia da riempire con l'identificatore di sezione generato automaticamente, che verrà usato per eliminare i dati di una sezione specifica quando viene nuovamente eseguita. Vedere di seguito la sezione ripetibilità per ulteriori dettagli. | Nome di colonna di una colonna con tipo di dati binario (32). | No |
| sqlWriterStoredProcedureName | Nome della stored procedure che esegue l'upsert (aggiornamenti/inserimenti) nella tabella di destinazione. | Nome della stored procedure. | No |
| storedProcedureParameters | Parametri per la stored procedure. | Coppie nome/valore. I nomi e le maiuscole e minuscole dei parametri devono corrispondere ai nomi e alle maiuscole e minuscole dei parametri della stored procedure. | No | 
| sqlWriterTableType | Nome del tipo di tabella specificato dall'utente da usare nella stored procedure precedente. L'attività di copia rende i dati spostati disponibili in una tabella temporanea con questo tipo di tabella. Il codice della stored procedure può quindi unire i dati copiati con i dati esistenti. | Nome del tipo di tabella. | No |

#### Esempio SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## Colonne Identity nel database di destinazione
In questa sezione viene fornito un esempio per la copia di dati da una tabella di origine senza una colonna identity in una tabella di destinazione con una colonna identity.

**Tabella di origine:**

	create table dbo.SourceTbl
	(
	       name varchar(100),
	       age int
	)

**Tabella di destinazione:**

	create table dbo.TargetTbl
	(
	       id int identity(1,1),
	       name varchar(100),
	       age int
	)


Si noti che la tabella di destinazione contiene una colonna identity.

**Definizione JSON del set di dati di origine**

	{
	    "name": "SampleSource",
	    "properties": {
	        "published": false,
	        "type": " SqlServerTable",
	        "linkedServiceName": "TestIdentitySQL",
	        "typeProperties": {
	            "tableName": "SourceTbl"
	        },
	        "availability": {
	            "frequency": "Hour",
	            "interval": 1
	        },
	        "external": true,
	        "policy": {}
	    }
	}

**Definizione JSON del set di dati di destinazione**

	{
	    "name": "SampleTarget",
	    "properties": {
	        "structure": [
	            { "name": "name" },
	            { "name": "age" }
	        ],
	        "published": false,
	        "type": "AzureSqlTable",
	        "linkedServiceName": "TestIdentitySQLSource",
	        "typeProperties": {
	            "tableName": "TargetTbl"
	        },
	        "availability": {
	            "frequency": "Hour",
	            "interval": 1
	        },
	        "external": false,
	        "policy": {}
	    }	
	}


Si noti che la tabella di origine e la tabella di destinazione hanno schemi diversi (la destinazione include una colonna aggiuntiva identity). In questo scenario è necessario specificare la proprietà **structure** nella definizione del set di dati di destinazione che non include la colonna identity.

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### Mapping dei tipi di SQL Server e del database SQL di Azure

Come accennato nell'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md), l'attività di copia esegue conversioni di tipo automatico da tipi di origine a tipi di sink con l'approccio seguente in 2 passaggi:

1. Conversione dai tipi di origine nativi al tipo .NET
2. Conversione dal tipo .NET al tipo di sink nativo

Quando si spostano dati da e verso SQL Azure, SQL Server, Sybase verranno usati i mapping seguenti dal tipo SQL di tipo .NET e viceversa.

Il mapping è uguale al mapping del tipo di dati di SQL Server per ADO.NET.

| Tipo di motore di database di SQL Server | Tipo di .NET Framework |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| binary | Byte |
| Bit | Boolean |
| char | String, Char |
| date | DateTime |
| Datetime | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Decimale | Decimale |
| FILESTREAM attribute (varbinary(max)) | Byte |
| Float | Double |
| immagine | Byte | 
| int | Int32 | 
| money | Decimale |
| nchar | String, Char |
| ntext | String, Char |
| numeric | Decimale |
| nvarchar | String, Char |
| real | Single |
| rowversion | Byte |
| smalldatetime | DateTime |
| smallint | Int16 |
| smallmoney | Decimale | 
| sql\_variant | Object * |
| text | String, Char |
| time | TimeSpan |
| timestamp | Byte |
| tinyint | Byte |
| uniqueidentifier | Guid |
| varbinary | Byte |
| varchar | String, Char |
| xml | Xml |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## Ottimizzazione delle prestazioni  
Per informazioni sui fattori chiave che influiscono sulle prestazioni dello spostamento dei dati, ovvero dell'attività di copia, in Azure Data Factory e sui vari modi per ottimizzare tali prestazioni, vedere la [Guida alle prestazioni delle attività di copia e all'ottimizzazione](data-factory-copy-activity-performance.md).

<!---HONumber=AcomDC_0629_2016-->