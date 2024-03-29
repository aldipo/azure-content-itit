<properties 
	pageTitle="Abilitare il monitoraggio e la diagnostica" 
	description="Informazioni su come configurare la diagnostica per le risorse in Azure." 
	authors="stepsic-microsoft-com" 
	manager="ronmart" 
	editor="" 
	services="azure-portal" 
	documentationCenter="na"/>

<tags 
	ms.service="azure-portal" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/08/2015" 
	ms.author="stepsic"/>

# Abilitare il monitoraggio e la diagnostica

Nel [portale di Azure](https://portal.azure.com), è possibile configurare rich data di monitoraggio e diagnostica frequenti relativi alle risorse. È possibile usare anche l'[API REST](https://msdn.microsoft.com/library/azure/dn931932.aspx) o [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) per configurare la diagnostica a livello di codice.

I dati di diagnostica, di monitoraggio e delle metriche di Azure vengono salvati nell'account di archiviazione preferito. Questo consente di usare qualsiasi strumento per leggere i dati, ad esempio uno strumento di esplorazione di archiviazione, Power BI o uno strumento di terze parti.

## Quando si crea una risorsa

La maggior parte dei servizi consente di abilitare la diagnostica quando li si crea nel [portale di Azure](https://portal.azure.com).

1. Andare a **Nuovo** e scegliere la risorsa a cui si è interessati. 

2. Selezionare **Configurazione facoltativa**. ![Pannello Diagnostica](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Selezionare **Diagnostica** e fare clic sull'opzione di attivazione. Occorre scegliere l'account di archiviazione in cui salvare la diagnostica. Per l'invio della diagnostica a un account di archiviazione, verranno addebitate le normali tariffe dati per l'archiviazione e le transazioni.

4. Fare clic su **OK** e creare la risorsa.

## Modificare le impostazioni per una risorsa esistente

Se si è già creata una risorsa e si vuole modificare le impostazioni di diagnostica (per modificare il livello di raccolta dati, ad esempio), è possibile farlo direttamente nel portale di Azure.

1. Andare alla risorsa e fare clic sul comando **Impostazioni**.

2. Selezionare **Diagnostica**.

3. Il pannello **Diagnostica** contiene tutte le possibili diagnostiche e i dati delle raccolte di monitoraggio della risorsa. Per alcune risorse è inoltre possibile scegliere un criterio **Conservazione** per i dati, per eliminarli dall'account di archiviazione. ![Diagnostica dell'archiviazione](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Dopo aver scelto le impostazioni, fare clic sul comando **Salva**. La visualizzazione dei dati di monitoraggio potrebbe richiedere un po' di tempo se li si sta abilitando per la prima volta.

### Categorie di raccolta dati per le macchine virtuali
Per le macchine virtuali, tutte le metriche e i log verranno registrati a intervalli di un minuto per far sì che l'utente disponga sempre delle informazioni più aggiornate sulla macchina in uso.

- **Metriche di base**: metriche sull'integrità relative alla macchina virtuale in uso, ad esempio il processore e la memoria 
- **Metriche Web e di rete**: metriche relative alle connessioni di rete e ai servizi Web
- **Metriche di .NET**: metriche relative alle applicazioni .NET e ASP.NET in esecuzione sulla macchina virtuale in uso
- **Metriche di SQL**: metriche delle prestazioni, se si esegue il servizio Microsoft SQL
- **Log eventi dell'applicazione di Windows**: eventi Windows inviati al canale delle applicazioni
- **Log eventi del sistema di Windows**: eventi Windows inviati al canale del sistema, inclusi tutti gli eventi [Microsoft Antimalware](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409) 
- **Log eventi di sicurezza di Windows**: eventi Windows inviati al canale di sicurezza
- **Log dell'infrastruttura di diagnostica**: registrazioni relative all'infrastruttura per la raccolta della diagnostica
- **Log di IIS**: log relativi al server IIS in uso

Tenere presente che in questo momento non sono supportate alcune distribuzioni di Linux e l'agente guest deve essere installato nella macchina virtuale.

## Passaggi successivi

* [Ricevere notifiche di avviso](insights-receive-alert-notifications.md) ogni volta che si verificano eventi operativi o le metriche superano una soglia.
* [Monitorare le metriche del servizio](insights-how-to-customize-monitoring.md) per assicurarsi che il servizio sia disponibile e reattivo.
* [Scalare automaticamente il numero di istanze](insights-how-to-scale.md) per assicurarsi che il servizio venga scalato in base alla domanda.
* [Monitorare le prestazioni dell'applicazione](insights-perf-analytics.md) se si desidera comprendere esattamente il comportamento del codice nel cloud.
* [Visualizzare eventi e registri di controllo](insights-debugging-with-events.md) per informazioni su tutti gli eventi che si sono verificati nel servizio.
* [Tenere traccia dello stato del servizio](insights-service-health.md) per scoprire quando Azure ha subito un calo delle prestazioni o interruzioni del servizio. 
 

<!---HONumber=AcomDC_0128_2016-->