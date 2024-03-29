<properties 
    pageTitle="Esercitazione: Integrazione di Azure Active Directory con Panorama9 | Microsoft Azure" 
    description="Informazioni su come usare Panorama9 con Azure Active Directory per abilitare l'accesso Single Sign-On, il provisioning automatizzato e altro ancora." 
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
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#Esercitazione: Integrazione di Azure Active Directory con Panorama9
  
In questa esercitazione viene illustrata l'integrazione di Azure e Panorama9. Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

-   Sottoscrizione di Azure valida
-   Sottoscrizione di Panorama9 abilitata per l'accesso Single Sign-On
  
Al termine dell'esercitazione, gli utenti di Azure AD assegnati a Panorama9 saranno in grado di eseguire l’accesso Single Sign-On all'applicazione tramite il sito aziendale di Panorama9 (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md)
  
Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1.  Abilitazione dell'integrazione dell'applicazione per Panorama9
2.  Configurazione dell'accesso Single Sign-On
3.  Configurazione del provisioning utente
4.  Assegnazione degli utenti

![Scenario](./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scenario")
##Abilitazione dell'integrazione dell'applicazione per Panorama9
  
In questa sezione viene descritto come abilitare l'integrazione dell'applicazione per Panorama9.

###Per abilitare l'integrazione dell'applicazione per Panorama9, eseguire la procedura seguente:

1.  Nel portale di Azure classico fare clic su **Active Directory** nel riquadro di spostamento sinistro.

    ![Active Directory](./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3.  Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

    ![Applicazioni](./media/active-directory-saas-panorama9-tutorial/IC700994.png "Applicazioni")

4.  Fare clic su **Add** nella parte inferiore della pagina.

    ![Aggiunta di un'applicazione](./media/active-directory-saas-panorama9-tutorial/IC749321.png "Aggiunta di un'applicazione")

5.  Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-panorama9-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6.  Nella **casella di ricerca** digitare **Panorama9**.

    ![Raccolta di applicazioni](./media/active-directory-saas-panorama9-tutorial/IC790017.png "Raccolta di applicazioni")

7.  Nel riquadro dei risultati selezionare **Panorama9**, quindi fare clic su **Completa** per aggiungere l'applicazione.

    ![Panorama9](./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##Configurazione dell'accesso Single Sign-On
  
In questa sezione viene descritto come consentire agli utenti di eseguire l'autenticazione a Panorama9 tramite il proprio account di Azure AD usando la federazione basata sul protocollo SAML. La configurazione dell'accesso Single Sign-On per Panorama9 richiede di recuperare un valore di identificazione personale da un certificato. Se non si ha familiarità con questa procedura, vedere il video che descrive [come recuperare un valore di identificazione personale del certificato](http://youtu.be/YKQF266SAxI).

###Per configurare l'accesso Single Sign-On, eseguire la procedura seguente:

1.  Nella pagina di integrazione dell'applicazione **Panorama9** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-panorama9-tutorial/IC790019.png "Configura accesso Single Sign-On")

2.  Nella pagina **Stabilire come si desidera che gli utenti accedano a Panorama9** selezionare **Single Sign-On di Microsoft Azure AD**, quindi fare clic su **Avanti**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-panorama9-tutorial/IC790020.png "Configura accesso Single Sign-On")

3.  Nella pagina **Configura URL app** nella casella di testo **URL di accesso a Panorama9**, digitare l'URL utilizzato dagli utenti per accedere a Panorama9 (ad esempio: "*https://dashboard.panorama9.com/saml/access/3262*"), quindi fare clic su **Avanti**.

    ![Configura URL app](./media/active-directory-saas-panorama9-tutorial/IC790021.png "Configura URL app")

4.  Nella pagina **Configura accesso Single Sign-On in Panorama9** fare clic su **Scarica certificato** per scaricare il certificato e salvare il file di certificato localmente nel computer.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-panorama9-tutorial/IC790022.png "Configura accesso Single Sign-On")

5.  In un'altra finestra del Web browser accedere al sito aziendale di Panorama9 come amministratore.

6.  Nella barra degli strumenti in alto, fare clic su **Gestisci** quindi fare clic su **Estensioni**.

    ![Estensioni](./media/active-directory-saas-panorama9-tutorial/IC790023.png "Estensioni")

7.  Nella finestra di dialogo **Estensioni**, fare clic su **Single Sign-On**.

    ![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/IC790024.png "Single Sign-On")

8.  Nella sezione **Impostazioni**, eseguire la procedura seguente:

    ![Impostazioni](./media/active-directory-saas-panorama9-tutorial/IC790025.png "Impostazioni")

    1.  Nella finestra di dialogo **Configura accesso Single Sign-On in Panorama9** del portale di Azure classico copiare il valore di **URL servizio Single Sign-On** e incollarlo nella casella di testo **URL provider di identità**.
    2.  Copiare il valore **Identificazione personale** dal certificato esportato e quindi incollarlo nella casella di testo **Impronta digitale del certificato**.  

        >[AZURE.TIP]Per informazioni dettagliate, vedere il video che illustra [come recuperare il valore di identificazione personale di un certificato](http://youtu.be/YKQF266SAxI).

    3.  Fare clic su **Save**.

9.  Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Completa** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-panorama9-tutorial/IC790026.png "Configura accesso Single Sign-On")
##Configurazione del provisioning utente
  
Per consentire agli utenti di Azure AD di accedere a Panorama9, è necessario eseguirne il provisioning in Panorama9. Nel caso di Panorama9, il provisioning è un'attività manuale.

###Per configurare il provisioning utente, eseguire la procedura seguente:

1.  Accedere al sito aziendale di **Panorama9** come amministratore.

2.  Nel menu in alto fare clic su **Gestisci**, quindi fare clic su **Utenti**.

    ![Utenti](./media/active-directory-saas-panorama9-tutorial/IC790027.png "Utenti")

3.  Fare clic su **+**.

4.  Nella sezione Dati utente, eseguire la procedura seguente:

    ![Utenti](./media/active-directory-saas-panorama9-tutorial/IC790028.png "Utenti")

    1.  Nella casella di testo **Posta elettronica** digitare l'indirizzo di posta elettronica di un utente valido di Azure Active Directory di cui si desidera eseguire il provisioning.
    2.  Fare clic su **Save**.

>[AZURE.NOTE]È possibile usare qualsiasi altro strumento o API di creazione di account utente offerti da Panorama9 per eseguire il provisioning degli account utente di Azure AD.

##Assegnazione degli utenti
  
Per testare la configurazione, è necessario concedere l'accesso all’applicazione agli utenti di Azure AD a cui si desidera consentirne l’uso, assegnando tali utenti all'applicazione.

###Per assegnare gli utenti a Panorama9, eseguire la procedura seguente:

1.  Nel portale di Azure classico creare un account di test.

2.  Nella pagina relativa all'integrazione dell'applicazione **Panorama9**, fare clic su **Assegna utenti**.

    ![Assegna utenti](./media/active-directory-saas-panorama9-tutorial/IC790029.png "Assegna utenti")

3.  Selezionare l'utente test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.

    ![Sì](./media/active-directory-saas-panorama9-tutorial/IC767830.png "Sì")
  
Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per informazioni dettagliate sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0413_2016-->