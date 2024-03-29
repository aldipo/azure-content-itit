<properties 
	pageTitle="Scalare un'app Web in Servizio app di Azure" 
	description="Come aumentare e scalare orizzontalmente un'app Web nel servizio app di Azure e applicare la scalabilità automatica." 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="mollybos"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/25/2016" 
	ms.author="cephalin"/>

# Scalare un'app Web in Servizio app di Azure #

Per migliorare le prestazioni e la velocità effettiva delle app Web in Microsoft Azure, è possibile utilizzare il [portale di Azure](http://portal.azure.com) e scalare il piano [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) dalla modalità **Gratuita** a quella **Condivisa**, **Base**, **Standard** o **Premium**.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Scalare le app Web di Azure implica due azioni correlate: modifica della modalità del piano App Service a un livello più elevato e configurazione di determinate impostazioni dopo essere passati a un livello di servizio più elevato. In questo articolo vengono illustrati entrambi gli argomenti. Livelli di servizio più elevati quali le modalità **Standard** e **Premium** offrono maggiore potenza e flessibilità nella determinazione della modalità di utilizzo delle risorse in Azure.

Le modifiche di scalabilità diventano effettive in pochi secondi e interessano tutte le app Web nel piano servizio app. Non richiedono alcuna modifica del codice o la ridistribuzione delle applicazioni.

Per informazioni su Azure App Service Per informazioni sui piani di servizio app, vedere [Informazioni sui piani di servizio app](../app-service/app-service-how-works-readme.md) e [Panoramica approfondita dei piani di servizio app di Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Per informazioni sui prezzi e le funzionalità dei singoli piani di servizio app, vedere [Dettagli prezzi del servizio app](/pricing/details/web-sites/).

> [AZURE.NOTE] Prima di passare un'app Web dalla modalità **Gratuita** a **Base**, **Standard** o **Premium**, è necessario innanzitutto rimuovere i limiti di spesa applicati alla sottoscrizione per Azure App Service. Per visualizzare o modificare le opzioni per la sottoscrizione del servizio app di Microsoft Azure, vedere [Sottoscrizioni di Microsoft Azure][azuresubscriptions].

<a name="scalingsharedorbasic"></a>
<!-- ===================================== -->
## Passaggio alla modalità Condivisa o Base
<!-- ===================================== -->

1. Accedere al [portale di Azure][portal] dal browser.
	
2. Nel pannello dell'app Web fare clic su **Tutte le impostazioni** e quindi su **Aumentare**.
	
	![Scegliere il piano][ChooseWHP]
	
4. Nel pannello **Scegliere il piano tariffario** selezionare tra le modalità di piano **Condiviso** o **Basic**, quindi fare clic su **Seleziona**.
	
	Dopo il completamento dell'operazione, nella scheda **Notifiche** verrà visualizzata la scritta verde lampeggiante **OPERAZIONE RIUSCITA**.
	
5. In impostazioni fare clic su **Scalare orizzontalmente**, selezionare *Conteggio istanze scelto manualmente* nell'elenco a discesa, far scorrere la barra **Istanza** da sinistra a destra per aumentare il numero di istanze, quindi fare clic su **Salva** nella barra dei comandi. L'opzione relativa alle dimensioni delle istanze non è disponibile nella modalità **condivisa**. Per altre informazioni sulle dimensioni delle istanze, vedere [Prezzi di Servizio app][vmsizes].
	
	![Dimensioni delle istanze per la modalità base][ChooseBasicInstances]
	
	Dopo il completamento dell'operazione, nella scheda **Notifiche** verrà visualizzata la scritta verde lampeggiante **OPERAZIONE RIUSCITA**.
	
<a name="scalingstandard"></a>
<!-- ================================= -->
## Passaggio alla modalità Standard o Premium
<!-- ================================= -->

> [AZURE.NOTE] Prima di passare alla modalità del piano App Service **Standard** o **Premium**, è necessario rimuovere i limiti di spesa applicati alla sottoscrizione per Microsoft Azure App Service. In caso contrario, qualora si raggiungano i limiti di spesa prima della fine del periodo di fatturazione, l'app Web potrebbe non essere più disponibile. Per visualizzare o modificare le opzioni per la sottoscrizione del servizio app di Microsoft Azure, vedere [Sottoscrizioni di Microsoft Azure][azuresubscriptions].

1. Per scalare alla modalità **Standard** o **Premium**, attenersi agli stessi passaggi iniziali del passaggio a **Condivisa** o **Base**, quindi scegliere la modalità **Standard** or **Premium** in **Scegliere il livello di prezzo** e fare clic su **Seleziona**. 
	
	Dopo il completamento dell'operazione, nella scheda **Notifiche** verrà visualizzata la scritta verde lampeggiante **OPERAZIONE RIUSCITA** e verrà abilitata la **Scalabilità automatica**.
	
	![Passare alla modalità Standard o Premium][ScaleStandard]
	
	Sarà comunque possibile far scorrere il cursore della barra **Istanza** per applicare manualmente la scalabilità su più istanze, come descritto in precedenza per la modalità **Basic**. In questo articolo, tuttavia, viene descritto come scalare automaticamente l'app.
	
2. In **Ridimensiona di** selezionare **regole pianificazione e le prestazioni** per scalare automaticamente l'app.
	
	![Modalità di ridimensionamento automatico impostato su Prestazioni][Autoscale]
	
3. In **Impostazioni** fare clic su **Scala predefinita 1-1**, spostare i due dispositivi di scorrimento per definire il numero minimo e massimo di istanze da scalare automaticamente per il piano del servizio app. Per questa esercitazione, spostare il dispositivo di scorrimento su **6** istanze.
	
4. Fare clic su **OK**.
	
4. In **impostazioni** fare clic su **Percentuale CPU > 80 (aumenta numero di 1)** per configurare le regole di scalabilità automatica per la metrica predefinita.
	
	![Impostare le metriche di destinazione][SetTargetMetrics]
	
	È possibile configurare le regole di scalabilità automatica per varie metriche di prestazioni, quali CPU, memoria, coda dischi, coda HTTP e flusso di dati. In questa esercitazione si configurerà la scalabilità automatica per la percentuale di CPU in modo da ottenere:
	
	- Aumento di 1 istanza se l'utilizzo della CPU è oltre il 80% negli ultimi 10 minuti
	- Aumento fino a 3 istanze se l'utilizzo della CPU è oltre il 90% negli ultimi 5 minuti
	- Riduzione di 1 istanza se l'utilizzo della CPU è inferiore al 50% negli ultimi 30 minuti 
	
	
4. Lasciare la voce **Percentuale CPU** selezionata nell'elenco a discesa **Nome metrica**.
	
5. In **Regole di aumento** configurare la prima regola impostando **Operatore** su **Maggiore di**, **Soglia** su **70** (%), **Durata** su **10** (minuti), **Aggregazione temporale** su Media, **Azione** su **aumenta numero di** su **1** (istanza) e **Disattiva regole dopo** su **10** (minuti).
	
	![Impostare la prima regola di scalabilità automatica][SetFirstRule]
	
	>[AZURE.NOTE] L'impostazione **Disattiva regole** dopo specifica l'intervallo di attesa applicato alla regola dopo l'azione di scalabilità precedente, prima di eseguire una nuova azione di scalabilità.
	
6. Fare clic su **Aggiungi regola** e configurare la seconda regola impostando **Operatore** su **Maggiore di**, **Soglia** su **90** (%), **Durata** su **1** (minuti), **Aggregazione temporale** su Media, **Azione** su **aumenta numero di**, **Valore** su **3** (istanza) e **Disattiva regole dopo** su **1** (minuti).

7. Fare clic su **OK**.
	
	![Impostare la seconda regola scalabilità automatica][SetSecondRule]
	
5. In **Impostazioni**, fare clic su **Aggiungi regola** per configurare la terza regola impostando **Operatore** su **Minore di**, **Soglia** su **50** (%), **Durata** su **30** (minuti), **Aggregazione temporale** su **Media**, **Azione** su **riduci numero di**,**Valore** su **1** (istanza) e **Disattiva regole dopo** su **60** (minuti).
	
	![Impostare la terza regola scalabilità automatica][SetThirdRule]
	
7. Fare clic su **OK**. La regola di scalabilità automatica dovrebbe ora essere riflessa nel pannello **Impostazioni scalabilità**.
	
	![Impostare il risultato della regola di scalabilità automatica][SetRulesFinal]

<a name="ScalingSQLServer"></a>
##Ridimensionamento di un database SQL Server connesso all'app Web
Se sono presenti uno o più database SQL Server collegati all'app Web, indipendentemente dalla modalità di piano di servizio app, è possibile ridimensionarli rapidamente secondo le esigenze.

1. Per scalare uno dei database collegati, aprire il pannello dell'app Web nel [portale di Azure][portal]. Nell'elenco a discesa comprimibile **Informazioni di base** fare clic sul collegamento **Gruppo di risorse**. Quindi, nella parte **Riepilogo** del pannello relativo al gruppo di risorse, fare clic su uno dei database collegati.

	![Database collegato][ResourceGroup]
	
2. Nel pannello relativo ai database SQL collegati fare clic sulla parte **Impostazioni** > **Piano tariffario** selezionare uno dei piani in base alle esigenze di prestazioni, quindi scegliere **Seleziona**.
	
	![Scalare il database SQL][ScaleDatabase]
	
3. È anche possibile impostare la replica geografica per potenziare le funzionalità di disponibilità elevata e ripristino di emergenza del database SQL. Per eseguire questa operazione, fare clic sulla parte **Replica geografica**.
	
	![Configurare la replica geografica per il database SQL][GeoReplication]

<a name="devfeatures"></a>
## Funzionalità per sviluppatori
A seconda della modalità dell'app Web, sono disponibili le seguenti funzionalità orientate agli sviluppatori:

### Numero di bit ###

- Le modalità **Base**, **Standard** e **Premium** supportano applicazioni a 64 bit e a 32 bit.
- Le modalità del piano **gratuita** e **condivisa** supportano solo applicazioni a 32 bit.

### Supporto del debugger ###

- Il supporto del debugger è disponibile per le modalità **Gratuita**, **Condivisa** e **Base** a una connessione simultanea per piano App Service.
- Il supporto del debugger è disponibile per le modalità **Standard** e **Premium** a 5 connessioni simultanee per piano App Service.

<a name="OtherFeatures"></a>
## Altre funzionalità

### Monitoraggio degli endpoint Web ###

- Il monitoraggio degli endpoint Web è disponibile nelle modalità **Base**, **Standard** e **Premium**. Per altre informazioni sul monitoraggio degli endpoint Web, vedere [Come monitorare le app Web](web-sites-monitor.md).

- Per informazioni dettagliate su tutte le altre funzionalità disponibili nei piani di servizio app, inclusi i prezzi e le funzionalità di interesse per tutti gli utenti (compresi gli sviluppatori), vedere [Dettagli prezzi del servizio app](/pricing/details/web-sites/).

>[AZURE.NOTE] Per iniziare a usare Servizio app di Azure prima di registrarsi per ottenere un account Azure, andare a [Prova il servizio app](http://go.microsoft.com/fwlink/?LinkId=523751), dove è possibile creare un'app Web iniziale temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.

<a name="Next Steps"></a>
## Passaggi successivi

- Per iniziare a usare Azure, vedere la pagina relativa alla [versione di valutazione gratuita di Microsoft Azure](/pricing/free-trial/).
- Per informazioni su prezzi, supporto e contratti di servizio, visitare i collegamenti seguenti:
	
	[Dettagli prezzi dei trasferimenti di dati](/pricing/details/data-transfers/)
	
	[Piani di supporto per Azure](/support/plans/)
	
	[Contratti di servizio](/support/legal/sla/)
	
	[Dettagli prezzi - Database SQL](/pricing/details/sql-database/)
	
	[Dimensioni delle macchine virtuali e dei servizi cloud per Azure][vmsizes]
	
	[Dettagli prezzi del servizio app](/pricing/details/web-sites/)
	
	[Dettagli prezzi del servizio app - Connessioni SSL](/pricing/details/web-sites/#ssl-connections)

- Per informazioni sulle procedure consigliate per Azure App Service, inclusa la creazione di un'architettura scalabile e resiliente, vedere l'articolo di blog relativo alle [procedure consigliate per App Web del servizio app di Azure](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).

- Video sul ridimensionamento delle app Web:
	
	- [Quando è necessario ridimensionare i siti Web di Azure - con Stefan Schackow](/documentation/videos/azure-web-sites-free-vs-standard-scaling/)
	- [Scalabilità automatica per i siti Web di Azure, pianificata o in base alla CPU - con Stefan Schackow](/documentation/videos/auto-scaling-azure-web-sites/)
	- [Che cosa accade durante il ridimensionamento dei siti Web di Azure - con Stefan Schackow](/documentation/videos/how-azure-web-sites-scale/)

## Modifiche apportate
* Per una guida relativa al passaggio da Siti Web al servizio app, vedere [Servizio app di Azure e impatto sui servizi di Azure esistenti](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- LINKS -->
[vmsizes]: /pricing/details/app-service/
[SQLaccountsbilling]: http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]: http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
 

<!---HONumber=AcomDC_0518_2016-->