<properties
   pageTitle="Ruoli in PIM | Microsoft Azure"
   description="Informazioni sui ruoli utilizzati per le identità con privilegi con l'estensione Azure Privileged Identity Management."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="05/19/2016"
   ms.author="kgremban"/>

# Ruoli in Azure AD Privileged Identity Management

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

È possibile assegnare gli utenti dell'organizzazione a diversi ruoli amministrativi in Azure AD. Queste assegnazioni di ruolo controllano le attività, ad esempio l'aggiunta o la rimozione di utenti o la modifica delle impostazioni del servizio, che gli utenti possono eseguire in Azure AD, Office 365 e altri Microsoft Online Services e applicazioni connesse.

Un amministratore globale può aggiornare gli utenti assegnati in modo **permanente** ai ruoli di Azure AD, usando i cmdlet di PowerShell, ad esempio `Add-MsolRoleMember` e `Remove-MsolRoleMember`, o il portale classico come descritto in [Assegnazione dei ruoli amministratore in Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD Privileged Identity Management (PIM) gestisce i criteri per l'accesso con privilegi per gli utenti in Azure AD. PIM assegna gli utenti a uno o più ruoli in Azure AD e le assegnazioni possono essere permanenti o temporanee. Quando un utente viene assegnato permanentemente a un ruolo o attiva un'assegnazione di ruolo temporanea, può gestire Azure Active Directory, Office 365 e altre applicazioni con le autorizzazioni assegnate ai relativi ruoli.


## Ruoli gestiti in PIM

Privileged Identity Management consente di assegnare utenti a ruoli di amministratore comuni, tra cui:


- L'**amministratore globale**, noto anche come amministratore della società, ha accesso a tutte le funzionalità amministrative. Si può avere più di un amministratore globale nell'organizzazione. La persona che esegue l'iscrizione per l'acquisto di Office 365 diventa automaticamente un amministratore globale.
- L'**amministratore dei ruoli con privilegi** gestisce Azure AD PIM e aggiorna le assegnazioni dei ruoli per gli altri utenti.  
- L'**amministratore fatturazione** effettua acquisti, gestisce gli abbonamenti e i ticket di supporto ed effettua il monitoraggio dell'integrità del servizio.
- L'**amministratore password** reimposta le password, gestisce le richieste di servizio ed effettua il monitoraggio dell'integrità del servizio. Gli amministratori password sono limitati alla reimpostazione delle password per gli utenti.
- L'**amministratore del servizio** gestisce le richieste di servizio ed effettua il monitoraggio dell'integrità del servizio.

  > [AZURE.NOTE] Se si usa Office 365, prima di assegnare il ruolo di amministratore del servizio a un utente, prima di tutto assegnare all'utente autorizzazioni amministrative per un servizio, ad esempio Exchange Online.

- L'**amministratore Gestione utenti** reimposta le password, effettua il monitoraggio dell'integrità del servizio e gestisce gli account utente, i gruppi di utenti e le richieste di servizio. L'amministratore Gestione utenti non può eliminare un amministratore globale, creare altri ruoli amministrativi o reimpostare le password per gli amministratori fatturazione, globali e dei servizi.
- L'**amministratore di Exchange** ha accesso amministrativo a Exchange Online tramite l'interfaccia di amministrazione di Exchange e può eseguire quasi tutte le attività in Exchange Online.
- L'**amministratore di SharePoint** ha accesso amministrativo a SharePoint Online tramite l'interfaccia di amministrazione di SharePoint Online e può eseguire quasi tutte le attività in SharePoint Online.
- L'**amministratore di Skype for Business** ha accesso amministrativo a Skype for Business tramite l'interfaccia di amministrazione di Skype for Business e può eseguire quasi tutte le attività in Skype for Business Online.

Leggere gli articoli seguenti per altri dettagli sull'[assegnazione dei ruoli di amministratore in Azure Active Directory](active-directory-assign-admin-roles.md) e sull'[assegnazione di ruoli di amministratore in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Da PIM è possibile [assegnare questi ruoli a un utente come temporanei](active-directory-privileged-identity-management-how-to-add-role-to-user.md), in modo che l'utente possa [attivare il ruolo quando necessario](active-directory-privileged-identity-management-how-to-activate-role.md).

Se si vuole concedere a un altro utente l'accesso per la gestione all'interno del servizio PIM stesso, vedere [come concedere l'accesso a PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md) per altre informazioni sui ruoli necessari all'utente per l'uso di PIM.


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## Ruoli non gestiti in PIM

I ruoli in Exchange Online o SharePoint Online, ad eccezione di quelli indicati in precedenza, non sono rappresentati in Azure AD e quindi non sono visibili in PIM. Per altre informazioni sulla modifica delle assegnazioni di ruolo specifiche in questi servizi di Office 365, vedere [Autorizzazioni in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Anche le sottoscrizioni di Azure e i gruppi di risorse non sono rappresentati in Azure AD. Per gestire gli abbonamenti di Azure, vedere [Come aggiungere o modificare i ruoli di amministratore di Azure](../billing-add-change-azure-subscription-administrator.md). Per altre informazioni sul controllo degli accessi in base al ruolo di Azure, vedere [Usare le assegnazioni di ruolo per gestire l'accesso alle risorse di Azure Active Directory](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## Ruoli utente e accesso
Per alcuni servizi e applicazioni Microsoft l'assegnazione di un utente a un ruolo potrebbe non essere sufficiente per abilitarlo al ruolo di amministratore.

L'accesso al portale di Azure classico richiede che l'utente sia un amministratore del servizio o un coamministratore in una sottoscrizione di Azure, anche se l'utente non deve gestire le sottoscrizioni di Azure. Ad esempio, per gestire le impostazioni di configurazione per Azure AD nel portale classico, un utente deve essere sia un amministratore globale di Azure AD sia un coamministratore della sottoscrizione in una sottoscrizione di Azure. Per informazioni su come aggiungere utenti agli abbonamenti di Azure, vedere [Come aggiungere o modificare i ruoli di amministratore di Azure](../billing-add-change-azure-subscription-administrator.md).

L'accesso a Microsoft Online Services può richiedere che all'utente sia assegnata anche una licenza prima di poter aprire il portale del servizio o eseguire attività amministrative.

## Assegnazione di una licenza a un utente in Azure AD

1. Accedere al [portale di Azure classico](http://manage.windowsazure.com) con un account di amministratore globale o di coamministratore.
2. Selezionare **Tutti gli elementi** dal menu principale.
3. Selezionare la directory con cui si desidera lavorare e a cui sono associate licenze.
4. Selezionare **Licenze**. Verrà visualizzato l'elenco delle licenze disponibili.
5. Fare clic sul piano di licenza che contiene le licenze da distribuire.
6. Selezionare **Assegna utenti**.
7. Selezionare l'utente a cui si desidera assegnare una licenza.
8. Scegliere il pulsante **Assegna**. L'utente ora può accedere ad Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Passaggi successivi
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!---HONumber=AcomDC_0525_2016-->