<properties
	pageTitle="Creare un'app web WordPress nel servizio App Azure | Microsoft Azure"
	description="Informazioni su come creare una nuova app Web di Azure per un blog WordPress mediante il portale di Azure."
	services="app-service\web"
	documentationCenter="php"
	authors="rmcmurray"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="PHP"
	ms.topic="hero-article"
	ms.date="06/24/2016"
	ms.author="robmcm"/>

# Creare un'app WordPress nel servizio app di Azure

[AZURE.INCLUDE [schede](../../includes/app-service-web-get-started-nav-tabs.md)]

Questa esercitazione illustra come distribuire un sito blog di WordPress da Azure Marketplace.

Al termine dell'esercitazione si otterrà un sito di blog WordPress attivo e in esecuzione sul cloud.

![Sito WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Si apprenderà come:

* Come trovare un modello di applicazione in Azure Marketplace.
* Come creare un'app Web basata sul modello nel servizio app di Azure.
* Come configurare le impostazioni del servizio app di Azure per la nuova app Web e il database.

Azure Marketplace rende disponibile un'ampia varietà di applicazioni Web ampiamente diffuse, sviluppate da Microsoft, da terze parti e tramite iniziative software open source. Le app Web sono basate su una vasta gamma di framework noti, ad esempio [PHP](/develop/nodejs/) in questo esempio WordPress, [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/) e [Python](/develop/python/), per citarne alcuni. Per creare un'app Web da Azure Marketplace, l'unico software necessario è il browser usato per il [portale di Azure](https://portal.azure.com/).

Il sito WordPress distribuito in questa esercitazione usa MySQL come database. Se invece si vuole usare il database SQL come database, vedere [progetto Nami](http://projectnami.org/). **Progetto Nami** è disponibile anche tramite il Marketplace.

> [AZURE.NOTE]
Per completare l'esercitazione, è necessario un account Microsoft Azure. Se non si dispone di un account, è possibile [attivare i benefici della sottoscrizione Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oppure [iscriversi per ottenere una versione di valutazione gratuita](/it-IT/pricing/free-trial/?WT.mc_id=A261C142F).
>
> Per iniziare a usare il servizio app di Azure prima di registrarsi per ottenere un account Azure, passare alla pagina [Prova il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751). In questa pagina è possibile creare immediatamente un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.

## Selezionare WordPress ed eseguire la configurazione per il servizio app di Azure

1. Accedere al [Portale di Azure](https://portal.azure.com/).

2. Fare clic su **New**.
	
    ![Creazione di un nuovo sito][5]
	
3. Cercare **WordPress**, quindi fare clic su **WordPress**. Se si vuole usare il database SQL invece di MySQL, cercare **progetto Nami**.

	![WordPress nell'elenco][7]
	
5. Dopo aver letto la descrizione dell'app WordPress, fare clic su **Crea**.

	![Creazione](./media/web-sites-php-web-site-gallery/create.png)

4. Immettere un nome per l'app Web nella casella **App Web**.

	Il nome deve essere univoco nel dominio azurewebsites.net perché l'URL dell'app Web sarà {nome}.azurewebsites.net. Se il nome immesso non è univoco, nella casella di testo verrà visualizzato un punto esclamativo rosso.

8. Se sono disponibili più sottoscrizioni, scegliere quella da usare.

5. Selezionare un **Gruppo di risorse** o crearne uno nuovo.

	Per altre informazioni sui gruppi di risorse, vedere [Uso del portale di Azure per gestire le risorse di Azure](../resource-group-portal.md).

5. Selezionare un **Piano di servizio app/Posizione** o crearne uno nuovo.

	Per altre informazioni sui piani del servizio app, vedere [Panoramica approfondita dei piani del servizio app di Azure](../azure-web-sites-web-hosting-plans-in-depth-overview.md).

7. Fare clic su **Database**, quindi nel pannello **Nuovo database MySQL** specificare i valori necessari per configurare il database MySQL.

	a. Immettere un nuovo nome o lasciare il nome predefinito.

	b. Lasciare **Tipo database** impostato su **Condiviso**.

	c. Scegliere la stessa posizione usata per l'app Web.

	d. Scegliere un piano tariffario. Mercury (gratuito con quantità minima di connessioni consentite e di spazio su disco) è ottimale per questa esercitazione.

8. Nel pannello **Nuovo database MySQL** fare clic su **OK**.

8. Nel pannello **WordPress** accettare le condizioni legali, quindi fare clic su **Crea**.

	![Configurazione dell'app Web](./media/web-sites-php-web-site-gallery/configure.png)

	Il servizio app di Azure crea l'app Web, in genere in meno di un minuto. È possibile verificare lo stato facendo clic sull'icona a forma di campana nella parte superiore della pagina del portale.

	![Indicatore di stato](./media/web-sites-php-web-site-gallery/progress.png)

## Avviare e gestire l'app Web WordPress
	
7. Al termine della creazione dell'app Web, nel portale di Azure passare al gruppo di risorse in cui è stata creata l'applicazione. L'app Web e il database saranno visualizzati nel gruppo.

	La risorsa aggiuntiva con l'icona a forma di lampadina è [Application Insights](/services/application-insights/), che fornisce servizi di monitoraggio per l'app Web.

1. Nel pannello **Gruppo di risorse** fare clic sulla riga dell'app Web.

	![Configurazione dell'app Web](./media/web-sites-php-web-site-gallery/resourcegroup.png)

2. Nel pannello dell'app Web fare clic su **Sfoglia**.

    ![URL sito][browse]

3. Nella pagina **iniziale** di WordPress immettere le informazioni di configurazione richieste da WordPress, quindi fare clic su **Install WordPress**.

	![Configurazione di WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)

4. Accedere usando le credenziali create nella pagina **iniziale**.

5. Verrà visualizzata la pagina del dashboard del sito.

	![Sito WordPress](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## Passaggi successivi

Si è appreso come creare e distribuire un'app Web PHP dalla raccolta. Per altre informazioni sull'uso di PHP in Azure, vedere il [centro per sviluppatori PHP](/develop/php/).

Per altre informazioni su come utilizzare le app Web del servizio app, vedere i collegamenti nel lato sinistro della pagina (nelle finestre del browser di grandi dimensioni) o nella parte superiore della pagina (per le finestre del browser ridotte).

## Modifiche apportate
* Per indicazioni sul passaggio dai Siti Web al servizio app, vedere [Servizio app di Azure e i servizi di Azure esistenti](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png

<!---HONumber=AcomDC_0629_2016-->