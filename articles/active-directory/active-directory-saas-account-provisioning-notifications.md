<properties
	pageTitle="Notifiche relative al provisioning dell'account | Microsoft Azure"
	description="Come assicurarsi di ricevere notifiche relative ai problemi di provisioning utente che richiedono l'attenzione dell'utente abilitando le notifiche di provisioning dell'account."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/30/2016"
	ms.author="markusvi"/>


# Notifiche relative al provisioning dell'account

Con il provisioning utente, è possibile automatizzare il processo di gestione degli utenti nelle applicazioni SaaS di terze parti. <br> Anche se si tratta di un processo automatizzato, l'interazione con questo processo è richiesta di tanto in tanto. <br> È il caso, ad esempio, quando scade la password dell'account configurato per lo scambio dei dati con un'applicazione SaaS di terze parti.

Abilitando le notifiche di provisioning dell'account è possibile assicurarsi di ricevere notifiche relative ai problemi di provisioning utente che richiedono l'attenzione dell'utente.

È possibile attivare o disattivare le notifiche di provisioning dell'account come parte della configurazione del provisioning utenti per un'applicazione SaaS di terze parti.

![Provisioning utente][1]



Per attivare le notifiche di provisioning dell'account, selezionare la casella di controllo relativa nella finestra di dialogo **Conferma** e quindi digitare l'alias di posta elettronica del destinatario.

![Notifiche relative al provisioning dell'account][2]
 


È possibile immettere un elenco di distribuzione come destinatario; tuttavia, è importante notare che l'e-mail di notifica contiene collegamenti ai report che sono accessibili solo agli amministratori di Azure AD.

Se sono state abilitate le notifiche di provisioning dell'account si riceveranno e-mail riguardo problemi di importanza critica correlati al provisioning utente. Tuttavia, per evitare un sovraccarico di posta elettronica, si riceverà un solo messaggio di notifica al giorno per ogni applicazione SaaS per cui sono abilitate le notifiche.


##Articoli correlati

- [Indice di articoli per la gestione di applicazioni in Azure Active Directory](active-directory-apps-index.md)
- [Automatizzare il provisioning e il deprovisioning utenti in app SaaS](active-directory-saas-app-provisioning.md)
- [Personalizzazione dei mapping degli attributi per il Provisioning dell’utente](active-directory-saas-customizing-attribute-mappings.md)
- [Scrittura di espressioni per i mapping degli attributi](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Ambito dei filtri per il Provisioning utente](active-directory-saas-scoping-filters.md)
- [Uso di SCIM per abilitare il provisioning automatico di utenti e gruppi da Azure Active Directory alle applicazioni](active-directory-scim-provisioning.md)
- [Elenco di esercitazioni pratiche sulla procedura di integrazione delle applicazioni SaaS](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png

<!---HONumber=AcomDC_0504_2016-->