<properties
	pageTitle="Risoluzione dei problemi: Riduzione delle prestazioni in un'app Web"
	description="Questo articolo fornisce informazioni utili per la risoluzione dei problemi nell'app Web ospitata nel servizio app di Azure."
	services="app-service\web"
	documentationCenter=""
	authors="cephalin"
	manager="wpickett"
	editor=""
	tags="top-support-issue"/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/13/2016"
	ms.author="cephalin"/>

# Risoluzione dei problemi: Riduzione delle prestazioni in un'app Web

Questo articolo fornisce informazioni utili per la risoluzione dei problemi nell'app Web ospitata nel [Servizio app di Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

Se in qualsiasi punto dell'articolo sono necessarie altre informazioni, è possibile contattare gli esperti di Azure nei [forum MSDN e Stack Overflow dedicati ad Azure](https://azure.microsoft.com/support/forums/). In alternativa, è anche possibile archiviare un evento imprevisto di supporto tecnico di Azure. Passare al [sito di supporto per Azure](https://azure.microsoft.com/support/options/) e fare clic su **Ottenere supporto**.

## Sintomo

Quando si esplora l'app Web, le pagine vengono caricate lentamente e a volte si verifica il timeout.

## Causa

Spesso la causa dell'errore deriva da problemi a livello dell'applicazione, ad esempio:

-	le richieste impiegano troppo tempo
-	utilizzo elevato di memoria/CPU da parte dell'applicazione
-	arresto anomalo dell'applicazione a causa di un'eccezione.

## Passaggi per la risoluzione dei problemi

La risoluzione dei problemi prevede tre attività distinte, in ordine sequenziale:

1.	[Osservare e monitorare il comportamento dell'applicazione](#observe)
2.	[Raccogliere i dati](#collect)
3.	[Attenuare il problema](#mitigate)

In [app Web del servizio app](/services/app-service/web/) vengono presentate diverse opzioni per ogni passaggio.

<a name="observe" />
### 1\. Osservare e monitorare il comportamento dell'applicazione

#### Tenere traccia dell'integrità del servizio

Microsoft Azure pubblica un annuncio ogni volta che si verifica un'interruzione del servizio o una riduzione delle prestazioni. È possibile verificare l'integrità del servizio nel [portale di Azure](https://portal.azure.com/). Per altre informazioni, vedere [Tenere traccia dell'integrità del servizio](../azure-portal/insights-service-health.md).

#### Monitorare l'app Web

Questa opzione consente di trovare eventuali problemi nell'applicazione. Nel pannello dell'app Web fare clic sul riquadro **Richieste ed errori**. Il pannello **Metrica** mostra le metriche che si possono aggiungere.

Le metriche più comunemente monitorate per le app Web sono

-	Working set della memoria medio
-	Tempo medio di risposta
-	Tempo CPU
-	Working set della memoria
-	Richieste

![](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Per altre informazioni, vedere:

-	[Eseguire il monitoraggio delle app Web nel servizio app di Azure](web-sites-monitor.md)
-	[Ricevere notifiche di avviso](../azure-portal/insights-receive-alert-notifications.md)

#### Monitorare lo stato degli endpoint

Se si esegue l'app Web nel piano tariffario **Standard**, App Web consente di monitorare 2 endpoint da 3 aree geografiche.

Il monitoraggio degli endpoint configura i test Web da posizioni distribuite a livello geografico che testano il tempo di risposta e di attività degli URL Web. Il test esegue un'operazione HTTP GET sull'URL Web per determinare il tempo di risposta e di attività da ogni posizione. Ogni posizione configurata esegue un testo ogni cinque minuti.

Il tempo di attività viene monitorato mediante codici di risposta HTTP e il tempo di risposta è misurato in millisecondi. Un test di monitoraggio ha esito negativo se il codice della risposta HTTP è maggiore di o uguale a 400 o se la risposta richiede più di 30 secondi. Un endpoint è considerato disponibile se il test di monitoraggio ha esito positivo da tutte le posizioni specificate.

Per configurarlo, vedere [Procedura: Monitorare lo stato degli endpoint](web-sites-monitor.md#webendpointstatus).

Per altre informazioni sul monitoraggio degli endpoint, vedere anche il video che illustra come [mantenere attivi i siti Web di Azure e monitorare gli endpoint con Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/).

#### Monitoraggio delle prestazioni dell'applicazione usando le estensioni

È anche possibile monitorare le prestazioni dell'applicazione sfruttando le _estensioni del sito_.

Ogni app Web del servizio app fornisce un endpoint di gestione estensibile che consente di sfruttare un set avanzato di strumenti distribuiti come estensioni del sito. Questi strumenti includono ad esempio editor del codice sorgente come [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) e strumenti di gestione per risorse connesse come un database MySQL connesso a un'app Web.

[Azure Application Insights](/services/application-insights/) e [New Relic](/marketplace/partners/newrelic/newrelic/) sono due delle estensioni disponibili di monitoraggio delle prestazioni. Per utilizzare New Relic, è necessario installare un agente in fase di esecuzione. Per usare Azure Application Insights, è necessario ricompilare il codice con un SDK ed è anche possibile installare un'estensione che consente l'accesso a dati aggiuntivi. SDK consente di scrivere il codice per monitorare l'utilizzo e prestazioni dell'applicazione in modo più dettagliato.

Per usare Application Insights, vedere [Monitorare le prestazioni di applicazioni Web](../application-insights/app-insights-web-monitor-performance.md).

Per usare New Relic, vedere [Gestione delle prestazioni delle applicazioni con New Relic in Siti Web di Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### 2\. Raccogliere i dati

####	Abilitare la registrazione diagnostica per l'app Web

L'ambiente App Web offre funzionalità diagnostiche per la registrazione di informazioni sia dal server Web sia dall'applicazione Web, logicamente separate in diagnostica del server Web e diagnostica dell'applicazione.

##### Diagnostica del server Web

È possibile abilitare o disabilitare i seguenti tipi di log:

-	**Registrazione degli errori dettagliata**: consente di registrare informazioni dettagliate sugli errori relativi ai codici di stato HTTP che indicano un'operazione non riuscita (codice di stato 400 o superiore), incluse eventualmente le informazioni che aiutano a determinare il motivo per cui il server ha restituito il codice di errore.
-	**Traccia delle richieste non riuscita**: consente di registrare informazioni dettagliate sulle richieste non riuscite, inclusa una traccia dei componenti IIS utilizzati per elaborare la richieste e il tempo impiegato in ciascun componente. Ciò può essere utile se si sta provando a migliorare le prestazioni delle app Web o a isolare la causa di un errore HTTP specifico.
-	**Registrazione del server Web**: consente di registrare informazioni sulle transazioni HTTP tramite il formato di file di log esteso W3C. Ciò è utile nel determinare le metriche generali dell'app Web, ad esempio il numero delle richieste gestite oppure quante di esse provengono da uno specifico indirizzo IP.

##### Diagnostica applicazioni

La diagnostica applicazioni consente di acquisire le informazioni prodotte da un'applicazione Web. Le applicazioni ASP.NET possono usare la classe `System.Diagnostics.Trace` per registrare le informazioni nel log di diagnostica applicazioni.

Per istruzioni dettagliate su come configurare l'applicazione per la registrazione, vedere [Abilitare la registrazione diagnostica per le app Web nel servizio app di Azure](web-sites-enable-diagnostic-log.md).

#### Usare la profilatura remota

Nel servizio app di Azure è possibile profilare in modalità remota app Web, app per le API e processi Web. Se l'esecuzione del processo è più lenta del previsto o se la latenza delle richieste HTTP è superiore al normale e anche l'utilizzo della CPU da parte del processo è elevato, è possibile profilare il processo in modalità remota e ottenere campioni di stack di chiamate della CPU per analizzare l'attività del processo e i percorsi critici del codice.

Per altre informazioni, vedere il post di blog sul [supporto per la profilatura remota nel servizio app di Azure](/blog/remote-profiling-support-in-azure-app-service).


#### Usare il portale di supporto del servizio app di Azure

Il servizio app Web consente di risolvere i problemi relativi all'app Web grazie ai dati disponibili nei log HTTP, nei log eventi, nei dump dei processi e così via. È possibile accedere a tutte queste informazioni tramite il portale di supporto disponibile all'indirizzo **http://&lt;your nome app>.scm.azurewebsites.net/Support**

Il portale di supporto del servizio app di Azure contiene tre schede separate per supportare i tre passaggi di uno scenario di risoluzione dei problemi comune:

1.	Osservare il comportamento corrente
2.	Eseguire l'analisi tramite la raccolta di informazioni di diagnostica e l'esecuzione di analizzatori predefiniti
3.	Attenuare il problema

Se si riscontra il problema in questo preciso istante, fare clic su **Analizza** > **Diagnostica** > **Esegui diagnosi adesso** per creare una sessione di diagnostica in cui verranno raccolti log HTTP, log del visualizzatore eventi, dump della memoria, log degli errori PHP e report sui processi PHP.

Una volta raccolti i dati, verrà eseguita l'analisi dei dati e verrà generato un report HTML.

Per impostazione predefinita, i dati verranno archiviati nella cartella D:\\home\\data\\DaaS, da cui sarà possibile scaricarli.

Per altre informazioni sul portale di supporto del servizio app di Azure, vedere il post di blog relativo ai [nuovi aggiornamenti all'estensione del sito di supporto per Siti Web di Azure](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### Usare la console di debug Kudu

Il servizio App Web include una console di debug che è possibile usare per il debug, l'esplorazione e il caricamento di file, nonché endpoint JSON per ottenere informazioni sull'ambiente in uso. Questa console è chiamata _console Kudu_ o _dashboard SCM_ dell'app Web.

È possibile accedere a questo dashboard selezionando il collegamento **https://&lt;Your nome app>.scm.azurewebsites.net/**.

Elementi forniti dalla console Kudu:

-	impostazioni di ambiente per l'applicazione
-	log in streaming
-	dump di diagnostica
-	console di debug in cui è possibile eseguire cmdlet di PowerShell e comandi DOS di base.


Inoltre, nel caso in cui l'applicazione generi eccezioni first-chance, è possibile usare Kudu e l'utilità della riga di comando Procdump dello strumento SysInternals per creare dump della memoria. I dump della memoria sono snapshot del processo e semplificano la risoluzione di problemi più complessi riscontrati nell'app Web.

Per altre informazioni sulle funzionalità disponibili in Kudu, vedere il post di blog relativo agli [strumenti di Azure Websites Team Services che è opportuno conoscere](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### 3\. Attenuare il problema

####	Ridimensionare l'app Web

Nel servizio app di Azure, per ottimizzare le prestazioni e la velocità effettiva è possibile modificare il piano in cui è in esecuzione l'applicazione. Ridimensionare un'app Web di Azure implica due azioni correlate: passare a un piano tariffario superiore e configurare determinate impostazioni una volta adottato il nuovo piano.

Per altre informazioni sul ridimensionamento, vedere [Ridimensionare un'app Web nel servizio app di Azure](web-sites-scale.md).

È anche possibile scegliere di eseguire l'applicazione in più di un'istanza. In questo modo, non solo si ottiene una maggiore capacità di elaborazione, ma si può usufruire della tolleranza di errore. Se il processo si arresta in un'istanza, l'altra istanza continuerà a gestire le richieste.

È possibile impostare il ridimensionamento manuale o automatico.

####	Usare la funzionalità AutoHeal

La funzionalità AutoHeal consente di riciclare il processo di lavoro per l'app in base alle impostazioni specificate, ad esempio modifiche di configurazione, richieste, limiti basati sulla memoria o il tempo necessario per l'esecuzione di una richiesta. Nella maggior parte dei casi, riciclare il processo costituisce il modo più veloce per risolvere un problema. Anche se è possibile riavviare l'app Web direttamente dall'interno del portale di Azure, la funzionalità AutoHeal eseguirà questa operazione automaticamente. È sufficiente aggiungere alcuni trigger nel file web.config radice per l'app Web. Si noti che queste impostazioni funzionano allo stesso modo anche per le applicazioni non .NET.

Per altre informazioni, vedere il post di blog relativo alla [correzione automatica di Siti Web di Azure](/blog/auto-healing-windows-azure-web-sites/).

####	Riavviare l'app Web

Questo è spesso il modo più semplice per risolvere problemi occasionali. Nel pannello dell'app Web del [portale di Azure](https://portal.azure.com/) sono disponibili le opzioni per arrestare o riavviare l'app.

 ![](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

È anche possibile gestire l'app Web usando Azure PowerShell. Per altre informazioni, vedere [Uso di Azure PowerShell con Gestione risorse di Azure](../powershell-azure-resource-manager.md).

<!---HONumber=AcomDC_0211_2016-->