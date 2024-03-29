<properties
   pageTitle="Informazioni generali sulla sicurezza della gestione delle identità di Azure| Microsoft Azure"
   description="Le soluzioni Microsoft di gestione degli accessi e delle identità consentono all'IT di proteggere l'accesso ad applicazioni e risorse nel data center aziendale e nel cloud, abilitando altri livelli di convalida, ad esempio Multi-Factor Authentication e criteri di accesso condizionale. Questo articolo offre informazioni generali sulle funzionalità di sicurezza principali di Azure per la gestione delle identità."
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/22/2016"
   ms.author="terrylan"/>

# Informazioni generali sulla sicurezza della gestione delle identità di Azure

Le soluzioni Microsoft di gestione degli accessi e delle identità consentono all'IT di proteggere l'accesso ad applicazioni e risorse nel data center aziendale e nel cloud, abilitando altri livelli di convalida, ad esempio Multi-Factor Authentication e criteri di accesso condizionale. Il monitoraggio delle attività sospette tramite funzioni avanzate di report di sicurezza, controllo e avvisi consente di attenuare i potenziali problemi di sicurezza. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) offre l'accesso Single Sign-On a migliaia di app cloud (SaaS) e alle app Web eseguite in locale.

I vantaggi della sicurezza di Azure Active Directory (AD) includono la possibilità di:

- Creare e gestire una singola identità per ogni utente nell'azienda ibrida, mantenendo sincronizzati utenti, gruppi e dispositivi
- Offrire l'accesso Single Sign-On alle applicazioni, incluse migliaia di app SaaS preintegrate
- Abilitare la sicurezza dell'accesso alle applicazioni grazie a Multi-Factor Authentication basata su regole per applicazioni locali e cloud
- Consentire l'accesso remoto sicuro ad applicazioni Web locali con il proxy di applicazione di Azure AD

L'obiettivo di questo articolo è offrire informazioni generali sulle funzionalità di sicurezza principali di Azure per la gestione delle identità. Sono anche disponibili collegamenti ad articoli con informazioni dettagliate su ogni funzionalità.

Questo articolo è incentrato sulle seguenti funzionalità principali di gestione delle identità di Azure:

- Accesso Single Sign-On
- Proxy inverso
- Autenticazione a più fattori
- Monitoraggio della sicurezza, avvisi e report basati su Machine Learning
- Gestione delle identità e dell'accesso degli utenti
- Registrazione del dispositivo
- Privileged Identity Management
- Identity Protection
- Gestione delle identità ibrida

## Accesso Single Sign-On

Single Sign-On (SSO) consente di accedere a tutte le applicazioni e risorse necessarie per le attività aziendali effettuando l'accesso una sola volta con un singolo account utente. Una volta effettuato l'accesso, è possibile accedere a tutte le applicazioni senza dover effettuare l’autenticazione (ad esempio, digitare una password) una seconda volta.

Molte organizzazioni si basano sul software come un servizio (SaaS), ad esempio Office 365, Box e Salesforce, per la produttività dell'utente finale. In passato, il personale IT doveva creare e aggiornare singoli account utente in ogni applicazione SaaS e gli utenti dovevano ricordare una password per ogni applicazione SaaS.

Azure AD estende Active Directory locale nel cloud, consentendo agli utenti di usare il proprio account aziendale principale non solo per accedere ai dispositivi appartenenti a un dominio e alle risorse della società, ma anche a tutte le applicazioni Web e SaaS necessarie per svolgere il lavoro.

Non solo gli utenti non devono più gestire diversi set di nomi utente e password, ma è anche possibile eseguire il provisioning o deprovisioning automatico dell'accesso alle applicazioni in base ai gruppi aziendali e al relativo stato di dipendente. Azure AD introduce controlli della governance di sicurezza e accesso che consentono di gestire in modo centralizzato l'accesso degli utenti tra le applicazioni SaaS.

Altre informazioni:

- [Informazioni generali su Single Sign-On](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](../active-directory/active-directory-appssoaccess-whatis.md)
- [Integrare i servizi Single Sign-On di Azure Active Directory nelle app SaaS](../active-directory/active-directory-sso-integrate-saas-apps.md)

## Proxy inverso

Il proxy di applicazione di Azure AD consente di pubblicare applicazioni locali, ad esempio siti di [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=it-IT&rs=it-IT&ad=US) e app basate su [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx) e [IIS](http://www.iis.net/), all'interno della rete privata e offre l'accesso sicuro agli utenti esterni alla rete. Il proxy di applicazione offre l'accesso remoto e Single Sign-On (SSO) per molti tipi di applicazioni Web locali, con le migliaia di applicazioni SaaS supportate da Azure AD. I dipendenti possono accedere alle app dalla propria abitazione e con i propri dispositivi e possono eseguire l'autenticazione tramite questo proxy basato sul cloud.

Altre informazioni:

- [Abilitazione del proxy di applicazione di Azure AD](../active-directory/active-directory-application-proxy-enable.md)
- [Pubblicare applicazioni mediante il proxy di applicazione AD Azure](../active-directory/active-directory-application-proxy-publish.md)
- [Accesso Single Sign-On con il proxy di applicazione](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Utilizzo di access condizionale](../active-directory/active-directory-application-proxy-conditional-access.md)

## Autenticazione a più fattori
Azure Multi-Factor Authentication (MFA) è un metodo di autenticazione che richiede più di un metodo di verifica e con il quale viene aggiunto un secondo livello di sicurezza critico agli accessi e alle transazioni degli utenti. MFA consente di proteggere l'accesso ai dati e alle applicazioni dell'utente, garantendo al tempo stesso una procedura di accesso semplice. Offre autenticazione avanzata tramite una gamma di opzioni di verifica, ad esempio una telefonata, un SMS, una notifica dell'app per dispositivi mobili o un codice di verifica e token OAuth di terze parti.

Altre informazioni:

- [Autenticazione a più fattori](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Informazioni su Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
- [Come funziona Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## Monitoraggio della sicurezza, avvisi e report basati su Machine Learning

Monitoraggio della sicurezza, avvisi e report basati su Machine Learning che identificano i modelli di accesso non coerenti per contribuire alla protezione dell'azienda. È possibile usare i report di utilizzo e accesso di Azure Active Directory per ottenere informazioni sull'integrità e sicurezza della directory dell'organizzazione. Con queste informazioni un amministratore di directory può stabilire meglio dove potrebbero esserci possibili rischi per la sicurezza in modo da poterne pianificare adeguatamente la riduzione.

Nel portale di Azure classico, i report sono classificati nei modi seguenti:

- Report anomalie: contengono eventi di accesso considerati anomali. L'obiettivo è rendere gli utenti consapevoli di tale attività e consentire loro di essere in grado di stabilire se un evento è sospetto.
- Report applicazioni integrate: offrono informazioni dettagliate sull'uso delle applicazioni cloud nell'organizzazione. Azure Active Directory offre l'integrazione con migliaia di applicazioni cloud.
- Report di errori: segnalano gli errori che possono verificarsi durante il provisioning di account in applicazioni esterne.
- Report specifici dell'utente: visualizzano i dati del dispositivo/dell'attività di accesso per un utente specifico.
- Log attività: contengono un record di tutti gli eventi controllati nelle ultime 24 ore, negli ultimi 7 giorni o negli ultimi 30 giorni, nonché le modifiche alle attività del gruppo e le attività di registrazione e reimpostazione password.

Altre informazioni:

- [Visualizzare i report di accesso e utilizzo](../active-directory/active-directory-view-access-usage-reports.md)
- [Introduzione ad Azure Active Directory Reporting](../active-directory/active-directory-reporting-getting-started.md)
- [Guida alla creazione di report in Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## Gestione delle identità e dell'accesso degli utenti

Azure Active Directory B2C è un servizio di gestione delle identità globale a disponibilità elevata per le applicazioni rivolte agli utenti, con scalabilità fino a centinaia di milioni di identità. Il servizio può essere integrato tra piattaforme mobili e Web. Gli utenti possono accedere a tutte le applicazioni attraverso esperienze personalizzabili usando gli account dei propri social network esistenti o creando nuove credenziali.

In passato, gli sviluppatori di applicazioni che desideravano iscriversi e accedere ai consumer nelle proprie applicazioni dovevano scrivere il proprio codice. E dovevano utilizzare database locali o sistemi per archiviare nomi utente e password. Azure Active Directory B2C offre all'organizzazione un modo migliore per integrare la gestione delle identità degli utenti nelle applicazioni con una piattaforma sicura, basata su standard e un set completo di criteri estendibili.

Con Azure Active Directory B2C, gli utenti possono registrarsi alle applicazioni usando gli account di social networking esistenti (Facebook, Google, Amazon, LinkedIn) o creando nuove credenziali (indirizzo di posta elettronica e password o nome utente e password).

Altre informazioni:

- [Informazioni su Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/)
- [Anteprima di Azure Active Directory B2C: iscrizione e accesso degli utenti alle applicazioni](../active-directory-b2c/active-directory-b2c-overview.md)
- [Anteprima di Azure Active Directory B2C: Tipi di applicazioni](../active-directory-b2c/active-directory-b2c-apps.md)

## Registrazione del dispositivo

Registrazione dispositivo Azure AD rappresenta il fondamento per gli scenari di [accesso condizionale](../active-directory/active-directory-conditional-access-on-premises-setup.md) basato su dispositivo. Quando un dispositivo viene registrato, Registrazione dispositivo Azure Active Directory fornisce al dispositivo un'identità che viene usata per autenticare il dispositivo quando l'utente esegue l'accesso. Il dispositivo autenticato e gli attributi del dispositivo possono quindi essere usati per imporre criteri di accesso condizionale per le applicazioni locali e ospitate nel cloud.

Insieme a una soluzione di gestione dei dispositivi mobili (MDM) come Intune, gli attributi del dispositivo in Azure Active Directory vengono aggiornati con informazioni aggiuntive sul dispositivo. Ciò permette di creare regole di accesso condizionale che subordinano l'accesso dai dispositivi al rispetto dei propri standard di sicurezza e conformità.

Altre informazioni:

- [Introduzione a Registrazione dispositivo Azure Active Directory](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Configurazione dell'accesso condizionale locale usando il servizio Registrazione dispositivo di Azure Active Directory](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Registrazione automatica dei dispositivi con Azure Active Directory per i dispositivi Windows aggiunti a un dominio](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management consente di gestire, controllare e monitorare le identità con privilegi e l'accesso alle risorse in Azure AD e in altri Microsoft Online Services, ad esempio Office 365 o Microsoft Intune.

In alcuni casi gli utenti hanno la necessità di eseguire operazioni privilegiate in risorse di Azure o Office 365 o altre app SaaS. Per questa ragione, è spesso necessario che le organizzazioni concedano agli utenti l'accesso con privilegi permanente in Azure AD. Questo rappresenta un rischio di sicurezza crescente per le risorse ospitate nel cloud poiché le organizzazioni non sono in grado di monitorare completamente le operazioni eseguite dagli utenti con i privilegi amministrativi. Inoltre, se un account utente con accesso privilegiato è compromesso, tale singola violazione può compromettere la sicurezza dell'intero cloud. Gestione identità con privilegi di Azure AD consente di risolvere questo rischio.

Azure AD Privileged Identity Management consente di effettuare le operazioni seguenti:

- Individuare gli utenti amministratori di Azure AD
- Abilitare l'accesso come amministratore Just-In-Time su richiesta ai Microsoft Online Services, ad esempio Office 365 e Intune
- Ottenere report sulla cronologia degli accessi degli amministratori e sulle modifiche alle assegnazioni degli amministratori
- Ricevere avvisi relativi all'accesso a un ruolo con privilegi

Altre informazioni:

- [Gestione identità con privilegi di Azure AD](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Ruoli in Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD Privileged Identity Management: Come aggiungere o rimuovere un ruolo utente](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## Identity Protection
Azure AD Identity Protection è un servizio di sicurezza che offre un quadro consolidato degli eventi di rischio e delle potenziali vulnerabilità che interessano le identità dell'organizzazione. Identity Protection si avvale delle funzionalità esistenti di rilevamento anomalie di Azure Active Directory, disponibili tramite i report Anomalie dell'attività di Azure AD, e introduce nuovi tipi di eventi di rischio che consentono di rilevare anomalie in tempo reale.

Altre informazioni:

- [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD and Identity Show: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## Gestione delle identità ibrida

L'approccio di Microsoft alle identità abbraccia funzionalità locali e basate sul cloud, creando una singola identità utente per l’autenticazione e l’autorizzazione per tutte le risorse, indipendentemente dalla loro posizione.

Altre informazioni:

- [White paper sull'identità ibrida](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Blog del team di Active Directory](https://blogs.technet.microsoft.com/ad/)

<!---HONumber=AcomDC_0525_2016-->