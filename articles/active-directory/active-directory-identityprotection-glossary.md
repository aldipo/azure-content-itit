<properties
	pageTitle="Glossario di Azure Active Directory Identity Protection | Microsoft Azure"
	description="Glossario di Azure Active Directory Identity Protection"
	services="active-directory"
	keywords="azure active directory identity protection, cloud app discovery, gestione applicazioni, sicurezza, rischio, livello di rischio, vulnerabilità, criteri di sicurezza, glossario"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="markvi"/>

# Glossario di Azure Active Directory Identity Protection 


### A rischio (utente)	
Utente con uno o più eventi di rischio attivi.

### Posizione di accesso atipica 	
Accesso da una posizione geografica non tipica per l'utente specifico, utenti simili o il tenant.

### Azure AD Identity Protection 	
Modulo di sicurezza di Azure Active Directory che fornisce una visualizzazione consolidata degli eventi di rischio e delle potenziali vulnerabilità che interessano le identità di un'organizzazione.

### Accesso condizionale 	
Criteri per la protezione dell'accesso alle risorse. Le regole di accesso condizionale sono archiviate in Azure Active Directory e vengono valutate da Azure AD prima di concedere l'accesso alla risorsa. Queste regole possono includere la limitazione dell'accesso in base alla posizione dell'utente, all'integrità del dispositivo o al metodo di autenticazione dell'utente.

### Credenziali 	
Informazioni che includono l'identificazione e la prova dell'identificazione usata per ottenere l'accesso alle risorse locali e di rete. Tra le credenziali sono inclusi nomi utente e password, smart card e certificati.

### Evento 	
Record di un'attività in Azure Active Directory.

### Falso positivo (evento di rischio) 
Stato dell'evento di rischio impostato manualmente da un utente di Identity Protection, che indica che l'evento di rischio è stato esaminato e che era stato erroneamente contrassegnato come evento di rischio.

### Identità	
Persona o entità che deve essere verificata tramite autenticazione, in base a criteri quali password o certificato.

### Evento di rischio di identità 	
Evento di AAD che è stato contrassegnato come anomalo da Identity Protection e che può indicare che un'identità è stata compromessa.

### Ignorato (evento di rischio) 	
Stato dell'evento di rischio impostato manualmente da un utente di Identity Protection, che indica che l'evento di rischio è stato chiuso senza eseguire azioni correttive.

### Trasferimento impossibile con posizioni atipiche 	
Evento di rischio che viene attivato quando per lo stesso utente vengono rilevati due accessi, almeno uno dei quali proviene da una posizione di accesso atipica e per i quali il tempo trascorso tra un accesso e l'altro è più breve del tempo minimo necessario per spostarsi da una posizione all'altra.

###Analisi	
Processo di analisi di attività, log e altre informazioni rilevanti relative a un evento di rischio per decidere se sono necessarie procedure di correzione o mitigazione, comprendere se e come l'identità è stata compromessa e come è stata usata l'identità compromessa.

### Credenziali perse 	

Evento di rischio che viene attivato quando le credenziali dell'utente corrente (nome utente e password) vengono trovate pubblicate nel Dark Web dai nostri ricercatori.

### Mitigazione	
Azione che consente di limitare o eliminare la possibilità che un utente malintenzionato sfrutti un'identità o un dispositivo compromesso senza ripristinare l'identità o il dispositivo a uno stato sicuro. La mitigazione non risolve gli eventi di rischio precedenti associati all'identità o al dispositivo.

### Autenticazione a più fattori 
Metodo di autenticazione che richiede due o più fattori di autenticazione. Può includere un elemento di cui l'utente è in possesso, ad esempio un certificato, un'informazione nota all'utente, ad esempio nomi utente, password o passphrase, attributi fisici, ad esempio un'impronta digitale, e attributi personali, ad esempio una firma personale.

### Rilevamento offline 	
Rilevamento di anomalie e valutazione del rischio di un evento, ad esempio un tentativo di accesso dopo il fatto, per un evento già accaduto.

### Condizione dei criteri 	
Parte dei criteri di sicurezza che definisce le entità (gruppi, utenti, app, piattaforme dei dispositivi, stati dei dispositivi, intervalli di indirizzi IP, tipi di client) inclusi nei criteri o esclusi da essi.

### Regola dei criteri 	
Parte dei criteri di sicurezza che descrive le circostanze che attivano i criteri e le azioni intraprese dopo che i criteri sono stati attivati.

### Prevenzione 	
Azione per prevenire i danni arrecati all'organizzazione derivanti dall'uso non corretto di un'identità o di un dispositivo che si sospetta o si ritiene sia compromesso. Un'azione di prevenzione non protegge il dispositivo o l'identità né risolve gli eventi di rischio precedenti.

### Con privilegi (utente)
Utente che al momento dell'evento di rischio dispone di autorizzazioni di amministratore permanenti o provvisorie per una o più risorse in Azure Active Directory, ad esempio amministratore globale, amministratore fatturazione, amministratore del servizio, amministratore utenti e amministratore password.

###Tempo reale 	
Vedere Rilevamento in tempo reale.

###Rilevamento in tempo reale 	
Rilevamento di anomalie e valutazione del rischio di un evento, ad esempio un tentativo di accesso prima che all'evento sia consentito di procedere.

### Con correzione (evento di rischio) 	
Stato dell'evento di rischio impostato automaticamente da Identity Protection, che indica che l'evento di rischio è stato corretto tramite l'azione di correzione standard per questo tipo di evento di rischio. Ad esempio, quando la password utente viene reimpostata, molti eventi di rischio che indicano che la password precedente era compromessa vengono corretti automaticamente.

### Correzione 	
Azione che consente di proteggere un'identità o un dispositivo che in precedenza è stato ritenuto o sospettato essere compromesso. Un'azione di correzione ripristina l'identità o il dispositivo a uno stato sicuro e risolve gli eventi di rischio precedenti associati all'identità o al dispositivo.

### Risolto (evento di rischio) 	
Stato dell'evento di rischio impostato manualmente da un utente di Identity Protection che indica che l'utente ha eseguito un'azione di correzione appropriata all'esterno di Identity Protection e che l'evento di rischio deve essere considerato chiuso.

###Stato dell'evento di rischio 	
Proprietà di un evento di rischio che indica se l'evento è attivo e, se chiuso, il motivo della chiusura.

###Tipo di evento di rischio 	
Categoria per l'evento di rischio che indica il tipo di anomalia in base a cui l'evento viene considerato rischioso.

###Livello di rischio (evento di rischio) 	
Può essere Alto, Medio o Basso e indica la gravità dell'evento di rischio per consentire agli utenti di Identity Protection di assegnare correttamente le priorità alle azioni da eseguire al fine di ridurre i rischi per l'organizzazione.

###Livello di rischio (accesso) 
Può essere Alto, Medio o Basso e indica la probabilità che per un accesso specifico qualcun altro stia tentando di usare l'identità dell'utente.

###Livello di rischio (compromissione dell'utente) 
Può essere Alto, Medio o Basso e indica la probabilità che un'identità sia stata compromessa.

### Livello di rischio (vulnerabilità) 	
Può essere Alto, Medio o Basso e indica la gravità della vulnerabilità per consentire agli utenti di Identity Protection di assegnare correttamente le priorità alle azioni da eseguire al fine di ridurre i rischi per l'organizzazione.

### Protezione (identità)	
Indica di eseguire un'azione di correzione, ad esempio una modifica della password o la ricreazione di un'immagine del computer, per ripristinare un'identità potenzialmente compromessa a uno stato non compromesso.

### Criteri di sicurezza
Raccolta di regole e condizioni per i criteri. È possibile applicare criteri a entità come utenti, gruppi, app, dispositivi, piattaforme dei dispositivi, stati dei dispositivi, intervalli di indirizzi IP e tipi di client OAuth 2.0. Quando un criterio è abilitato, questo viene valutato ogni volta che a un'entità inclusa nel criterio viene rilasciato un token per una risorsa.

### Accedere (v) 
Consente di eseguire l'autenticazione a un'identità in Azure Active Directory.

### Accesso (s) 
Processo o azione di autenticazione di un'identità in Azure Active Directory ed evento che acquisisce questa operazione.

###Accesso da indirizzo IP anonimo 	
Evento di rischio attivato dopo che un accesso da un indirizzo IP è stato identificato come indirizzo IP proxy anonimo.

###Accesso da dispositivo infetto 
Evento di rischio attivato quando l'accesso è originato da un indirizzo IP noto per essere usato da uno o più dispositivi compromessi che tentano di comunicare attivamente con un server bot.

###Accesso da indirizzo IP con attività sospetta 
Evento di rischio attivato dopo un accesso riuscito da un indirizzo IP con un numero elevato di tentativi di accesso non riusciti tra più account utente in un breve periodo di tempo.

###Accesso da posizione non nota 
Evento di rischio attivato quando un utente esegue l'accesso da una nuova posizione (indirizzo IP, latitudine/longitudine e numero ASN).

###Rischio di accesso 	
Vedere Livello di rischio (accesso)

###Criteri di rischio di accesso 	
Criteri di accesso condizionale che valutano il rischio di un accesso specifico e applicano le azioni di mitigazione appropriate in base a condizioni e regole predefinite.

###Rischio di compromissione dell'utente 	
Vedere Livello di rischio (compromissione dell'utente)

###Rischio utente 	
Vedere Livello di rischio (compromissione dell'utente)

###Criteri di rischio utente 	
Criteri di accesso condizionale che valutano l'accesso specifico e applicano le azioni di mitigazione appropriate in base a condizioni e regole predefinite.

###Utenti contrassegnati per il rischio 	
Utenti con eventi di rischio attivi o corretti

###Vulnerabilità 	
Configurazione o condizione in Azure Active Directory che rende la directory vulnerabile ad attacchi o minacce.


## Vedere anche

- [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

<!---HONumber=AcomDC_0518_2016-->