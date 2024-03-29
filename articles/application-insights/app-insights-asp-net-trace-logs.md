<properties 
	pageTitle="Esplorare i log di traccia .NET in Application Insights" 
	description="Cercare i log generati con Trace, NLog o Log4Net." 
	services="application-insights" 
    documentationCenter=".net"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/22/2016" 
	ms.author="awills"/>
 
# Esplorare i log di traccia .NET in Application Insights  

Se si usa NLog, log4Net o System.Diagnostics.Trace per l'analisi diagnostica nell'applicazione ASP.NET, è possibile fare in modo che i log vengano inviati a [Visual Studio Application Insights][start], dove è possibile esplorarli ed eseguirvi ricerche. I log verranno uniti con gli altri eventi di telemetria provenienti dall'applicazione, in modo da potere identificare le tracce associate alla gestione di ogni richiesta dell'utente e metterle in correlazione con altri eventi e i report di eccezioni.

> [AZURE.NOTE] Se è necessario un modulo di acquisizione dei log, questo adattatore è utile per i logger terze parti, ma se non si usa già NLog, log4Net o System.Diagnostics.Trace, chiamare direttamente [TrackTrace() di Application Insights](app-insights-api-custom-events-metrics.md#track-trace).


## Installare la registrazione nell'applicazione

Installare il framework di registrazione scelto nel progetto. Verrà inserita una voce nel file app.config o web.config.

> Se si usa System.Diagnostics.Trace, è necessario aggiungere una voce in web.config.

## Configurare Application Insights per la raccolta dei log

Se l'operazione non è già stata eseguita, **[aggiungere Application Insights al progetto](app-insights-asp-net.md)**. Verrà visualizzata un'opzione per includere la raccolta dei log.

In alternativa, **configurare Application Insights** facendo clic con il pulsante destro del mouse in Esplora soluzioni. Selezionare le opzioni per includere la raccolta dei log.

*Il menu di Application Insights o l'opzione di raccolta non viene visualizzata?* Vedere [Risoluzione dei problemi](#troubleshooting).


## Installazione manuale

Usare questo metodo se il tipo di progetto non è supportato dal programma di installazione di Application Insights, ad esempio un progetto Desktop di Windows.

1. Se si intende usare log4Net o NLog, installarlo nel progetto. 
2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
3. Cercare "Application Insights"

    ![Ottenere la versione preliminare dell'adattatore appropriato](./media/app-insights-asp-net-trace-logs/appinsights-36nuget.png)

4. Selezionare il pacchetto appropriato tra:
  + Microsoft.ApplicationInsights.TraceListener (per acquisire le chiamate System.Diagnostics.Trace)
  + Microsoft.ApplicationInsights.NLogTarget
  + Microsoft.ApplicationInsights.Log4NetAppender

Il pacchetto NuGet installa gli assembly necessari e modifica inoltre web.config o app.config.

## Inserire chiamate di log di diagnostica

Se si usa System.Diagnostics.Trace, una tipica chiamata sarà simile alla seguente:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Se si preferisce log4net o NLog:

    logger.Warn("Slow response - database01");


## Uso diretto dell'API di traccia

È possibile chiamare direttamente l'API di traccia di Application Insights. Gli adattatori di registrazione usano questa API.

Ad esempio:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Un vantaggio di TrackTrace è che è possibile inserire dati relativamente lunghi nel messaggio. Ad esempio, è possibile codificare dati POST.


## Esplorare i log

Eseguire l'app in modalità debug o distribuirla.

Nel pannello Panoramica dell'app nel [portale di Application Insights][portal] scegliere [Cerca][diagnostic].

![In Application Insights scegliere Cerca](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Ricerca diagnostica](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Ad esempio, è possibile:

* Filtrare in base alle tracce dei log o agli elementi con proprietà specifiche
* Esaminare un elemento specifico in modo dettagliato
* Trovare altri eventi di telemetria relativi alla stessa richiesta dell'utente (ovvero, con lo stesso valore OperationId) 
* Salvare la configurazione di questa pagina come preferita

> [AZURE.NOTE] **Campionamento.** Se l'applicazione invia una grande quantità di dati e si sta utilizzando la versione 2.0.0-beta3 o versioni successive dell’SDK di Application Insights per ASP.NET, la funzionalità del campionamento adattivo può operare e inviare solo una percentuale dei dati di telemetria. [Altre informazioni sul campionamento.](app-insights-sampling.md)

## Passaggi successivi

[Diagnosticare errori ed eccezioni in ASP.NET][exceptions]

[Altre informazioni sulla ricerca diagnostica][diagnostic].



## Risoluzione dei problemi

### Come procedere per Java?

Usare gli [adattatori log Java](app-insights-java-trace-logs.md).

### Non è disponibile alcuna opzione di Application Insights nel menu di scelta rapida del progetto

* Verificare che Strumenti Application Insights sia installato nel computer di sviluppo. In Visual Studio, scegliere Strumenti, Estensioni e Aggiornamenti e cercare Strumenti Application Insights. Se non è visualizzato nella scheda degli elementi installati, aprire la scheda Online e installarlo.
* Potrebbe trattarsi di un tipo di progetto non supportato da Strumenti Application Insights. Usare l'[installazione manuale](#manual-installation).

### Nello strumento di configurazione non è disponibile alcuna opzione per l'adattatore log

* È necessario installare innanzitutto il framework di registrazione.
* Se si usa System.Diagnostics.Trace, assicurarsi di [aver eseguito la configurazione in `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Si dispone della versione più recente di Strumenti Application Insights? Nel menu **Strumenti** di Visual Studio scegliere **Estensioni e aggiornamenti** e aprire la scheda **Aggiornamenti**. Se Strumenti Application Insights è presente, fare clic per eseguire l'aggiornamento.


### <a name="emptykey"></a>Viene visualizzato l'errore: "La chiave di strumentazione non può essere vuota"

Risulta che l'utente abbia installato il pacchetto NuGet dell'adattatore di registrazione senza aver installato Application Insights.

In Esplora soluzioni fare clic con il pulsante destro del mouse su `ApplicationInsights.config` e scegliere **Aggiorna Application Insights**. Verrà visualizzata una finestra di dialogo che invita ad accedere ad Azure e a creare una risorsa di Application Insights o a riusarne una esistente. Il problema verrà in tal modo risolto.

### Nella ricerca diagnostica vengono visualizzate le tracce ma non gli altri eventi

Talvolta la visualizzazione di tutti gli eventi e le richieste nella pipeline può richiedere un po' di tempo.

### <a name="limits"></a>Quanti dati vengono conservati?

Fino a 500 eventi al secondo da ciascuna applicazione. Gli eventi vengono conservati per sette giorni.

### Non è possibile vedere alcune delle voci di log previste

Se l'applicazione invia una grande quantità di dati e si sta utilizzando la versione 2.0.0-beta3 o versioni successive dell’SDK di Application Insights per ASP.NET, la funzionalità del campionamento adattivo può operare e inviare solo una percentuale dei dati di telemetria. [Altre informazioni sul campionamento.](app-insights-sampling.md)

## <a name="add"></a>Passaggi successivi

* [Configurare i test di disponibilità e velocità di risposta][availability]
* [Risoluzione dei problemi][qna]





<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-web-failures-exceptions.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

 

<!---HONumber=AcomDC_0224_2016-->