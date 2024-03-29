<properties 
	pageTitle="Usare Active Directory per l'autenticazione nel servizio app di Azure" 
	description="Informazioni sulle differenti opzioni di autenticazione e autorizzazione per applicazioni line-of-business distribuite alle app Web del servizio app di Azure" 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="web" 
	ms.date="02/26/2016" 
	ms.author="cephalin"/>

# Utilizzare Active Directory per l'autenticazione in Servizio app di Azure #

[App Web del servizio app di Azure](http://go.microsoft.com/fwlink/?LinkId=529714) consente scenari di applicazione line-of-business aziendali supportando la modalità Single Sign-On (SSO) di utenti che accedono all'applicazione dall'ambiente locale o Internet pubblico. Può essere integrato con [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) o con il servizio token di sicurezza (STS) locale, come Active Directory Federation Services (AD FS), per autenticare gli utenti Active Directory (AD) interni e autorizzarli correttamente.

## Autenticazione e autorizzazione senza problemi ##

Con pochi clic è possibile abilitare l'autenticazione e l'autorizzazione per l'app Web. La configurazione dello stile della casella di testo in ciascuna app Web di Azure fornisce il controllo di accesso di base dell'app Web line-of-business. A tale scopo, prima di garantire agli utenti l'accesso al contenuto dell'app Web, vengono applicati HTTPS e l'autenticazione a un tenant di Azure AD di propria scelta. Per ulteriori informazioni, vedere [Autenticazione/Autorizzazione app Web](https://azure.microsoft.com/blog/2014/11/13/azure-websites-authentication-authorization/).

>[AZURE.NOTE] Questa funzionalità è attualmente in anteprima.

## Implementare manualmente l'autenticazione e l'autorizzazione ##

In molti scenari, si richiede la personalizzazione del comportamento di autenticazione e autorizzazione dell'applicazione, ad esempio, di una pagina di accesso e disconnessione, della logica di autorizzazione personalizzata, del comportamento dell'applicazione multitenant e così via. In tali casi, potrebbe essere più conveniente configurare manualmente l'autenticazione e l'autorizzazione per una maggiore flessibilità delle proprie funzionalità. Di seguito sono disponibili due opzioni principali

-	[AD di Azure](web-sites-dotnet-lob-application-azure-ad.md): è possibile implementare l'autenticazione e l'autorizzazione per l'app Web con AD di Azure. Utilizzando l'AD di Azure come provider di identità, le caratteristiche sono le seguenti:
	-	supporto dei popolari protocolli di autenticazione, come [OAuth 2.0](http://oauth.net/2/), [OpenID Connect](http://openid.net/connect/) e [SAML 2.0](http://en.wikipedia.org/wiki/SAML_2.0). Per l'elenco completo dei protocolli supportati, vedere [Protocolli di autenticazione di Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151124.aspx).
	-	Può usare un provider di identità solo di Azure senza infrastruttura locale.
	-	Può anche configurare la sincronizzazione della directory con un'istanza di AD locale (gestita in locale).
	-	Azure AD con la sincronizzazione della directory dal dominio AD locale abilita un uso SSO uniforme dell'app Web quando gli utenti di AD accedono dalla Intranet e da Internet. Dalla intranet, gli utenti AD possono accedere automaticamente all'app Web tramite Autenticazione integrata. Da Internet, gli utenti AD possono accedere all'app Web utilizzando le credenziali Windows.
	-	Fornisce l'accesso SSO a [tutte le applicazioni supportate da Azure AD](/marketplace/active-directory/), tra cui Azure, Office 365, Dynamics CRM Online, Microsoft InTune e migliaia di applicazioni cloud non Microsoft. 
	-	L'AD di Azure delega la gestione delle applicazioni [relying party](http://en.wikipedia.org/wiki/Relying_party) a ruoli di non amministratore, mentre l'accesso dell'applicazione ai dati della directory sensibili deve essere configurato dagli amministratori globali.
	-	Invia un set di scopi generali dei tipi di attestazioni per tutte le applicazioni relying party. Per l'elenco dei tipi di attestazioni, vedere [Tipi di attestazioni e token supportati](http://msdn.microsoft.com/library/azure/dn195587.aspx). Le attestazioni non sono personalizzabili.
	-	L'[API di Azure AD Graph](http://msdn.microsoft.com/library/azure/hh974476.aspx) abilita l'accesso all'applicazione per i dati della directory in Azure AD.
-	[Servizio token di sicurezza (STS) locale, ad esempio ADFS](web-sites-dotnet-lob-application-adfs.md): è possibile implementare l'autenticazione e l'autorizzazione per l'app Web con un STS locale come ADFS. L'uso di ADFS locale presenta le seguenti caratteristiche:
	-	La topologia ADFS deve essere distribuita in locale, con sovraccarico di costi e gestione.
	-	È preferibile che i criteri aziendali richiedano l'archiviazione dei dati di AD in locale.
	-	Solo gli amministratori di ADFS possono configurare i [trust e le regole di attestazione relying party](http://technet.microsoft.com/library/dd807108.aspx).
	-	Possono gestire le [attestazioni](http://technet.microsoft.com/library/ee913571.aspx) su ogni singola applicazione.
	-	È necessario avere una soluzione separata per accedere ai dati di AD in locale tramite il firewall aziendale.

>[AZURE.NOTE] Per iniziare a usare il servizio app di Azure prima di registrarsi per ottenere un account Azure, andare a [Prova il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751), dove è possibile creare un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.

## Modifiche apportate
* Per una guida relativa al passaggio da Siti Web al servizio app, vedere [Servizio app di Azure e impatto sui servizi di Azure esistenti](http://go.microsoft.com/fwlink/?LinkId=529714)
 

<!---HONumber=AcomDC_0504_2016-->