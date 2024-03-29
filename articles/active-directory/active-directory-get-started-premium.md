<properties
	pageTitle="Introduzione ad Azure Active Directory Premium"
	description="Questo argomento descrive come effettuare l'iscrizione ad Azure Active Directory Premium tramite il sito Web del contratti multilicenza."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="femila" 
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="05/25/2016"
	ms.author="markvi"/>

# Introduzione ad Azure Active Directory Premium


Sono disponibili diverse opzioni per l'iscrizione ad Active Directory Premium:

**Azure oppure Office 365**: un sottoscrittore di Azure oppure Office 365 può acquistare Active Directory Premium online. Per informazioni dettagliate vedere i video relativi a [come acquistare Azure Active Directory Premium (clienti esistenti)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-Existing-Customer) o [come acquistare Azure Active Directory Premium (nuovi clienti)](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/How-to-Purchase-Azure-Active-Directory-Premium-New-Customers).

**Enterprise Mobility Suite**: Enterprise Mobility Suite è una soluzione economicamente conveniente che consente alle organizzazioni di usare con un unico piano di licenze i servizi Active Directory Premium, Azure Rights Management, Microsoft Intune. Per altre informazioni, visitare il sito Web di [Enterprise Mobility Suite](https://www.microsoft.com/it-IT/server-cloud/enterprise-mobility/overview.aspx). Per ottenere una versione di valutazione gratuita di 30 giorni, fare clic [qui](https://portal.office.com/Signup/Signup.aspx?OfferId=2E63A04D-BE0B-4A0F-A8CF-407C1C299221&dl=EMS&ali=1#0).


**Contratti multilicenza Microsoft**: Azure Active Directory Premium è disponibile tramite un programma [Microsoft Enterprise Agreement](https://www.microsoft.com/it-IT/licensing/licensing-programs/enterprise.aspx) (250 o più licenze) o un programma [Open Volume License](https://www.microsoft.com/it-IT/licensing/licensing-programs/open-license.aspx) (da 5 a 250 licenze).


Questo argomento illustra come iniziare a usare un'istanza di Azure Active Directory Premium acquistata tramite il programma multilicenza. Se non si ha familiarità con le diverse edizioni di Azure Active Directory, vedere [Edizioni di Azure Active Directory](active-directory-editions.md).

> [AZURE.NOTE]
Le edizioni Premium e Basic di Azure Active Directory sono disponibili per i clienti in Cina tramite l'istanza globale di Azure Active Directory. Azure Active Directory Premium e le edizioni Basic non sono attualmente supportati nel servizio di Microsoft Azure gestito da 21Vianet in Cina. Per altre informazioni, è possibile contattare Microsoft attraverso il [forum di Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).




## Passaggio 1: Iscriversi ad Active Directory Premium

Per iscriversi, vedere [Come acquistare tramite i contratti multilicenza](http://www.microsoft.com/it-IT/licensing/how-to-buy/how-to-buy.aspx).



## Passaggio 2: Attivare un piano di licenze

Se questo è il primo acquisto di un piano di licenze tramite il programma di contratti multilicenza Enterprise di Microsoft, si riceverà un messaggio di posta elettronica di conferma al completamento dell'acquisto, che è necessario per attivare il primo piano di licenze.

Per ogni acquisto successivo relativo a questa directory, le licenze vengono attivate automaticamente nella stessa directory.



**Per attivare il piano di licenze, eseguire uno di questi passaggi:**


1. Per avviare l'attivazione, fare clic su **Accedi** o **Iscriviti**.

    ![Accesso][1]



    - Se è già disponibile un tenant, fare clic su **Accedi** per accedere con l'account amministratore esistente. È necessario eseguire l'accesso con le credenziali di amministratore globale dalla directory in cui devono essere attivate le licenze.

    - Se si vuole creare un nuovo tenant di Azure Active Directory da usare con il piano di licenze, fare clic su **Iscriviti** per aprire la finestra di dialogo **Creazione del profilo dell'account**.

        ![Creare un profilo di account][2]

Al termine, viene visualizzata la finestra di dialogo seguente a conferma dell'attivazione del piano di licenze per il tenant.

![Conferma][3]

## Passaggio 3: Attivare l'accesso ad Azure Active Directory

Se Microsoft Azure è già stato usato, è possibile procedere al [Passaggio 4](#step-4-assign-license-to-user-accounts).

Quando viene effettuato il provisioning delle licenze nella directory, viene inviato all'utente un **messaggio di posta elettronica di benvenuto**. Il messaggio conferma che è possibile iniziare a gestire le licenze di Azure Active Directory Premium o Enterprise Mobility Suite e le relative funzionalità.

Se si prova ad attivare l'accesso ad Azure Active Directory prima di ricevere il messaggio di posta elettronica di benvenuto, viene visualizzato il messaggio di errore seguente.

![L'accesso non è disponibile][9]

Dopo la ricezione del messaggio di posta elettronica, attendere alcuni minuti prima di ripetere il tentativo.

Anche i nuovi amministratori inclusi nella sottoscrizione possono attivare l'accesso al portale di Azure classico con questo collegamento.






**Per attivare l'accesso ad Azure Active Directory, seguire questa procedura:**

1. Nel **messaggio di posta elettronica di benvenuto** fare clic su **Accedi**. 
    
    ![Messaggio di posta elettronica di benvenuto][4]

2. Dopo aver eseguito l'accesso, è necessario completare un'autenticazione a due fattori mediante una verifica per dispositivi mobili:

    ![Verifica per dispositivi mobili][5]

L'attivazione potrebbe richiedere alcuni minuti. Quando l'accesso viene attivato, la barra marrone scompare ed è possibile fare clic su **Portale**.

![Attendere. Configurazione in corso][6]

In questo caso, l'accesso ad Azure è limitato ad Azure Active Directory.

![Funzionalità di Azure][7]

Se invece si riesce ad accedere ad Azure grazie a un'iscrizione precedente, è possibile eseguire l'aggiornamento dell'accesso di Azure Active Directory all'accesso di Azure completo attivando sottoscrizioni Azure aggiuntive. In questi casi il portale di Azure classico offre maggiori funzionalità.

![Funzionalità di Azure][8]



## Passaggio 4: Assegnare le licenze agli account utente

Prima di iniziare a usare il piano acquistato, è necessario assegnare manualmente le licenze agli account utente dell'organizzazione per consentire loro di usufruire dell'ampia gamma di funzionalità fornite dal livello Premium. Attenersi alla procedura seguente per assegnare le licenze agli utenti e consentire loro di usare le funzionalità di Azure Active Directory Premium.

**Per assegnare licenze agli utenti, seguire questa procedura:**

1. Accedere al portale di Azure classico come amministratore globale della directory che si vuole personalizzare.
2. Fare clic su **Active Directory**, quindi selezionare la directory in cui si desidera assegnare le licenze.
3. Fare clic sulla scheda **Licenze**, selezionare **Active Directory Premium** o **Enterprise Mobility Suite**, quindi fare clic su **Assegna**.

    ![Piani di licenze][10]

4. Nella finestra di dialogo selezionare gli utenti a cui assegnare le licenze, quindi fare clic sull'icona con il segno di spunta per salvare le modifiche.

    ![Assegnare licenze][11]

### Limitazioni relative alle licenze

Alcuni piani di licenze fanno parte di altri piani di licenze o includono a loro volta altri piani. In genere, non è possibile assegnare a un utente un piano di licenze che gli è già stato assegnato. Se si prevede di assegnare un piano di licenze che ne include un altro, è necessario prima rimuovere il piano secondario.

### Requisiti relativi alle licenze

Quando si assegna una licenza a un utente, è possibile specificare una località di utilizzo primaria nelle proprietà dell'account. Se non viene specificata una località di utilizzo, all'utente verrà assegnata automaticamente la località del tenant.

![Posizione degli utenti][12]

La disponibilità di servizi e funzionalità per un servizio cloud Microsoft varia in base al paese o all'area geografica. È possibile che un servizio, ad esempio VoIP (Voice over Internet Protocol), sia disponibile solo in alcuni paesi o aree geografiche. È inoltre possibile che alcune funzionalità di un servizio siano limitate per motivi legali in determinati paesi o aree geografiche. Per verificare se un servizio o una funzionalità è disponibile con o senza limitazioni, cercare il proprio paese o area geografica nel sito dedicato alle limitazioni relative alle licenze del servizio.

## Passaggi successivi

- [Aggiungere informazioni personalizzate distintive dell'azienda alla pagina di accesso e al pannello di accesso](active-directory-add-company-branding.md)
- [Visualizzare i report di accesso e utilizzo](active-directory-view-access-usage-reports.md)

<!--Image references-->
[1]: ./media/active-directory-get-started-premium/MOLSEmail.png
[2]: ./media/active-directory-get-started-premium/MOLSAccountProfile.png
[3]: ./media/active-directory-get-started-premium/MOLSThankYou.png
[4]: ./media/active-directory-get-started-premium/AADEmail.png
[5]: ./media/active-directory-get-started-premium/SignUppage.png
[6]: ./media/active-directory-get-started-premium/Subscriptionspage.png
[7]: ./media/active-directory-get-started-premium/Premiuminportal.png
[8]: ./media/active-directory-get-started-premium/Premiuminportal_large.png
[9]: ./media/active-directory-get-started-premium/Signuppage_oops.png
[10]: ./media/active-directory-get-started-premium/contosolicenseplan.png
[11]: ./media/active-directory-get-started-premium/Assignlicensespicker.png
[12]: ./media/active-directory-get-started-premium/Usagelocation.png

<!---HONumber=AcomDC_0525_2016-->