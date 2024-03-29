<properties 
	pageTitle="Connettere un'app Web a un'app per le API nel servizio app di Azure" 
	description="Questa esercitazione illustra come usare un'app per le API da un'app Web ASP.NET ospitata nel servizio app di Azure." 
	services="app-service\web" 
	documentationCenter=".net" 
	authors="syntaxc4" 
	manager="yochayk" 
	editor="jimbe"/>

<tags
	ms.service="app-service-web"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="na" 
	ms.date="02/26/2016"
	ms.author="cfowler"/>

# Connettere un'app Web a un'app per le API nel servizio app di Azure

Questa esercitazione illustra come usare un'app per le API da un'app Web ASP.NET ospitata nel [servizio app](https://azure.microsoft.com/services/app-service/).

## Prerequisiti

Questa esercitazione si basa su [Creare un'app per le API di Azure](../app-service-api/app-service-api-dotnet-get-started.md).

## Creare un'applicazione ASP.NET MVC in Visual Studio

1. Aprire Visual Studio. Usare la finestra di dialogo **Nuovo progetto** per aggiungere una nuova **applicazione Web ASP.NET**. Fare clic su **OK**.

	![File > Nuovo > Web > Applicazione Web ASP.NET](./media/app-service-web-connect-web-app-to-saas-api/1-Create-New-MVC-App-For-Consumption.png)

1. Selezionare il modello **MVC**. Fare clic su **Modifica autenticazione**, selezionare **Nessuna autenticazione** e quindi fare clic su **OK** per due volte.

	![Nuova applicazione ASP.NET](./media/app-service-web-connect-web-app-to-saas-api/2-Change-Auth-To-No-Auth.png)

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto di applicazione Web appena creato e selezionare **Aggiungi** > **Client dell'API REST**.

	![Aggiungere un riferimento a un'app per le API di Azure...](./media/app-service-web-connect-web-app-to-saas-api/3-Add-Azure-API-App-SDK.png)

1. In **Aggiungi il client dell'API REST** selezionare Scarica dall'app per le API di Microsoft Azure e fare clic su Sfoglia. Selezionare l'app per le API a cui ci si vuole connettere.

	![Selezionare un'app per le API esistente](./media/app-service-web-connect-web-app-to-saas-api/4-Add-Azure-API-App-SDK-Dialog.png)

	>[AZURE.NOTE] Il codice client per la connessione all'app per le API verrà generato automaticamente da un endpoint API Swagger.

1. Per usare il codice API generato, aprire il file HomeController.cs e sostituire l'azione `Contact` con il codice seguente:

	    public async Task<ActionResult> Contact()
        {
            ViewBag.Message = "Your contact page.";

            var contacts = new ContactsList12242015();
            var contactList = await contacts.Contacts.GetAsync();
            
            return View(contactList);
        }

	![Aggiornamenti codice HomeController.cs](./media/app-service-web-connect-web-app-to-saas-api/5-Write-Code-Which-Leverages-Swagger-Generated-Code.png)

1. Aggiornare la visualizzazione `Contact` in modo che rifletta l'elenco dinamico di contatti con il codice seguente:
	<pre>// Add to the very top of the view file
	@model IList&lt;MyContactsList.Web.Models.Contact>
	
	// Replace the default email addresses with the following
	&lt;h3>Public Contacts&lt;/h3>
	&lt;ul>
	    @foreach (var contact in Model)
	    {
	        &lt;li>&lt;a href="mailto:@contact.EmailAddress">@contact.Name &amp;lt;@contact.EmailAddress&amp;gt;&lt;/a>&lt;/li>
	    }
	&lt;/ul> 
	</pre>

	![Aggiornamenti codice Contact.cshtml](./media/app-service-web-connect-web-app-to-saas-api/6-Update-View-To-Reflect-Changes.png)

## Distribuire l'applicazione Web nelle app Web del servizio app

Seguire le istruzioni fornite in [Come distribuire un'app Web di Azure](web-sites-deploy.md).

>[AZURE.NOTE] Per iniziare a usare il servizio app di Azure prima di registrarsi per ottenere un account Azure, andare a [Prova il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751), dove è possibile creare un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.

## Modifiche apportate
* Per una guida relativa al passaggio da Siti Web al servizio app, vedere [Servizio app di Azure e impatto sui servizi di Azure esistenti](http://go.microsoft.com/fwlink/?LinkId=529714)
 

<!---HONumber=AcomDC_0518_2016-->