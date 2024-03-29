<properties 
	pageTitle="Configurare l'analisi di app Web per ASP.NET con Application Insights" 
	description="Configurare l'analisi delle prestazioni, della disponibilità e dell'uso per un sito Web ASP.NET, ospitato in locale o in Azure." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="06/21/2016" 
	ms.author="awills"/>


# Installare Application Insights per ASP.NET

[Visual Studio Application Insights](app-insights-overview.md) consente di monitorare un'applicazione live per [rilevare e diagnosticare i problemi di prestazioni e le eccezioni](app-insights-detect-triage-diagnose.md) e [individuare la modalità di uso dell'app](app-insights-overview-usage.md). Può essere usato per app ospitate nei server IIS locali o in VM del cloud oppure per app Web di Azure.


## Prima di iniziare

È necessario:

* Visual Studio 2013 Update 3 o versioni successive. È preferibile una versione successiva.
* Una sottoscrizione a [Microsoft Azure](http://azure.com). Se il team o l'organizzazione ha una sottoscrizione di Azure, il proprietario potrà aggiungere l'utente alla sottoscrizione usando il rispettivo [account Microsoft](http://live.com). 

Se si è interessati, sono disponibili articoli alternativi sugli argomenti seguenti:

* [Strumentazione di un'app Web in fase di esecuzione](app-insights-monitor-performance-live-website-now.md)
* [Servizi cloud di Azure](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Aggiungere Application Insights SDK


### Se è un nuovo progetto...

Quando si crea un nuovo progetto in Visual Studio, verificare che Application Insights sia selezionato.


![Creazione di un progetto ASP.NET](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


### o se è un progetto esistente

Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere **Aggiungi telemetria di Application Insights** o **Configura Application Insights**.

![Scegliere Aggiungi Application Insights](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* Se si usa un progetto ASP.NET Core, [seguire queste istruzioni per correggere alcune righe di codice](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 



## <a name="run"></a> 2. Eseguire l'app

Eseguire l'applicazione con F5 e provarla aprendo pagine diverse per generare alcuni dati di telemetria.

In Visual Studio verrà visualizzato il conteggio degli eventi che sono stati registrati.

![In Visual Studio il pulsante Application Insights viene visualizzato durante il debug.](./media/app-insights-asp-net/54.png)

## 3\. Visualizzare i dati di telemetria

### In Visual Studio

Aprire la finestra di Application Insights in Visual Studio facendo clic sul pulsante Application Insights oppure facendo clic con il pulsante destro del mouse sul progetto in Esplora soluzioni:

![In Visual Studio il pulsante Application Insights viene visualizzato durante il debug.](./media/app-insights-asp-net/55.png)

Questa visualizzazione contiene i dati di telemetria generati sul lato server dell'app. Sperimentare i filtri e fare clic su qualsiasi evento per visualizzare altri dettagli.

[Uso di Application Insights in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### Nel portale

Se non è stata scelta l'opzione *Installa solo SDK*, i dati di telemetria possono essere visualizzati anche nel portale Web di Application Insights.

Il portale offre un maggior numero di grafici, strumenti di analisi e dashboard rispetto a Visual Studio.


Aprire la risorsa Application Insights nel [portale di Azure](https://portal.azure.com/).

![Fare clic con il pulsante destro del mouse sul progetto e aprire il portale di Azure](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Nel portale verrà visualizzata la telemetria dell'app: ![](./media/app-insights-asp-net/66.png)

* I singoli eventi vengono visualizzati in **Cerca** (1). I dati vengono visualizzati prima qui e in [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) (Flusso metriche attivo). Fare clic su qualsiasi evento per visualizzarne le proprietà. 
* Le metriche aggregate vengono visualizzate nei grafici (2). Potrebbe essere necessario qualche minuto prima che i dati vengano visualizzati qui. Fare clic su qualsiasi grafico per aprire un pannello con altri dettagli.

[Spostamento e dashboard nel portale Application Insights](app-insights-dashboards.md).

##<a name="land"></a> Cosa fa il comando 'Aggiungi Application Insights'

Application Insights invia i dati di telemetria dell'app al portale di Application Insights, ospitato in Microsoft Azure:

![](./media/app-insights-asp-net/01-scheme.png)

Il comando esegue quindi tre operazioni:

1. Aggiunge il pacchetto NuGet della versione Web di Application Insights SDK al progetto. In Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere Gestisci pacchetti NuGet.
2. Crea una risorsa di Application Insights nel [portale di Azure](https://portal.azure.com/). È qui che verranno visualizzati i dati. Recupera la *chiave di strumentazione*, che identifica la risorsa.
3. Inserisce la chiave di strumentazione in `ApplicationInsights.config`, in modo che l'SDK possa inviare dati di telemetria al portale.

Se si vuole, è possibile eseguire questi passaggi manualmente per [ASP.NET 4](app-insights-asp-net-manual.md) o [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

## Per eseguire l'aggiornamento a future versioni di SDK

Per eseguire l'aggiornamento a una [nuova versione dell'SDK](app-insights-release-notes-dotnet.md), riaprire Gestione pacchetti NuGet e filtrare i pacchetti installati. Selezionare Microsoft.ApplicationInsights.Web e scegliere Aggiorna

Se sono state eseguite tutte le personalizzazioni apportate al file ApplicationInsights.config, salvarne una copia prima di eseguire l'aggiornamento e, successivamente, unire le modifiche nella nuova versione.



## Passaggi successivi

| | 
|---|---
|**[Uso di Application Insights in Visual Studio](app-insights-visual-studio.md)**<br/>Debug con telemetria, ricerca diagnostica e drill-through nel codice.|![Visual Studio](./media/app-insights-asp-net/61.png)
|**[Navigazione e dashboard nel portale Application Insights](app-insights-dashboards.md)**<br/>Dashboard, strumenti avanzati di diagnostica e di analisi, avvisi, mappa attiva delle dipendenze dell'applicazione ed esportazione dei dati di telemetria. |![Visual Studio](./media/app-insights-asp-net/62.png)
|**[Altri dati di telemetria da Application Insights](app-insights-asp-net-more.md)**<br/> Monitorare l'uso, la disponibilità, le dipendenze e le eccezioni, integrare le tracce dei framework di registrazione e scrivere telemetria personalizzata. | ![Visual Studio](./media/app-insights-asp-net/64.png)

<!---HONumber=AcomDC_0622_2016-->