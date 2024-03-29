<properties
	pageTitle="Esercitazione: Integrazione di Azure Active Directory con Hightail | Microsoft Azure"
	description="Informazioni su come configurare l'accesso Single Sign-On tra Azure Active Directory e Hightail."
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
	ms.date="04/11/2016"
	ms.author="jeedes"/>


# Esercitazione: Integrazione di Azure Active Directory con Hightail

Questa esercitazione descrive l'integrazione di Hightail con Azure Active Directory (Azure AD).

L'integrazione di Hightail con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Hightail
- È possibile abilitare gli utenti per l'accesso automatico a Hightail (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account in una posizione centrale, il portale di Azure Active Directory.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisiti

Per configurare l'integrazione di Azure AD con Hightail, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di Hightail abilitata per l'accesso Single Sign-On


> [AZURE.NOTE] Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## Descrizione dello scenario
L'obiettivo di questa esercitazione è testare l'accesso Single Sign-On di Azure AD in un ambiente di test.

Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Hightail dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## Aggiunta di Hightail dalla raccolta
Per configurare l'integrazione di Hightail in Azure AD, è necessario aggiungere Hightail dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Hightail dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 
 
	![Active Directory][1]

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

	![Applicazioni][2]

4. Fare clic su **Add** nella parte inferiore della pagina.

	![Applicazioni][3]

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

	![Applicazioni][4]

6. Nella casella di ricerca, digitare **Hightail**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_01.png)

7. Nel riquadro dei risultati selezionare **Hightail** e quindi fare clic su **Completa** per aggiungere l'applicazione.

	![Selezione dell'app nella raccolta](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_02.png)

##  Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con Hightail con un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Hightail che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Hightail.

La relazione di collegamento viene stabilita assegnando al valore di **nome utente** in Azure AD lo stesso valore di **Username** in Hightail.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Hightail, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)**: per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
4. **[Creazione di un utente test di Hightail](#creating-a-hightail-test-user)**: per avere una controparte di Britta Simon in Hightail collegata alla rispettiva rappresentazione in Azure AD.
5. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)**: per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)**: per verificare se la configurazione funziona.

### Configurazione dell'accesso Single Sign-On di Azure AD

Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione Hightail.

L'applicazione Hightail si aspetta che le asserzioni SAML abbiano un formato specifico. Configurare le asserzioni seguenti per questa applicazione. È possibile gestire i valori di questi attributi dalla scheda **"Attribute" (Attributo)** dell'applicazione. La schermata seguente illustra un esempio relativo a questa operazione.

![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_51.png)

**Per configurare l'accesso Single Sign-On di Azure AD con Hightail, seguire questa procedura:**


1. Nel menu visualizzato nella parte superiore della pagina di integrazione dell'applicazione **Hightail** del portale di Azure classico fare clic su **Attributi**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_81.png)


1. Nella finestra di dialogo **Attributi token SAML**, per ciascuna riga della tabella seguire questa procedura:

	| Nome attributo | Valore attributo |
	| --- | --- |    
	| FirstName | user.givenname |
    | LastName | user.surname |
	| Email | user.mail |
	| UserIdentity | user.mail |

	a. Fare clic su **aggiungi attributo utente** per aprire la finestra di dialogo **Aggiungi attributo utente**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_82.png)


	b. Nella casella di testo **Nome attributo** digitare il nome dell'attributo indicato per quella riga.

    c. Nell'elenco **Valore attributo** selezionare il valore dell'attributo indicato per quella riga.

    d. Fare clic su **Complete**.
	



1. Nel menu in alto fare clic su **Avvio rapido**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)



2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Hightail** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_03.png)


3. Nella pagina della finestra di dialogo **Configurare le impostazioni dell'app** se si desidera configurare l'applicazione in **modalità iniziata da IDP** seguire la procedura in basso e fare clic su **Avanti**:

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_04.png)



    a. Nella casella di testo **URL di risposta**, digitare l'URL adottando il modello seguente: **"https://www.hightail.com/samlLogin?phi\_action=app/samlLogin&subAction=handleSamlResponse"**

	b. Fare clic su **Avanti**

4. Se si desidera configurare l'applicazione in **modalità iniziata da SP** sulla pagina della finestra di dialogo **Configurare le impostazioni dell'app**, fare clic su **Mostra opzioni avanzate (facoltativo)**, immettere l'**URL di accesso** e fare clic su **Avanti**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_06.png)

	a. Nella casella di testo URL di accesso, digitare l'URL usato dagli utenti per accedere all'applicazione Hightail adottando il modello seguente: **https://www.hightail.com/loginSSO**. Questa è la pagina di accesso tradizionale per tutti i clienti che desiderano utilizzare l'accesso SSO.

	b. Fare clic su **Avanti**

5. Nella pagina **Configura accesso Single Sign-On in Hightail** seguire la procedura in basso e fare clic su **Avanti**:

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_05.png)


    a. Fare clic su **Scarica certificato** e quindi salvare il file del certificato con codifica Base 64 nel computer.

    b. Fare clic su **Avanti**.

	> [AZURE.NOTE] Prima di configurare l'accesso Single Sign-On sull'app Hightail, inserisci il dominio di posta elettronica nell'elenco dei consentiti con il team Hightail in modo che tutti gli utenti che usano il dominio possano beneficiare della funzionalità Single Sign-On.

6. Per configurare l'accesso SSO sull'applicazione, è necessario accedere al tenant di Hightail come amministratore.

	a. Nel menu in alto, fare clic sulla scheda **Account (Account)** e selezionare **Configure SAML (Configura SAML)**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png)


	b. Selezionare la casella di controllo di **Enable SAML Authentication (Abilita autenticazione SAML)**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png)

	c. Aprire il certificato con codifica Base 64 nel Blocco note, copiarne il contenuto negli Appunti e quindi incollarlo nella casella di testo **SAML Token Signing Certificate (Certificato per la firma di token SAML)**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png)


	d. Copiare l'URL di accesso remoto da Azure AD a **SAML Authority (Identity Provider) (Autorità SAML (provider di identità))** in Hightail.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_005.png)
	
	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

	e. Se si desidera configurare l'applicazione in **modalità iniziata da IDP**, selezionare **"Identity Provider (IdP) initiated log in" ("Accesso iniziato con provider di identità (IdP)")**. Se si utilizza la **modalità iniziata da SP**, selezionare **"Service Provider (SP) initiated log in" ("Accesso iniziato con provider di servizi (SP)")**.
	
	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

	f. Copiare l'URL consumer di SAML dell'istanza in uso e incollarlo nella casella di testo **Reply URL (URL di risposta)** come mostrato nel passaggio 4.

	g. Fare clic su **Save**.



6. Nel portale di Azure classico, selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.

	![Accesso Single Sign-On di Azure AD][10]

7. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.
 
	![Accesso Single Sign-On di Azure AD][11]




### Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.

Nell'elenco di utenti selezionare **Britta Simon**.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_09.png)

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti** nel menu in alto.
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png)


4. Per aprire la finestra di dialogo **Aggiungi utente**, nella barra degli strumenti in basso fare clic su **Aggiungi utente**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png)

5. Nella pagina della finestra di dialogo **Informazioni sull'utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_05.png)

    a. In **Tipo di utente** selezionare **Nuovo utente nell'organizzazione**.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Avanti**.

6.  Nella pagina della finestra di dialogo **Profilo utente** seguire questa procedura:

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_06.png)

    a. Nella casella di testo **Nome** digitare **Britta**.

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo**, selezionare **Utente**.

    e. Fare clic su **Avanti**.

7. Nella pagina della finestra di dialogo **Ottieni password temporanea**, fare clic su **crea**.

	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_07.png)


8. Nella pagina della finestra di dialogo **Ottieni password temporanea** seguire questa procedura:
 
	![Creazione di un utente test di Azure AD](./media/active-directory-saas-hightail-tutorial/create_aaduser_08.png)

    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Complete**.



### Creazione di un utente test di Hightail

Questa sezione descrive come creare un utente chiamato Britta Simon in Hightail.

Non è necessario alcun intervento dell'utente in questa sezione. Hightail supporta il provisioning utente just-in-time basato su attestazioni personalizzate. Se sono state configurate le attestazioni personalizzate come illustrato nella sezione **[Configuring Azure AD Single Sign-On (Configurazione di Single Sign-On di Azure AD)](#configuring-azure-ad-single-sign-on)** in alto, viene automaticamente creato un utente nell'applicazione se ancora non esiste.

> [AZURE.NOTE] Per creare un utente manualmente, è necessario contattare il team di supporto di Hightail all'indirizzo [support@hightail.com](mailto:support@hightail.com).


### Assegnazione dell'utente test di Azure AD

Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Hightail.

![Assegna utente][200]

**Per assegnare Britta Simon a Hightail, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.
 
	![Assegna utente][201]

2. Nell'elenco delle applicazioni, selezionare **Hightail**.

	![Configura accesso Single Sign-On](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_50.png)


1. Scegliere **Utenti** dal menu in alto.

	![Assegna utente][203]

1. Nell'elenco di utenti selezionare **Britta Simon**.

2. Fare clic su **Assegna** sulla barra degli strumenti in basso.

![Assegna utente][205]



### Test dell'accesso Single Sign-On

Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Hightail nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Hightail.


## Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0413_2016-->