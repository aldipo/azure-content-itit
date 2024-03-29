<properties
	pageTitle="Anteprima di Azure Active Directory B2C | Microsoft Azure"
	description="Come compilare app direttamente usando i protocolli supportati dall'anteprima di Azure Active Directory B2C."
	services="active-directory-b2c"
	documentationCenter=""
	authors="dstrockis"
	manager="msmbaldwin"
	editor=""/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/16/2016"
	ms.author="dastrock"/>

# Anteprima AD B2C Azure: Protocolli di autenticazione

Azure Active Directory (Azure AD) B2C fornisce l'identità come servizio per le app grazie al supporto di due protocolli standard del settore, OpenID Connect e OAuth 2.0. Anche se il servizio è conforme agli standard, possono esistere sottili differenze tra le implementazioni di questi protocolli. Le informazioni in questa guida sono utili se si scrive codice inviando e gestendo direttamente le richieste HTTP, anziché usando una libreria open source. Si consiglia di leggere questa pagina prima di approfondire i dettagli dei protocolli specifici. Se si ha già familiarità con Azure AD B2C, è possibile passare direttamente alle [guide di riferimento dei protocolli](#protocols).

<!-- TODO: Need link to libraries above -->

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Nozioni di base
Ogni app che usa Azure AD B2C deve essere registrata nella directory B2C nel [portale di Azure](https://portal.azure.com). Il processo di registrazione app raccoglie e assegna all'app alcuni valori:

- Un **ID applicazione** che identifica l'app in modo univoco.
- Un **URI di reindirizzamento** o un **identificatore di pacchetto** che può essere usato per indirizzare le risposte all'app.
- Altri valori specifici dello scenario. Per altre informazioni, vedere [come registrare l'applicazione](active-directory-b2c-app-registration.md).

Dopo la registrazione, l'app comunica con Azure AD inviando richieste all'endpoint 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

In quasi tutti i flussi di OAuth e OpenID Connect sono coinvolte nello scambio quattro parti:

![Ruoli di OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- Il **server di autorizzazione** è l'endpoint di Azure AD 2.0. Gestisce in modo sicuro tutto ciò che ha a che fare con l'accesso e le informazioni sull'utente, nonché le relazioni di trust tra le parti in un flusso. Verifica l'identità dell'utente, concede e revoca l'accesso alle risorse e rilascia i token. È anche noto come provider di identità.
- Il **proprietario della risorsa** è in genere l'utente finale. È la parte che possiede i dati e può consentire a terze parti di accedere a tali dati o risorse.
- Il **client OAuth** è l'app ed è identificato dal relativo ID applicazione. Si tratta in genere della parte con cui interagiscono gli utenti finali. Richiede i token dal server di autorizzazione. Il proprietario della risorsa deve concedere al client l'autorizzazione ad accedere alla risorsa.
- Il **server della risorsa** è la posizione in cui si trova la risorsa o i dati. Considera attendibile il server di autorizzazione per autenticare e autorizzare il client OAuth in modo sicuro. Usa i token di accesso di connessione per fare in modo che sia possibile concedere l'accesso a una risorsa.

## Criteri
I criteri di Azure AD B2C sono probabilmente la caratteristica più importante del servizio. Azure AD B2C estende i protocolli OAuth 2.0 e OpenID Connect standard con l'introduzione dei criteri. Questi consentono ad Azure AD B2C di andare oltre la semplice autenticazione e autorizzazione. I criteri descrivono in modo completo le esperienze di identità dell'utente, inclusi l'iscrizione, l'accesso e la modifica del profilo. I criteri possono essere definiti in un'interfaccia utente amministrativa ed eseguiti usando un parametro di query speciale nelle richieste di autenticazione HTTP. I criteri non sono una funzionalità standard di OAuth 2.0 e OpenID Connect, è quindi consigliabile cercare di approfondirli. Per altre informazioni, vedere la [guida di riferimento dei criteri di Azure AD B2C](active-directory-b2c-reference-policies.md).

## Tokens
L'implementazione di OAuth 2.0 e OpenID Connect in Azure AD B2C fa un uso intensivo dei token di connessione, inclusi quelli rappresentati come token Web JSON (JWT). Un token di connessione è un token di sicurezza leggero che consente al "portatore" di accedere a una risorsa protetta. Per "portatore" si intende qualsiasi parte che possa presentare il token. Per poter ricevere un token di connessione, Azure AD deve prima autenticare una parte. Se non vengono adottate le misure necessarie per proteggere il token durante la trasmissione e l'archiviazione, potrebbe essere intercettato e usato da parti non autorizzate.

Alcuni token di sicurezza hanno un meccanismo predefinito che ne impedisce l'uso da parti non autorizzate, ma i token di connessione non presentano questo meccanismo. Devono essere trasportati usando un canale protetto, ad esempio il protocollo Transport Layer Security (HTTPS). Se un token di connessione viene trasmesso al di fuori di un canale protetto, un utente malintenzionato potrebbe usare un attacco man-in-the-middle per acquisire il token e usarlo per l'accesso non autorizzato a una risorsa protetta. Gli stessi principi di sicurezza si applicano quando i token di connessione vengono archiviati o memorizzati nella cache per un uso futuro. Assicurarsi sempre che l'app trasmetta e archivi i token di connessione in modo sicuro.

Per altre considerazioni sulla sicurezza dei token di connessione, vedere la [sezione 5 della specifica RFC 6750](http://tools.ietf.org/html/rfc6750).

Informazioni più dettagliate sui diversi tipi di token usati in Azure AD B2C sono disponibili nel [riferimento al token in Azure AD B2C](active-directory-b2c-reference-tokens.md).

## Protocolli

Per esaminare alcuni esempi di richieste, è possibile iniziare con una delle esercitazioni indicate di seguito. Ognuna corrisponde a uno scenario di autenticazione specifico. Per determinare quale sia il flusso più adatto alle esigenze, vedere i [tipi di app che è possibile compilare con Azure AD B2C](active-directory-b2c-apps.md).

- [Compilare applicazioni native e per dispositivi mobili con OAuth 2.0](active-directory-b2c-reference-oauth-code.md).
- [Compilare app Web con OpenID Connect](active-directory-b2c-reference-oidc.md).
- Compilare app a singola pagina con il flusso implicito OAuth 2.0 (presto disponibile).
- Compilare daemon e processi sul lato server con il flusso di credenziali client OAuth 2.0 (presto disponibile).
- Usare nomi utente e password per ottenere i token con il flusso di credenziali del proprietario della risorsa di OAuth 2.0 (presto disponibile).
- Ottenere i token in un'API Web con il flusso On-Behalf-Of di OAuth 2.0 (presto disponibile).

<!---HONumber=AcomDC_0518_2016-->