<properties 
	pageTitle="Aggiungere notifiche push all'app universale per Windows 8.1 | Microsoft Azure" 
	description="Informazioni su come inviare notifiche push all'app di Windows 8.1 universale dal servizio mobile back-end JavaScript usando Hub di notifica di Azure." 
	services="mobile-services,notification-hubs" 
	documentationCenter="windows" 
	authors="ggailey777" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="mobile-services" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-windows" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="03/05/2016" 
	ms.author="glenga"/>


# Aggiungere notifiche push all'app di Servizi mobili

[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Per la versione equivalente di questo argomento per le app per dispositivi mobili, vedere [Aggiungere notifiche push all'app Windows](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
 
Questo argomento illustra come usare Servizi mobili di Azure con un back-end JavaScript per inviare notifiche push a un'app di Windows universale. In questa esercitazione si usa Hub di notifica di Azure per abilitare le notifiche push nel progetto di app di Windows universale. Al termine dell'esercitazione, il servizio mobile invierà una notifica push dal back-end JavaScript a tutte le app Windows Store e Windows Phone Store registrate ogni volta che viene inserita una tabella TodoList. L'hub di notifica creato può essere utilizzato gratuitamente con il servizio mobile, può essere gestito indipendentemente da quest'ultimo e può essere utilizzato da altri servizi e applicazioni.

>[AZURE.NOTE]Questo argomento descrive come usare gli strumenti in Visual Studio 2013 con aggiornamento 3 per aggiungere il supporto per le notifiche push di Servizi mobili a un'app di Windows universale. La stessa procedura può essere eseguita per aggiungere le notifiche push da Servizi mobili a un'app di Windows Store o Windows Phone Store 8.1. Per aggiungere le notifiche push a un'app di Windows Phone 8 o Windows Phone Silverlight 8.1, vedere questa versione della guida [Introduzione alle notifiche push in Servizi mobili](mobile-services-javascript-backend-windows-phone-get-started-push.md).

Questa esercitazione descrive le operazioni di base per abilitare le notifiche push:

1. [Registrare l'app per le notifiche push](#register)
2. [Aggiornare il servizio per l'invio di notifiche push](#update-service)
3. [Testare le notifiche push nell'app](#test)

Per completare l'esercitazione, sono necessari gli elementi seguenti:

* Un [account di Microsoft Store](http://go.microsoft.com/fwlink/p/?LinkId=280045) attivo.
* [Visual Studio 2013 Express per Windows](http://go.microsoft.com/fwlink/?LinkId=257546) con aggiornamento 3 o versione successiva

##<a id="register"></a>Registrare l'app per le notifiche push

[AZURE.INCLUDE [mobile-services-create-new-push-vs2013](../../includes/mobile-services-create-new-push-vs2013.md)]

&nbsp;&nbsp;6. Passare alla cartella del progetto `\Services\MobileServices\your_service_name`, aprire il file di codice push.register.cs generato ed esaminare il metodo **UploadChannel** che registra l'URL del canale del dispositivo con l'hub di notifica.

&nbsp;&nbsp;7. Aprire il file di codice App.xaml.cs condiviso. Si noti come una chiamata al nuovo metodo **UploadChannel** è stata aggiunta al gestore dell'evento **OnLaunched**. In questo modo verrà effettuato un tentativo di registrazione del dispositivo a ogni avvio dell'app.

&nbsp;&nbsp;8. Ripetere i passaggi precedenti per aggiungere notifiche push al progetto di app di Windows Phone Store, quindi nel file App.xaml.cs condiviso rimuovere la chiamata supplementare a **Client di servizi mobili** **UploadChannel** e il wrapper condizionale `#if...#endif` restante. Ora entrambi i progetti possono condividere un'unica chiamata a **UploadChannel**.

&nbsp;&nbsp;Si noti che è anche possibile semplificare il codice generato unendo le definizioni con wrapping `#if...#endif` [MobileServiceClient] in un'unica definizione senza wrapping usata da entrambe le versioni dell'app.

Ora che le notifiche push sono abilitate nell'app, è necessario aggiornare il servizio mobile per l'invio delle notifiche push.

##<a id="update-service"></a>Aggiornare il servizio per l'invio di notifiche push

I seguenti passaggi aggiornano lo script insert registrato nella tabella TodoItem. È possibile implementare un codice simile in qualsiasi script del server o in qualsiasi altro punto dei propri servizi di back-end.

[AZURE.INCLUDE [mobile-services-javascript-update-script-notification-hubs](../../includes/mobile-services-javascript-update-script-notification-hubs.md)]


##<a id="test"></a> Testare le notifiche push nell'app

[AZURE.INCLUDE [mobile-services-javascript-backend-windows-universal-test-push](../../includes/mobile-services-javascript-backend-windows-universal-test-push.md)]

## <a name="next-steps"> </a>Passaggi successivi

Questa esercitazione ha illustrato le nozioni di base per consentire a un'app di Windows Store di usare Servizi mobili e Hub di notifica per l'invio delle notifiche push. In seguito, è consigliabile eseguire una delle esercitazioni seguenti:

+ [Inviare notifiche push agli utenti autenticati](mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md) <br/>Informazioni sull'uso dei tag per inviare notifiche push dal servizio mobile solo agli utenti autenticati.

+ [Inviare notifiche di trasmissione ai sottoscrittori](../notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) <br/>Informazioni su come gli utenti possono registrarsi e ricevere notifiche push per le categorie a cui sono interessati.

+ [Inviare notifiche indipendenti dalla piattaforma ad altri sottoscrittori](../notification-hubs/notification-hubs-aspnet-cross-platform-notification.md) <br/>Informazioni su come usare i modelli per inviare notifiche push dal servizio mobile senza che sia necessario creare payload specifici della piattaforma nel back-end.

Per altre informazioni su Servizi mobili e su Hub di notifica, fare riferimento ai seguenti argomenti:

* [Hub di notifica di Azure - Linee guida sulla diagnostica](../notification-hubs/notification-hubs-push-notification-fixer.md) <br/>Informazioni sulla risoluzione dei problemi relativi alle notifiche push.

* [Introduzione all'autenticazione] <br/>Informazioni sull'autenticazione degli utenti dell'app con tipi di account diversi tramite Servizi mobili.

* [Informazioni su Hub di notifica] <br/>Altre informazioni sull'uso di Hub di notifica per recapitare le notifiche alle app in tutte le principali piattaforme client.

* [Come usare un client .NET per Servizi mobili di Azure] <br/>Altre informazioni su come usare Servizi mobili dalle app C# per Windows.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Introduzione all'autenticazione]: mobile-services-javascript-backend-windows-universal-dotnet-get-started-users.md

[Send push notifications to authenticated users]: mobile-services-javascript-backend-windows-store-dotnet-push-notifications-app-users.md

[Informazioni su Hub di notifica]: ../notification-hubs-overview.md

[Come usare un client .NET per Servizi mobili di Azure]: mobile-services-windows-dotnet-how-to-use-client-library.md
[MobileServiceClient]: http://go.microsoft.com/fwlink/p/?LinkId=302030
 

<!---HONumber=AcomDC_0622_2016-->