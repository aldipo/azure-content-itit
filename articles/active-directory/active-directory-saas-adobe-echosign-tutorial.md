<properties 
    pageTitle="Esercitazione: Integrazione di Azure Active Directory con Adobe EchoSign | Microsoft Azure" 
    description="Informazioni su come usare Adobe EchoSign con Azure Active Directory per abilitare l'accesso Single Sign-On, il provisioning automatizzato e altro ancora." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="stevenpo"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="01/14/2016" 
    ms.author="jeedes" />

#Esercitazione: Integrazione di Azure Active Directory con Adobe EchoSign

Questa esercitazione descrive l'integrazione di Azure e Adobe EchoSign. Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

-   Sottoscrizione di Azure valida
-   Sottoscrizione di Adobe EchoSign abilitata per l'accesso Single Sign-On

Al termine dell'esercitazione, gli utenti di Azure AD assegnati ad Adobe EchoSign potranno accedere all'applicazione tramite il sito aziendale di Adobe EchoSign (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al pannello di accesso](active-directory-saas-access-panel-introduction.md)

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1.  Abilitazione dell'integrazione dell'applicazione per Adobe EchoSign
2.  Configurazione dell'accesso Single Sign-On
3.  Configurazione del provisioning utente
4.  Assegnazione degli utenti

![Scenario](./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scenario")
##Abilitazione dell'integrazione dell'applicazione per Adobe EchoSign

Questa sezione descrive come abilitare l'integrazione dell'applicazione per Adobe EchoSign.

###Per abilitare l'integrazione dell'applicazione per Adobe EchoSign, seguire questa procedura:

1.  Nel portale di gestione di Azure fare clic su **Active Directory** nel pannello di navigazione sinistro.

    ![Active Directory](./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3.  Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

    ![Applicazioni](./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Applicazioni")

4.  Fare clic su **Add** nella parte inferiore della pagina.

    ![Aggiunta di un'applicazione](./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Aggiunta di un'applicazione")

5.  Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6.  Nella **casella di ricerca** digitare **Adobe EchoSign**.

    ![Raccolta di applicazioni](./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Raccolta di applicazioni")

7.  Nel riquadro dei risultati selezionare **Adobe EchoSign** e quindi fare clic su **Completa** per aggiungere l'applicazione.

    ![Adobe EchoSign](./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##Configurazione dell'accesso Single Sign-On

Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione ad Adobe EchoSign tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML. Come parte di questa procedura, verrà richiesto di creare un file di certificato con codifica Base 64. Se non si ha familiarità con questa procedura, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o).

###Per configurare l'accesso Single Sign-On, seguire questa procedura:

1.  Nella pagina di integrazione dell'applicazione **Adobe EchoSign** del portale di Azure AD fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Configura accesso Single Sign-On")

2.  Nella pagina **Stabilire come si desidera che gli utenti accedano a Adobe EchoSign** selezionare **Single Sign-On di Microsoft Azure AD**, quindi fare clic su **Avanti**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Configura accesso Single Sign-On")

3.  Nella pagina **Configura URL app**nella casella di testo **URL di accesso a Adobe EchoSign** digitare l'URL usando il modello seguente "**https://company.echosign.com/*" e fare clic su **Avanti**.

    ![Configura URL app](./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Configura URL app")

4.  Nella pagina **Configura accesso Single Sign-On in Adobe EchoSign** fare clic su **Download certificato** e quindi salvare il file di certificato nel computer.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Configura accesso Single Sign-On")

5.  In un'altra finestra del Web browser accedere al sito aziendale di Adobe EchoSign come amministratore.

6.  Nel menu in alto fare clic su **Account** e quindi nel riquadro di spostamento a sinistra fare clic su **SAML Settings** in **Account Settings**.

    ![Account](./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Account")

7.  Nella sezione relativa alle impostazioni SAML, seguire questa procedura:

    ![Impostazioni SAML](./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "Impostazioni SAML")

    1.  Per **SAML Mode**, selezionare **SAML Mandatory**.
    2.  Selezionare **Allow EchoSign Account Administrators to log in using their EchoSign Credentials**.
    3.  Per **User Creation**, selezionare **Automatically add users authenticated through SAML**.

8.  Continuare e seguire questa procedura:

    ![Impostazioni SAML](./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "Impostazioni SAML")

    1.  Nella finestra di dialogo **Configura accesso Single Sign-On in Adobe EchoSign** del portale di Azure copiare il valore di **ID entità** e incollarlo nella casella di testo **IdP Entity ID**.
    2.  Nella finestra di dialogo **Configura accesso Single Sign-On in Adobe EchoSign** del portale di Azure copiare il valore di **URL accesso remoto** e incollarlo nella casella di testo **IdP Login URL**.
    3.  Nella finestra di dialogo **Configura accesso Single Sign-On in Adobe EchoSign** del portale di Azure copiare il valore di **URL disconnessione remota** e incollarlo nella casella di testo **IdP Logout URL**.
    4.  Creare un file **con codifica Base 64** dal certificato scaricato.  

		>[AZURE.TIP]Per informazioni dettagliate, vedere [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o)
    5.  Aprire il certificato con codifica Base 64 nel Blocco note, copiarne il contenuto negli Appunti e incollarlo nella casella di testo **IdP Certificate**
    6.  Fare clic su **Save Changes**.

9.  Nel portale di Azure AD, selezionare la conferma della configurazione dell'accesso Single Sign-On, quindi fare clic su **Completa** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Configura accesso Single Sign-On")
##Configurazione del provisioning utente

Per consentire agli utenti di Azure AD di accedere ad Adobe EchoSign, è necessario eseguirne il provisioning in Adobe EchoSign. Nel caso di Adobe EchoSign, il provisioning è un'attività manuale.

###Per eseguire il provisioning di un account utente, seguire questa procedura:

1.  Accedere al sito aziendale di **Adobe EchoSign** come amministratore.

2.  Nel menu in alto fare clic su **Account** e quindi nel riquadro di spostamento a sinistra fare clic su **Users & Groups** e su **Create a new user**.

    ![Account](./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Account")

3.  Nella sezione **Create New User** seguire questa procedura:

    ![Create User](./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Create User")

    1.  Nelle caselle di testo **Email Address**, **First Name** e **Last Name** digitare l'indirizzo di posta elettronica, il nome e il cognome di un account utente ADD valido di cui si vuole eseguire il provisioning.
    2.  Fare clic su **Create User**.

		>[AZURE.NOTE]Il titolare dell'account Azure Active Directory riceverà un messaggio di posta elettronica con un collegamento da selezionare per confermare l'account e attivarlo.

>[AZURE.NOTE]È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Adobe EchoSign per eseguire il provisioning degli account utente di AAD.

##Assegnazione degli utenti

Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

###Per assegnare gli utenti ad Adobe EchoSign, seguire questa procedura:

1.  Nel portale di Azure AD creare un account di test.

2.  Nella pagina di integrazione dell'applicazione **Adobe EchoSign** fare clic su **Assegna utenti**.

    ![Assegna utenti](./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Assegna utenti")

3.  Selezionare l'utente test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.

    ![Sì](./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Sì")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per informazioni dettagliate sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0121_2016-->