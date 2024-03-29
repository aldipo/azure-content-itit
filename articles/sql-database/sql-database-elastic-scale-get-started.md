<properties 
	pageTitle="Iniziare a utilizzare gli strumenti di database elastici" 
	description="Spiegazione di base della funzionalità relativa agli strumenti di database elastici del database SQL di Azure, che include un'app di esempio di facile esecuzione." 
	services="sql-database" 
	documentationCenter="" 
	manager="jhubbard" 
	authors="alancameronwills" 
	editor="carlrabeler"/>

<tags 
	ms.service="sql-database" 
	ms.workload="sql-database" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/27/2016" 
	ms.author="ddove"/>

# Iniziare a utilizzare gli strumenti di database elastici

In questo documento viene presentata l'esperienza di sviluppo eseguendo l’app di esempio. Nell'esempio viene creata una semplice applicazione partizionata si esplorano le funzionalità chiave degli strumenti di database elastici. L'esempio illustra le funzioni della [libreria client di database elastico](sql-database-elastic-database-client-library.md)

Per installare la libreria, visitare [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Si noti che la libreria viene installata con l'app di esempio descritta di seguito.

## Prerequisiti

1. È richiesto Visual Studio 2012 o versioni successive con C#. Scaricare una versione gratuita dalla pagina [Download di Visual Studio](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 o versioni successive. Per ottenere la versione più recente, vedere il documento relativo all'[installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## Scaricare ed eseguire l'app di esempio

L'applicazione di esempio **Introduzione al database elastico con SQL di Azure** illustra gli aspetti più importanti dell'esperienza di sviluppo delle applicazioni partizionate usando gli strumenti di database elastici con SQL di Azure. L'applicazione è incentrata sui casi di utilizzo chiave per la [gestione delle mappe di partizione](sql-database-elastic-scale-shard-map-management.md), il [routing dipendente dai dati](sql-database-elastic-scale-data-dependent-routing.md) e l'[esecuzione di query su più partizionamenti](sql-database-elastic-scale-multishard-querying.md). Per scaricare ed eseguire l'applicazione di esempio, seguire questa procedura:

1. Aprire Visual Studio e selezionare **File -> Nuovo -> Progetto**.
2. Nella finestra di dialogo, fare clic su **Online**.

    ![New Project>Online][2]
3. Quindi fare clic su **Visual C#** in **Esempi**.

    ![Click Visual C#][3]
4. Nella casella di ricerca digitare **elastic db** per cercare l'esempio. Viene visualizzato il titolo **Introduzione al database elastico con SQL di Azure**.

    ![Search Box][1]
 
5. Selezionare l'esempio, scegliere un nome e una posizione per il nuovo progetto e premere **OK** per creare il progetto.
6. Aprire il file **app.config** nella soluzione per il progetto di esempio e seguire le istruzioni del file per aggiungere il nome del server di database SQL Azure e le informazioni di accesso (nome utente e password).
7. Compilare ed eseguire l'applicazione. Quando viene richiesto, consentire a Visual Studio di ripristinare i pacchetti NuGet della soluzione. In questo modo verrà scaricata la versione più recente delle librerie client del database elastico da NuGet.
8. Provare a usare le varie opzioni per ottenere altre informazioni sulle funzionalità della libreria client. Annotare i passaggi eseguiti dall'applicazione nell'output della console ed esplorare liberamente il codice sottostante.

    ![progress][4]

È stata creata ed eseguita la prima applicazione partizionata usando gli strumenti di database elastici nel database SQL di Azure. Esaminare rapidamente i partizionamenti creati dall'applicazione di esempio effettuando la connessione con Visual Studio o SQL Server Management Studio al server del database di Azure. Si noteranno nuovi database con partizionamento e un database di gestione delle mappe con partizionamento creati dall'applicazione di esempio.

**Nota** Se non è disponibile SQL Server Management Studio, vedere [Gestione di database SQL di Azure tramite SQL Server Management Studio](sql-database-manage-azure-ssms.md), che include istruzioni per scaricare lo strumento.

### Elementi chiave dell'esempio di codice

1. **Gestione di partizionamenti e di mappe con partizionamento**: il codice illustra come usare i partizionamenti, gli intervalli e i mapping nel file **ShardMapManagerSample.cs**. Altre informazioni su questo argomento sono disponibili nella pagina [Gestione mappe partizioni](http://go.microsoft.com/?linkid=9862595).  
2. **Routing dipendente dai dati**: il routing delle transazioni al partizionamento appropriato è mostrato in **DataDependentRoutingSample.cs**. Per altri dettagli, vedere [Routing dipendente dai dati](http://go.microsoft.com/?linkid=9862596). 
3. **Esecuzione di query su più partizionamenti**: l'esecuzione di query su più partizionamenti è illustrata nel file **MultiShardQuerySample.cs**. Per altri dettagli, vedere [Esecuzione di query su più partizionamenti](http://go.microsoft.com/?linkid=9862597).
4. **Aggiunta di partizionamenti vuoti**: l'aggiunta iterativa di nuovi partizionamenti vuoti viene eseguita dal codice nel file **AddNewShardsSample.cs**. Informazioni dettagliate su questo argomento sono disponibili nella pagina. [Shard Map Management](http://go.microsoft.com/?linkid=9862595).

### Altre operazioni di scalabilità elastica

1. **Suddivisione di un partizionamento esistente**: la possibilità di suddividere partizionamenti viene fornita tramite lo **strumento di suddivisione/unione**. È possibile trovare ulteriori informazioni su questo strumento nella pagina [Panoramica sullo strumento di suddivisione/unione](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Unione di partizionamenti esistenti**: le unioni di partizioni esistenti vengono inoltre eseguite tramite lo **strumento di suddivisione/unione**. Per ulteriori informazioni, vedere [Panoramica sullo strumento di suddivisione/unione](sql-database-elastic-scale-overview-split-and-merge.md).   


## Costi

Gli strumenti di database elastici sono gratuiti. Gli strumenti di database elastici non comportano costi aggiuntivi oltre a quelli applicati per l'uso di Azure.

Quindi, nel caso dei nuovi database creati dall'applicazione di esempio, il costo dipende dall'edizione del database SQL di Azure scelta e dall'uso di Azure da parte dell'applicazione.

Per informazioni sui prezzi, vedere [Dettagli prezzi del database SQL](https://azure.microsoft.com/pricing/details/sql-database/).

## Passaggi successivi
Per ulteriori informazioni sugli strumenti di database elastici, vedere:

* [Mappa della documentazione sugli strumenti di database elastici](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Esempi di codice: 
    -    [Introduzione al database elastico con SQL di Azure](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Database elastico con SQL di Azure - Integrazione con Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Elasticità di partizionamento in Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [Annuncio della scalabilità elastica](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Channel 9: [Video sulla panoramica della scalabilità elastica](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Forum di discussione: [Forum sul database SQL di Azure](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Per misurare le prestazioni: [Contatori delle prestazioni per Gestore mappe partizioni](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 

<!---HONumber=AcomDC_0615_2016-->
