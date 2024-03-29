<properties
	pageTitle="Gestire la prima API in Gestione API di Azure | Microsoft Azure"
	description="Informazioni su come creare API, aggiungere operazioni e iniziare a usare Gestione API."
	services="api-management"
	documentationCenter=""
	authors="steved0x"
	manager="erikre"
	editor=""/>

<tags
	ms.service="api-management"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="05/25/2016"
	ms.author="sdanie"/>

# Gestire la prima API in Gestione API di Azure

## <a name="overview"> </a>Panoramica

Questa guida illustra le procedure per iniziare subito a usare Gestione API di Azure ed effettuare la prima chiamata API.

## <a name="concepts"> </a>Cos'è Gestione API di Azure?

È possibile usare Gestione API di Azure per avviare un programma API completo basato su qualsiasi back-end.

Gli scenari comuni includono:

* La **protezione dell'infrastruttura mobile** con la delega dell'accesso con le chiavi API, la prevenzione degli attacchi DOS con la limitazione oppure l'uso di criteri di sicurezza avanzata come la convalida dei token JWT.
* L'**abilitazione degli ecosistemi del partner ISV**, offrendo al partner un rapido caricamento attraverso il portale degli sviluppatori e compilando un'interfaccia API per la separazione dalle implementazioni interne non pronte per l'utilizzo da parte del partner.
* L'**esecuzione di un programma API interno**, offrendo all'organizzazione una posizione centralizzata per la comunicazione della disponibilità e delle ultime modifiche apportate alle API e delegando l'accesso in base agli account aziendali, tutti basati su un canale protetto tra il gateway dell'API e il back-end.


Il sistema è costituito dai componenti seguenti:

* Il **gateway dell'API** è l'endpoint che:
  * Accetta chiamate API e le indirizza al back-end.
  * Verifica le chiavi API, i token JWT, i certificati e altre credenziali.
  * Applica le quote di utilizzo e i limiti di frequenza.
  * Trasforma le API in modo immediato senza modifiche del codice.
  * Memorizza nella cache le risposte di back-end durante l'installazione.
  * Registra i metadati della chiamata a scopi di analisi.

* Il **portale di pubblicazione** è l'interfaccia amministrativa in cui si configura il programma API. Può essere usato per:
	* Definire o importare lo schema API.
	* Creare pacchetti di API nei prodotti.
	* Configurare criteri come quote o trasformazioni sulle API.
	* Ottenere informazioni dall'analisi.
	* Gestire gli utenti.

* Il **portale per sviluppatori** funge da principale presenza Web per gli sviluppatori, che potranno:
	* Leggere la documentazione relativa alle API.
	* Provare un'API con la console interattiva.
	* Creare un account ed eseguire la sottoscrizione per ottenere le chiavi API.
	* Accedere all'analisi di utilizzo personalizzata.


## <a name="create-service-instance"></a>Creare un'istanza di Gestione API

>[AZURE.NOTE] Per completare l'esercitazione, è necessario un account Azure. Se non si ha un account, è possibile creare un account gratuito in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure][].

Per poter usare Gestione API, occorre innanzitutto creare un'istanza del servizio. Accedere al [portale di Azure classico][] e fare clic su **Nuovo**, **Servizi app**, **Gestione API**, **Crea**.

![API Management new instance][api-management-create-instance-menu]

Per **URL** specificare un nome univoco di sottodominio da usare per l'URL del servizio.

Selezionare i valori di **Sottoscrizione** e **Area** desiderati per l'istanza del servizio. Una volta effettuate le selezioni, fare clic sul pulsante **Avanti**.

![New API Management service][api-management-create-instance-step1]

Immettere **Contoso Ltd.** in **Nome organizzazione** e specificare l'indirizzo di posta elettronica nel campo **Indirizzo di posta elettronica dell'amministratore**.

>[AZURE.NOTE] Questo indirizzo di posta elettronica viene usato per le notifiche inviate dal sistema Gestione API. Per altre informazioni, vedere [Come configurare notifiche e modelli di posta elettronica in Gestione API di Azure][].

![New API Management service][api-management-create-instance-step2]

Le istanze del servizio Gestione API sono disponibili in tre livelli: Developer, Standard e Premium. Per impostazione predefinita, le nuove istanze del servizio Gestione API vengono create nel livello Developer. Per selezionare il livello Standard o Premium, selezionare la casella di controllo **Impostazioni avanzate** e scegliere il livello desiderato nella schermata seguente.

>[AZURE.NOTE] Il livello Developer è dedicato allo sviluppo, al test e alla distribuzione pilota di programmi API in cui l'elevata disponibilità non è un fattore rilevante. Nei livelli Standard e Premium è possibile aumentare il numero di unità riservate per gestire un maggior volume di traffico. I livelli Standard e Premium forniscono al servizio Gestione API anche una potenza di elaborazione e prestazioni maggiori. È possibile completare questa esercitazione usando qualsiasi livello. Per altre informazioni sui livelli di Gestione API, vedere [Gestione API - Prezzi][].

Selezionare la casella di controllo per creare l'istanza del servizio.

![New API Management service][api-management-instance-created]

Dopo aver creato l'istanza del servizio, l'operazione successiva consiste nel creare o importare un'API.

## <a name="create-api"> </a>Importare un'API

Un'API rappresenta un set di operazioni che possono essere richiamate da un'applicazione client. Le operazioni API vengono trasmesse tramite proxy ai servizi Web esistenti.

È possibile creare le API (e aggiungere operazioni) in modo manuale oppure è possibile importarle. In questa esercitazione, verrà importata l'API per un servizio Web calcolatrice di esempio fornito da Microsoft e ospitato in Azure.

>[AZURE.NOTE] Per informazioni aggiuntive sulla creazione di un'API e l'aggiunta manuale delle operazioni, vedere [Come creare le API](api-management-howto-create-apis.md) e [Come aggiungere operazioni a un'API](api-management-howto-add-operations.md).

Le API vengono configurate dal portale di pubblicazione, accessibile dal portale di Azure classico. Per accedere al portale di pubblicazione, fare clic su **Gestisci** nel portale di Azure classico per il servizio Gestione API.

![Portale di pubblicazione][api-management-management-console]

Per importare l'API Calcolatrice, fare clic su **API** dal menu **Gestione API** sulla sinistra, quindi scegliere **Importa API**.

![Pulsante Importa API][api-management-import-api]

Per configurare l'API Calcolatrice, seguire questa procedura:

1. Fare clic su **Da URL**, immettere **http://calcapi.cloudapp.net/calcapi.json** nella casella di testo **URL documento specifica** e fare clic sul pulsante di opzione **Swagger**.
2. Digitare **calc** nella casella di testo **Suffisso URL API Web**.
3. Fare clic sulla casella **Prodotti (facoltativo)** e scegliere **Starter**.
4. Fare clic su **Salva** per importare l'API.

![Aggiunta nuova API][api-management-import-new-api]

>[AZURE.NOTE] **Gestione API** supporta attualmente sia la versione 1.2 che la versione 2.0 del documento Swagger per l'importazione. Anche se la [specifica Swagger 2.0](http://swagger.io/specification) dichiara che le proprietà `host`, `basePath` e `schemes` sono facoltative, il documento Swagger 2.0 **DEVE** contenere queste proprietà. In caso contrario, non verrà importato.

Una volta importata l'API, la pagina di riepilogo dell'API viene visualizzata nel portale di pubblicazione.

![API summary][api-management-imported-api-summary]

La sezione API comprende diverse schede. Nella scheda **Riepilogo** sono visualizzate le informazioni e le metriche di base sull'API. È possibile usare la scheda [Impostazioni](api-management-howto-create-apis.md#configure-api-settings) per visualizzare e modificare la configurazione di un'API. La scheda [Operazioni](api-management-howto-add-operations.md) consente di gestire le operazioni dell'API. La scheda **Sicurezza** consente di configurare l'autenticazione del gateway per il server back-end usando l'autenticazione di base o l'[autenticazione reciproca dei certificati](api-management-howto-mutual-certificates.md) e per configurare l'[autorizzazione utente con OAuth 2.0](api-management-howto-oauth2.md). La scheda **Problemi** consente di visualizzare i problemi segnalati dagli sviluppatori che stanno usando le API. La scheda **Prodotti** viene usata per configurare i prodotti contenenti l'API.

Per impostazione predefinita, con ogni istanza di Gestione API vengono forniti due prodotti di esempio:

-	**Starter**
-	**Illimitato**

In questa esercitazione, è stata aggiunta l'API Calcolatrice di base al prodotto Starter quando l'API è stata importata.

Per effettuare chiamate a un'API, gli sviluppatori devono prima sottoscrivere un prodotto che offre l'accesso all'API. La sottoscrizione dei prodotti può essere effettuata direttamente dagli sviluppatori nel portale per sviluppatori; in alternativa, gli amministratori possono sottoscrivere i prodotti per conto degli sviluppatori nel portale di pubblicazione. Dal momento che nei passaggi precedenti di questa esercitazione è stata creata un'istanza di Gestione API, l'utente corrente ha già effettuato la sottoscrizione a ogni prodotto per impostazione predefinita.

## <a name="call-operation"> </a>Chiamare un'operazione dal portale per sviluppatori

È possibile chiamare le operazioni direttamente dal portale per sviluppatori, che consente di visualizzare e testare le operazioni di un'API in tutta comodità. In questo passaggio dell'esercitazione verrà chiamata l'operazione **Aggiungere due integer** dell'API Calcolatrice di base. Fare clic su **Portale per sviluppatori** nel menu in alto a destra del portale di pubblicazione.

![Portale per sviluppatori][api-management-developer-portal-menu]

Fare clic su **API** nel menu superiore e quindi su **Calcolatrice di base** per visualizzare le operazioni disponibili.

![Portale per sviluppatori][api-management-developer-portal-calc-api]

Notare le descrizioni e i parametri di esempio che sono stati importati assieme alle API e alle operazioni, che costituiscono la documentazione destinata agli sviluppatori che useranno l'operazione. È possibile aggiungere queste descrizioni anche quando le operazioni vengono aggiunte manualmente.

Per chiamare l'operazione **Aggiungere due integer**, fare clic su **Prova**.

![Prova][api-management-developer-portal-calc-api-console]

È possibile immettere alcuni valori per i parametri o mantenere quelli predefiniti e quindi fare clic su **Invia**.

![GET HTTP][api-management-invoke-get]

Una volta richiamata un'operazione, nel portale per sviluppatori vengono visualizzati lo **Stato della risposta**, le **Intestazioni della risposta** ed eventualmente il **contenuto della risposta**.

![Response][api-management-invoke-get-response]

## <a name="view-analytics"></a>Visualizzare l'analisi

Per visualizzare l'analisi per la Calcolatrice di base, tornare al portale di pubblicazione selezionando **Gestisci** nel menu in alto a destra nel portale per sviluppatori.

![Manage][api-management-manage-menu]

La visualizzazione predefinita del portale di pubblicazione è il **dashboard**, che offre una panoramica dell'istanza di Gestione API.

![Dashboard][api-management-dashboard]

Passare il puntatore del mouse sul grafico della **Calcolatrice di base** per visualizzare le metriche specifiche relative all'utilizzo dell'API in un dato periodo di tempo.

>[AZURE.NOTE] Se il grafico non contiene linee, tornare al portale per sviluppatori ed effettuare alcune chiamate all'API, attendere qualche secondo e quindi tornare al dashboard.

Fare clic su **Visualizza dettagli** per visualizzare la pagina di riepilogo per l'API, inclusa una versione più estesa delle metriche visualizzate.

![Analytics][api-management-mouse-over]

![Riepilogo][api-management-api-summary-metrics]

Per report e metriche dettagliate fare clic su **Analisi** nel menu **Gestione API** sulla sinistra.

![Panoramica][api-management-analytics-overview]

La sezione **Analisi** include le quattro schede seguenti:

-	**Riepilogo**: fornisce metriche complessive sull'utilizzo e l'integrità, oltre a indicare API, operazioni e prodotti più usati e gli sviluppatori più attivi.
-	**Utilizzo**: fornisce informazioni dettagliate sulle chiamate API e sulla larghezza di banda, inclusa una rappresentazione geografica.
-	**Integrità**: è incentrata sui codici di stato, sulle percentuali di operazioni sulla cache completate, sui tempi di risposta, oltre che sui tempi di risposta di API e servizi.
-	**Attività**: fornisce report che illustrano in dettaglio l'attività specifica in base a sviluppatore, prodotto, API e operazione.

## <a name="next-steps"> </a>Passaggi successivi

-	Vedere gli altri argomenti nell'esercitazione [Introduzione alla configurazione API avanzata][].

[versione di valutazione gratuita di Azure]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Come configurare notifiche e modelli di posta elettronica in Gestione API di Azure]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Introduzione alla configurazione API avanzata]: api-management-get-started-advanced.md
[Gestione API - Prezzi]: http://azure.microsoft.com/pricing/details/api-management/

[portale di Azure classico]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png

<!---HONumber=AcomDC_0525_2016-->