<properties 
    pageTitle="Esercitazione: Integrazione di Azure Active Directory con Igloo Software | Microsoft Azure" 
    description="Informazioni su come usare Igloo Software con Azure Active Directory per abilitare l'accesso Single Sign-On, il provisioning automatizzato e altro ancora." 
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
    ms.date="05/16/2016" 
    ms.author="jeedes" />

#Esercitazione: Integrazione di Azure Active Directory con Igloo Software
  
Questa esercitazione descrive l'integrazione di Azure e Igloo Software. Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

-   Sottoscrizione di Azure valida
-   Sottoscrizione di [Igloo Software](http://www.igloosoftware.com/) abilitata per l'accesso Single Sign-On
  
Al termine dell'esercitazione, gli utenti di Azure AD assegnati a Igloo Software potranno accedere all'applicazione tramite il sito aziendale di Igloo Software (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).
  
Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1.  Abilitazione dell'integrazione dell'applicazione per Igloo Software
2.  Configurazione dell'accesso Single Sign-On
3.  Configurazione del provisioning utente
4.  Assegnazione degli utenti

![Scenario](./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenario")
##Abilitazione dell'integrazione dell'applicazione per Igloo Software
  
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Igloo Software.

###Per abilitare l'integrazione dell'applicazione per Igloo Software, seguire questa procedura:

1.  Nel portale di Azure classico fare clic su **Active Directory** nel riquadro di spostamento sinistro.

    ![Active Directory](./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3.  Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

    ![Applicazioni](./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Applicazioni")

4.  Fare clic su **Add** nella parte inferiore della pagina.

    ![Aggiunta di un'applicazione](./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Aggiunta di un'applicazione")

5.  Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6.  Nella **casella di ricerca** digitare **Igloo Software**.

    ![Raccolta di applicazioni](./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Raccolta di applicazioni")

7.  Nel riquadro dei risultati selezionare **Igloo Software** e quindi fare clic su **Completa** per aggiungere l'applicazione.

    ![Igloo](./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##Configurazione dell'accesso Single Sign-On
  
Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Igloo Software tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML. Come parte di questa procedura, verrà richiesto di caricare un file di certificato con codifica Base 64 nel tenant di Central Desktop. Se non si ha familiarità con questa procedura, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o).

###Per configurare l'accesso Single Sign-On, eseguire la procedura seguente:

1.  Nella pagina di integrazione dell'applicazione **Igloo Software** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Configura accesso Single Sign-On")

2.  Nella pagina **Stabilire come si desidera che gli utenti accedano a Igloo Software** selezionare **Single Sign-On di Microsoft Azure AD**, quindi fare clic su **Avanti**.

    ![Single Sign-On di Microsoft Azure AD](./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Single Sign-On di Microsoft Azure AD")

3.  Nella casella di testo **URL di accesso a Igloo Software** della pagina **Configura URL app** digitare l'URL usando il modello seguente "*https://company.igloocommunities.com/?signin*" e fare clic su **Avanti**.

    ![Configura URL app](./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Configura URL app")

4.  Nella pagina **Configura accesso Single Sign-On in Igloo Software** fare clic su **Download certificato** e infine salvare il file di certificato localmente nel computer.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Configura accesso Single Sign-On")

5.  In un'altra finestra del Web browser accedere al sito aziendale di Igloo Software come amministratore.

6.  Passare a **Control panel**.

    ![Pannello di controllo](./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Pannello di controllo")

7.  Nella scheda **Membership** fare clic su **Sign In Settings**.

    ![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Sign in Settings")

8.  Nella sezione relativa alla configurazione SAML fare clic su **Configure SAML Authentication**.

    ![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML Configuration")

9.  Nella sezione **General Configuration** seguire questa procedura:

    ![General Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783970.png "General Configuration")

    1.  Nella casella di testo **Connection Name** digitare un nome personalizzato per la configurazione.
    2.  Nella pagina **Configura accesso Single Sign-On in Igloo Software** del portale di Azure classico copiare il valore di **URL accesso remoto** e incollarlo nella casella di testo **URL di accesso IdP**.
    3.  Nella pagina **Configura accesso Single Sign-On in Igloo Software** del portale di Azure classico copiare il valore di **URL disconnessione remota** e incollarlo nella casella di testo **URL disconnessione IdP**.
    4.  In **Logout Response and Request HTTP Type** selezionare **POST**.
    5.  Creare un file di testo dal certificato scaricato.
        
		>[AZURE.TIP]Per informazioni dettagliate, vedere [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o)

    6.  Rimuovere la prima e l'ultima riga dal certificato in formato file di testo, copiare il testo rimanente del certificato e quindi incollarlo nella casella di testo **Certificato pubblico**.

10. In **Configurazione risposta e autenticazione** seguire questa procedura:

    ![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")

    1.  In **Provider di identità** selezionare **Microsoft AD FS**.
    2.  In **Tipo di identificatore** selezionare **Indirizzo di posta elettronica**.
    3.  Nella casella di testo **Attributo posta elettronica** digitare **emailaddress**.
    4.  Nella casella di testo **Attributo nome** digitare **givenname**.
    5.  Nella casella di testo **Attributo cognome** digitare **surname**.

11. Per completare la configurazione seguire questa procedura:

    ![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")

    1.  In **User creation on Sign in** selezionare **Create a new user in your site when they sign in**.
    2.  In **Impostazioni di accesso** selezionare **Usa pulsante SAML nella schermata di accesso**.
    3.  Fare clic su **Salva**.

12. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Completa** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Configura accesso Single Sign-On")
##Configurazione del provisioning utente
  
Non è richiesto alcun intervento dell'utente per configurare il provisioning degli utenti in Igloo Software. Quando un utente assegnato prova ad accedere a Igloo Software usando il pannello di accesso, Igloo Software verifica se l'utente esiste. Se l'account utente non è presente, Igloo Software lo crea automaticamente.
##Assegnazione degli utenti
  
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

###Per assegnare gli utenti a Igloo Software, seguire questa procedura:

1.  Nel portale di Azure classico creare un account di test.

2.  Nella pagina di integrazione dell'applicazione **Igloo Software** fare clic su **Assegna utenti**.

    ![Assegna utenti](./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Assegna utenti")

3.  Selezionare l'utente test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.

    ![Sì](./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Sì")
  
Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per informazioni dettagliate sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0518_2016-->