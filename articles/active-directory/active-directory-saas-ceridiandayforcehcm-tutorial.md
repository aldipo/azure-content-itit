<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con Ceridian Dayforce HCM | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Ceridian Dayforce HCM."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="jeedes"/>


# Esercitazione: Integrazione di Azure Active Directory con Ceridian Dayforce HCM

Questa esercitazione descrive l'integrazione di Ceridian Dayforce HCM con Azure Active Directory (Azure AD). L'integrazione di Ceridian Dayforce HCM con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Ceridian Dayforce HCM.
- È possibile abilitare gli utenti per l'accesso automatico a Ceridian Dayforce HCM (Single Sign-On) con i propri account Azure AD.
- È possibile gestire gli account da una posizione centrale: il portale di Azure classico


Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti

Per configurare l'integrazione di Azure AD con Ceridian Dayforce HCM, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di Ceridian Dayforce HCM abilitata per l'accesso Single Sign-On.


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## Descrizione dello scenario
L'obiettivo di questa esercitazione è testare l'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Ceridian Dayforce HCM dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di Ceridian Dayforce HCM dalla raccolta
Per configurare l'integrazione di Ceridian Dayforce HCM in Azure AD, è necessario aggiungere Ceridian Dayforce HCM dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Ceridian Dayforce HCM dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 

	![Active Directory][1]

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

	![Applicazioni][2]

4. Fare clic su **Add** nella parte inferiore della pagina.

	![Applicazioni][3]

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

	![Applicazioni][4]

6. Nella casella di ricerca digitare **Ceridian Dayforce HCM**.

	![Applicazioni](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Nel riquadro dei risultati selezionare **Ceridian Dayforce HCM**, quindi fare clic su **Completa** per aggiungere l'applicazione.

	![Applicazioni](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con Ceridian Dayforce HCM con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Ceridian Dayforce HCM che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Ceridian Dayforce HCM.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Ceridian Dayforce HCM, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente test di Ceridian Dayforce HCM](#creating-a-ceridian-dayforce-hcm-test-user)**: per avere una controparte di Britta Simon in Ceridian Dayforce HCM collegata alla relativa rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene descritto come abilitare Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione Ceridian Dayforce HCM.

L'applicazione Ceridian Dayforce HCM si aspetta che le asserzioni SAML abbiano un formato specifico. Collaborare prima di tutto con il team di Dayforce HCM per individuare l'identificatore utente corretto. Microsoft consiglia di utilizzare l'attributo **"name"** come identificatore utente. Il valore di questo attributo può essere gestito nella finestra di dialogo **"Attribute"**. La schermata seguente illustra un esempio relativo a questa operazione.

![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png)

**Per configurare Single Sign-On di Azure AD con Ceridian Dayforce HCM, seguire questa procedura:**




1. Nella pagina di integrazione dell'applicazione **Ceridian Dayforce HCM** del portale di Azure classico fare clic su **Attributi** nel menu in alto.

	![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png)


1. Nell'elenco degli **attributi del token saml** selezionare l'attributo di nome e quindi fare clic su **Modifica**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png)


1. Nella finestra di dialogo **Modifica attributo utente** seguire questa procedura:
 
    a. Nell'elenco **Valore attributo** selezionare l'attributo utente da utilizzare per l'implementazione. Ad esempio, se si desidera utilizzare il valore EmployeeID come identificatore utente univoco e il valore dell'attributo è stato archiviato in ExtensionAttribute2, selezionare **user.extensionattribute2**.

    b. Fare clic su **Complete**.
	



1. Nel menu in alto fare clic su **Avvio rapido**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)





1. Nella pagina di integrazione dell'applicazione **Ceridian Dayforce HCM** del portale di Azure classico, fare clic su **Configura accesso Single Sign-On**.

	![Configura accesso Single Sign-On][6]

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Ceridian Dayforce HCM** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png)

3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png)


    a. Nella casella di testo **URL di accesso**, digitare l'URL utilizzato dagli utenti per accedere all'applicazione Ceridian Dayforce HCM. Per gli ambienti di produzione, utilizzare il formato di URL seguente: `https://sso.dayforcehcm.com/DayforcehcmNamespace`

	Per maggiore chiarezza, sostituire DayforcehcmNamespace con lo spazio dei nomi del proprio ambiente oppure l'ID aziendale; ad esempio: `https://sso.dayforcehcm.com/contoso`
	
	Per gli ambienti di testing, utilizzare il formato di URL seguente: `https://ssotest.dayforcehcm.com/DayforcehcmNamespace`

	b. Nella casella di testo **URL di risposta** digitare l'URL utilizzato da Azure AD per inserire la risposta. Per gli ambienti di produzione, utilizzare: `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` Per gli ambienti di test, utilizzare: `https://fs-test.dayforcehcm.com/sp/ACS.saml2`
   

4. Nella pagina **Configura accesso Single Sign-On in Ceridian Dayforce HCM** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png)

    a. Fare clic su **Scarica certificato** e quindi salvare il file nel computer.

    b. Fare clic su **Avanti**.


5. Per ottenere l'accesso Single Sign-On configurato per l'applicazione, contattare il team di supporto Ceridian Dayforce HCM tramite posta elettronica e fornire i seguenti elementi:

    - File del certificato scaricato
    - **URL dell'autorità di certificazione**
    - **URL SAML SSO** 
    - **URL del servizio Single Sign-Out** 


6. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.

	![Accesso Single Sign-On di Azure AD][10]

7. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.

	![Accesso Single Sign-On di Azure AD][11]



### Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png)

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l’elenco di utenti, fare clic su **Utenti** nel menu in alto.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** sulla barra degli strumenti in basso.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

5. Nella pagina **Informazioni sull’utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png)

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Avanti**.

6.  Nella pagina **Profilo utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png)

    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo**, selezionare **Utente**.

    e. Fare clic su **Avanti**.

7. Nella pagina **Ottieni password temporanea**, fare clic su **crea**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png)

8. Nella pagina della finestra di dialogo **Ottieni password temporanea** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png)

    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Complete**.



### Creazione di un utente di test Ceridian Dayforce HCM

Questa sezione descrive come creare un utente chiamato Britta Simon in Ceridian Dayforce HCM.

Collaborare con il team di supporto di Ceridian Dayforce HCM per far aggiungere utenti alla propria applicazione Ceridian Dayforce HCM.





### Assegnazione dell'utente test di Azure AD

L'obiettivo di questa sezione consiste nell'abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Ceridian Dayforce HCM.

![Assegna utente][200]

**Per assegnare Britta Simon a Ceridian Dayforce HCM, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.

	![Assegna utente][201]

2. Nell'elenco delle applicazioni, selezionare **Ceridian Dayforce HCM**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png)

1. Scegliere **Utenti** dal menu in alto.

	![Assegna utente][203]

1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** sulla barra degli strumenti in basso.

	![Assegna utente][205]



### Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso. Quando si fa clic sul riquadro Ceridian Dayforce HCM nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Ceridian Dayforce HCM.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0615_2016-->