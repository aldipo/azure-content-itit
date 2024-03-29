<properties
	pageTitle="Anteprima di Azure Active Directory B2C: Domande frequenti | Microsoft Azure"
	description="Domande frequenti su Azure Active Directory B2C"
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
	ms.date="06/27/2016"
	ms.author="swkrish"/>

# Anteprima di Azure Active Directory B2C: Domande frequenti

Questa pagina include le risposte a domande frequenti relative all'anteprima di Azure Active Directory (Azure AD) B2C. Controllarla costantemente per eventuali aggiornamenti.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

### È possibile usare le funzionalità di Azure AD B2C nel tenant di Azure AD esistente per dipendenti aziendali?

Attualmente le funzionalità di Azure AD B2C non possono essere attivate nel tenant di Azure AD esistente. È consigliabile creare un tenant separato per usare le funzionalità di Azure AD B2C per gestire gli utenti.

### È possibile usare Azure AD B2C per fornire l'accesso a piattaforme di social networking (Facebook e Google+) in Office 365?

Non è possibile usare Azure AD B2C con Microsoft Office 365 e in generale per fornire l'autenticazione per app SaaS (Office 365, Salesforce, Workday e così via). Fornisce solo la gestione di identità e accessi per applicazioni Web e per dispositivi mobili destinate agli utenti e non è applicabile a scenari di dipendenti o partner.

### Che cosa sono gli account locali in Azure AD B2C? In che cosa differiscono dagli account aziendali o dell'istituto di istruzione in Azure AD?

In un tenant di Azure AD ogni utente nel tenant (tranne quelli con account Microsoft esistenti) accede tramite un indirizzo di posta elettronica nel formato `<xyz>@<tenant domain>`, dove `<tenant domain>` è uno dei domini verificati nel tenant o il dominio `<...>.onmicrosoft.com` iniziale. Questo tipo di account è un account aziendale o dell'istituto di istruzione.

In un tenant di Azure AD B2C, la maggior parte delle app richiede l'accesso dell'utente con un indirizzo di posta elettronica arbitrario, ad esempio joe@comcast.net, bob@gmail.com, sarah@contoso.com o jim@live.com. Questo tipo di account è un account locale. Attualmente sono supportati anche nomi utente arbitrari (semplici stringhe) come account locali (ad esempio, joe, bob, sarah o jim). Nel servizio Azure AD B2C è possibile scegliere uno di questi due tipi di account locale.

### Quali provider di identità di social networking sono attualmente supportati? Per quali provider è previsto il supporto in futuro?

Sono attualmente supportati Facebook, Google+, LinkedIn e Amazon. Verrà aggiunto il supporto per altri provider di identità di social networking noti, in base alle esigenze dei clienti.

### È possibile configurare ambiti per raccogliere altre informazioni sugli utenti da diversi provider di identità di social networking?

No, ma questa funzionalità verrà implementata in futuro. Gli ambiti predefiniti usati per il gruppo di provider di identità di social networking supportato sono:

- Facebook: email
- Google+: email
- Account Microsoft: profilo di posta elettronica openid
- Amazon: profile
- LinkedIn: r\_emailaddress e r\_basicprofile

### Per essere compatibile con Azure AD B2C, un'applicazione deve essere eseguita in Azure?

No, l'applicazione può essere ospitata ovunque (nel cloud o in locale). Per interagire con Azure AD B2C deve avere la possibilità di inviare e ricevere richieste HTTP su endpoint accessibili pubblicamente.

### Nel caso di più tenant Azure AD B2C, come è possibile gestirli nel portale di Azure?

Ogni tenant di Azure AD B2C ha uno specifico pannello delle funzionalità B2C nel portale di Azure. Per informazioni su come passare al pannello delle funzionalità B2C di un tenant specifico nel portale di Azure, vedere [Anteprima di Azure Active Directory B2C: Registrare l'applicazione](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade). Quando si passa da una directory di Azure AD B2C all'altra nel portale di Azure, il pannello delle funzionalità B2C non rimane aperto nella maggior parte dei browser.

### Come si personalizzano i messaggi di posta elettronica di verifica (il contenuto e il campo "Da:") inviati da Azure AD B2C?

Usare la [funzionalità di personalizzazione della società](../active-directory/active-directory-add-company-branding.md) per personalizzare il contenuto dei messaggi di posta elettronica di verifica. Il campo "Da:" può essere modificato tramite il team di supporto.

### Come si esegue la migrazione di nomi utente, password e profili esistenti dal database personale in Azure AD B2C?

È possibile usare l'API Graph di Azure AD per creare lo strumento di migrazione. Per informazioni dettagliate, vedere l'[esempio relativo all'API Graph](active-directory-b2c-devquickstarts-graph-dotnet.md). In futuro Microsoft fornirà vari strumenti e opzioni di migrazione pronti per l'uso.

### Quali sono i criteri password usati per gli account locali in Azure AD B2C?

I criteri password di Azure AD B2C per gli account locali si basano su quelli di Azure AD. Azure AD B2C usa il livello "avanzato" di complessità della password e password senza scadenza. Per altre informazioni dettagliate, vedere [Criteri password in Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx).

### È possibile usare Azure AD Connect per eseguire la migrazione in Azure AD B2C delle identità utente archiviate in Active Directory locale?

No, Azure AD Connect non è progettato per funzionare con Azure AD B2C. In futuro Microsoft fornirà vari strumenti e opzioni di migrazione pronti per l'uso.

### Azure AD B2C è compatibile con i sistemi CRM come Microsoft Dynamics?

No, per il momento. L'integrazione di questi sistemi verrà implementata in futuro.

### Azure AD B2C è compatibile con SharePoint 2016 locale o versione precedente?

No, per il momento. Azure AD B2C non fornisce i token SAML 1.1 che i portali e le applicazioni e-commerce costruiscono sulla base delle esigenze di SharePoint in locale. Si noti che Azure AD B2C non è usato per lo scenario di condivisione esterna con i partner di SharePoint. A questo scopo vedere l'articolo relativo ad [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx).

### È necessario usare Azure AD B2C o B2B per gestire le identità esterne?

Per altre informazioni su come applicare le funzionalità appropriate agli scenari relativi alle identità esterne, leggere questo articolo sulle [identità esterne](../active-directory/active-directory-b2b-compare-external-identities.md).

### Quali funzionalità di reporting e controllo offre Azure AD B2C? Offre le stesse funzionalità di Azure AD Premium?

No, Azure AD B2C non supporta lo stesso set di report di Azure AD Premium. Per Azure AD B2C verranno presto rilasciate API di reporting e controllo.

### È possibile localizzare l'interfaccia utente delle pagine servite da Azure AD B2C? Quali lingue sono supportate?

Attualmente, Azure AD B2C è ottimizzato solo per l'inglese. Si prevede di implementare le funzionalità di localizzazione appena possibile.

### È possibile usare l'URL personale nelle pagine di iscrizione e accesso servite da Azure AD B2C? È ad esempio possibile sostituire l'URL login.microsoftonline.com con login.contoso.com?

No, per il momento. Questa funzionalità verrà implementata in futuro. Si noti anche che la procedura di verifica del dominio personalizzato nella scheda **Domini** del tenant nel portale di Azure classico non eseguirà questa operazione.

### Come si elimina il tenant di Azure AD B2C?

Attenersi a questa procedura per eliminare il tenant di Azure AD B2C:

- Seguire questa procedura per [passare al pannello delle funzionalità B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) nel portale di Azure.
- Passare ai pannelli **Applicazioni**, **Provider di identità** e **Tutti i criteri** ed eliminare tutte le voci in ognuno.
- Accedere ora al [portale di Azure classico](https://manage.windowsazure.com/) come amministratore della sottoscrizione. Si tratta dello stesso account aziendale o dell'istituto d'istruzione o dello stesso account Microsoft usato per l'iscrizione ad Azure.
- Passare all'estensione di Active Directory a sinistra e fare clic sul nome del tenant B2C.
- Fare clic sulla scheda **Utenti**.
- Selezionare un utente alla volta, escludendo l'utente con il quale si è connessi attualmente, ovvero l'amministratore della sottoscrizione. Fare clic su **Elimina** nella parte inferiore della pagina, quindi su **SÌ** quando richiesto.
- Fare clic sulla scheda **Applicazioni**.
- Selezionare **Applicazioni di proprietà dell'azienda** nel campo a discesa **Mostra** e fare clic sul segno di spunta.
- Verrà indicata un'applicazione denominata **b2c-extensions-app**. Fare clic su **Elimina** nella parte inferiore della pagina, quindi su **SÌ** quando richiesto.
- Passare di nuovo all'estensione di Active Directory e selezionare il tenant di B2C.
- Nella parte inferiore della pagina fare clic su **Elimina**. Seguire le istruzioni sullo schermo per completare il processo.

### È possibile ottenere Azure AD B2C come parte di Enterprise Mobility Suite?

No, Azure AD B2C è un servizio di Azure con pagamento in base al consumo e non fa parte di Enterprise Mobility Suite.

### Come è possibile segnalare problemi relativi ad Azure AD B2C?

Vedere [Anteprima di Azure Active Directory B2C: invio di richieste di supporto](active-directory-b2c-support.md).

### Quando sarà disponibile a livello generale Azure AD B2C?

Non è attualmente possibile fornire una stima riguardo alla data di disponibilità generale.

## Altre informazioni

È consigliabile rivedere le informazioni relative a [vincoli, restrizioni e limitazioni correnti della versione di anteprima](active-directory-b2c-limitations.md).

<!---HONumber=AcomDC_0629_2016-->