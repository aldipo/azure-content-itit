<properties
	pageTitle="Guida per il rinnovo del certificato per gli utenti di Office 365 e Azure AD. | Microsoft Azure"
	description="In questo articolo viene illustrato agli utenti di Office 365 come risolvere i problemi con i messaggi e-mail contenenti la notifica sul rinnovo di un certificato."
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
	ms.date="06/16/2016"
	ms.author="billmath"/>


# Rinnovo dei certificati di federazione per Office 365 e Azure AD

##Panoramica

Per una federazione corretta tra Azure AD e AD FS, è necessario che i certificati usati da AD FS per la firma di token di sicurezza in Azure AD corrispondano alle informazioni configurate in Azure AD. Le eventuali impostazioni non corrispondenti possono comportare l'interruzione della relazione di trust tra AD FS e AAD. Azure AD assicura che tali informazioni rimangano sincronizzate durante la distribuzione di AD FS e Proxy applicazione Web (per l'accesso Extranet).

Questo articolo fornisce informazioni aggiuntive per gestire i certificati per la firma di token e mantenerli sincronizzati con Azure AD nei casi seguenti:

* Non si distribuisce Proxy applicazione Web e quindi i metadati della federazione non sono disponibili nella Extranet.
* Non si usa la configurazione predefinita di AD FS per i certificati per la firma di token.
* Si usa un provider di identità di terze parti.

## Configurazione predefinita di AD FS per i certificati per la firma di token

I certificati di decrittografia token e per la firma di token sono in genere certificati autofirmati e sono validi per un anno. La configurazione predefinita di AD FS per i certificati di decrittografia token e per la firma di token include un processo di rinnovo automatico denominato **AutoCertificateRollover**. Se si utilizza AD FS 2.0 o versioni successive, Office 365 e Azure AD aggiornerà automaticamente il certificato prima della scadenza.

### Notifica di rinnovo: notifica di posta elettronica e avviso nel portale di Office 365

>[AZURE.NOTE] Se si è ricevuta una notifica di posta elettronica o un avviso nel portale con la richiesta di rinnovare il certificato per Office, è possibile seguire la procedura riportata di seguito in [Gestione delle modifiche ai certificati per la firma di token](#managecerts) per verificare se è necessario intraprendere un'azione. Microsoft è a conoscenza di un possibile problema che può comportare l'invio di notifiche all'utente per il rinnovo dei certificati, anche quando non è richiesta alcuna azione.

Azure AD prova a monitorare i metadati della federazione e ad aggiornare i certificati per la firma di token come indicato dai metadati della federazione. 30 giorni prima della scadenza dei certificati per la firma di token, Azure AD verifica se i nuovi certificati sono disponibili eseguendo il poll dei metadati della federazione.

* Se riesce a eseguire il poll dei metadati della federazione e a recupeRare i nuovi certificati, l'utente non riceverà alcuna notifica di posta elettronica né un avviso nel portale di Office 365.
* Se non riesce a recuperare i nuovi certificati per la firma di token, perché i metadati della federazione non sono raggiungibili o perché il rollover automatico dei certificati non è abilitato, invierà una notifica di posta elettronica e visualizzerà un avviso nel portale di Office 365.

![Notifica nel portale di Office 365](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Se si usa AD FS, per garantire la continuità aziendale, verificare che nei server siano state applicate le patch seguenti, in modo evitare che si verifichino errori di autenticazione per problemi noti. Si ridurranno così i problemi noti relativi al server proxy di AD FS per questo periodo di rinnovo e per quelli futuri:
>
>Server 2012 R2 - [Aggiornamento cumulativo per Windows Server: maggio 2014](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 and 2012 - [L'autenticazione tramite proxy ha esito negativo in Windows Server 2012 o Windows Server 2008 R2 SP1](http://support.microsoft.com/kb/3094446)

## Verificare se è necessario aggiornare i certificati <a name="managecerts"></a>

### Passaggio 1: Verificare lo stato di AutoCertificateRollover

Nel Server AD FS aprire Microsoft Powershell. Verificare che il valore AutoCertRollover sia impostato su TRUE.

	Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Se si usa AD FS 2.0, sarà necessario eseguire prima Add-Pssnapin Microsoft.Adfs.Powershell.

### Passaggio 2: Verificare che AD FS e Azure AD siano sincronizzati

Nel server AD FS aprire il prompt di Azure AD Powershell e connettersi ad Azure AD.

>[AZURE.NOTE] Se Azure AD Powershell non è già stato installato, è possibile scaricarlo [qui](https://technet.microsoft.com/library/jj151815.aspx).

	Connect-MsolService

Verificare i certificati configurati in AD FS e le proprietà del trust di Azure AD per il dominio specificato.

	Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Se le identificazioni personali in entrambi gli output corrispondono, significa che i certificati sono sincronizzati con Azure AD.

### Passaggio 3: Verificare se il certificato sta per scadere

Nell'output di Get-MsolFederationProperty o Get-AdfsCertificate verificare la data in "Not after". Se mancano meno di 30 giorni alla scadenza, è necessario intervenire.

### Passaggi successivi

| AutoCertificateRollover | I certificati sincronizzati con Azure AD | I metadati della federazione sono accessibili pubblicamente | Validità | Azione |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Sì | Sì | Sì | - | Nessuna azione necessaria. Vedere [Rinnovare automaticamente il certificato per la firma di token](#autorenew) |
| Sì | No | - | Meno di 15 giorni | Rinnovare immediatamente. Vedere [Rinnovare manualmente il certificato per la firma di token](#manualrenew) |
| No | - | - | Meno di 30 giorni | Rinnovare immediatamente. Vedere [Rinnovare manualmente il certificato per la firma di token](#manualrenew) |

[-] Non è rilevante

## Rinnovare automaticamente il certificato per la firma di token (scelta consigliata) <a name="autorenew"></a>

Se è stato distribuito Proxy applicazione Web che consente di abilitare l'accessp ai metadati della federazione dalla Extranet e si usa la configurazione predefinita di AD FS, ad esempio AutoCertificateRollover è abilitato, **non è necessario eseguire operazioni manuali.** Verificare quanto segue per assicurarsi che l'aggiornamento automatico del certificato possa essere eseguito:

**N. 1 La proprietà AutoCertificateRollover di AD FS deve essere impostata su True**

Ciò indica che AD FS genererà automaticamente nuovi certificati per la firma di token e certificati di decrittografia token prima della scadenza di quelli precedenti.

**N. 2 I metadati della federazione di AD FS sono accessibili pubblicamente**

Verificare che i metadati di federazione siano accessibili pubblicamente, passare all'URL seguente da un computer sulla rete Internet pubblica (all'esterno della rete aziendale):


https://(your_FS_name)/federationmetadata/2007-06/federationmetadata.xml

dove `(your_FS_name) ` viene sostituito con il nome host del servizio federativo usato dall'organizzazione, ad esempio fs.contoso.com. Se si è in grado di verificare entrambe le impostazioni correttamente, non occorre eseguire altre operazioni.

Esempio: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Rinnovare manualmente il certificato per la firma di token<a name="manualrenew"></a>

Si può scegliere di rinnovare manualmente i certificati per la firma di token. Ecco alcuni degli scenari comuni in cui è consigliabile aggiornare manualmente i certificati per la firma di token:
* I certificati per la firma di token non sono certificati autofirmati. Il motivo più comune è che l'organizzazione gestisce i certificati AD FS registrati da un'autorità di certificazione aziendale. 
* La sicurezza di rete non consente la disponibilità pubblica dei metadati della federazione.

In questi scenari, ogni volta che si aggiornano i certificati per la firma di token, è necessario aggiornare anche il dominio di Office 365 con il comando di PowerShell Update-MsolFederatedDomain.

### Passaggi per rinnovare il certificato per la firma di token e aggiornare il trust federativo di Office 365

**Passaggio 1: Verificare che in ADFS siamo disponibili nuovi certificati per la firma di token**

### Configurazione non predefinita
Se è abilitata una configurazione non predefinita di AD FS in cui **AutoCertificateRollover** è impostato su **False**, si usano probabilmente certificati personalizzati (non autofirmati). Per informazioni complete sul rinnovo dei certificati per la firma di token di AD FS, vedere [Linee guida per i clienti che non usano certificati autofirmati di AD FS](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

### I metadati della federazione non sono disponibili pubblicamente
D'altra parte, se **AutoCertificateRollover** è impostato su **True**, ma i metadati della federazione non sono accessibili pubblicamente, assicurarsi prima di tutto che i nuovi certificati per la firma di token siano stati generati da AD FS. Per verificare la disponibilità di nuovi certificati per la firma di token, seguire questi passaggi:

1. Verificare di essere connessi al server AD FS primario.
2. Verificare i certificati di firma correnti in AD FS aprendo una finestra di comando di PowerShell ed eseguendo il comando seguente:

	PS C:\>Get-ADFSCertificate –CertificateType token-signing

	>[AZURE.NOTE] Se si usa AD FS 2.0, sarà necessario eseguire prima Add-Pssnapin Microsoft.Adfs.Powershell.

3. Esaminare l'output del comando in tutti i certificati elencati. Se AD FS ha generato un nuovo certificato, sono presenti due certificati nell'output: uno per cui il valore IsPrimary è True e la data NotAfter è entro 5 giorni e uno per cui IsPrimary è False e NotAfter è circa un anno nel futuro.

4. Se è presente un solo certificato e la data di NotAfter è entro 5 giorni, è necessario generare un nuovo certificato eseguendo i passaggi seguenti.

5. Per generare un nuovo certificato, eseguire il comando seguente al prompt dei comandi di PowerShell: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Verificare l'aggiornamento eseguendo di nuovo il comando seguente: PS C:\>Get-ADFSCertificate –CertificateType token-signing

A questo punto dovrebbero essere elencati due certificati, uno dei quali ha una data NotAfter di circa un anno nel futuro e il valore IsPrimary è False.

**Passaggio 2: Aggiornare i nuovi certificati per la firma di token per il trust di Office 365**

Per aggiornare Office 365 con i nuovi certificati per la firma di token da usare per il trust, seguire questi passaggi.

1.	Aprire il modulo di Microsoft Azure Active Directory per Windows PowerShell.
2.	Eseguire $cred=Get-Credential. Quando questo cmdlet richiede le credenziali, digitare le credenziali dell'account amministratore servizio cloud.
3.	Eseguire Connect-MsolService –Credential $cred. Questo cmdlet consente di connettersi al servizio cloud. La creazione di un contesto che consente di connettersi al servizio cloud è necessaria prima di eseguire i cmdlet aggiuntivi installati dallo strumento.
4.	Se si eseguono questi comandi in un computer che non è il server federativo primario di ADFS, eseguire Set-MSOLAdfscontext -Computer <AD FS primary server>, dove <AD FS primary server> è il nome FQDN interno del server ADFS primario. Questo cmdlet crea un contesto che consente la connessione ad AD FS.
5.	Eseguire Update-MSOLFederatedDomain –DomainName <domain>. Questo cmdlet aggiorna le impostazioni di AD FS nel servizio cloud e configura la relazione di trust tra i due.


>[AZURE.NOTE] Se è necessario supportare più domini di primo livello, ad esempio contoso.com e fabrikam.com, è necessario utilizzare l'opzione SupportMultipleDomain con tutti i cmdlet. Per altre informazioni, vedere[Supporto di più domini per la federazione con Azure AD](active-directory-aadconnect-multiple-domains.md).

## Ripristinare il trust di Azure AD con AAD Connect <a name="connectrenew"></a>

Se la farm AD FS e il trust di Azure AD sono stati installati e configurati con Azure AD Connect, è possibile usare quest'ultimo per rilevare se è richiesta l'esecuzione di eventuali azioni per i certificati per la firma di token. Se occorre rinnovare i certificati, è possibile usare Azure AD Connect per eseguire le operazioni necessarie con pochi e semplici clic.

Per altre informazioni, vedere [Ripristino del trust](./active-directory-aadconnect-federation-management.md#repairing-the-trust)

<!---HONumber=AcomDC_0622_2016-->