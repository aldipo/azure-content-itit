<properties 
	pageTitle="Gestire i prezzi e la quota per Application Insights" 
	description="Scegliere il piano tariffario necessario" 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2016" 
	ms.author="awills"/>

# Gestire i prezzi e la quota per Application Insights

*Application Insights è disponibile in anteprima.*

I [prezzi][pricing] per [Application Insights di Visual Studio][start] si basano sul volume di dati per ogni applicazione. È anche disponibile un valido livello gratuito, che consente di usufruire di gran parte delle funzionalità, ma con alcune limitazioni.

Ogni risorsa di Application Insights viene addebitata come un servizio separato e contribuisce alla fatturazione per la sottoscrizione di Azure.

[Vedere lo schema dei prezzi][pricing].

## Esaminare la quota e il piano tariffario per la propria risorsa di Application Insights

È possibile aprire il pannello Quota + Prezzi dalle Impostazioni della risorsa dell'applicazione.

![Scegliere le impostazioni, la quota e i prezzi](./media/app-insights-pricing/01-pricing.png)

La scelta dei prezzi di schema riguarda:

* [Quota mensile](#monthly-quota) - la quantità di dati di telemetria che è possibile analizzare ogni mese.
* [Velocità dati](#data-rate) - la velocità massima, in cui è possibile elaborare i dati dall'applicazione.
* [Esportazione continua](#continuous-export) - se è possibile esportare dati in altri strumenti e servizi.

Questi limiti vengono impostati separatamente per ogni risorsa Application Insights.

### Versione di valutazione Premium gratuita

Quando si crea una nuova risorsa di Application Insights, viene avviata nel livello gratuito.

In qualsiasi momento, è possibile passare alla versione valutazione Premium gratuita di 30 giorni, che offre tutti i vantaggi del livello Premium. Dopo 30 giorni, verrà automaticamente ripristinato il livello precedente, a meno che non si scelga esplicitamente un altro livello. È possibile selezionare il livello desiderato in qualsiasi momento durante il periodo di valutazione, ma si otterrà comunque la versione di valutazione gratuita fino alla fine del periodo di 30 giorni.


## Quota mensile

* In ogni mese di calendario, l'applicazione può inviare fino a una quantità specificata di telemetria ad Application Insights. Attualmente, la quota per il piano tariffario gratuito è 5 milioni di punti dati al mese, e sostanzialmente più elevato per gli altri schemi; è possibile acquistarne ulteriori se si raggiunge la quota. Per i numeri effetti, vedere lo [schema dei prezzi][pricing].
* La quota dipende dal piano tariffario scelto.
* La quota viene conteggiata da mezzanotte UTC il primo giorno di ogni mese.
* Il grafico Punti dati mostra la quantità di quota che è stata usata questo mese.
* La quota viene misurata in *punti dati*. Un singolo punto dati è una chiamata a uno dei metodi Track, se chiamato in modo esplicito nel codice o da uno dei moduli di telemetria standard. Può presentare più metriche e proprietà associate.
* I punti dati vengono generati da:
 * [Moduli di SDK](app-insights-configuration-with-applicationinsights-config.md) che raccolgono automaticamente i dati, ad esempio per segnalare una richiesta o un arresto anomalo oppure per misurare le prestazioni.
 * Chiamate [API](app-insights-api-custom-events-metrics.md) `Track...` scritte dall'utente, ad esempio `TrackEvent` o `trackPageView`.
 * [Test Web di disponibilità](app-insights-monitor-web-app-availability.md) configurati dall'utente.
* Durante il debug, è possibile visualizzare i punti dati inviati dall'app nella finestra di output di Visual Studio. Gli eventi client possono essere visualizzati aprendo la scheda di rete nel riquadro di debug del browser (in genere F12).
* I *dati della sessione* non vengono conteggiati nella quota. Ciò include il numero di utenti, sessioni, ambienti e dati del dispositivo.
* Se si desidera determinare il numero di punti dati in ispezione, è possibile ottenerli in varie posizioni:
 * Ogni elemento visualizzato in [ricerca diagnostica](app-insights-diagnostic-search.md), che include le richieste, le eccezioni, le tracce del log, le visualizzazioni di pagina, gli eventi di dipendenza e gli eventi personalizzati HTTP.
 * Ogni misurazione non elaborata di una [metrica](app-insights-metrics-explorer.md), ad esempio un contatore delle prestazioni. I punti visualizzati nel grafico sono di solito aggregazioni di più punti dati non elaborati.
 * Ogni punto in un grafico di disponibilità web è anche un'aggregazione di più punti dati.
* È anche possibile esaminare singoli punti dati all'origine durante il debug:
 * Se si esegue l'app in modalità di debug in Visual Studio, i punti dati vengono registrati nella finestra di output.
 * Per visualizzare i punti dati client, aprire il riquadro di debug del browser (in genere F12) e aprire la scheda di rete.
* Per impostazione predefinita, la velocità dei dati può essere ridotta dal [campionamento adattivo](app-insights-sampling.md). Ciò significa che, con l'aumento dell'utilizzo dell'app, la velocità dei dati di telemetria non aumenta come previsto.

### Eccedenza

Se l'applicazione invia una quantità superiore alla quota mensile, è possibile:

* Pagare i dati aggiuntivi. Per i dettagli, vedere lo [schema dei prezzi][pricing]. È possibile scegliere questa opzione in anticipo. Questa opzione non è disponibile nel piano tariffario Gratuito.
* Aggiornare il piano tariffario.
* Non eseguire alcuna operazione. I dati della sessione continueranno a essere registrati, ma altri dati non verranno visualizzati in ricerca diagnostica o in Esplora metriche.


### Quanto dati vengono inviati?

Il grafico nella parte inferiore del pannello dei prezzi mostra l'utilizzo dei punti dati dell'applicazione, raggruppati per tipo di punto dati. (È possibile inoltre creare il grafico in Esplorazione di metrica.)

![Nella parte inferiore del pannello dei prezzi](./media/app-insights-pricing/03-allocation.png)

Fare clic sul grafico per altri dettagli o trascinare e fare clic su (+) per visualizzare i dettagli relativi a un intervallo di tempo.

Il grafico indica il volume di dati in arrivo nel servizio Application Insights dopo il [campionamento](app-insights-sampling.md).


## Velocità dei dati

Oltre alla quota mensile, esistono limiti della limitazione sulla percentuale di dati. Per il [piano tariffario][pricing] gratuito il limite è di 200 punti dati al secondo in base a una media calcolata su 5 minuti; per piani a pagamento è di 500 punti dati al secondo in base a una media calcolata su 1 minuto.

Esistono tre bucket che vengono conteggiati separatamente:

* [Chiamate TrackTrace](app-insights-api-custom-events-metrics.md#track-trace) e [log acquisiti](app-insights-asp-net-trace-logs.md).
* [Eccezioni](app-insights-api-custom-events-metrics.md#track-exception), limitate a 50 punti al secondo.
* Tutti gli altri dati di telemetria (visualizzazioni pagina, sessioni, richieste, dipendenze, metrica, eventi personalizzati, risultati dei test Web).



*Cosa accade se l'app supera la velocità al secondo.*

* Il volume dei dati inviati dall'app viene valutato ogni minuto. Se il valore supera la velocità media al secondo calcolata nel minuto, il server rifiuta alcune richieste. Alcune versioni dell’SDK tentano quindi di inviarle di nuovo, distribuendo un picco per diversi minuti; altre, ad esempio l’SDK JavaScript, semplicemente eliminano i dati rifiutati.

In caso di avvenuta limitazione, verrà visualizzata una notifica che avviserà che ciò si è verificato.

*Come è possibile sapere quanti punti dati vengono inviati dall'app?*

* Aprire Impostazioni/Quota e Prezzi per vedere il grafico Volume dati.
* Altrimenti, in Esplora metriche, aggiungere un nuovo grafico e selezionare **Volume punti dati** come metrica. Attivare il raggruppamento in base a **Tipo di dati**.


### Suggerimenti per ridurre la velocità dei dati

Se si verificano i limiti della limitazione, ecco alcune operazioni da eseguire:

* Utilizzare [Campionamento](app-insights-sampling.md). Questa tecnologia riduce la frequenza dei dati senza deviare le metriche e senza compromettere la possibilità di spostarsi tra elementi correlati nella Ricerca.
* [Limitare il numero di chiamate Ajax che possono essere segnalate](app-insights-javascript.md#detailed-configuration) in ogni visualizzazione pagina o disattivare la creazione di report Ajax.
* Disattivare i moduli di raccolta non necessari [modificando il file ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Ad esempio, è possibile che i contatori delle prestazioni o dati sulle dipendenze siano non essenziali.
* Pre-aggregare metriche. Se sono state inserite chiamate a TrackMetric nell'applicazione, è possibile ridurre il traffico utilizzando l'overload che accetta il calcolo della media e la deviazione standard di un batch di misurazioni. In alternativa è possibile usare un [pacchetto di pre-aggregazione](https://www.myget.org/gallery/applicationinsights-sdk-labs).


## Campionamento

Il [campionamento](app-insights-sampling.md) consente di ridurre la frequenza con cui i dati di telemetria vengono inviati all'app, pur mantenendo la possibilità di trovare gli eventi correlati durante le ricerche di diagnostica e il conteggio corretto degli eventi. Il campionamento permette di evitare il superamento della quota mensile.

Sono disponibili diversi tipi di campionamento. È consigliabile usare il [campionamento adattivo](app-insights-sampling.md) che si adatta automaticamente al volume dei dati di telemetria inviati dall'app. Il campionamento viene eseguito nell'SDK dell'app Web riducendo il traffico dei dati di telemetria sulla rete. È possibile usarlo se il framework dell'app Web è .NET. Installare quindi l'ultima versione (beta) dell'SDK.

In alternativa, è possibile impostare il *campionamento per inserimento* nel pannello Quota + prezzi. Questo tipo di campionamento opera nel punto in cui i dati di telemetria provenienti dall'app raggiungono il servizio Application Insights. Non ha alcun effetto sul volume dei dati di telemetria inviati dall'app, ma riduce tuttavia il volume mantenuto dal servizio.

![Dal pannello Quota + prezzi, fare clic sul riquadro degli esempi e selezionare una frazione di campionamento.](./media/app-insights-pricing/04.png)

Il campionamento consente di ridurre in modo efficace i costi e di non superare la quota mensile. L'algoritmo di campionamento conserva gli elementi correlati ai dati di telemetria, in modo che, quando si usa la ricerca, ad esempio, è possibile trovare la richiesta correlata a una particolare eccezione. L'algoritmo mantiene inoltre i conteggi corretti e consente di visualizzare in Esplora metriche i valori corretti della frequenza delle richieste, della frequenza delle eccezioni e di altri contatori.


## Esaminare la fattura per la sottoscrizione ad Azure

Gli addebiti di Application Insights vengono aggiunti alla fatturazione di Azure. È possibile visualizzare i dettagli della fattura Azure nella sezione Fatturazione del portale di Azure oppure nel [portale di fatturazione di Azure](https://account.windowsazure.com/Subscriptions).

![Nel menu laterale scegliere Fatturazione.](./media/app-insights-pricing/02-billing.png)



## Limiti del nome

1.	Al massimo 200 nomi di metrica univoci e 200 nomi di proprietà univoci per l'applicazione. Le metriche includono l'invio di dati tramite TrackMetric, nonché le misurazioni di altri tipi di dati, ad esempio gli eventi. I [nomi di metriche e di proprietà][api] sono globali per una chiave di strumentazione.
2.	Le [proprietà][apiproperties] possono essere usate per le operazioni di filtro e di raggruppamento solo quando hanno meno di 100 valori univoci per ogni proprietà. Dopo che il numero di valori univoci ha superato i 100, è ancora possibile cercare la proprietà, ma non è più possibile usarla per le operazioni di filtro o di raggruppamento.
3.	Proprietà standard, ad esempio Nome richiesta e URL pagina sono limitate a 1000 valori univoci alla settimana. Superati i 1000 valori univoci, i valori aggiuntivi vengono contrassegnati come "Altri valori". I valori originali possono essere ancora usati per la ricerca full-text e il filtro.

Se l'applicazione sta per superare questi limiti, considerare la possibilità di dividere i dati tra chiavi di strumentazione diverse, ovvero di [creare nuove risorse di Application Insights](app-insights-create-new-resource.md) e inviare alcuni dati alle nuove chiavi di strumentazione. È possibile che il risultato venga strutturato meglio. È possibile usare i [dashboard](app-insights-dashboards.md#dashboards) per visualizzare le diverse metriche nella stessa schermata, quindi questo approccio non impedisce di confrontare metriche diverse.

## Riepilogo dei limiti

[AZURE.INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

 

<!---HONumber=AcomDC_0629_2016-->