<properties 
    pageTitle="Esercitazione: Integrazione di Azure Active Directory con Zscaler | Microsoft Azure" 
    description="Informazioni su come usare Zscaler con Azure Active Directory per abilitare l'accesso Single Sign-On, il provisioning automatizzato e altro ancora." 
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
    ms.date="05/25/2016" 
    ms.author="jeedes" />

#Esercitazione: Integrazione di Azure Active Directory con Zscaler
  
Questa esercitazione descrive l'integrazione di Azure e Zscaler. Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

-   Sottoscrizione di Azure valida
-   Un tenant in Zscaler
  
Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1.  Abilitazione dell'integrazione dell'applicazione per Zscaler
2.  Configurazione dell'accesso Single Sign-On
3.  Configurazione delle impostazioni proxy
4.  Configurazione del provisioning utente
5.  Assegnazione degli utenti

![Scenario](./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenario")

##Abilitazione dell'integrazione dell'applicazione per Zscaler
  
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Zscaler.

###Per abilitare l'integrazione dell'applicazione per Zscaler, seguire questa procedura:

1.  Nel portale di Azure classico fare clic su **Active Directory** nel riquadro di spostamento sinistro.

    ![Active Directory](./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3.  Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

    ![Applicazioni](./media/active-directory-saas-zscaler-tutorial/IC700994.png "Applicazioni")

4.  Fare clic su **Add** nella parte inferiore della pagina.

    ![Aggiunta di un'applicazione](./media/active-directory-saas-zscaler-tutorial/IC749321.png "Aggiunta di un'applicazione")

5.  Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-zscaler-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6.  Nella **casella di ricerca** digitare **Zscaler**.

    ![Raccolta di applicazioni](./media/active-directory-saas-zscaler-tutorial/IC769227.png "Raccolta di applicazioni")

7.  Nel riquadro dei risultati selezionare **Zscaler** e quindi fare clic su **Completa** per aggiungere l'applicazione.

    ![Zscaler](./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##Configurazione dell'accesso Single Sign-On
  
Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Zscaler tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML. Come parte di questa procedura, verrà richiesto di caricare un certificato in Zscaler.

###Per configurare l'accesso Single Sign-On, seguire questa procedura:

1.  Nella pagina di integrazione dell'applicazione **Zscaler** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Abilita Single Sign-On](./media/active-directory-saas-zscaler-tutorial/IC769229.png "Abilita Single Sign-On")

2.  Nella pagina **Stabilire come si desidera che gli utenti accedano a Zscaler** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscaler-tutorial/IC769230.png "Configura accesso Single Sign-On")

3.  Nella casella di testo **URL di accesso Zscaler** della pagina **Configura URL app** digitare l'URL di accesso ottenuto da Zscaler e quindi fare clic su **Avanti**:

    >[AZURE.NOTE] Se non si conosce l'URL di accesso, contattare il team di supporto Zscaler.

    ![Configura URL app](./media/active-directory-saas-zscaler-tutorial/IC769231.png "Configura URL app")

4.  Nella pagina **Configura accesso Single Sign-On in Zscaler** seguire questa procedura:

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscaler-tutorial/IC769232.png "Configura accesso Single Sign-On")

    1.  Fare clic su **Scarica certificato** e quindi salvare il file di il certificato in locale come **c:\\Zscaler.cer**.
    2.  Copiare l'**URL richiesta di autenticazione** negli Appunti.

5.  Accedere al tenant Zscaler.

6.  Scegliere **Administration** dal menu disponibile nella parte superiore.

    ![Amministrazione](./media/active-directory-saas-zscaler-tutorial/IC769486.png "Amministrazione")

7.  In **Manage Administrators & Roles** fare clic su **Mange Users & Authentication**.

    ![Manage Administrators & Roles](./media/active-directory-saas-zscaler-tutorial/IC769487.png "Manage Administrators & Roles")

8.  Nella sezione **Choose Authentication Option for your Organization** seguire questa procedura:

    ![Choose Authentication Options](./media/active-directory-saas-zscaler-tutorial/IC769488.png "Choose Authentication Options")

    1.  Selezionare **Authenticate using SAML Single Sign-On**.
    2.  Fare clic su **Configure SAML Single Sign-On Parameters**.

9.  Nella pagina della finestra di dialogo **Configure SAML Single Sign-On Parameters** procedere come descritto di seguito e quindi fare clic su **Done**:

    ![Caricamento del certificato](./media/active-directory-saas-zscaler-tutorial/IC769489.png "Caricamento del certificato")

    1.  Nella casella di testo **URL of the SAML Portal to which users are sent for authentication** incollare il valore del campo **URL richiesta di autenticazione** dal portale di Azure classico.
    2.  Nella casella di testo **Attribute containing Login Name** digitare **NameID**.
    3.  Nel campo **Upload SSL Public Certificate** caricare il certificato scaricato dal portale di Azure classico.
    4.  Selezionare **Enable SAML Auto-Provisioning**.

10. Nella pagina della finestra di dialogo **Configure User Authentication** seguire questa procedura:

    ![Configure User Authentication](./media/active-directory-saas-zscaler-tutorial/IC769490.png "Configure User Authentication")

    1.  Fare clic su **Save**.
    2.  Fare clic su **Activate Now**.

11. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Completa** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscaler-tutorial/IC769491.png "Configura accesso Single Sign-On")

##Configurazione delle impostazioni proxy

###Per configurare le impostazioni proxy in Internet Explorer

1.  Avviare **Internet Explorer**.

2.  Selezionare **Opzioni Internet** dal menu **Strumenti** per aprire la finestra di dialogo **Opzioni Internet**.

    ![Opzioni Internet](./media/active-directory-saas-zscaler-tutorial/IC769492.png "Opzioni Internet")

3.  Fare clic sulla scheda **Connessioni**.

    ![Connessioni](./media/active-directory-saas-zscaler-tutorial/IC769493.png "Connessioni")

4.  Fare clic su **Impostazioni LAN** per aprire la finestra di dialogo **Impostazioni LAN**.

5.  Nella sezione del server proxy seguire questa procedura:

    ![Server proxy](./media/active-directory-saas-zscaler-tutorial/IC769494.png "Server proxy")

    1.  Selezionare Usa un server di proxy per la rete LAN.
    2.  Nella casella di testo Indirizzo digitare **gateway.zscalertwo.net**.
    3.  Nella casella di testo Porta digitare **80**.
    4.  Selezionare **Ignora server proxy per indirizzi locali**.
    5.  Fare clic su **OK** per chiudere la finestra di dialogo **Impostazioni rete locale (LAN)**.

6.  Fare clic su **OK** per chiudere la finestra di dialogo **Opzioni Internet**.

##Configurazione del provisioning utente
  
Per consentire agli utenti di Azure AD di accedere a Zscaler, è necessario eseguirne il provisioning in Zscaler. Nel caso di Zscaler, il provisioning è un'attività manuale.

###Per configurare il provisioning utente, seguire questa procedura:

1.  Accedere al tenant **Zscaler**.

2.  Fare clic su **Administration**.

    ![Amministrazione](./media/active-directory-saas-zscaler-tutorial/IC781035.png "Amministrazione")

3.  Fare clic su **User Management**.

    ![User Management](./media/active-directory-saas-zscaler-tutorial/IC781036.png "User Management")

4.  Nella scheda **Users** fare clic su **Add**.

    ![Aggiungi](./media/active-directory-saas-zscaler-tutorial/IC781037.png "Aggiungi")

5.  Nella sezione Add User seguire questa procedura:

    ![Aggiunta di un utente](./media/active-directory-saas-zscaler-tutorial/IC781038.png "Aggiunta di un utente")

    1.  Digitare **UserID**, **User Display Name**, **Password** e **Confirm Password** e quindi selezionare **Groups** e **Department** di un account Azure AD valido di cui si vuole eseguire il provisioning.
    2.  Fare clic su **Salva**.

>[AZURE.NOTE] È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Zscaler per eseguire il provisioning degli account utente Azure AD.

##Assegnazione degli utenti
  
Per testare la configurazione, è necessario assegnare l'accesso all'applicazione agli utenti di Azure AD a cui si desidera consentire l'uso dell'applicazione.

###Per assegnare gli utenti a Zscaler, seguire questa procedura:

1.  Nel portale di Azure classico creare un account di test.

2.  Nella pagina di integrazione dell’applicazione **Zscaler** fare clic su **Assegna utenti**.

    ![Assegna utenti](./media/active-directory-saas-zscaler-tutorial/IC769495.png "Assegna utenti")

3.  Selezionare l'utente test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.

    ![Sì](./media/active-directory-saas-zscaler-tutorial/IC767830.png "Sì")
  
Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per informazioni dettagliate sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0525_2016-->