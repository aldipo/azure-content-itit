<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con StatusPage | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e StatusPage."
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
	ms.date="04/26/2016"
	ms.author="jeedes"/>


# Esercitazione Integrazione di Azure Active Directory con StatusPage

Questa esercitazione descrive l'integrazione di StatusPage con Azure Active Directory (Azure AD).

L'integrazione di StatusPage con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a StatusPage 
- È possibile abilitare gli utenti per l'accesso automatico a StatusPage (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti 

Per configurare l'integrazione di Azure AD con StatusPage, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di StatusPage abilitata per l'accesso Single Sign-On


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/). 

 
## Descrizione dello scenario
L'obiettivo di questa esercitazione è testare l'accesso Single Sign-On di Azure AD in un ambiente di test.

Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di StatusPage dalla raccolta 
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di StatusPage dalla raccolta
Per configurare l'integrazione di StatusPage in Azure AD, è necessario aggiungere StatusPage dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere StatusPage dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 

	![Active Directory][1]

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

	![Applicazioni][2]

4. Fare clic su **Add** nella parte inferiore della pagina.
 
	![Applicazioni][3]

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

	![Applicazioni][4]

6. Nella casella di ricerca digitare **StatusPage**.
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_01.png)

7. Nel riquadro dei risultati selezionare **StatusPage** e quindi fare clic su **Completa** per aggiungere l'applicazione.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_02.png)


##  Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con StatusPage con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di StatusPage che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in StatusPage.

La relazione di collegamento viene stabilita assegnando al valore di **nome utente** in Azure AD lo stesso valore di **Username** in StatusPage.
 
Per configurare e testare l'accesso Single Sign-On di Azure AD con StatusPage, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente test di StatusPage](#creating-a-statuspage-test-user)**: per avere una controparte di Britta Simon in StatusPage collegata alla relativa rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione StatusPage.



**Per configurare l'accesso Single Sign-On di Azure AD con StatusPage, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **StatusPage** nel portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

	![Configura accesso Single Sign-On][6]

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a StatusPage** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.
 
	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_03.png)


3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:
 
	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_04.png)

    > [AZURE.NOTE] Per richiedere i metadati necessari per configurare l'accesso Single Sign-On, contattare il team di supporto di StatusPage all'indirizzo [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io).


    a. Dal file di metadati, copiare il valore relativo all'autorità di certificazione e quindi incollarlo nella casella di testo **Identificatore**.

    b. Dal file di metadati copiare il valore relativo all'URL di risposta e quindi incollarlo nella casella di testo **Reply URL**.

    c. Fare clic su **Avanti**.
 
 
4. Nella pagina **Configura accesso Single Sign-On in StatusPage** seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_05.png)

    a. Fare clic su **Scarica certificato** e quindi salvare il file nel computer.

    b. Fare clic su **Avanti**.


1. In un'altra finestra del browser accedere al sito aziendale di StatusPage come amministratore.

1. Nella barra degli strumenti principale scegliere **Manage Account**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)


1. Fare clic sulla scheda **Single Sign-on**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png)


1. Nella pagina di configurazione dell’accesso Single Sign-On, seguire questa procedura:

	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png)

    a. Nella pagina **Configura accesso Single Sign-On in StatusPage** del portale di Azure classico copiare il valore di **URL servizio Single Sign-On** e quindi incollarlo nella casella di testo **SSO Target URL**.

    b. Aprire il certificato scaricato nel Blocco note, copiarne il contenuto e incollarlo nella casella di testo **Certificato**.

    c. Fare clic su **Save**.


6. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.

	![Accesso Single Sign-On di Azure AD][10]

7. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.
  
	![Accesso Single Sign-On di Azure AD][11]




### Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.



![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_09.png)

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti** nel menu in alto.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png)
 
4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** sulla barra degli strumenti in basso.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png)

5. Nella pagina **Informazioni sull'utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_05.png)

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Avanti**.

6.  Nella pagina **Profilo utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_06.png)
 
    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo** selezionare **Utente**. e. Fare clic su **Avanti**.

7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_07.png)
 
8. Nella pagina **Ottieni password temporanea** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_08.png)
  
    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Completa**.

  
 
### Creazione di un utente test per StatusPage

Questa sezione descrive come creare un utente chiamato Britta Simon in StatusPage. StatusPage supporta il provisioning JIT (just-in-time), È già stato abilitato in [Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on).


**Per creare un utente denominato Britta Simon in StatusPage, seguire questa procedura:**

1. Accedere al sito aziendale di StatusPage come amministratore.

1. Nel menu in alto fare clic su **Gestisci Account**.

1. Fare clic sulla scheda Team Members.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png)

1. Fare clic su **Add Team Member**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png)

1. Digitare i valori **Email Address**, **First Name** e **Sur Name** di un utente valido di cui si vuole effettuare il provisioning nelle caselle di testo corrispondenti.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png)

1. Per **Role** scegliere **Client Administrator**.

1. Fare clic su **Create Account**.

### Assegnazione dell'utente test di Azure AD

Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a StatusPage.

![Assegna utente][200]

**Per assegnare Britta Simon a StatusPage, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.

	![Assegna utente][201]

2. Nell'elenco di applicazioni selezionare **StatusPage**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_50.png)

1. Scegliere **Utenti** dal menu in alto.

	![Assegna utente][203]

1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** sulla barra degli strumenti in basso.

	![Assegna utente][205]



### Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro StatusPage nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione StatusPage.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0511_2016-->