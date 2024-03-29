<properties
    pageTitle="Informazioni sull'accesso alle risorse in Azure | Microsoft Azure" 
    description="Questo argomento illustra i concetti relativi all'uso di amministratori della sottoscrizione per controllare l'accesso alle risorse nel portale di Azure completo."
    services="active-directory"
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
    ms.date="05/03/2016"
    ms.author="markusvi"/>


# Informazioni sull'accesso alle risorse in Azure


> [AZURE.NOTE] Questo argomento illustra i concetti relativi all'uso di amministratori della sottoscrizione per controllare l'accesso alle risorse nel portale di Azure completo. In alternativa, il portale di anteprima di Azure fornisce la funzionalità di [controllo degli accessi in base al ruolo](role-based-access-control-configure.md) che permette di gestire le risorse di Azure con maggiore precisione.

Nel mese di ottobre 2013 il portale di Azure classico e le API Gestione dei servizi sono stati integrati con Azure Active Directory per creare i presupposti per migliorare l'esperienza utente per la gestione dell'accesso alle risorse di Azure. Azure Active Directory offre già potenti funzionalità per la gestione degli utenti, per la sincronizzazione della directory locale, per l'autenticazione a più fattori e il controllo di accesso alle applicazioni. Naturalmente, queste funzionalità dovrebbero essere rese disponibili per la gestione delle risorse di Azure a livello generale.

La fatturazione costituisce il punto di partenza per il controllo di accesso in Azure. Il proprietario di un account Azure, a cui si accede tramite il [Centro account di Azure](https://account.windowsazure.com/subscriptions), è l'amministratore account. Le sottoscrizioni sono un contenitore per la fatturazione, ma fungono anche da limite di sicurezza. Ogni sottoscrizione ha un amministratore del servizio che può aggiungere, rimuovere, modificare le risorse di Azure incluse nella sottoscrizione specifica tramite il [portale di Azure classico](https://manage.windowsazure.com/). L'amministratore dei servizi predefinito di una nuova sottoscrizione è l'amministratore dell'account, che, a sua volta, può assegnare il ruolo di amministratore dei servizi a un altro utente nel Centro account di Azure.

<br><br>![Account Azure][1]

Le sottoscrizioni sono anche associate a una directory. Una directory definisce un set di utenti. Possono essere utenti dell'azienda o dell'istituto di istruzione che ha creato la directory o utenti esterni, ovvero utenti con un account Microsoft. Le sottoscrizioni sono accessibili da un sottoinsieme di utenti della directory alla quale sono stati assegnati come amministratore dei servizi o coamministratore. Fanno eccezione, per motivi che risalgono al passato, gli account Microsoft (in precedenza Windows Live ID), i quali possono essere assegnati come amministratore dei servizi o coamministratore senza essere presenti nella directory.

<br><br>![Controllo di accesso in Azure][2]


Le funzionalità disponibili nel portale di Azure classico consentono agli amministratori del servizio che hanno eseguito l'accesso con un account Microsoft di modificare la directory a cui è associata la sottoscrizione tramite il comando **Modifica directory** nella sezione **Impostazioni** della pagina **Sottoscrizioni**. Si noti che questa operazione può influire sul controllo di accesso di quella sottoscrizione.



> [AZURE.NOTE] Il comando **Modifica directory** nel portale di Azure classico non è disponibile per gli utenti che hanno eseguito l'accesso con un account aziendale o dell'istituto di istruzione, perché con questi account è possibile accedere solo alla directory a cui appartengono.

<br><br>![Flusso di accesso utente semplice][3]

Nel caso più semplice un'organizzazione, ad esempio Contoso, imporrà la fatturazione e il controllo di accesso nello stesso set di sottoscrizioni. Questo significa che la directory è associata alle sottoscrizioni di proprietà di un singolo account Azure. Dopo l'accesso al portale di Azure classico, gli utenti visualizzeranno due raccolte di risorse, illustrate in arancione nella figura precedente:


- Directory in cui è presente l'account utente (originato o aggiunto come entità esterna). Si noti che la directory usata per l'accesso non è rilevante ai fini di questo calcolo, di conseguenza verranno sempre visualizzate tutte le directory, indipendentemente da quella usata per l'accesso.

- Risorse che fanno parte delle sottoscrizioni associate alla directory usata per l'accesso E a cui l'utente può accedere (perché è amministratore dei servizi o coamministratore).


<br><br>![Utente con più sottoscrizioni e directory][4]


Gli utenti con sottoscrizioni in più directory possono cambiare il contesto corrente del portale di Azure classico con il filtro della sottoscrizione. Questo comporta un accesso separato a una directory diversa, operazione che viene eseguita automaticamente tramite l'accesso Single Sign-On.

Operazioni quali lo spostamento di risorse tra sottoscrizioni possono risultare più difficoltose a causa della visualizzazione a directory singola delle sottoscrizioni. Per procedere al trasferimento di risorse, potrebbe essere necessario usare prima il comando **Modifica directory** nella sezione **Impostazioni** della pagina Sottoscrizioni per associare le sottoscrizioni alla stessa directory.

## Passaggi successivi

- Per altre informazioni su come cambiare gli amministratori per una sottoscrizione di Azure, vedere [Come aggiungere o modificare i ruoli di amministratore di Azure](../billing-add-change-azure-subscription-administrator.md).

- Per altre informazioni sul modo in cui Azure Active Directory è correlato alla sottoscrizione di Azure, vedere [Associare le sottoscrizioni di Azure ad Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- Per altre informazioni su come assegnare i ruoli in Azure AD, vedere [Assegnazione dei ruoli di amministratore in Azure Active Directory](active-directory-assign-admin-roles.md).



<!--Image references-->
[1]: ./media/active-directory-understanding-resource-access/IC707931.png
[2]: ./media/active-directory-understanding-resource-access/IC707932.png
[3]: ./media/active-directory-understanding-resource-access/IC707933.png
[4]: ./media/active-directory-understanding-resource-access/IC707934.png

<!---HONumber=AcomDC_0504_2016-->