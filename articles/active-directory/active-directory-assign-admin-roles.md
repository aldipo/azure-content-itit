<properties
	pageTitle="Assegnazione dei ruoli di amministratore in Azure Active Directory | Microsoft Azure"
	description="Illustra i ruoli di amministratore disponibili con Azure Active Directory e come assegnarli."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="curtand"/>

# Assegnazione dei ruoli di amministratore in Azure Active Directory (Azure AD)

A seconda delle dimensioni della società, potrebbe essere opportuno designare più amministratori per le diverse funzioni. Questi amministratori hanno accesso a diverse funzionalità nel portale di Azure o nel portale di Azure classico e, a seconda del ruolo, possono, tra le altre cose, creare o modificare gli utenti, assegnare ruoli amministrativi ad altri, reimpostare le password utente, gestire le licenze utente e gestire i domini.

È importante comprendere che un utente a cui è assegnato un ruolo amministrativo disporrà delle stesse autorizzazioni in tutti i servizi cloud sottoscritti dall'organizzazione, indipendentemente dal fatto che il ruolo venga assegnato nel portale di Office 365 o nel portale di Azure classico oppure usando il modulo di Azure AD per Windows PowerShell.

Sono disponibili i ruoli di amministratore seguenti:

- **Amministratore fatturazione**: effettua acquisti, gestisce le sottoscrizioni, gestisce i ticket di supporto e monitora l'integrità del servizio.

- **Amministratore globale**: dispone di accesso a tutte le funzionalità amministrative. La persona che effettua l'iscrizione per l'account di Azure diventa amministratore globale. Solo gli amministratori globali possono assegnare altri ruoli di amministratore. In una società possono essere presenti più amministratori globali.

	> [AZURE.NOTE] In Microsoft Graph API, Azure AD Graph API e Azure AD PowerShell, questo ruolo è identificato come "Amministratore società".

- **Amministratore password**: reimposta le password, gestisce le richieste di servizio e monitora l'integrità del servizio. Gli amministratori password possono reimpostare le password solo per gli utenti e gli altri amministratori password.

	> [AZURE.NOTE] In Microsoft Graph API, Azure AD Graph API e Azure AD PowerShell, questo ruolo è identificato come "Amministratore supporto tecnico".

- **Amministratore del servizio**: gestisce le richieste di servizio e monitora l'integrità del servizio.

	> [AZURE.NOTE] Per assegnare il ruolo di amministratore del servizio a un utente, è prima necessario che l’amministratore globale assegni le autorizzazioni amministrative all’utente nel servizio, come Exchange Online, e poi che assegni il ruolo di amministratore del servizio all’utente nel portale di Azure classico.

- **Amministratore utenti**: reimposta le password, monitora l'integrità del servizio e gestisce gli account utente, i gruppi di utenti e le richieste di servizio. Alle autorizzazioni di un amministratore Gestione utenti si applicano alcune limitazioni. Ad esempio, non possono eliminare un amministratore globale o creare altri amministratori. Non possono inoltre reimpostare le password per gli amministratori fatturazione, globali e del servizio.

## Autorizzazioni degli amministratori

### Amministratore fatturazione

Operazione consentita | Operazione non consentita
------------- | -------------
<p>Visualizzare le informazioni della società e degli utenti</p><p>Gestire i ticket di supporto di Office</p><p>Eseguire attività di fatturazione e acquisto per i prodotti Office</p> | <p>Reimpostare le password utente</p><p>Creare e gestire viste utente</p><p>Creare, modificare ed eliminare utenti e gruppi, nonché gestire le licenze utente</p><p>Gestire i domini</p><p>Gestire le informazioni della società</p><p>Delegare ruoli amministrativi ad altri utenti</p><p>Usare la sincronizzazione della directory</p>

### Amministratore globale

Operazione consentita | Operazione non consentita
------------- | -------------
<p>Visualizzare le informazioni della società e degli utenti</p><p>Gestire i ticket di supporto di Office</p><p>Eseguire attività di fatturazione e acquisto per i prodotti Office</p> <p>Reimpostare le password utente</p><p>Creare e gestire viste utente</p><p>Creare, modificare ed eliminare utenti e gruppi, nonché gestire le licenze utente</p><p>Gestire i domini</p><p>Gestire le informazioni della società</p><p>Delegare ruoli amministrativi ad altri utenti</p><p>Usare la sincronizzazione della directory</p><p>Abilitare o disabilitare l’autenticazione a più fattori</p> | N/D

### Amministratore password

Operazione consentita | Operazione non consentita
------------- | -------------
<p>Visualizzare le informazioni della società e degli utenti</p><p>Gestire i ticket di supporto di Office</p><p>Reimpostare le password utente</p> | <p>Eseguire attività di fatturazione e acquisto per i prodotti Office</p><p>Creare e gestire viste utente</p><p>Creare, modificare ed eliminare utenti e gruppi, nonché gestire le licenze utente</p><p>Gestire i domini</p><p>Gestire le informazioni della società</p><p>Delegare ruoli amministrativi ad altri utenti</p><p>Usare la sincronizzazione della directory</p>

### Amministratore del servizio

Operazione consentita | Operazione non consentita
------------- | -------------
<p>Visualizzare le informazioni della società e degli utenti</p><p>Gestire i ticket di supporto di Office</p> | <p>Reimpostare le password utente</p><p>Eseguire attività di fatturazione e acquisto per i prodotti Office</p><p>Creare e gestire viste utente</p><p>Creare, modificare ed eliminare utenti e gruppi, nonché gestire le licenze utente</p><p>Gestire i domini</p><p>Gestire le informazioni della società</p><p>Delegare ruoli amministrativi ad altri utenti</p><p>Usare la sincronizzazione della directory</p>

### Amministratore utenti

Operazione consentita | Operazione non consentita
------------- | -------------
<p>Visualizzare le informazioni della società e degli utenti</p><p>Gestire i ticket di supporto di Office</p><p>Reimpostare le password utente, con alcune limitazioni. Può reimpostare le password per gli amministratori fatturazione, globali e del servizio.</p><p>Creare e gestire viste utente</p><p>Creare, modificare ed eliminare utenti e gruppi, nonché gestire le licenze utente, con alcune limitazioni. Non può eliminare un amministratore globale o creare altri amministratori.</p> | <p>Eseguire attività di fatturazione e acquisto per i prodotti Office</p><p>Gestire i domini</p><p>Gestire le informazioni della società</p><p>Delegare ruoli amministrativi ad altri utenti</p><p>Usare la sincronizzazione della directory</p><p>Abilitare o disabilitare l’autenticazione a più fattori</p>

## Dettagli sul ruolo di amministratore globale

L'amministratore globale ha accesso a tutte le funzionalità amministrative. Per impostazione predefinita, la persona che effettua l'iscrizione per una sottoscrizione di Azure riceve il ruolo di amministratore globale per la directory. Solo gli amministratori globali possono assegnare altri ruoli di amministratore.

## Assegnare o rimuovere ruoli di amministratore

1. Nel portale di Azure classico, fare clic su **Active Directory** e poi sul nome della directory dell'organizzazione.

2. Nella pagina **Utenti** fare clic sul nome visualizzato dell'utente da modificare.

3. Nell’elenco **Ruolo aziendale**, scegliere il ruolo di amministratore da assegnare all'utente, oppure selezionare **Utente** per rimuovere un ruolo di amministratore esistente.

4. Nella casella **Indirizzo di posta elettronica alternativo** digitare un indirizzo di posta elettronica. Questo indirizzo viene usato per notifiche importanti, inclusa la reimpostazione self-service della password, pertanto l'utente deve poter accedere all'account di posta elettronica indipendentemente dal fatto di poter accedere o meno ad Azure.

5. Selezionare **Consenti** o **Blocca** per specificare se l'utente è autorizzato o meno a effettuare l'accesso e ad accedere ai servizi.

6. Specificare una località nell'elenco a discesa **Località di utilizzo**.

7. Al termine, fare clic su **Salva**.

## Passaggi successivi

- Per altre informazioni su come cambiare gli amministratori per una sottoscrizione di Azure, vedere [Come aggiungere o modificare i ruoli di amministratore di Azure](../billing-add-change-azure-subscription-administrator.md)

- Per altre informazioni sul modo in cui l'accesso alle risorse viene controllato in Microsoft Azure, vedere [Informazioni sull'accesso alle risorse in Azure](active-directory-understanding-resource-access.md)

- Per altre informazioni sul modo in cui Azure Active Directory è correlato alla sottoscrizione di Azure, vedere [Associare le sottoscrizioni di Azure ad Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Gestire gli utenti](active-directory-create-users.md)

- [Gestire le password](active-directory-manage-passwords.md)

- [Gestire i gruppi](active-directory-manage-groups.md)

<!---HONumber=AcomDC_0608_2016-->