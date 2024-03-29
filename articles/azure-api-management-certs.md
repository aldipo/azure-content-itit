<properties 
	pageTitle="Caricare un certificato dell'API di gestione di Azure | Microsoft Azure" 
	description="Informazioni su come caricare il certificato di gestione API per il portale di Azure classico." 
	services="cloud-services" 
	documentationCenter=".net" 
	authors="Thraka" 
	manager="timlt" 
	editor=""/>

<tags 
	ms.service="na" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/18/2016"
	ms.author="adegeo"/>


# Caricare un certificato di gestione dell'API di gestione di Azure

I certificati di gestione consentono di eseguire l'autenticazione con l'API di gestione dei servizi fornita da Azure. Molti programmi e strumenti, ad esempio Visual Studio o Azure SDK, useranno questi certificati per automatizzare la configurazione e la distribuzione dei vari servizi di Azure. **Si applica solo al portale di Azure classico**.

>[AZURE.WARNING] Fare attenzione. Questi tipi di certificati consentono a chiunque si autentichi di gestire la sottoscrizione a cui sono associati.

Se necessario, sono [disponibili](cloud-services/cloud-services-certs-create.md#what-are-management-certificates) altre informazioni sui certificati di Azure, compresa la creazione di un certificato autofirmato.

È inoltre possibile usare [Azure Active Directory](/services/active-directory/) per autenticare il codice client per scopi di automazione.

## Creare un certificato di gestione

Dopo aver creato un certificato di gestione (file con estensione cer solo con la chiave pubblica), è possibile caricarlo nel portale. Quando il certificato è disponibile nel portale, chiunque disponga di un certificato corrispondete (chiave privata) può connettersi tramite l'API di gestione e accedere alle risorse per la sottoscrizione associata.

1. Accedere al [portale di Azure classico](http://manage.windowsazure.com).
2. Fare clic su **impostazioni** sul lato sinistro del portale (potrebbe essere necessario scorrere verso il basso). 
    
    ![Impostazioni](./media/azure-api-management-certs/settings.png)

3. Scegliere la scheda **Certificati di gestione**.

    ![Impostazioni](./media/azure-api-management-certs/certificates-tab.png)
    
4. Fare clic sul pulsante **Upload**.

    ![Impostazioni](./media/azure-api-management-certs/upload.png)
    
5. Compilare le informazioni della finestra di dialogo e fare clic sul **segno di spunta** al completamento.

    ![Impostazioni](./media/azure-api-management-certs/upload-dialog.png)

## Passaggi successivi

Ora che si dispone di un certificato di gestione associato a una sottoscrizione, è possibile (dopo aver installato il certificato corrispondente in locale) connettersi a livello di codice alla [API REST di Gestione dei servizi](https://msdn.microsoft.com/library/azure/mt420159.aspx) e automatizzare le varie risorse di Azure che possono essere associate a tale sottoscrizione.

<!---HONumber=AcomDC_0511_2016-->