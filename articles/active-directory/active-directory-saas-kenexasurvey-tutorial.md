<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con IBM Kenexa Survey Enterprise | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e IBM Kenexa Survey Enterprise."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/20/2016"
	ms.author="jeedes"/>


# Esercitazione: Integrazione di Azure Active Directory con IBM Kenexa Survey Enterprise

Questa esercitazione descrive come integrare IBM Kenexa Survey Enterprise con Azure Active Directory (Azure AD).

L'integrazione di IBM Kenexa Survey Enterprise con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a IBM Kenexa Survey Enterprise
- È possibile abilitare gli utenti per l'accesso automatico a IBM Kenexa Survey Enterprise (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti

Per configurare l'integrazione di Azure AD con IBM Kenexa Survey Enterprise, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di IBM Kenexa Survey Enterprise abilitata per l'accesso Single Sign-On


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di IBM Kenexa Survey Enterprise dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di IBM Kenexa Survey Enterprise dalla raccolta
Per configurare l'integrazione di IBM Kenexa Survey Enterprise in Azure AD, è necessario aggiungere IBM Kenexa Survey Enterprise dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere IBM Kenexa Survey Enterprise dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 

	![Active Directory][1]

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

	![Applicazioni][2]

4. Fare clic su **Add** nella parte inferiore della pagina.

	![Applicazioni][3]

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

	![Applicazioni][4]

6. Nella casella di ricerca digitare **IBM Kenexa Survey Enterprise**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)

7. Nel riquadro dei risultati selezionare **IBM Kenexa Survey Enterprise**, quindi fare clic su **Completa** per aggiungere l'applicazione.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)


##  Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con IBM Kenexa Survey Enterprise con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di IBM Kenexa Survey Enterprise che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in IBM Kenexa Survey Enterprise.

La relazione di collegamento viene stabilita assegnando il valore del **nome utente** in Azure AD come valore di **Username** in IBM Kenexa Survey Enterprise.

Per configurare e testare l'accesso Single Sign-On di Azure AD con IBM Kenexa Survey Enterprise, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente test di IBM Kenexa Survey Enterprise](#creating-an-kenexasurvey-test-user)**: per avere una controparte di Britta Simon in IBM Kenexa Survey Enterprise collegata alla relativa rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure classico e viene configurato l'accesso Single Sign-On nell'applicazione IBM Kenexa Survey Enterprise.


**Per configurare Single Sign-On di Azure AD con IBM Kenexa Survey Enterprise, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **IBM Kenexa Survey Enterprise** del portale classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

	![Configura accesso Single Sign-On][6]

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a IBM Kenexa Survey Enterprise** selezionare **Accesso Single Sign-On di Azure AD**, quindi fare clic su **Avanti**.
 
	![Configura accesso Single Sign-On](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)

3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)


    a. Nella casella di testo **URL accesso** digitare l'URL usato dagli utenti per accedere all'applicazione IBM Kenexa Survey Enterprise adottando il modello seguente: **"https://surveys.kenexa.com/<codice società>"**.

    b. Nella casella di testo **IDENTIFICATORE** digitare l'URL con il modello seguente: **https://surveys.kenexa.com/\<CodiceSocietà>/strumenti**.

	> [AZURE.NOTE] Specificare l'URL dell'IDENTIFICATORE del team di IBM Kenexa Survey Enterprise e indicare di impostarlo come ID entità per la propria istanza.
  

    c. Fare clic su **Avanti**.


4. Nella pagina **Configura accesso Single Sign-On in IBM Kenexa Survey Enterprise** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png)

    a. Fare clic su **Scarica certificato** e quindi salvare il file nel computer.

    b. Fare clic su **Avanti**.


5. Per ottenere l'accesso Single Sign-On configurato per l'applicazione, contattare il team di supporto di IBM Kenexa Survey Enterprise e fornire i seguenti elementi:

 - File del certificato scaricato
 - **URL dell'autorità di certificazione**
 - **URL SAML SSO** 
 - **URL del servizio Single Sign-Out**

	> [AZURE.NOTE] Si noti che il valore attestazione NameID nella risposta deve corrispondere con l'ID SSO configurato nel sistema Kenexa. È quindi necessario collaborare con il team di supporto di Kenexa per mappare l'identificatore utente appropriato all'interno dell'organizzazione come ID SSO. Per impostazione predefinita Azure AD verranno imposterà NameIdentifier come valore UPN. È possibile modificarlo dalla scheda Attributo come illustrato nella schermata seguente. L'integrazione funzionerà solo dopo aver completato correttamente il mapping.
 	
	![Configura accesso Single Sign-On](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)

 
6. Nel portale classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.

	![Accesso Single Sign-On di Azure AD][10]

7. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.
  
	![Accesso Single Sign-On di Azure AD][11]




### Creazione di un utente test di Azure AD
In questa sezione viene creato un utente test chiamato Britta Simon nel portale classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png)

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti** nel menu in alto.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png)

4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png)

5. Nella pagina della finestra di dialogo **Informazioni sull'utente** seguire questa procedura:
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png)

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Avanti**.

6.  Nella pagina della finestra di dialogo **Profilo utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png)

    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo**, selezionare **Utente**.

    e. Fare clic su **Avanti**.

7. Nella pagina della finestra di dialogo **Ottieni password temporanea** fare clic su **crea**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png)

8. Nella pagina **Ottieni password temporanea** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png)

    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Complete**.



### Creazione di un utente test di IBM Kenexa Survey Enterprise

In questa sezione viene creato un utente chiamato Britta Simon in IBM Kenexa Survey Enterprise. Collaborare con il team di supporto di IBM Kenexa Survey Enterprise per mappare l'ID SSO per tutti gli utenti. Questo valore di ID SSO dovrà essere mappato anche al valore NameIdentifier di Azure AD. È possibile modificare questa impostazione predefinita nella scheda Attributo.


> [AZURE.NOTE] Per creare un utente manualmente, è necessario contattare il team di supporto di IBM Kenexa Survey Enterprise.


### Assegnazione dell'utente test di Azure AD

In questa sezione si abilita Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a IBM Kenexa Survey Enterprise.

![Assegna utente][200]

**Per assegnare Britta Simon a IBM Kenexa Survey Enterprise, seguire questa procedura:**

1. Per aprire la visualizzazione delle applicazioni nel portale classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.

	![Assegna utente][201]

2. Nell'elenco di applicazioni selezionare **IBM Kenexa Survey Enterprise**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png)

1. Scegliere **Utenti** dal menu in alto.

	![Assegna utente][203]

1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** nella barra degli strumenti in basso.
	
	![Assegna utente][205]



### Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro IBM Kenexa Survey Enterprise nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione IBM Kenexa Survey Enterprise.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0518_2016-->