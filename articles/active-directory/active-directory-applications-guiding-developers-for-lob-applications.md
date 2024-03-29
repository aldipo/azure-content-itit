<properties
	pageTitle="Azure AD e applicazioni: guida per gli sviluppatori | Microsoft Azure"
	description="Scritto per i professionisti IT, questo articolo include linee guida per l'integrazione delle applicazioni di Azure in Active Directory."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/09/2016"
	ms.author="kgremban"/>

# Azure AD e applicazioni: sviluppare app line-of-business

Questa guida offre una panoramica dello sviluppo di applicazioni line-of-business (LoB) per Azure Active Directory (AD) ed è stata redatta in modo specifico per gli amministratori globali di Active Directory/Office 365.

## Panoramica

La creazione di applicazioni integrate con Azure AD offre agli utenti dell'organizzazione servizi Single Sign-On per Office 365. La disponibilità dell'applicazione in Azure AD consente di avere il controllo dei criteri di autenticazione impostati per l'applicazione. Per altre informazioni sull'accesso condizionale e su come proteggere le applicazioni con l'autenticazione a più fattori (MFA), vedere il documento seguente: [Configurare le regole di accesso](active-directory-conditional-access-azuread-connected-apps.md).

Per usare Azure Active Directory, l'applicazione deve essere registrata. La registrazione dell'applicazione consente agli sviluppatori dell'organizzazione di autenticare i membri dell'organizzazione con Azure AD e richiedere l'accesso alle loro risorse, ad esempio posta elettronica, calendario, documenti e così via.

Qualsiasi membro della directory (non guest) può registrare un'applicazione, operazione nota anche come *creazione di un oggetto applicazione*.

La registrazione di un'applicazione consente a qualsiasi utente di eseguire le operazioni seguenti:

- Ottenere un'identità per l'applicazione riconosciuta da Azure AD
- Ottenere uno o più segreti/chiavi utilizzabili dall'applicazione per l'autenticazione in AD
- Personalizzare l'applicazione con nome, logo e altri elementi nel portale di Azure
- Sfruttare le funzionalità di autorizzazione di Azure AD per la propria app, tra cui:
  - Controllo degli accessi in base al ruolo
  - Azure Active Directory come server di autorizzazione oAuth (proteggere un'API esposta dall'applicazione)

- Dichiarare le autorizzazioni necessarie per il funzionamento dell'applicazione nel modo previsto, tra cui:
	  - Autorizzazioni per l'app (solo amministratori globali). Ad esempio:
	    - Appartenenza ai ruoli in un'altra appartenenza di ruolo o applicazione Azure AD relativa a una risorsa, un gruppo di risorse o una sottoscrizione di Azure
	  - Autorizzazioni delegate (qualsiasi utente). Ad esempio:
	    - (Azure AD) Profilo di accesso e lettura
	    - (Exchange) Lettura di posta elettronica, invio di posta elettronica
	    - (SharePoint) Lettura

> [AZURE.NOTE]Per impostazione predefinita, qualsiasi membro può registrare un'applicazione. Per informazioni su come limitare le autorizzazioni per la registrazione delle applicazioni per membri specifici, vedere il documento [Procedura per l'aggiunta delle applicazioni ad Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Ecco cosa dovrà fare l'amministratore globale per aiutare gli sviluppatori a rendere le loro applicazioni pronte per la produzione:

- Configurare regole di accesso (criteri di accesso/autenticazione a più fattori)
- Configurare l'app per richiedere l'assegnazione di utenti e assegnare gli utenti
- Evitare l'esperienza di autorizzazione utente predefinita

## Configurare regole di accesso

Configurare regole di accesso per ogni applicazione nelle app SaaS. Può trattarsi di richiedere anche l'autenticazione a più fattori oppure solo di consentire l'accesso agli utenti connessi a reti attendibili. I dettagli a questo scopo sono disponibili nel documento [Configurazione di regole di accesso](active-directory-conditional-access-azuread-connected-apps.md).

## Configurare l'app per richiedere l'assegnazione di utenti e assegnare gli utenti

Per impostazione predefinita, l'assegnazione degli utenti non è obbligatoria per consentire l'accesso a un'applicazione. Tuttavia, se l'applicazione espone ruoli o si desidera che venga visualizzata nel pannello di accesso dell'utente, l'assegnazione degli utenti dovrebbe essere richiesta, così come l'assegnazione di utenti e gruppi.

[Richiedere l'assegnazione degli utenti](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Per gli abbonati ad Azure AD Premium o Enterprise Mobility Suite (EMS), è consigliabile sfruttare i gruppi. L'assegnazione di gruppi per l'applicazione consente di delegare le attività di gestione continuativa degli accessi al proprietario del gruppo. È possibile creare il gruppo oppure richiedere al responsabile dell'organizzazione di creare il gruppo con gli strumenti per la gestione dei gruppi.

[Assegnazione di utenti a un'applicazione](active-directory-applications-guiding-developers-assigning-users.md) [Assegnazione di gruppi a un'applicazione](active-directory-applications-guiding-developers-assigning-groups.md)

## Rimuovere l'esperienza di consenso dell'utente

Per impostazione predefinita, ogni utente è sottoposto a un'esperienza di consenso per poter accedere. L'esperienza di consenso, in cui viene chiesto di concedere le autorizzazioni per un'applicazione, può creare confusione per gli utenti che non hanno familiarità con questo tipo di decisioni.

Per le applicazioni attendibili, è possibile semplificare l'esperienza utente dando il consenso per l'applicazione per conto dell'organizzazione.

Per altre informazioni sul consenso dell'utente e l'esperienza di consenso in Azure, vedere [Integrazione di applicazioni con Azure Active Directory](active-directory-integrating-applications.md).

##Articoli correlati

- [Consentire l'accesso remoto sicuro ad applicazioni locali con il proxy di applicazione di Azure AD](active-directory-application-proxy-get-started.md)
- [Anteprima dell'accesso condizionale di Azure per app SaaS](active-directory-conditional-access-azuread-connected-apps.md)
- [Gestione dell'accesso alle app tramite Azure AD](active-directory-managing-access-to-apps.md)
- [Indice di articoli per la gestione di applicazioni in Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0511_2016-->