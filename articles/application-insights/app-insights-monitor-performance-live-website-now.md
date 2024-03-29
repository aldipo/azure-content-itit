<properties
	pageTitle="Diagnosi dei problemi di prestazioni in un sito IIS in esecuzione | Microsoft Azure"
	description="Monitorare le prestazioni di un sito Web senza ripetere la distribuzione. Usare la versione autonoma o con Application Insights SDK per ottenere la telemetria di dipendenza."
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
	ms.date="03/09/2016"
	ms.author="awills"/>


# Installare Application Insights Status Monitor per monitorare le prestazioni del sito Web

*Application Insights è disponibile in anteprima.*

Application Insights Status Monitor di Visual Studio consente di diagnosticare le eccezioni e i problemi di prestazioni nelle applicazioni ASP.NET.

![grafici di esempio](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

> [AZURE.TIP] Sono disponibili articoli separati sulla strumentazione [app web live J2EE](app-insights-java-live.md) e [servizi Cloud di Azure](app-insights-cloudservices.md).


È possibile scegliere di applicare Application Insights alle applicazioni Web IIS in tre modi:

* **Tempo di compilazione:** [Aggiungere Application Insights SDK][greenbrown] al codice dell'app Web: Il risultato è il seguente:
 * Una gamma di diagnostica standard e telemetria relativa all'utilizzo.
 * L’[Application Insights API][api] consente di scrivere i propri dati di telemetria per tenere traccia delle informazioni dettagliate sull'utilizzo o diagnosticare i problemi.
* **Tempo di esecuzione**: usare Status Monitor per instrumentare l'app Web sul server.
 * Monitorare le app Web che sono già in esecuzione, senza doverle ricompilare o ripubblicare.
 * Una gamma di diagnostica standard e telemetria relativa all'utilizzo.
 * Diagnostica delle dipendenze&#151;trovare gli errori o le prestazioni scarse in cui l'app usa altri componenti come database, API REST o altri servizi.
 * Risolvere eventuali problemi di telemetria.
* **Entrambi:** compilare l'SDK nel codice dell'app Web ed eseguire Status Monito nel server Web. Offre le migliori funzionalità:
 * Diagnostica standard e telemetria relativa all'utilizzo.
 * Diagnostica delle dipendenze.
 * L'API consente di scrivere dati di telemetria personalizzati.
 * Risolvere eventuali problemi dell'SDK e di telemetria.


## Installare Application Insights Status Monitor

È necessaria una sottoscrizione di [Microsoft Azure](http://azure.com).

### Se l'app è in esecuzione nel server IIS

1. Nel server Web IIS accedere con le credenziali di amministratore.
2. Scaricare e installare il [programma di installazione di Status Monitor](http://go.microsoft.com/fwlink/?LinkId=506648).
4. Nell'Installazione guidata accedere a Microsoft Azure.

    ![Accedere ad Azure con le credenziali dell'account Microsoft](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

    *Errori di connessione Vedere [Risoluzione dei problemi](#troubleshooting).*

5. Selezionare l'applicazione Web installata o il sito Web da monitorare, quindi configurare la risorsa in cui visualizzare i risultati nel portale Application Insights.

    ![Scegliere un'applicazione e una risorsa.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    In genere, si sceglie di configurare una nuova risorsa e un nuovo [gruppo di risorse][roles].

    In alternativa, è possibile usare una risorsa esistente se sono già stati configurati [test Web][availability] per il sito o il [monitoraggio del client Web][client].

6. Riavviare IIS.

    ![Scegliere Riavvia nella parte superiore della finestra di dialogo.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Il servizio Web verrà interrotto per un breve periodo di tempo.

6. Si noti che ApplicationInsights.config è stato inserito tra le app Web da monitorare.

    ![Trovare il file con estensione config insieme ai file di codice dell'app Web.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   Sono inoltre state apportate alcune modifiche al file web.config.

#### Configurare o riconfigurare in un secondo momento

Dopo aver completato la procedura guidata, è possibile riconfigurare l'agente in qualsiasi momento. È inoltre possibile usare questa stessa procedura se l'agente è stato installato ma la configurazione iniziale presenta alcuni problemi.

![Fare clic sull'icona di Application Insights sulla barra delle applicazioni](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)


### Se l'applicazione viene eseguita come un'app Web di Azure

Nel pannello di controllo dell'app Web di Azure aggiungere l'estensione di Application Insights.

![Nell'app Web scegliere Impostazioni, Estensioni, Aggiungi, Application Insights](./media/app-insights-monitor-performance-live-website-now/05-extend.png)


### è un progetto di Servizi cloud di Azure

[Aggiungere gli script ai ruoli Web e di lavoro](app-insights-cloudservices.md)


## Visualizzare i dati di telemetria relativi alle prestazioni

Accedere al [portale di Azure](https://portal.azure.com), passare ad Application Insights e aprire la risorsa creata.

![Scegliere Sfoglia, Application Insights, quindi selezionare l'app](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

Aprire il pannello delle prestazioni per visualizzare una richiesta, il tempo di risposta, le dipendenze e altri dati.

![Prestazioni](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Fare clic per modificare i dettagli della descrizione o aggiungere un nuovo grafico.


![](./media/app-insights-monitor-performance-live-website-now/appinsights-038-dependencies.png)

## Dipendenze

Il grafico di durata delle dipendenze mostra il tempo impiegato dalle chiamate dall'app a componenti esterni quali database, API REST o archiviazione BLOB di Azure.

Per segmentare il grafico in base alle chiamate a diverse dipendenze, selezionare il grafico, attivare il raggruppamento e poi la dipendenza, il tipo di dipendenza o la prestazione di dipendenza.

È inoltre possibile filtrare il grafico per esaminare un bucket specifico di dipendenza, tipo o prestazione. Fare clic su filtri.

## Contatori delle prestazioni

(Non per le app web di Azure.) Fare clic su Server nel pannello della panoramica per visualizzare i grafici dei contatori delle prestazioni del server, come l'utilizzo di memoria e di occupazione della CPU.

Aggiungere un nuovo grafico, o fare clic su qualsiasi grafico per modificare quello che viene visualizzato.

È inoltre possibile [modificare il set di contatori delle prestazioni segnalati da SDK](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3).

## Eccezioni

![Fare clic nel grafico di eccezioni del server](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

È possibile eseguire il drill down di eccezioni specifiche (degli ultimi sette giorni) e ottenere le analisi dello stack e i dati di contesto.

## Campionamento

Se l'applicazione invia una grande quantità di dati ed è in uso Application Insights SDK per ASP.NET 2.0.0 Beta3 o versioni successive, la funzionalità del campionamento adattivo può operare e inviare solo una percentuale dei dati di telemetria. [Altre informazioni sul campionamento.](app-insights-sampling.md)


## Risoluzione dei problemi

### Errori di connessione

È necessario aprire [alcune porte in uscita](app-insights-ip-addresses.md#outgoing-ports) nei firewall del server per consentire il funzionamento di Status Monitor.

### Nessun dato di telemetria?

  * Usare il sito per generare alcuni dati.
  * Attendere qualche minuto l'arrivo dei dati, quindi fare clic su **Aggiorna**.
  * Aprire il riquadro Ricerca diagnostica (il riquadro Ricerca) per visualizzare i singoli eventi. Gli eventi sono spesso visibili in Ricerca diagnostica prima che vengano visualizzati i dati aggregati nei grafici.
  * Aprire Status Monitor e selezionare la propria applicazione nel pannello a sinistra. Verificare se sono presenti messaggi di diagnostica per l'applicazione nella sezione "Configuration notifications":

  ![](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * Assicurarsi che il firewall del server consenta il traffico in uscita sulle porte sopra elencate.
  * Se sul server viene visualizzato un messaggio relativo alle autorizzazioni insufficienti, provare a seguire questa procedura:
    * In Gestione IIS selezionare il pool di applicazioni, aprire **Impostazioni avanzate** e prendere nota dell'identità in **Modello di processo**.
    * Nel pannello di controllo Gestione computer, aggiungere questa identità al gruppo Utenti di Performance Monitor.
  * Se nel server è installato MMA/SCOM, è possibile che alcune versioni entrino in conflitto. Disinstallare SCOM e Status Monitor e reinstallare le versioni più recenti.
  * Vedere [Risoluzione dei problemi][qna].

## Requisiti di sistema

Supporto del sistema operativo per Application Insights Status Monitor sul server

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

con la Service Pack più recente e .NET Framework 4.0 e 4.5

Sul lato client Windows 7, 8 e 8.1, con .NET Framework 4.0 e 4.5

Il supporto IIS è: IIS 7, 7.5, 8, 8.5 (IIS è obbligatorio)

## Automazione con PowerShell

È possibile usare PowerShell per avviare e arrestare il monitoraggio.

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (facoltativo) Nome di un'app Web.
* Visualizza lo stato del monitoraggio di Application Insights per ogni app Web o per l'app denominata nel server IIS.

* Restituisce `ApplicationInsightsApplication` per ogni app:
 * `SdkState==EnabledAfterDeployment`: l'app viene monitorata ed è stata instrumentata in fase di esecuzione dallo strumento Status Monitor oppure da `Start-ApplicationInsightsMonitoring`.
 * `SdkState==Disabled`: l'app non è instrumentata per Application Insights. Non è stata instrumentata oppure il monitoraggio in fase di esecuzione è stato disattivato con lo strumento Status Monitor o con `Stop-ApplicationInsightsMonitoring`.
 * `SdkState==EnabledByCodeInstrumentation`: l'app è stata instrumentata aggiungendo l'SDK al codice sorgente. Il relativo SDK non può essere aggiornato o arrestato.
 * `SdkVersion`: mostra la versione usata per il monitoraggio dell'app.
 * `LatestAvailableSdkVersion`: mostra la versione attualmente disponibile nella raccolta NuGet. Per aggiornare l'app a questa versione, usare `Update-ApplicationInsightsMonitoring`.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name`: nome dell'app in IIS
* `-InstrumentationKey`: valore ikey della risorsa di Application Insights in cui visualizzare i risultati.

* Questo cmdlet influisce solo sulle app che non sono già instrumentate, ovvero SdkState==NotInstrumented.

    Il cmdlet non influisce sulle app già instrumentate, in fase di compilazione aggiungendo l'SDK al codice o in fase di esecuzione da un uso precedente di questo cmdlet.

    La versione SDK usata per instrumentare l'app è la versione scaricata più di recente nel server.

    Per scaricare l'ultima versione, usare Update-ApplicationInsightsVersion.

* Se l'esito è positivo, restituisce `ApplicationInsightsApplication`. Se l'esito è negativo, registra una traccia in stderr.

    
          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name`: nome di un'app in IIS
* `-All`: arresta il monitoraggio di tutte le app nel server IIS per le quali `SdkState==EnabledAfterDeployment`

* Arresta il monitoraggio delle app specificate e rimuove la strumentazione. Funziona solo per le app instrumentate in fase di esecuzione usando lo strumento Status Monitor o Start-ApplicationInsightsApplication, ovvero `SdkState==EnabledAfterDeployment`.

* Restituisce ApplicationInsightsApplication.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: nome di un'app Web in IIS.
* `-InstrumentationKey` Facoltativo. consente di modificare la risorsa a cui vengono inviati i dati di telemetria dell'app.
* Questo cmdlet:
 * Aggiorna l'app denominata alla versione dell'SDK scaricata più di recente nel computer. Funziona solo se `SdkState==EnabledAfterDeployment`.
 * Se si specifica una chiave di strumentazione, l'app denominata viene riconfigurata per l'invio di dati di telemetria alla risorsa con tale chiave. Funziona se `SdkState != Disabled`.

`Update-ApplicationInsightsVersion`

* Scarica l'ultima versione di Application Insights SDK nel server.

## Modello di Azure

Se l'app Web è in Azure e si creano le risorse usando un modello di Azure Resource Manager, è possibile configurare Application Insights aggiungendo quanto segue al nodo risorse:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`: nome della risorsa di Application Insights
* `myWebAppName`: ID dell'app Web

## <a name="next"></a>Passaggi successivi

* [Creare test Web][availability] per assicurarsi che il sito rimanga attivo.
* [Cercare eventi e log][diagnostic] per facilitare la diagnosi dei problemi.
* [Aggiungere dati di telemetria del client Web][usage] per visualizzare le eccezioni dal codice della pagina Web e consentire di inserire le chiamate di traccia.
* [Aggiungere Application Insights SDK al codice del servizio Web][greenbrown] per poter inserire chiamate di traccia e log nel codice del server.

## Video

#### Monitoraggio delle prestazioni

[AZURE.VIDEO app-insights-performance-monitoring]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-web-track-usage.md

<!---HONumber=AcomDC_0629_2016-->