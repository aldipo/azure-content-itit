<properties 
	pageTitle="Altre informazioni: Gestione delle password di Azure AD | Microsoft Azure" 
	description="Argomenti avanzati sulla gestione delle password di Azure Active Directory, ad esempio come funziona il writeback delle password, sicurezza del writeback delle password, come funziona il portale di reimpostazione delle password e quali dati vengono usati per la reimpostazione della password." 
	services="active-directory" 
	documentationCenter="" 
	authors="asteen" 
	manager="kbrint" 
	editor="billmath"/>

<tags 
	ms.service="active-directory" 
	ms.workload="identity" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/16/2016" 
	ms.author="asteen"/>

# Altre informazioni sulla gestione delle password
Se si è già distribuita la funzionalità di gestione delle password o se si è semplicemente interessati agli aspetti tecnici essenziali del funzionamento prima della distribuzione, in questa sezione viene fornita una panoramica dei concetti tecnici alla base del servizio. Verranno trattati gli argomenti seguenti:

* [**Panoramica del writeback delle password**](#password-writeback-overview)
  - [Funzionamento del writeback delle password](#how-password-writeback-works)
  - [Scenari supportati per il writeback delle password](#scenarios-supported-for-password-writeback)
  - [Modello di sicurezza del writeback della password](#password-writeback-security-model)
* [**Funzionamento del portale di reimpostazione delle password**](#how-does-the-password-reset-portal-work)
  - [Dati usati per la reimpostazione della password](#what-data-is-used-by-password-reset)
  - [Come accedere ai dati di reimpostazione della password per gli utenti](#how-to-access-password-reset-data-for-your-users)

## Panoramica del writeback delle password
Writeback password è un componente di [Azure Active Directory Connect](active-directory-aadconnect) che può essere abilitato e usato dagli attuali sottoscrittori di Azure Active Directory Premium. Per altre informazioni, vedere [Edizioni di Azure Active Directory](active-directory-editions.md).

Il writeback delle password consente di configurare il tenant cloud per scrivere automaticamente le password nuovamente in Active Directory locale. Evita di dover configurare e gestire una complessa soluzione di reimpostazione della password self-service locale, e offre agli utenti un modo pratico e basato sul cloud per reimpostare le password locali ovunque essi si trovino. Di seguito sono riportate altre informazioni su alcune delle funzionalità principali del writeback delle password:

- **Feedback immediato.** Il writeback delle password è un'operazione sincrona. Gli utenti saranno informati immediatamente se la password non soddisfa i criteri o non può essere reimpostata o modificata per qualsiasi motivo.
- **Supporta la reimpostazione delle password per gli utenti con ADFS o altre tecnologie di federazione.** Con il writeback delle password gli account utente federati potranno gestire le proprie password AD locali dal cloud, purché siano sincronizzati nel tenant di Azure AD.
- **Supporta la reimpostazione delle password per gli utenti con la sincronizzazione degli hash delle password.** Quando il servizio di reimpostazione delle password rileva che un account utente sincronizzato è abilitato per la sincronizzazione degli hash delle password, la password locale e la password cloud dell'account vengono reimpostate simultaneamente.
- **Supporta la modifica delle password dal pannello di accesso e da Office 365.** Quando gli utenti federati o sincronizzati con password modificano le password scadute o non scadute, queste verranno scritte anche nell'ambiente AD locale.
- **Supporta il writeback delle password in caso di reimpostazione da parte degli amministratori nel ** [**portale di gestione di Azure**](https://manage.windowsazure.com). Quando un amministratore reimposta la password di un utente nel [portale di gestione di Azure](https://manage.windowsazure.com), se tale utente è federato o sincronizzato con password, la password selezionata dall'amministratore verrà impostata anche nell'ambiente AD locale. Ciò non è attualmente supportato nel portale di amministrazione di Office.
- **Applica i criteri password AD locali.** Quando un utente Reimposta la propria password, verificare che soddisfi i criteri AD locali prima di eseguirne il commit a tale directory. Sono inclusi cronologia, complessità, validità, filtri delle password e altre restrizioni per le password definite in AD locale.
- **Non richiede regole del firewall per il traffico in ingresso.** Per il writeback delle password viene usato Inoltro del bus di servizio di Azure come canale di comunicazione sottostante. In questo modo non è necessario aprire porte in ingresso nel firewall per usare questa funzionalità.
- **Non è supportato per gli account utente esistenti nei gruppi protetti in Active Directory locale.** Per altre informazioni sui gruppi protetti, vedere l'articolo relativo ad [account e gruppi protetti in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### Funzionamento del writeback delle password
Il writeback delle password include tre componenti principali:

- Servizio cloud Reimpostazione password (integrato anche nelle pagine di modifica delle password di Azure AD)
- Inoltro del bus di servizio di Azure specifico del tenant
- Endpoint di reimpostazione delle password locale

Interagiscono come descritto nel diagramma seguente:

  ![][001]

Quando un utente federato e con sincronizzazione di hash della password reimposta o modifica la propria password nel cloud, si verifica quanto segue:

1.	Viene verificato il tipo di password usato dall'utente. Se risulta che la password è gestita in locale, viene verificato se il servizio di writeback è in esecuzione. In questo caso viene consentito all'utente di procedere; in caso contrario, l'utente viene informato che la password non può essere reimpostata qui.
2.	L'utente supera quindi i controlli di autenticazione appropriati e raggiunge la schermata Reimposta password.
3.	L'utente seleziona una nuova password e la conferma.
4.	Facendo clic su Invia, la password non crittografata viene crittografata con una chiave simmetrica creata durante il processo di installazione della funzionalità di writeback.
5.	Dopo essere stata crittografata, la password viene inclusa in un payload inviato tramite un canale HTTPS al servizio Inoltro del bus di servizio specifico del tenant (che viene configurato automaticamente durante il processo di installazione della funzionalità di writeback). L'inoltro è protetto da una password generata casualmente, nota solo all'installazione locale.
6.	Dopo che il messaggio avrà raggiunto il bus di servizio, l'endpoint di reimpostazione della password si attiva automaticamente e rileva la richiesta di reimpostazione in sospeso.
7.	Il servizio cerca quindi l'utente in questione usando l'attributo di ancoraggio cloud. Perché la ricerca abbia esito positivo, l'oggetto utente deve esistere nello spazio connettore AD ed essere collegato all'oggetto MV corrispondente e all'oggetto connettore AAD corrispondente. Infine, la sincronizzazione individuerà l'account utente solo se il collegamento dell'oggetto connettore AD all'oggetto MV include la regola di sincronizzazione `Microsoft.InfromADUserAccountEnabled.xxx`. Questo è necessario perché, alla ricezione della chiamata dal cloud, il motore di sincronizzazione usa l'attributo cloudAnchor per cercare l'oggetto spazio connettore AAD, quindi segue a ritroso il collegamento all'oggetto MV e quindi all'oggetto AD. Poiché è possibile che siano presenti più oggetti AD (più foreste) per lo stesso utente, il motore di sincronizzazione si basa sul collegamento `Microsoft.InfromADUserAccountEnabled.xxx` per scegliere quello corretto.
8.	Dopo aver trovato l'account utente, viene tentata la reimpostazione della password direttamente nella foresta Active Directory appropriata.
9.	Se l'operazione di impostazione della password riesce, l'utente viene informato che la password è stata modificata e che può proseguire.
10.	Se l'operazione di impostazione della password non riesce, l'errore viene restituito all'utente, consentendogli di riprovare. L'operazione potrebbe non riuscire perché il servizio è inattivo, la password selezionata non soddisfa i criteri dell'organizzazione, l'utente non viene trovato in Active Directory locale o per altri motivi. È disponibile un messaggio specifico per molti di questi casi e all'utente viene indicato cosa può fare per risolvere il problema.

### Scenari supportati per il writeback delle password
La tabella seguente descrive quali scenari sono supportati per le versioni delle nostre funzionalità di sincronizzazione. In generale, è consigliabile installare la versione più recente di [Azure AD Connect](active-directory-aadconnect.md#download-azure-ad-connect) se si vuole usare il writeback delle password.

  ![][002]

### Modello di sicurezza del writeback della password
Il writeback delle password è un servizio altamente sicuro e affidabile. Per garantire che le informazioni siano protette, viene abilitato un modello di protezione a 4 livelli descritto di seguito.

- **Inoltro del bus di servizio specifico del tenant**: quando si configura il servizio, viene configurato automaticamente Inoltro del bus di servizio specifico del tenant, protetto da una password complessa generata in modo casuale, a cui Microsoft non può mai accedere.
- **Chiave di crittografia bloccata e crittograficamente complessa per le password**: dopo la creazione di Inoltro del bus di servizio, viene creata una chiave asimmetrica complessa, usata per la crittografia della password durante la trasmissione. Questa chiave si trova solo nell'archivio segreto dell’azienda nel cloud, bloccato in modo sicuro e controllato, come qualsiasi password nella directory.
- **TLS standard di settore**: quando nel cloud si verifica un'operazione di reimpostazione o modifica della password, la password non crittografata viene crittografata automaticamente con la chiave pubblica dell'utente. Viene quindi inclusa in un messaggio HTTPS inviato tramite un canale crittografato con certificati SSL Microsoft a Inoltro del bus di servizio. Dopo l'arrivo del messaggio arriva nel bus di servizio, l'agente locale si riattiva, si autentica con il bus di servizio usando la password complessa generata in precedenza, preleva il messaggio crittografato, lo decrittografa con la chiave privata generata e quindi tenta di impostare la password tramite l'API SetPassword di Servizi di dominio Active Directory. Questo passaggio consente di applicare i criteri password locali di AD (complessità, validità, cronologia, filtri e così via) nel cloud.
- **Criteri di scadenza del messaggio**: se per qualche motivo il messaggio rimane nel bus di servizio poiché il servizio locale non è disponibile, verrà infine applicato il timeout e il messaggio verrà rimosso dopo alcuni minuti, per assicurare una sicurezza maggiore.

## Funzionamento del portale di reimpostazione delle password
Quando un utente accede al portale di reimpostazione della password, viene avviato un flusso di lavoro per stabilire se l'account utente è valido e se l'utente dispone della licenza per usare la funzionalità e per determinare l'organizzazione a cui l'utente appartiene e la posizione in cui viene gestita la password dell'utente. Leggere i passaggi seguenti per informazioni sulla logica alla base della pagina di reimpostazione della password.

1.	L'utente fa clic sul collegamento relativo all'impossibilità di accedere al proprio account o accede direttamente alla pagina [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.	L'utente immette un ID utente e un captcha.
3.	Azure AD verifica se l'utente è in grado di usare questa funzionalità tramite le seguenti operazioni:
    - Verifica che questa funzionalità sia abilitata per l'utente e che all'utente sia assegnata una licenza Azure AD.
        - Se la funzionalità non è abilitata o se l'utente non dispone della licenza, per reimpostare la password l'utente deve rivolgersi al proprio amministratore.
    - Verifica che per l'account dell'utente siano stati definiti i dati di test corretti, in conformità con i criteri dell'amministratore.
        - Se i criteri prevedono un solo test, viene verificato se per l'utente sono definiti i dati appropriati per almeno uno dei test abilitati dai criteri dell'amministratore.
          - Se l'utente non è configurato, verrà invitato a contattare l'amministratore per reimpostare la password.
        - Se i criteri prevedono due test, viene verificato se per l'utente sono definiti i dati appropriati per almeno due dei test abilitati dai criteri dell'amministratore.
          - Se l'utente non è configurato, verrà invitato a contattare l'amministratore per reimpostare la password.
    - Verifica se la gestione della password dell'utente viene eseguita in locale o meno (con federazione o con sincronizzazione degli hash delle password).
       - Se il writeback è stato distribuito e la password dell'utente è gestita in locale, l'utente può continuare con il processo di autenticazione e di reimpostazione della password.
       - Se il writeback non è stato distribuito e la password dell'utente viene gestita in locale, all'utente viene richiesto di contattare l'amministratore per reimpostare la password.
4.	Se viene stabilito che l'utente è in grado di reimpostare la password, viene avviata la procedura di reimpostazione.

Per altre informazioni su come distribuire il writeback delle password, vedere [Introduzione alla gestione delle password](active-directory-passwords-getting-started.md).

### Dati usati per la reimpostazione della password
La seguente tabella indica il punto e la modalità d'uso dei dati durante la reimpostazione della password e consente di decidere quali opzioni di autenticazione sono appropriate per l'organizzazione. La tabella illustra inoltre i requisiti di formattazione per i casi in cui vengono forniti dati per conto di utenti da percorsi di input che non prevedono la convalida dei dati.

> [AZURE.NOTE] Il metodo di contatto basato sul telefono dell'ufficio non viene visualizzato nel portale di registrazione perché gli utenti non sono attualmente in grado di modificare questa proprietà nella directory.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Nome del metodo di contatto</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Elemento dati di Azure Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Punto di uso e impostazione</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Requisiti relativi al formato</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefono ufficio</p>
            </td>
            <td>
              <p>PhoneNumber</p>
              <p>ad esempio: Set-MsolUser -UserPrincipalName JWarner@contoso.com -PhoneNumber "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Usato in:</p>
              <p>Portale di reimpostazione delle password</p>
              <p>Impostabile in:</p>
              <p>PhoneNumber è impostabile in PowerShell, DirSync, nel portale di gestione di Azure e nel portale di amministrazione di Office</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (ad esempio +1 1234567890)</p>
              <ul>
                <li class="unordered">
										Deve includere un codice paese.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Deve includere un prefisso (dove applicabile).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Deve includere un segno + prima del codice paese.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Deve includere uno spazio tra il codice paese e il resto del numero.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Gli interni non sono supportati; gli eventuali interni specificati verranno cancellati dal numero prima di effettuare la chiamata.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Cellulare</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OPPURE</p>
              <p>MobilePhone</p>
              <p>Il campo Telefono per l'autenticazione viene usato se sono presenti dati, in caso contrario viene usato il campo relativo al telefono cellulare.</p>
              <p>ad esempio: Set-MsolUser -UserPrincipalName JWarner@contoso.com -MobilePhone "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Usato in:</p>
              <p>Portale di reimpostazione delle password</p>
              <p>Portale di registrazione</p>
              <p>Impostabile in: </p>
              <p>AuthenticationPhone è impostabile nel portale di registrazione per la reimpostazione della password o nel portale di registrazione per l'autenticazione a più fattori.</p>
              <p>MobilePhone è impostabile in PowerShell, DirSync, nel portale di gestione di Azure e nel portale di amministrazione di Office</p>
            </td>
            <td>
              <p>+ccc xxxyyyzzzz (ad esempio +1 1234567890)</p>
              <ul>
                <li class="unordered">
										Deve includere un codice paese.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Deve includere un prefisso (dove applicabile).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Deve includere un segno + prima del codice paese.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Deve includere uno spazio tra il codice paese e il resto del numero.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Gli interni non sono supportati; gli eventuali interni specificati verranno ignorati quando viene effettuata la chiamata.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Indirizzo di posta elettronica alternativo</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OPPURE</p>
              <p>AlternateEmailAddresses[0] </p>
              <p>Il campo Indirizzo di posta elettronica per l'autenticazione viene usato se sono presenti dati, in caso contrario viene usato il campo Indirizzo di posta elettronica alternativo.</p>
              <p>Nota: il campo Indirizzo di posta elettronica alternativo è specificato come una matrice di stringhe nella directory. In questa matrice viene usata la prima voce.</p>
              <p>ad esempio: Set-MsolUser -UserPrincipalName JWarner@contoso.com -AlternateEmailAddresses "email@live.com"</p>
            </td>
            <td>
              <p>Usato in:</p>
              <p>Portale di reimpostazione delle password</p>
              <p>Portale di registrazione</p>
              <p>Impostabile in: </p>
              <p>AuthenticationEmail è impostabile nel portale di registrazione per la reimpostazione della password o nel portale di registrazione per l'autenticazione a più fattori.</p>
              <p>AlternateEmail è impostabile in PowerShell, nel portale di gestione di Azure e nel portale di amministrazione di Office.</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a> o 甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
										I messaggi di posta elettronica devono seguire la formattazione standard prevista.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
										Gli indirizzi di posta elettronica Unicode sono supportati.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Domande di sicurezza e risposte</p>
            </td>
            <td>
              <p>Non disponibili per la modifica diretta nella directory.</p>
            </td>
            <td>
              <p>Usato in:</p>
              <p>Portale di reimpostazione delle password</p>
              <p>Portale di registrazione </p>
              <p>Impostabile in: </p>
              <p>Le domande di sicurezza possono essere impostate solo nel portale di gestione di Azure.</p>
              <p>Le risposte alle domande di sicurezza per un determinato utente possono essere impostate solo nel portale di registrazione.</p>
            </td>
            <td>
              <p>Le domande di sicurezza possono essere costituite da un numero di caratteri compreso tra 3 e 200.</p>
              <p>Le risposte di sicurezza possono essere costituite da un numero di caratteri compreso tra 3 e 40.</p>
            </td>
          </tr>
        </tbody></table>

###Come accedere ai dati di reimpostazione della password per gli utenti
####Dati configurabili tramite la sincronizzazione
I campi seguenti possono essere sincronizzati in locale:

* Cellulare
* Telefono ufficio

####Dati impostabili con Azure AD PowerShell
I campi seguenti sono accessibili con Azure AD PowerShell e l'API Graph:

* Indirizzo di posta elettronica alternativo
* Cellulare
* Telefono ufficio
* Telefono per l'autenticazione
* Indirizzo di posta elettronica per l'autenticazione

####Dati impostabili solo con l'interfaccia utente della registrazione
I campi seguenti sono accessibili solo tramite l'interfaccia utente della registrazione SSPR (https://aka.ms/ssprsetup):

* Domande di sicurezza e risposte

####Cosa accade quando un utente si registra?
Quando un utente si registra, i campi seguenti verranno **sempre** impostati nella pagina di registrazione:

* Telefono per l'autenticazione
* Indirizzo di posta elettronica per l'autenticazione
* Domande di sicurezza e risposte

Se è stato specificato un valore per **Cellulare** o **Indirizzo di posta elettronica alternativo**, gli utenti possono usare immediatamente queste informazioni per reimpostare le password, anche se hanno effettuato la registrazione per il servizio. Inoltre, gli utenti visualizzano (e possono modificare) tali valori quando si registrano per la prima volta. Tuttavia, dopo aver completato la registrazione, questi valori vengono salvati in modo permanente rispettivamente nei campi **Telefono per l'autenticazione** e **Indirizzo di posta elettronica per l'autenticazione**.

Ciò può risultare utile per sbloccare l'uso di SSPR da parte di un numero elevato di utenti pur consentendo loro di convalidare tali informazioni tramite il processo di registrazione.

####Impostazione dei dati di reimpostazione delle password con PowerShell
È possibile impostare i valori dei campi seguenti con Azure AD PowerShell.

* Indirizzo di posta elettronica alternativo
* Cellulare
* Telefono ufficio

Per iniziare, è prima necessario [scaricare e installare il modulo di Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Al termine dell'installazione è possibile eseguire la procedura seguente per configurare ogni campo.

#####Indirizzo di posta elettronica alternativo
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####Cellulare
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####Telefono ufficio
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####Lettura dei dati di reimpostazione delle password con PowerShell
È possibile leggere i valori dei campi seguenti con Azure AD PowerShell.

* Indirizzo di posta elettronica alternativo
* Cellulare
* Telefono ufficio
* Telefono per l'autenticazione
* Indirizzo di posta elettronica per l'autenticazione

Per iniziare, è prima necessario [scaricare e installare il modulo di Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Al termine dell'installazione è possibile eseguire la procedura seguente per configurare ogni campo.

#####Indirizzo di posta elettronica alternativo
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####Cellulare
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####Telefono ufficio
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####Telefono per l'autenticazione
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####Indirizzo di posta elettronica per l'autenticazione
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## Collegamenti alla documentazione relativa alla reimpostazione della password
Di seguito vengono forniti collegamenti a tutte le pagine della documentazione relative alla reimpostazione della password in Azure AD:

* [**Reimpostare la password personale**](active-directory-passwords-update-your-own-password.md): informazioni su come reimpostare o modificare la password personale come utente del sistema
* [**Funzionamento**](active-directory-passwords-how-it-works.md): informazioni sui sei diversi componenti del servizio e sulle relative funzioni
* [**Introduzione**](active-directory-passwords-getting-started.md): informazioni su come consentire agli utenti di reimpostare e modificare le password cloud o locali
* [**Personalizzazione**](active-directory-passwords-customize.md): informazioni su come personalizzare l'aspetto e il comportamento del servizio in base alle esigenze dell'organizzazione
* [**Procedure consigliate**](active-directory-passwords-best-practices.md): informazioni su come distribuire rapidamente e gestire in modo efficace le password nell'organizzazione
* [**Informazioni dettagliate**](active-directory-passwords-get-insights.md): informazioni sulle funzionalità di creazione report integrate
* [**Domande frequenti**](active-directory-passwords-faq.md): risposte alle domande frequenti
* [**Risoluzione dei problemi**](active-directory-passwords-troubleshoot.md): informazioni su come risolvere rapidamente eventuali problemi con il servizio



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"

<!---HONumber=AcomDC_0218_2016-->