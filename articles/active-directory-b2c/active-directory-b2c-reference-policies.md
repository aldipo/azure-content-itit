<properties
	pageTitle="Anteprima di Azure Active Directory B2C: Framework di criteri estendibile | Microsoft Azure"
	description="Argomento che descrive il framework di criteri estendibile di Azure Active Directory B2C e che illustra come creare vari tipi di criteri"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="msmbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/06/2016"
	ms.author="swkrish"/>

# Anteprima di Azure Active Directory B2C: Framework di criteri estendibile

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Nozioni di base

Il framework di criteri estendibile di Azure Active Directory (Azure AD) B2C è il punto di forza del servizio. I criteri descrivono le esperienze di identità utente, ad esempio iscrizione, accesso o modifica del profilo. Ad esempio, i criteri di iscrizione consentono di controllare i comportamenti configurando le impostazioni seguenti:

- Tipi di account (account di social network come Facebook o account locali come un indirizzo di posta elettronica) che gli utenti possono usare per effettuare l'iscrizione all'applicazione.
- Attributi (ad esempio nome, codice postale, numero di scarpe) da raccogliere dall'utente durante la procedura di iscrizione.
- Uso di Multi-Factor Authentication.
- Aspetto di tutte le pagine di iscrizione.
- Informazioni (visualizzate come attestazioni in un token) che l'applicazione riceve al termine dell'esecuzione dei criteri.

È possibile creare più criteri di tipi diversi nel proprio tenant e usarli nelle applicazioni in base alle esigenze. I criteri possono essere usati per più applicazioni. In questo modo, gli sviluppatori possono definire e modificare le esperienze di identità degli utenti con modifiche minime o senza modifiche al codice.

Per usare i criteri, è disponibile una semplice interfaccia per sviluppatori. L'applicazione attiva i criteri tramite una richiesta di autenticazione HTTP standard (passando i parametri dei criteri nella richiesta) e riceve un token personalizzato come risposta. Ad esempio, l'unica differenza tra le richieste che richiamano i criteri di iscrizione e i criteri di accesso risiede nel nome del criterio usato nel parametro della stringa di query "p":

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Per altre informazioni sul framework di criteri, vedere questo [post di blog](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## Creare un criterio di iscrizione

Per abilitare l'iscrizione nell'applicazione, è necessario creare i criteri di iscrizione. I criteri descrivono l'esperienza utente durante la procedura di iscrizione e il contenuto dei token che l'applicazione riceverà al completamento dell'iscrizione.

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Criteri di iscrizione**.
3. Fare clic su **+Aggiungi** nella parte superiore del pannello.
4. Il **Nome** determina il nome dei criteri di iscrizione usati dall'applicazione. Ad esempio, immettere "SiUp".
5. Fare clic su **Provider di identità** e selezionare "Iscrizione tramite indirizzo di posta elettronica". Facoltativamente, è anche possibile selezionare i provider di identità tramite social network, se già configurati. Fare clic su **OK**.
6. Fare clic su **Attributi iscrizione**. Qui è possibile scegliere gli attributi che si desidera raccogliere dall'utente durante l'iscrizione. Ad esempio, selezionare "Paese/Area", "Nome visualizzato" e "Codice postale". Fare clic su **OK**.
7. Fare clic su **Attestazioni applicazione**. Qui si scelgono le attestazioni che verranno restituite all'applicazione nei token inviati al termine di una corretta esperienza di iscrizione. Selezionare ad esempio "Nome visualizzato", "Provider di identità", "Codice postale", "L'utente è nuovo" e l'"ID oggetto dell'utente".
8. Fare clic su **Crea**. Si noti che il criterio appena creato è visualizzato come "**B2C\_1\_SiUp**" (il frammento **B2C\_1\_** viene aggiunto automaticamente) nel pannello **Criteri di iscrizione**.
9. Aprire il criterio facendo clic su "**B2C\_1\_SiUp**".
10. Selezionare "Contoso B2C app" nel menu a discesa **Applicazioni** e `https://localhost:44321/` nel menu a discesa **URL di risposta / URI di reindirizzamento**.
11. Fare clic su **Esegui adesso**. Verrà visualizzata una nuova scheda del browser in cui è possibile eseguire l'esperienza utente di iscrizione per l'applicazione.

    > [AZURE.NOTE]
    La creazione e gli aggiornamenti dei criteri avranno effetto dopo circa un minuto.

## Creare un criterio di accesso

Per abilitare l'accesso nell'applicazione, è necessario creare i criteri di accesso. I criteri descrivono l'esperienza utente durante la procedura di accesso e il contenuto dei token che l'applicazione riceverà al completamento dell'accesso.

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Criteri di accesso**.
3. Fare clic su **+Aggiungi** nella parte superiore del pannello.
4. Il **Nome** determina il nome dei criteri di accesso usati dall'applicazione. Ad esempio, immettere "SiIn".
5. Fare clic su **Provider di identità** e selezionare "Accesso tramite account locale". Facoltativamente, è anche possibile selezionare i provider di identità tramite social network, se già configurati. Fare clic su **OK**.
6. Fare clic su **Attestazioni applicazione**. Qui è possibile scegliere le attestazioni che verranno restituite all'applicazione nei token inviati al termine di una corretta esperienza di accesso. Selezionare ad esempio "Nome visualizzato", "Provider di identità", "Codice postale" e l'"ID oggetto dell'utente". Fare clic su **OK**.
7. Fare clic su **Crea**. Si noti che il criterio appena creato è visualizzato come "**B2C\_1\_SiIn**" (il frammento **B2C\_1\_** viene aggiunto automaticamente come prefisso) nel pannello **Criteri di accesso**.
8. Aprire il criterio facendo clic su "**B2C\_1\_SiIn**".
9. Selezionare "Contoso B2C app" nel menu a discesa **Applicazioni** e `https://localhost:44321/` nel menu a discesa **URL di risposta / URI di reindirizzamento**.
10. Fare clic su **Esegui adesso**. Verrà visualizzata una nuova scheda del browser in cui è possibile eseguire l'esperienza utente di accesso all'applicazione.

    > [AZURE.NOTE]
    La creazione e gli aggiornamenti dei criteri avranno effetto dopo circa un minuto.

## Creare un criterio di iscrizione o accesso

Questo criterio consente di gestire le esperienze di iscrizione e accesso dei consumatori tramite una singola configurazione. I consumatori vengono indirizzati sul percorso corretto (iscrizione o accesso) a seconda del contesto. Vengono inoltre descritti i contenuti dei token che l'applicazione riceverà al completamento dell'iscrizione o dell'accesso. Un esempio di codice per i criteri di registrazione o di accesso è [disponibile qui](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Sign-up or sign-in policies** (Criteri di iscrizione o accesso).
3. Fare clic su **+Aggiungi** nella parte superiore del pannello.
4. Il **Nome** determina il nome dei criteri di iscrizione usati dall'applicazione. Ad esempio, immettere "SiUpIn".
5. Fare clic su **Provider di identità** e selezionare "Email signup" (Iscrizione tramite indirizzo di posta elettronica). Facoltativamente, è anche possibile selezionare i provider di identità tramite social network, se già configurati. Fare clic su **OK**.
6. Fare clic su **Attributi iscrizione**. Qui è possibile scegliere gli attributi che si desidera raccogliere dall'utente durante l'iscrizione. Ad esempio, selezionare "Paese/Area", "Nome visualizzato" e "Codice postale". Fare clic su **OK**.
7. Fare clic su **Attestazioni applicazione**. Qui è possibile scegliere le attestazioni che verranno restituite all'applicazione nei token inviati al termine di una corretta esperienza di iscrizione o accesso. Selezionare ad esempio "Nome visualizzato", "Provider di identità", "Codice postale", "L'utente è nuovo" e l'"ID oggetto dell'utente".
8. Fare clic su **Create**. Si noti che il criterio appena creato è visualizzato come "**B2C\_1\_SiUpIn**" (il frammento **B2C\_1\_** viene aggiunto in automatico) nel pannello **Sign-up or sign-in policies** (Criteri di iscrizione o accesso).
9. Aprire il criterio facendo clic su "**B2C\_1\_SiUpIn**".
10. Selezionare "Contoso B2C app" nel menu a discesa **Applicazioni** e `https://localhost:44321/` nel menu a discesa **URL di risposta / URI di reindirizzamento**.
11. Fare clic su **Esegui ora**. Verrà visualizzata una nuova scheda del browser in cui è possibile eseguire l'esperienza utente di iscrizione o accesso all'applicazione, a seconda della configurazione.

    > [AZURE.NOTE]
    La creazione e gli aggiornamenti dei criteri avranno effetto dopo circa un minuto.

## Creare i criteri di modifica del profilo

Per abilitare la modifica del profilo nell'applicazione, è necessario creare i criteri di modifica del profilo. Questi criteri descrivono l'esperienza utente durante la procedura di modifica del profilo e il contenuto dei token che l'applicazione riceverà al completamento della procedura.

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Criteri di modifica del profilo**.
3. Fare clic su **+Aggiungi** nella parte superiore del pannello.
4. Il **Nome** determina il nome dei criteri di modifica del profilo usati dall'applicazione. Ad esempio, immettere "SiPe".
5. Fare clic su **Provider di identità** e selezionare "Indirizzo di posta elettronica". Facoltativamente, è anche possibile selezionare i provider di identità tramite social network, se già configurati. Fare clic su **OK**.
6. Fare clic su **Attributi profilo**. Qui è possibile scegliere gli attributi che l'utente potrà visualizzare e modificare. Ad esempio, selezionare "Paese/Area", "Nome visualizzato" e "Codice postale". Fare clic su **OK**.
7. Fare clic su **Attestazioni applicazione**. Qui si scelgono le attestazioni che verranno restituite all'applicazione nei token inviati al termine di una corretta esperienza di modifica del profilo. Ad esempio, selezionare "Nome visualizzato" e "Codice postale".
8. Fare clic su **Create**. Si noti che il criterio appena creato è visualizzato come "**B2C\_1\_SiPe**" (il frammento **B2C\_1\_** viene aggiunto automaticamente) nel pannello **Profile editing policies** (Criteri di modifica del profilo).
9. Aprire il criterio facendo clic su "**B2C\_1\_SiPe**".
10. Selezionare "Contoso B2C app" nel menu a discesa **Applicazioni** e `https://localhost:44321/` nel menu a discesa **URL di risposta / URI di reindirizzamento**.
11. Fare clic su **Esegui ora**. Verrà visualizzata una nuova scheda del browser in cui è possibile eseguire l'esperienza utente di modifica del profilo nell'applicazione.

    > [AZURE.NOTE]
    La creazione e gli aggiornamenti dei criteri avranno effetto dopo circa un minuto.
    
## Creare i criteri di reimpostazione delle password

Per abilitare una reimpostazione dettagliata delle password nell'applicazione, è necessario creare criteri di reimpostazione delle password. Si noti che l'opzione di reimpostazione delle password a livello di tenant specificata [qui](active-directory-b2c-reference-sspr.md) è comunque applicabile per i criteri di accesso. Questi criteri descrivono l'esperienza utente durante la procedura di reimpostazione delle password e il contenuto dei token che l'applicazione riceverà al completamento della procedura.

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Password reset policies** (Criteri di reimpostazione della password).
3. Fare clic su **+Aggiungi** nella parte superiore del pannello.
4. **Nome** indica il nome dei criteri di reimpostazione delle password usati dall'applicazione. Ad esempio, immettere "SSPR".
5. Fare clic su **Provider di identità** e selezionare "Reset password using email address" (Reimposta la password usando l'indirizzo di posta elettronica). Fare clic su **OK**.
6. Fare clic su **Attestazioni applicazione**. Qui si scelgono le attestazioni che verranno restituite all'applicazione nei token inviati al termine di una corretta esperienza di reimpostazione delle password. Ad esempio, selezionare "ID oggetto dell'utente".
7. Fare clic su **Create**. Si noti che il criterio appena creato viene visualizzato come "**B2C\_1\_SSPR**" (il frammento **B2C\_1\_** viene aggiunto automaticamente) nel pannello **Password reset policies** (Criteri di reimpostazione della password).
8. Aprire il criterio facendo clic su "**B2C\_1\_SSPR**".
9. Selezionare "Contoso B2C app" nel menu a discesa **Applicazioni** e `https://localhost:44321/` nel menu a discesa **URL di risposta / URI di reindirizzamento**.
10. Fare clic su **Esegui ora**. Verrà visualizzata una nuova scheda del browser in cui è possibile eseguire l'esperienza utente di reimpostazione delle password nell'applicazione.

    > [AZURE.NOTE]
    La creazione e gli aggiornamenti dei criteri avranno effetto dopo circa un minuto.

## Risorse aggiuntive

- [Anteprima di Azure Active Directory B2C: configurazione di token, sessione e accesso Single Sign-On](active-directory-b2c-token-session-sso.md).

<!---HONumber=AcomDC_0608_2016-->