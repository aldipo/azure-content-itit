<properties 
	pageTitle="Django e MySQL in Azure con Python Tools 2.2 per Visual Studio" 
	description="Informazioni su come usare Python Tools per Visual Studio per creare un'app Web Django che archivia i dati in un'istanza di database MySQL e per distribuirla in App Web del servizio app di Azure." 
	services="app-service\web" 
	documentationCenter="python" 
	authors="huguesv" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service-web" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="python"
	ms.topic="get-started-article" 
	ms.date="06/01/2016"
	ms.author="huvalo"/>

# Django e MySQL in Azure con Python Tools 2.2 per Visual Studio 

[AZURE.INCLUDE [schede](../../includes/app-service-web-get-started-nav-tabs.md)]

In questa esercitazione si userà [Python Tools per Visual Studio](PTVS) al fine di creare una semplice app Web per sondaggi con uno dei modelli di esempio PTVS. Si apprenderà come usare un servizio MySQL ospitato in Azure, come configurare l'app Web per l'uso di MySQL e come pubblicare l'app Web in [App Web del servizio app di Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

> [AZURE.NOTE] Le informazioni contenute in questa esercitazione sono disponibili anche nel video seguente:
> 
> [PTVS 2.1: Django app with MySQL][video] (PTVS 2.1: app Django con MySQL)

Vedere il [Centro per sviluppatori Python] per consultare altri articoli che trattano lo sviluppo di app Web del servizio app di Azure con PTVS usando i framework Web di Bottle, Flask e Django con i servizi di MongoDB, archiviazione tabelle di Azure, MySQL e Database SQL. Sebbene questo articolo sia incentrato sul servizio app, i passaggi sono simili a quelli previsti per lo sviluppo dei [servizi cloud di Azure].

## Prerequisiti

 - Visual Studio 2013 o 2015
 - [Python 2.7 a 32 bit]
 - [Python Tools 2.2 per Visual Studio]
 - [VSIX degli esempi di Python Tools 2.2 per Visual Studio]
 - [Strumenti di Azure SDK per VS 2013] o [Strumenti di Azure SDK per VS 2015]
 - Django 1.6 o versione precedente

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [AZURE.NOTE] Per iniziare a usare il servizio app di Azure prima di registrarsi per ottenere un account Azure, andare a [Prova il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751), dove è possibile creare un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.

## Creare il progetto

In questa sezione verrà creato un progetto di Visual Studio usando un modello di esempio. Verrà creato un ambiente virtuale e verranno installati i pacchetti necessari. Si creerà un database locale usando sqlite, quindi verrà eseguita l'applicazione in locale.

1. In Visual Studio selezionare **File**, **Nuovo progetto**.

1. I modelli di progetto del pacchetto PTVS Samples VSIX sono disponibili in **Python**, **Esempi**. Selezionare **Polls Django Web Project** e fare clic su OK per creare il progetto.

    ![Finestra di dialogo Nuovo progetto](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)

1. Verrà richiesto di installare pacchetti esterni. Selezionare **Installa in un ambiente virtuale**.

    ![Finestra di dialogo dei pacchetti esterni](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)

1. Selezionare **Python 2.7** come interprete di base.

    ![Finestra di dialogo Aggiungi ambiente virtuale](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul nodo del progetto, scegliere **Python** e quindi selezionare **Django Sync DB** (DB sincronizzazione Django).

    ![Comando Django Sync DB](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSyncDB.png)

1. Verrà aperta una console di gestione Django e verrà creato un database sqlite database nella cartella del progetto. Seguire le istruzioni visualizzate per creare un utente.

    ![Finestra della console di gestione di Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1. Verificare che l'applicazione funzioni premendo `F5`.

1. Fare clic su **Log in** sulla barra di spostamento in alto.

    ![Barra di spostamento di Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)

1. Immettere le credenziali per l'utente creato al momento della sincronizzazione del database.

    ![Form di accesso](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)

1. Fare clic su **Create Sample Polls**.

    ![Creare poll di esempio](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)

1. Fare clic su un sondaggio e votare.

    ![Votazione nei poll di esempio](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## Creare un database MySQL

Per il database verrà creato un database ospitato MySQL di ClearDB in Azure.

In alternativa, è possibile creare una propria macchina virtuale in esecuzione in Azure, quindi installare e amministrare MySQL manualmente.

Per creare un database con un piano gratuito, attenersi alla procedura seguente.

1. Accedere al [Portale di Azure].

1. Nella parte superiore del riquadro di spostamento fare clic su **NUOVO**, quindi su **Dati e archiviazione** e infine su **Database MySQL**.

1. Digitare "**mysql**" nella casella di ricerca e quindi fare clic su **Database MySQL** e su **Crea**.

    <!-- ![Choose Add-on Dialog](./media/web-sites-python-ptvs-django-mysql/PollsDjangoClearDBAddon1.png) -->

1. Configurare il nuovo database MySQL creando un nuovo gruppo di risorse e selezionare il percorso appropriato.

    <!-- ![Personalize Add-on Dialog](./media/web-sites-python-ptvs-django-mysql/PollsDjangoClearDBAddon2.png) -->

1. Dopo aver creato il database MySQL, fare clic su **Proprietà** nel pannello del database.

1. Usare il pulsante Copia per inserire il valore della **STRINGA DI CONNESSIONE** negli Appunti.

## Configurare il progetto

In questa sezione verrà configurata l'app Web per usare il database MySQL appena creato. Verranno anche installati i pacchetti Python aggiuntivi necessari per usare i database MySQL con Django, quindi l'app Web verrà eseguita in locale.

1. In Visual Studio aprire **settings.py** dalla cartella *NomeProgetto*. Incollare temporaneamente la stringa di connessione nell'editor. La stringa di connessione è nel formato seguente:

        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>

    Modificare il database predefinito **ENGINE** in modo che usi MySQL e impostare i valori relativi a **NOME**, **UTENTE**, **PASSWORD** e **HOST** dalla **STRINGA DI CONNESSIONE**.

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }


1. In Esplora soluzioni, in **Python Environments** fare clic con il pulsante destro del mouse sull'ambiente virtuale e scegliere **Install Python Package**.

1. Installare il pacchetto `mysql-python` usando **easy\_install**.

    ![Finestra di dialogo per l'installazione del pacchetto](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul nodo del progetto, scegliere **Python** e quindi selezionare **Django Sync DB** (DB sincronizzazione Django).

    Verranno in tal modo create le tabelle per il database MySQL creato nella sezione precedente. Seguire le istruzioni per creare un utente, che non deve necessariamente corrispondere all'utente nel database sqlite creato nella prima sezione di questo articolo.

    ![Finestra della console di gestione di Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1. Eseguire l'applicazione con `F5`. I sondaggi creati con **Create Sample Polls** e i dati inviati mediante voto verranno serializzati nel database MySQL.

## Pubblicare l'app Web nel servizio app di Azure

L'SDK .NET di Azure offre un modo semplice di distribuire l'app Web nel servizio app di Azure.

1. In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sul nodo del progetto e scegliere **Pubblica**.

    ![Finestra di dialogo Pubblica sito Web](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)

1. Fare clic su **App Web di Microsoft Azure**.

1. Fare clic su **Nuovo** per creare una nuova app Web.

1. Compilare i campi seguenti, quindi fare clic su **Crea**:
	- **Nome dell'app Web**
	- **Piano di servizio app**
	- **Gruppo di risorse**
	- **Area**
	- Lasciare **Server database** impostato su **Nessun database**

    <!-- ![Create Site on Microsoft Azure Dialog](./media/web-sites-python-ptvs-django-mysql/PollsCommonCreateWebSite.png) -->

1. Accettare tutte le altre impostazioni predefinite e fare clic su **Pubblica**.

1. L'app Web pubblicata verrà aperto automaticamente nel Web browser. L'app Web dovrebbe funzionare come previsto, usando il database **MySQL** ospitato in Azure.

    ![Web browser](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)

    Congratulazioni. La pubblicazione in Azure dell'app Web basata su MySQL è stata completata.

## Passaggi successivi

Usare i collegamenti seguenti per altre informazioni su Python Tools per Visual Studio, Django e MySQL.

- [Documentazione di Python Tools per Visual Studio]
  - [Progetti Web]
  - [Progetti servizio cloud]
  - [Debug remoto in Microsoft Azure]
- [Documentazione di Django]
- [MySQL]

Per ulteriori informazioni, vedere il [Centro per sviluppatori di Python](/develop/python/).

<!--Link references-->

[Centro per sviluppatori Python]: /develop/python/
[servizi cloud di Azure]: ../cloud-services-python-ptvs.md

<!--External Link references-->

[Portale di Azure]: https://portal.azure.com
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 per Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[VSIX degli esempi di Python Tools 2.2 per Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Strumenti di Azure SDK per VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Strumenti di Azure SDK per VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 a 32 bit]: http://go.microsoft.com/fwlink/?LinkId=517190
[Documentazione di Python Tools per Visual Studio]: http://aka.ms/ptvsdocs
[Debug remoto in Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Progetti Web]: http://go.microsoft.com/fwlink/?LinkId=624027
[Progetti servizio cloud]: http://go.microsoft.com/fwlink/?LinkId=624028
[Documentazione di Django]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo

<!---HONumber=AcomDC_0608_2016-->