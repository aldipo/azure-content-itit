<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con 23 Video | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e 23 Video."
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
	ms.date="05/16/2016"
	ms.author="jeedes"/>


# Esercitazione: Integrazione di Azure Active Directory con 23 Video

Questa esercitazione illustra come integrare 23 Video con Azure Active Directory (Azure AD).<br>L'integrazione di 23 Video con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a 23 Video 
- È possibile abilitare gli utenti per l'accesso automatico a 23 Video (Single Sign-On) con i propri account Azure AD

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti 

Per configurare l'integrazione di Azure AD con 23 Video, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di 23 Video abilitata per l'accesso Single Sign-On


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/). 

 
## Descrizione dello scenario
L'obiettivo di questa esercitazione è testare l'accesso Single Sign-On di Azure AD in un ambiente di test. <br> Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di 23 Video dalla raccolta 
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di 23 Video dalla raccolta
Per configurare l'integrazione di 23 Video in Azure AD, è necessario aggiungere 23 Video dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere 23 Video dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. <br><br> ![Active Directory][1]<br>

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.<br><br> ![Applicazioni][2]<br>
4. Fare clic su **Aggiungi** nella parte inferiore della pagina.<br><br> ![Applicazioni][3]<br>
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.<br><br> ![Applicazioni][4]<br>
6. Nella casella di ricerca digitare **23 Video**.<br><br> ![Applicazioni][5]<br>
7. Nel riquadro dei risultati selezionare **23 Video**, quindi fare clic su **Completa** per aggiungere l'applicazione. <br><br>![Applicazioni][25]<br>

##  Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con 23 Video in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di 23 Video che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in 23 Video.<br> La relazione di collegamento viene stabilita assegnando al valore di **nome utente** in Azure AD lo stesso valore di **Username** in 23 Video.
 
Per configurare e testare l'accesso Single Sign-On di Azure AD con 23 Video, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente test di 23 Video](#creating-a-23-video-test-user)**: per avere una controparte di Britta Simon in 23 Video collegata alla relativa rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione 23 Video.<br>

**Per configurare l'accesso Single Sign-On di Azure AD con 23 Video, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **23 Video** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**. <br><br> ![Configura accesso Single Sign-On][6] <br>

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a 23 Video** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**. <br><br> ![Accesso Single Sign-On di Azure AD][7] <br>

3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura: <br><br>![Accesso Single Sign-On di Azure AD][8] <br>
 
     a. Nella casella di testo **URL di risposta** digitare l'URL usato dagli utenti per accedere al sito 23 Video, ad esempio **https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] L'integrazione di Active Directory tramite SAML 2.0 è disponibile per tutti gli utenti di 23 Video. Per ottenere i metadati corrispondenti, contattare il supporto tecnico all'indirizzo [support@23company.com](mailto:support@23company.com).

     b. Fare clic su **Avanti**.
 
4. Nella pagina **Configura accesso Single Sign-On in 23 Video** seguire questa procedura.<br><br>![Accesso Single Sign-On di Azure AD][9] <br>

    a. Fare clic su Download certificato e quindi salvare il file nel computer.

    b. Contattare il team di supporto di 23 Video inviando un messaggio all'indirizzo [support@23company.com](mailto:support@23company.com), fornire il certificato scaricato e le informazioni visualizzate nei campi **Issuer URL**, **Single Sign-On Service URL**, **Single Sign-Out URL** e quindi chiedere di configurare l'accesso Single Sign-On per l'app 23 Video.

    c. Fare clic su **Avanti**.


6. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**. <br><br>![Accesso Single Sign-On di Azure AD][10]<br>

7. Nella pagina **Conferma Single Sign-On** fare clic su **Completa**. <br><br>![Accesso Single Sign-On di Azure AD][11]




### Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.<br> Nell'elenco di utenti selezionare **Britta Simon**.<br><br>![Creare un utente di Azure AD][20]<br>

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. <br><br>![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png) <br> 

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti** nel menu in alto. <br><br> ![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) <br>
 
4. Per aprire la finestra di dialogo **Aggiungi utente**, nella barra degli strumenti in basso fare clic su **Aggiungi utente**. <br><br> ![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) <br>

5. Nella pagina della finestra di dialogo **Informazioni sull'utente** seguire questa procedura: <br><br> ![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png) <br>

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Avanti**.

6.  Nella pagina della finestra di dialogo **Profilo utente** seguire questa procedura: <br><br>![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) <br>
 
    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo** selezionare **Utente**. e. Fare clic su **Avanti**.

7. Nella pagina della finestra di dialogo **Ottieni password temporanea** fare clic su **crea**. <br><br> ![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) <br>
 
8. Nella pagina **Ottieni password temporanea** seguire questa procedura: <br><br>![Creazione di un utente test di Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) <br>
  
    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Complete**.

  
 
### Creazione di un utente test per 23 Video

Questa sezione descrive come creare un utente chiamato Britta Simon in 23 Video.

**Per creare un utente denominato Britta Simon in 23 Video, seguire questa procedura:**

1. Accedere al sito aziendale di 23 Video come amministratore.

1. Passare a **Impostazioni**.


2. Nella sezione **Utenti** fare clic su **Configura**. <br><br>![Assegna utente][400]<br>

3. Fare clic su **Aggiungi nuovo utente**. <br><br>![Assegna utente][401]<br>

4. Nella sezione **Invita qualcuno a questo sito** seguire questa procedura: <br><br>![Assegna utente][402]<br>

    a. Nella casella di testo **E-mail addresses** digitare l'indirizzo di posta elettronica di Britta Simon in Azure Active Directory.

    b. Fare clic su **Aggiungi questo utente**.


### Assegnazione dell'utente test di Azure AD

Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a 23 Video. <br><br>![Assegna utente][200] <br>

**Per assegnare Britta Simon a 23 Video, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.<br><br>![Assegna utente][201] <br>
2. Nell'elenco delle applicazioni selezionare **23 Video**. <br><br>![Assegna utente][202] <br>
1. Scegliere **Utenti** dal menu in alto. <br><br>![Assegna utente][203] <br>
1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** sulla barra degli strumenti in basso. <br><br>![Assegna utente][205]



### Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.<br> Quando si fa clic sul riquadro 23 Video nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione 23 Video.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png

<!---HONumber=AcomDC_0518_2016-->