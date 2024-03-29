<properties
	pageTitle="Anteprima di Azure Active Directory B2C: configurazione della Reimpostazione password self-service | Microsoft Azure"
	description="Un argomento che dimostra come configurare la reimpostazione della password self-service per gli utenti in Azure Active Directory B2C"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="msmbaldwin"
	editor="curtand"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/06/2016"
	ms.author="swkrish"/>


# Anteprima di Azure Active Directory B2C: Configurare la reimpostazione password self-service per gli utenti

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

La funzione di reimpostazione password self-service consente agli utenti che hanno effettuato la registrazione agli account locali di reimpostare le password in modo indipendente. Questo riduce notevolmente il carico per lo staff di supporto, soprattutto se l'applicazione dispone di milioni di clienti che la utilizzano regolarmente. Attualmente, è supportato solo l’utilizzo di un indirizzo di posta elettronica verificato come metodo di ripristino. Verranno aggiunti metodi di ripristino aggiuntivi (numero di telefono verificato, domande di sicurezza e così via) in futuro.

> [AZURE.NOTE]
Questo articolo si applica alla reimpostazione delle password self-service nel contesto di un criterio di accesso. Se è necessario richiamare dall'app criteri di reimpostazione delle password completamente personalizzabili, vedere [questo articolo](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Per impostazione predefinita, la directory non avrà la reimpostazione password self-service attivata. Usare i passaggi seguenti per attivarla:

1. Accedere al [portale di Azure classico](https://manage.windowsazure.com/) come amministratore della sottoscrizione. Si tratta dello stesso account aziendale o dell'istituto d'istruzione o dello stesso account Microsoft usato per la creazione della directory.
2. Passare all'estensione Active Directory sulla barra di spostamento sul lato sinistro.
3. Trovare la directory nella scheda **Directory** e fare clic su di essa.
4. Fare clic sulla scheda **Configure**.
5. Scorrere verso il basso fino alla sezione **Criteri di reimpostazione password utente** e impostare l'opzione **Utenti abilitati per la reimpostazione della password** su **SÌ**. Si noti che l'opzione **Indirizzo di posta elettronica alternativo** è selezionata. Non modificarla.

    ![Reimpostazione della password self-service](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Fare clic su **Save** nella parte inferiore della pagina. L'operazione è completata.

Per eseguire il test, usare la funzionalità "Esegui adesso" in ogni criterio di accesso che include gli account locali come provider di identità. Nella pagina di accesso dell'account locale in cui si immettono l'indirizzo di posta elettronica e la password o il nome utente e la password, fare clic su **Problemi di accesso all'account?** per verificare l'esperienza dell'utente.

> [AZURE.NOTE]
Le pagine di reimpostazione della password self-service possono essere personalizzate con la [funzionalità di aggiunta di informazioni distintive dell'azienda](../active-directory/active-directory-add-company-branding.md).

<!---HONumber=AcomDC_0608_2016-->