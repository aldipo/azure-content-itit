<properties
	pageTitle="Anteprima di Azure Active Directory B2C: attributi personalizzati | Microsoft Azure"
	description="Come utilizzare gli attributi personalizzati in Azure Active Directory B2C per raccogliere informazioni sugli utenti"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="msmbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/27/2016"
	ms.author="swkrish"/>

#  Anteprima di Azure Active Directory B2C: Usare attributi personalizzati per la raccolta di informazioni sugli utenti

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

La directory Azure Active Directory (Azure AD) B2C viene fornita con un set predefinito di informazioni (attributi), ad esempio nome, cognome, città, codice postale e altri attributi. Tuttavia, ogni applicazione consumatori ha requisiti specifici relativi agli attributi da raccogliere. Con Azure AD B2C, è possibile estendere il set di attributi archiviati in ogni account utente. È possibile creare attributi personalizzati nel [portale di Azure](https://portal.azure.com/) e usarli nei criteri di iscrizione, come illustrato di seguito. È inoltre possibile la lettura e la scrittura di questi attributi usando l'[API Graph di Azure AD](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Gli attributi personalizzati usano le [estensioni dello schema di directory dell'API Graph di Azure AD](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## Creare un attributo personalizzato

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Attributi utente**.
3. Fare clic su **+Aggiungi** nella parte superiore del pannello.
4. Fornire un **Nome** per l'attributo personalizzato (ad esempio, "ShoeSize") e, facoltativamente, una **Descrizione**. Fare clic su **Crea**.

    > [AZURE.NOTE]
    Attualmente è disponibile solo il **tipo di dati** "string".

L'attributo personalizzato è ora disponibile nell'elenco degli **Attributi utente** e per l'uso nei criteri di iscrizione.

## Usare un attributo personalizzato nei criteri di iscrizione

1. [Seguire questa procedura per passare al pannello delle funzionalità B2C nel portale di Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Fare clic su **Criteri di iscrizione**.
3. Fare clic sul criterio di iscrizione (ad esempio, "B2C\_1\_SiUp") per aprirlo. Fare clic su **Modifica** nella parte superiore del pannello.
4. Fare clic su **Attributi iscrizione** e selezionare l'attributo personalizzato (ad esempio, "ShoeSize"). Fare clic su **OK**.
5. Fare clic su **Attestazioni applicazione** e selezionare l'attributo personalizzato. Fare clic su **OK**.
6. Fare clic su **Salva** nella parte superiore del pannello.

È possibile utilizzare la funzionalità "Esegui ora" nei criteri per verificare l'esperienza utente. Ora si dovrebbe vedere "ShoeSize" nell'elenco di attributi che vengono raccolti durante l'iscrizione dell’utente e nel token inviato all'applicazione.

## Note

- Insieme ai criteri di iscrizione, gli attributi personalizzati possono essere usati anche nei criteri di iscrizione o accesso e nei criteri di modifica del profilo.
- Esiste una limitazione nota degli attributi personalizzati. Si verifica soltanto la prima volta che viene usato in un criterio e non quando viene aggiunto all'elenco **Attributi utente**. Il problema verrà risolto a breve.

<!---HONumber=AcomDC_0629_2016-->