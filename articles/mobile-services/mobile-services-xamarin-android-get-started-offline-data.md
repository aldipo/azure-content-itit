<properties
	pageTitle="Uso dei dati offline in Servizi mobili (Xamarin per Android) | Microsoft Azure"
	description="Informazioni su come usare Servizi mobili di Azure per memorizzare nella cache e sincronizzare i dati offline nell'applicazione per Xamarin Android"
	documentationCenter="xamarin"
	authors="lindydonna"
	editor="wesmc"
	manager="dwrede"
	services="mobile-services"/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin-android"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="03/16/2016"
	ms.author="donnam"/>

# Uso della sincronizzazione dei dati offline in Servizi mobili

[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Per la versione equivalente di questo argomento per le app per dispositivi mobili, vedere [Abilitare la sincronizzazione offline per l'app per dispositivi mobili Xamarin.Android](../app-service-mobile/app-service-mobile-xamarin-android-get-started-offline-data.md).

Questo argomento descrive le funzionalità di sincronizzazione offline di Servizi mobili di Azure nell'app di guida introduttiva TodoList. La sincronizzazione offline consente di creare facilmente app utilizzabili anche quando l'utente finale non ha accesso alla rete.

La sincronizzazione offline ha diversi usi potenziali:

* Migliorare la velocità di risposta dell'app memorizzando i dati del server nella cache locale del dispositivo
* Rendere le app resilienti rispetto alla connettività di rete intermittente
* Permettere agli utenti finali di creare e modificare i dati anche in mancanza di accesso di rete, supportando scenari con connettività scarsa o assente
* Sincronizzare i dati tra più dispositivi e rilevare i conflitti quando lo stesso record viene modificato da due dispositivi

>[AZURE.NOTE] Per completare l'esercitazione, è necessario un account Azure. Se non si ha un account, è possibile iscriversi per accedere a una versione di valutazione di Azure e ottenere un massimo di 10 servizi mobili gratuiti che potranno essere usati anche dopo il termine del periodo di valutazione. Per informazioni dettagliate, vedere la pagina relativa alla <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=AE564AB28" target="_blank">versione di valutazione gratuita di Azure</a>.
>
> Se si tratta della prima esperienza con Servizi mobili, è consigliabile iniziare dall'esercitazione [Introduzione a Servizi mobili].

Questa esercitazione descrive le operazioni di base seguenti:

1. [Esaminare il codice di sincronizzazione di Servizi mobili]
2. [Aggiornare il comportamento di sincronizzazione dell'app]
3. [Aggiornare l'app per la riconnessione al servizio mobile]

Per completare questa esercitazione, è necessario disporre di:

* Visual Studio con Xamarin su Windows o Xamarin Studio su Mac OS X. Per istruzioni complete sull'installazione , vedere [Configurazione e installazione di Visual Studio e Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* Esercitazione [Introduzione a Servizi mobili] completata.
 
## <a name="review-offline"></a>Esaminare il codice di sincronizzazione di Servizi mobili

La sincronizzazione offline di Servizi mobili di Azure consente agli utenti finali di interagire con un database locale quando la rete non è accessibile. Per usare queste funzionalità nell'app, inizializzare `MobileServiceClient.SyncContext` in un archivio locale. Quindi, fare riferimento alla tabella tramite l'interfaccia di `IMobileServiceSyncTable`. Questa sezione illustra il codice correlato alla sincronizzazione offline in `ToDoActivity.cs`.

1. In Visual Studio o Xamarin Studio aprire il progetto completato nell'esercitazione [Introduzione a Servizi mobili]. Aprire il file `ToDoActivity.cs`.

2. Notare che il tipo del membro `toDoTable` è `IMobileServiceSyncTable`. La sincronizzazione offline usa questa interfaccia della tabella di sincronizzazione anziché `IMobileServiceTable`. Quando si usa una tabella di sincronizzazione, tutte le operazioni vengono inviate all'archivio locale e vengono sincronizzate con il servizio remoto solo mediante operazioni push e pull esplicite.

    Per ottenere un riferimento a una tabella di sincronizzazione, viene usato il metodo `GetSyncTable()`. Per rimuovere la funzionalità di sincronizzazione offline, usare invece `GetTable()`.

3. Prima di poter eseguire qualsiasi operazione su tabella, è necessario inizializzare l'archivio locale. Questa operazione viene effettuata nel metodo `InitLocalStoreAsync`:

        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }

    Viene creato un archivio locale usando la classe `MobileServiceSQLiteStore`, disponibile in Mobile Services SDK. È anche possibile fornire un'implementazione di archivio locale diversa, implementando `IMobileServiceLocalStore`.

    Il metodo `DefineTable` crea nell'archivio locale una tabella corrispondente ai campi del tipo specificato, `ToDoItem` in questo caso. Il tipo non deve includere tutte le colonne presenti nel database remoto, è possibile anche archiviare solo un subset di colonne.

    Questo overload di `InitializeAsync` usa il gestore di conflitti predefinito, che non consente l'esecuzione di operazioni in caso di conflitto. Per fornire un gestore di conflitti personalizzato, vedere l'esercitazione [Gestione dei conflitti con il supporto offline per Servizi mobili].

4. Il metodo `SyncAsync` attiva l'effettiva operazione di sincronizzazione:

        private async Task SyncAsync()
        {
            await client.SyncContext.PushAsync();
            await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
        }

    Prima di tutto, viene eseguita una chiamata a `IMobileServiceSyncContext.PushAsync()`. Questo metodo fa parte di `IMobileServicesSyncContext` invece che della tabella di sincronizzazione perché effettuerà il push delle modifiche in tutte le tabelle. Solo i record che sono stati in qualche modo modificati localmente (tramite le operazioni CUD) verranno inviati al server.

    Successivamente, il metodo chiama `IMobileServiceSyncTable.PullAsync()` per eseguire il pull dei dati da una tabella del server all'app. Si noti che se nel contesto di sincronizzazione sono presenti modifiche in sospeso, un'operazione pull effettua sempre un'operazione push all'inizio. Lo scopo è assicurare che tutte le tabelle nell'archivio locale e le relazioni siano coerenti. In questo caso, è stata effettuata una chiamate esplicita a push.

    In questo esempio vengono recuperati tutti i record presenti nella tabella `TodoItem` remota, ma è anche possibile filtrare i record passando una query. Il primo parametro di `PullAsync()` è un ID di query usato per la sincronizzazione incrementale, che usa il timestamp `UpdatedAt` per ottenere i soli record modificati dopo l'ultima sincronizzazione. L'ID di query deve essere una stringa descrittiva univoca per ogni query logica presente nell'app. Per rifiutare esplicitamente la sincronizzazione incrementale, passare `null` come ID di query. In ogni operazione pull verranno recuperati tutti i record e questo potrebbe creare inefficienze.

    >[AZURE.NOTE] Per rimuovere i record dall'archivio locale del dispositivo quando sono stati eliminati dal database del servizio mobile, è necessario abilitare l'[eliminazione temporanea]. In alternativa, l'app deve periodicamente chiamare `IMobileServiceSyncTable.PurgeAsync()` per ripulire l'archivio locale.

    Si noti che può verificarsi `MobileServicePushFailedException` per un'operazione sia push che pull. L'esercitazione successiva, [Gestione dei conflitti con il supporto offline per Servizi mobili], mostra come gestire queste eccezioni relative alla sincronizzazione.

5. Nella classe `ToDoActivity` il metodo `SyncAsync()` viene chiamato dopo le operazioni che modificano i dati, `AddItem()` e `CheckItem()`. Viene inoltre chiamato da `OnRefreshItemsSelected()`, in modo che gli utenti ottengano i dati più recenti ogni volta che premono il pulsante per l'aggiornamento. L'app esegue anche una sincronizzazione all'avvio, in quanto `ToDoActivity.OnCreate()` chiama `OnRefreshItemsSelected()`.

    Poiché ogni volta che vengono modificati i dati viene chiamato `SyncAsync()`, quest'app presuppone che l'utente sia online quando modifica i dati. Nella sezione successiva, l'app verrà aggiornata in modo che gli utenti possano apportare modifiche anche offline.

## <a name="update-sync"></a>Aggiornare il comportamento di sincronizzazione dell'app

In questa sezione si procederà alla modifica dell'app in modo che non effettui la sincronizzazione all'avvio né quando vengono eseguite operazioni di inserimento e aggiornamento, ma solo quando viene premuto il pulsante di aggiornamento. Successivamente, verrà interrotta la connessione dell'app al servizio mobile per simulare uno scenario offline. Quando si aggiungono elementi di dati, questi vengono conservati nell'archivio locale, ma non immediatamente sincronizzati con il servizio mobile.

1. Nella classe `ToDoActivity` modificare i metodi `AddItem()` e `CheckItem()` per impostare come commento le chiamate a `SyncAsync()`.

2. In `ToDoActivity` impostare come commento le definizioni dei membri `applicationURL` e `applicationKey`. Aggiungere le righe seguenti, che fanno riferimento a un URL del servizio mobile non valido:

        const string applicationURL = @"https://your-mobile-service.azure-mobile.xxx/";
        const string applicationKey = @"AppKey";

3. In `ToDoActivity.OnCreate()` rimuovere la chiamata a `OnRefreshItemsSelected()` e sostituirla con:

        // Load the items from the Mobile Service
        // OnRefreshItemsSelected (); // don't sync on app launch
        await RefreshItemsFromTableAsync(); // load UI only

4. Compilare ed eseguire l'app. Aggiungere nuovi elementi todo. I nuovi elementi todo esistono solo nell'archivio locale finché non è possibile effettuarne il push al servizio mobile. L'app client si comporta come se fosse connessa al servizio mobile, supportando tutte le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione).

5. Chiudere l'app e riavviarla per verificare che i nuovi elementi creati siano salvati in modo permanente nell'archivio locale.

## <a name="update-online-app"></a>Aggiornare l'app per la riconnessione al servizio mobile

In questa sezione verrà effettuata la riconnessione dell'app al servizio mobile. Viene simulato il passaggio dell'app dallo stato offline allo stato online con il servizio mobile. Quando si preme il pulsante di aggiornamento, i dati vengono sincronizzati con il servizio mobile.

1. Aprire `ToDoActivity.cs`. Rimuovere l'URL del servizio mobile non valido e aggiungere nuovamente l'URL e la chiave dell'app corretti.

2. Ricompilare ed eseguire l'app. Si noti che i dati appaiono uguali a quelli dello scenario offline nonostante l'app sia ora connessa al servizio mobile. Il motivo è che l'app usa sempre l'interfaccia `IMobileServiceSyncTable` che fa riferimento all'archivio locale.

3. Accedere al [portale di Azure classico] ed esaminare il database del servizio mobile. Se il servizio usa il back-end JavaScript, è possibile visualizzare i dati dalla scheda **Dati** del servizio mobile.

    Se si usa il back-end .NET per il servizio mobile, in Visual Studio passare a **Esplora server** > **Azure** > **Database SQL**. Fare clic con il pulsante destro del mouse sul database e scegliere **Apri in Esplora oggetti di SQL Server**.

    Si noti che i dati *non* sono stati sincronizzati tra il database e l'archivio locale.

4. Nell'app premere il pulsante di aggiornamento. Verrà chiamato `OnRefreshItemsSelected()`, che a sua volta chiama `SyncAsync()`. Verranno eseguite le operazioni push e pull, prima inviando gli elementi dell'archivio locale al servizio mobile, quindi recuperando i nuovi dati dal servizio mobile.

5. Controllare il database del servizio mobile per verificare che le modifiche siano state sincronizzate.

##Riepilogo

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## Passaggi successivi

* [Gestione dei conflitti con il supporto offline per Servizi mobili]

* [Come usare il componente client Xamarin per Servizi mobili di Azure]

<!-- Anchors. -->
[Esaminare il codice di sincronizzazione di Servizi mobili]: #review-offline
[Aggiornare il comportamento di sincronizzazione dell'app]: #update-sync
[Aggiornare l'app per la riconnessione al servizio mobile]: #update-online-app

<!-- Images -->


<!-- URLs. -->
[Gestione dei conflitti con il supporto offline per Servizi mobili]: mobile-services-windows-store-dotnet-handling-conflicts-offline-data.md
[Introduzione a Servizi mobili]: mobile-services-android-get-started.md
[Come usare il componente client Xamarin per Servizi mobili di Azure]: partner-xamarin-mobile-services-how-to-use-client-library.md
[eliminazione temporanea]: mobile-services-using-soft-delete.md

[Mobile Services SDK Nuget]: http://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.0
[SQLite store nuget]: http://www.nuget.org/packages/WindowsAzure.MobileServices.SQLiteStore/1.0.0
[portale di Azure classico]: https://manage.windowsazure.com

<!---HONumber=AcomDC_0413_2016-->