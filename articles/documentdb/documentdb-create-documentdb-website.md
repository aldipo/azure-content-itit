<properties 
	pageTitle="Distribuire DocumentDB e app Web del servizio app di Azure mediante un modello di Gestione risorse di Azure | Microsoft Azure" 
	description="Informazioni su come distribuire un account DocumentDB, app Web del servizio app di Azure e un'applicazione Web di esempio usando un modello di Gestione risorse di Azure." 
	services="documentdb, app-service\web" 
	authors="stephbaron" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/04/2016" 
	ms.author="stbaro"/>

# Distribuire DocumentDB e app Web del servizio app di Azure mediante un modello di Gestione risorse di Azure

Questa esercitazione illustra come usare un modello di Gestione risorse di Azure per distribuire e integrare [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/), un'app Web del [servizio app di Azure](http://go.microsoft.com/fwlink/?LinkId=529714) e un'applicazione Web di esempio.

Con il modello di Azure Resource Manager è possibile automatizzare facilmente la distribuzione e la configurazione delle risorse di Azure. In questa esercitazione viene illustrato come distribuire un'applicazione Web e configurare automaticamente le informazioni relative alla connessione dell'account DocumentDB.

Dopo aver completato questa esercitazione, si potrà rispondere alle domande seguenti:

-	Come è possibile usare un modello di Gestione risorse di Azure per distribuire e integrare un account DocumentDB e un'app Web nel servizio app di Azure?
-	Come è possibile usare un modello di Gestione risorse di Azure per distribuire e integrare un account DocumentDB, un'app Web nelle app Web del servizio app e un'applicazione WebDeploy?

<a id="Prerequisites"></a>
## Prerequisiti
> [AZURE.TIP] Sebbene questa esercitazione non presupponga alcuna esperienza nell'uso dei modelli o JSON di Azure Resource Manager, se si desidera modificare i modelli o le opzioni di distribuzione cui viene fatto riferimento, è necessario conoscere ciascuna di tali aree.

Prima di seguire le istruzioni di questa esercitazione, verificare che siano disponibili gli elementi seguenti:

- Una sottoscrizione di Azure. Azure è una piattaforma basata su sottoscrizione. Per altre informazioni su come ottenere una sottoscrizione, vedere [Opzioni di acquisto](https://azure.microsoft.com/pricing/purchase-options/), [Offerte per i membri](https://azure.microsoft.com/pricing/member-offers/) oppure [Versione di prova gratuita](https://azure.microsoft.com/pricing/free-trial/).

##<a id="CreateDB"></a>Passaggio 1: Scaricare i file del modello ##
Per iniziare, scaricare i file del modello da usare in questa esercitazione.

1. Scaricare il modello di [esempio per creare un account DocumentDB, app Web e per distribuire un'applicazione demo](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) in una cartella locale, ad esempio C:\\DocumentDBTemplates. Questo modello distribuirà un account DocumentDB, un'app Web del servizio app e un'applicazione Web. L'applicazione Web verrà anche configurata automaticamente per connettersi all'account DocumentDB.

2. Scaricare il modello di [esempio per creare un account DocumentDB e app Web](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) in una cartella locale, ad esempio C:\\DocumentDBTemplates. Questo modello distribuirà un account DocumentDB, un'app Web del servizio app e modificherà le impostazioni dell'applicazione del sito per rilevare in modo semplice le informazioni relative alla connessione di DocumentDB. Non è tuttavia inclusa alcuna applicazione Web.

<a id="Build"></a>
##Passaggio 2: Distribuire l'esempio relativo ad account DocumentDB, app Web del servizio app e applicazione demo

Si procederà a questo punto alla distribuzione del primo modello.

> [AZURE.TIP] Il modello non verifica che il nome dell'app Web e il nome dell'account DocumentDB siano validi e/o disponibili. È consigliabile verificare la disponibilità dei nomi che si intende fornire prima della distribuzione.

1. Accedere al [portale di Azure](https://portal.azure.com), fare clic su Nuovo e ricercare "Distribuzione modello". ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment1.png)

2. Selezionare la voce Distribuzione modello e fare clic su **Crea** ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment2.png)

3.  Fare clic su **Modifica modello**, incollare il contenuto del file del modello DocDBWebsiteTodo.json e fare clic su **Salva**. ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment3.png)

4. Fare clic su **Modifica parametri**, specificare i valori per ognuno dei parametri obbligatori e fare clic su **OK**. I parametri sono i seguenti:

	1. SITENAME: specifica il nome dell'app Web del servizio app e viene usato per creare l'URL che consentirà di accedere all'app Web. Ad esempio, se si specifica "mydemodocdbwebapp", l'URL usato per accedere all'app Web sarà mydemodocdbwebapp.azurewebsites.net.

	2. HOSTINGPLANNAME: specifica il nome del piano di hosting del servizio app da creare.

	3. LOCATION: specifica la posizione di Azure in cui creare le risorse di DocumentDB e dell'app Web.

	4. DATABASEACCOUNTNAME: specifica il nome dell'account DocumentDB da creare.

	![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment4.png)

5. Scegliere un gruppo di risorse esistente o fornire un nome per creare un nuovo gruppo di risorse, quindi selezionare la relativa posizione. ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment5.png)
  
6.  Fare clic su **Rivedere le note legali**, **Acquista**, quindi su **Crea** per iniziare la distribuzione. Selezionare **Aggiungi al dashboard** per rendere facilmente visibile la distribuzione ottenuta nella home page del portale di Azure. ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment6.png)

7.  Al termine della distribuzione, verrà aperto il pannello del gruppo di risorse. ![Screenshot del pannello del gruppo di risorse](./media/documentdb-create-documentdb-website/TemplateDeployment7.png)

8.  Per usare l'applicazione, basta accedere all'URL dell'app Web. Nell'esempio precedente, l'URL sarà http://mydemodocdbwebapp.azurewebsites.net). Verrà visualizzata l'applicazione Web seguente:

	![Applicazione di esempio](./media/documentdb-create-documentdb-website/image2.png)

9. Proseguire e creare un paio di attività nell'app Web, quindi tornare al pannello del gruppo di risorse nel portale di Azure. Fare clic sulla risorsa account DocumentDB nell'elenco delle risorse, quindi su **Esplora query**. ![Schermata del filtro di riepilogo con l'app Web myotherdocumentdbwebapp evidenziata](./media/documentdb-create-documentdb-website/TemplateDeployment8.png)

10. Eseguire la query predefinita, "SELECT * FROM c" ed esaminarne i risultati. Si noti che la query ha recuperato la rappresentazione JSON delle attività create nel passaggio 7 precedente. È possibile sperimentare con le query. Ad esempio, provare a eseguire SELECT * FROM c WHERE c.isComplete = true per restituire tutti gli elementi ToDo che sono stati contrassegnati come completati.

	![Schermata dei pannelli Esplora query e Risultati con i risultati delle query](./media/documentdb-create-documentdb-website/image5.png)

11. È possibile verificare la funzionalità del portale di DocumentDB o modificare l'applicazione ToDo di esempio. A questo punto si è pronti per distribuire un altro modello.
	
<a id="Build"></a>
## Passaggio 3: Distribuire l'esempio relativo ad account DocumentDB e app Web

Si procederà ora alla distribuzione del secondo modello. Questo modello è utile per mostrare come inserire le informazioni relative alla connessione di DocumentDB, ad esempio l'endpoint dell'account e la chiave master in un'app Web come impostazioni dell'applicazione o come una stringa di connessione personalizzata. Ad esempio, quando si dispone di un'applicazione Web personale che si desidera distribuire con un account DocumentDB e le informazioni relative alla connessione vengono popolate automaticamente durante la distribuzione.

> [AZURE.TIP] Il modello non verifica che il nome dell'app Web e il nome dell'account DocumentDB siano validi e/o disponibili. È consigliabile verificare la disponibilità dei nomi che si intende fornire prima della distribuzione.

1. Nel [portale di Azure](https://portal.azure.com), fare clic su Nuovo e ricercare "Distribuzione modello". ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment1.png)

2. Selezionare la voce Distribuzione modello e fare clic su **Crea** ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment2.png)

3.  Fare clic su **Modifica modello**, incollare il contenuto del file del modello DocDBWebSite.json e fare clic su **Salva**. ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment3.png)

4. Fare clic su **Modifica parametri**, specificare i valori per ognuno dei parametri obbligatori e fare clic su **OK**. I parametri sono i seguenti:

	1. SITENAME: specifica il nome dell'app Web del servizio app e viene usato per creare l'URL che consentirà di accedere all'app Web. Ad esempio, se si specifica "mydemodocdbwebapp", l'URL usato per accedere all'app Web sarà mydemodocdbwebapp.azurewebsites.net.

	2. HOSTINGPLANNAME: specifica il nome del piano di hosting del servizio app da creare.

	3. LOCATION: specifica la posizione di Azure in cui creare le risorse di DocumentDB e dell'app Web.

	4. DATABASEACCOUNTNAME: specifica il nome dell'account DocumentDB da creare.

	![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment4.png)

5. Scegliere un gruppo di risorse esistente o fornire un nome per creare un nuovo gruppo di risorse, quindi selezionare la relativa posizione. ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment5.png)
  
6.  Fare clic su **Rivedere le note legali**, **Acquista**, quindi su **Crea** per iniziare la distribuzione. Selezionare **Aggiungi al dashboard** per rendere facilmente visibile la distribuzione ottenuta nella home page del portale di Azure. ![Screenshot dell'interfaccia utente della distribuzione del modello](./media/documentdb-create-documentdb-website/TemplateDeployment6.png)

7.  Al termine della distribuzione, verrà aperto il pannello del gruppo di risorse. ![Screenshot del pannello del gruppo di risorse](./media/documentdb-create-documentdb-website/TemplateDeployment7.png)

8. Fare clic sulla risorsa dell'app Web nell'elenco delle risorse, quindi su **Impostazioni dell'applicazione** ![Screenshot del gruppo di risorse](./media/documentdb-create-documentdb-website/TemplateDeployment9.png)

9. Notare la presenza di impostazioni dell'applicazione per l'endpoint di DocumentDB e per ognuna delle relative chiavi master. ![Screenshot delle impostazioni dell'applicazione](./media/documentdb-create-documentdb-website/TemplateDeployment10.png)

10. È possibile continuare a esplorare il portale di Azure o seguire uno degli [esempi](http://go.microsoft.com/fwlink/?LinkID=402386) di DocumentDB per creare la propria applicazione DocumentDB.

	
	
<a name="NextSteps"></a>
## Passaggi successivi

Congratulazioni. Si è completata la distribuzione di DocumentDB, di un'app Web del servizio app e di un'applicazione Web di esempio usando i modelli di Gestione risorse di Azure.

- Per altre informazioni su DocumentDB, vedere [qui](http://azure.com/docdb).
- Per altre informazioni sulle app Web del servizio app di Azure, vedere [qui](http://go.microsoft.com/fwlink/?LinkId=325362).
- Per altre informazioni sui modelli di Gestione risorse di Azure, vedere [qui](https://msdn.microsoft.com/library/azure/dn790549.aspx).


## Modifiche apportate
* Per una Guida per la modifica di siti Web al servizio App vedere: [servizio App Azure e il relativo impatto sui servizi di Azure esistente](http://go.microsoft.com/fwlink/?LinkId=529714)
* Per una guida relativa al passaggio dal portale precedente al nuovo portale vedere: [Informazioni di riferimento per l'esplorazione del portale di Azure classico](http://go.microsoft.com/fwlink/?LinkId=529715)

>[AZURE.NOTE] Per iniziare a usare il servizio app di Azure prima di registrarsi per ottenere un account Azure, andare a [Prova il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751), dove è possibile creare un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.
 

<!---HONumber=AcomDC_0504_2016-->