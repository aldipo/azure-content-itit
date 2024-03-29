<properties
	pageTitle="Elenco di compatibilità di federazione di Azure AD"
	description="Questa pagina include provider di identità non Microsoft che possono essere usati per implementare l'accesso Single Sign-On."
	services="active-directory"
	documentationCenter=""
	authors="billmath"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/12/2016"
	ms.author="billmath"/>

# Elenco di compatibilità di federazione di Azure AD
Azure Active Directory offre la sicurezza di accesso Single Sign-On e di applicazione avanzata per Office 365 e altri Microsoft Online Services per implementazioni ibride o solo cloud senza richiedere alcuna soluzione non Microsoft. Analogamente alla maggior parte dei Microsoft Online Services, Office 365 è integrato con Azure Active Directory per i servizi di directory, l'autenticazione e l'autorizzazione. Azure Active Directory offre anche l'accesso Single Sign-On a migliaia di applicazioni SaaS e di applicazioni Web locali. Per le applicazioni SaaS supportate, vedere la raccolta di applicazioni Azure Active Directory.

Per le organizzazioni che hanno investito in soluzioni di federazione non Microsoft, questo argomento include le istruzioni per la configurazione dell'accesso Single Sign-On per gli utenti Active Directory di Windows Server con Microsoft Online Services tramite i provider di identità non Microsoft inclusi nell'elenco di compatibilità di federazione di Azure Active Directory riportato di seguito.

Microsoft ha testato queste esperienze di Single Sign-On usando provider di identità non Microsoft in più casi d'uso comuni con Azure Active Directory.

>[AZURE.IMPORTANT] Microsoft si è limitata a testare la funzionalità di federazione di questi scenari di Single Sign-On. In questi scenari di Single Sign-On, Microsoft non ha eseguito alcun test sui componenti di sincronizzazione, autenticazione a due fattori e così via.

>Anche l'uso dell'accesso tramite ID alternativo all'UPN non è stato testato in questo programma.



- [Azure Active Directory](#azure-active-directory)
- [Virtual Identity Server Federation Services di Optimal IDM](#optimal-idm-virtual-identity-server-federation-services) 
- [PingFederate 6.11](#pingfederate-611) 
- [PingFederate 7.2](#pingfederate-72) 
- [Centrify](#centrify) 
- [IBM Tivoli Federated Identity Manager 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
- [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
- [CA SiteMinder 12.52](#ca-siteminder-1252) 
- [RadiantOne CFS 3.0](#radiantone-cfs-30) 
- [Okta](#okta) 
- [OneLogin](#onelogin) 
- [NetIQ Access Manager 4.0.1](#netiq-access-manager-401) 
- [BIG-IP con Access Policy Manager BIG-IP versione 11.3x – 11.6x](#big-ip-with-access-policy-manager-big-ip-ver-113x-116x) 
- [VMware Workspace Portal versione 2.1](#vmware-workspace-portal-version-21) 
- [Sign&go 5.3](#signampgo-53) 
- [IceWall Federation versione 3.0](#icewall-federation-version-30) 
- [CA Secure Cloud](#ca-secure-cloud) 
- [Dell One Identity Cloud Access Manager v7.1](#dell-one-identity-cloud-access-manager-v71) 
- [AuthAnvil Single Sign On 4.5](#authavil-single-sign-on-45) 

>[AZURE.IMPORTANT] Poiché si tratta di prodotti di terze parti, Microsoft non fornisce alcun supporto per la distribuzione, la configurazione, la risoluzione dei problemi, le procedure consigliate, nonché informazioni su problemi e domande su questi provider di identità. Per supporto e domande relative ai provider di identità, contattare direttamente le terze parti supportate.

>Per testare l'interoperabilità di questi provider di terze parti con i servizi cloud Microsoft sono stati usati solo i protocolli WS-Federation e WS-Trust. Il test non prevedeva l'uso del protocollo SAML.

## Azure Active Directory 
Azure Active Directory può autenticare gli utenti mediante la federazione con Active Directory locale o senza un server federativo locale tramite la sincronizzazione delle password.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso:


| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|
|Applicazioni moderne con ADAL, ad esempio Office 2016| Supportato|Nessuno|

Per altre informazioni sull'uso di Azure Active Directory con AD FS, vedere [Active Directory Federation Services (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

Per altre informazioni sull'uso di Azure Active Directory con la sincronizzazione delle password, vedere [Azure AD Connect](active-directory-aadconnect.md).


## Virtual Identity Server Federation Services di Optimal IDM 
Virtual Identity Server Federation Services di Optimal IDM consente di autenticare utenti che risiedono in Active Directory locali dei clienti.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:


| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Per altre informazioni sui criteri di accesso client, vedere, vedere [Limitare l'accesso ai servizi di Office 365 in base alla posizione del client.](https://technet.microsoft.com/library/hh526961.aspx)|



## PingFederate 6.11 

PingFederate 6.11 implementa il comune standard di identità WS Federation per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:


| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno (per le versioni precedenti è richiesto l'aggiornamento alla versione 6.11)|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per le istruzioni di PingFederate per la configurazione di questo servizio token di sicurezza per offrire l'esperienza Single Sign-On agli utenti di Active Directory, scaricare il file con estensione pdf [qui.](http://go.microsoft.com/fwlink/?LinkID=266321)

## PingFederate 7.2 
PingFederate 7.2 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:


| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per le istruzioni di PingFederate per la configurazione di questo servizio token di sicurezza per offrire l'esperienza Single Sign-On agli utenti di Active Directory, vedere [qui.](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)

## Centrify 
Centrify consente di offrire un'esperienza Single Sign-On federata per Office 365 senza l'hosting di un server federativo locale.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:


| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Controllo di accesso client non supportato 

Per altre informazioni su Centrify, vedere [qui.](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## IBM Tivoli Federated Identity Manager 6.2.2 
IBM Tivoli Federated Identity Manager 6.2.2 con IBM Security Access Manager for Microsoft Applications 1.4 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su IBM Tivoli Federated Identity Manager, vedere [IBM Security Access Manager for Microsoft Applications.](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)

## SecureAuth IdP 7.2.0 
SecureAuth IdP 7.2.0 implementa il comune standard di identità WS Federation/WS-Trust per offrire un'esperienza di accesso Single Sign-On e un framework per lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su SecureAuth, vedere [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293).

## CA SiteMinder 12.52 
CA SiteMinder Federation 12.52 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Autenticazione integrata di Windows|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su CA SiteMinder, vedere [CA SiteMinder Federation.](http://www.ca.com/us/products/ca-single-sign-on.html)

## RadiantOne CFS 3.0 
RadiantOne Cloud Federation Service (CFS) 3.0 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su RadiantOne CFS, vedere [RadiantOne CFS.](http://www.radiantlogic.com/products/radiantone-cfs/)


## Okta 
Okta implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:


| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Per Autenticazione integrata di Windows è richiesta la configurazione di altri server Web e dell'applicazione Okta.|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su Okta, vedere [Okta.](https://www.okta.com/)
 
## OneLogin 
OneLogin, così com'è stato testato nel maggio 2014, implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Autenticazione integrata di Windows|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su OneLogin, vedere [OneLogin.](https://www.onelogin.com/)

## NetIQ Access Manager 4.0.1 
NetIQ Access Manager 4.0.1 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |**Contratti Kerberos supportati|
| Applicazioni rich client, ad esempio Lync, Office Subscription, CRM | Supportato |Autenticazione integrata di Windows non supportata|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuna|

**NetIQ supporta l'autenticazione Kerberos tramite la configurazione di un contratto Kerberos. Per assistenza per questa configurazione, contattare NetIQ o consultare la guida all'installazione. Per altre informazioni su NetIQ Access Manager, vedere [NetIQ Access Manager.](https://www.netiq.com/documentation/netiqaccessmanager4/identityserverhelp/data/b12iqp0m.html)

## BIG-IP con Access Policy Manager BIG-IP versione 11.3x – 11.6x 
BIG-IP con Access Policy Manager, (APM) BIG-IP versione 11.3x – 11.6x implementa il comune standard di identità SAML per offrire un'esperienza di accesso Single Sign-On e un framework per lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Non supportato |Non supportato|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su BIG-IP Access Policy Manager, vedere [BIG-IP Access Policy Manager.](https://f5.com/products/modules/access-policy-manager)

Per le istruzioni di BIG-IP Access Policy Manager per la configurazione di questo servizio token di sicurezza per offrire l'esperienza Single Sign-On agli utenti di Active Directory, scaricare il file con estensione pdf [qui.](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)

## VMware Workspace Portal versione 2.1 
VMware Workspace Portal versione 2.1 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Autenticazione integrata di Windows non supportata.|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows non supportata.|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su VMware Workspace Portal versione 2.1, scaricare il file con estensione pdf [qui](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf).

## Sign&go 5.3 
Sign&go 5.3 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Contratti Kerberos supportati |
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|


Sign&go 5.3 supporta l'autenticazione Kerberos tramite la configurazione di un contratto Kerberos. Per assistenza per questa configurazione, contattare llex o consultare la guida all'installazione disponibile [qui](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf).


## IceWall Federation versione 3.0 
IceWall Federation Version 3.0 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Autenticazione integrata di Windows non supportata.|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows non supportata.|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su IceWall Federation, vedere [qui](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/) e [qui](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html).

## CA Secure Cloud 

CA Secure Cloud implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Autenticazione integrata di Windows non supportata.|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows non supportata.|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su CA Secure Cloud, vedere [CA Secure Cloud.](http://www.ca.com/us/products/security-as-a-service.aspx)

## Dell One Identity Cloud Access Manager v7.1 
Dell One Identity Cloud Access Manager implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Nessuno|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Nessuno|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|

Per altre informazioni su Dell One Identity Cloud Access Manager, vedere [Dell One Identity Cloud Access Manager](http://software.dell.com/products/cloud-access-manager).

 Per istruzioni su come configurare questo servizio token di sicurezza per fornire l'esperienza Single Sign-On agli utenti di Office 365, vedere l'articolo relativo alla [configurazione degli utenti di Office 365](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365).

## AuthAnvil Single Sign On 4.5 
AuthAnvil Single Sign On 4.5 implementa il comune standard di identità WS Federation/WS-Trust per offrire un framework per l'accesso Single Sign-On e lo scambio degli attributi.

Di seguito è riportata la matrice di supporto dello scenario per questa esperienza di accesso Single Sign-On:

| Client |Supporto |Eccezioni|
| --------- | --------- |--------- |
| Client basati sul Web, ad esempio Exchange Web Access e SharePoint Online | Supportato |Autenticazione integrata di Windows non supportata.|
| Applicazioni rich client, ad esempio Lync, Sottoscrizione Office, CRM | Supportato |Autenticazione integrata di Windows non supportata.|
| Client di posta elettronica, ad esempio Outlook e ActiveSync | Supportato |Nessuno|


Per altre informazioni, vedere l'articolo sulla [configurazione dell'accesso Single Sign-On in AuthAnvil](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-).

<!---HONumber=AcomDC_0518_2016-->