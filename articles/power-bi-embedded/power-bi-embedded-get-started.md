<properties
   pageTitle="Introduzione a Microsoft Power BI Embedded - Anteprima"
   description="Power BI Embedded, aggiungere report di Power BI interattivi nell'applicazione di Business Intelligence"
   services="power-bi-embedded"
   documentationCenter=""
   authors="minewiskan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="06/28/2016"
   ms.author="owend"/>

# Introduzione a Microsoft Power BI Embedded - Anteprima

**Power BI Embedded** è un servizio di Azure che consente agli sviluppatori di applicazioni di aggiungere report interattivi di Power BI alle proprie applicazioni. **Power BI Embedded** interagisce con le applicazioni esistenti senza che sia necessario riprogettarle o modificare la modalità di accesso da parte degli utenti.

Per altre informazioni su Power BI Embedded, vedere [Informazioni su Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md).

Come descritto in [Informazioni su Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md), il provisioning delle risorse per **Microsoft Power BI Embedded** viene effettuato tramite le [API di Azure Resource Manager](https://msdn.microsoft.com/library/mt712306.aspx). In questo caso, la risorsa di cui si effettua il provisioning è una **Raccolta di aree di lavoro di Power BI**. La sezione seguente illustra come creare una raccolta di aree di lavoro.

![](media\power-bi-embedded-get-started\introduction.png)

## Creare una raccolta di aree di lavoro
Una **Raccolta di aree di lavoro** è la risorsa di primo livello di Azure e un contenitore per il contenuto che verrà incorporato nell'applicazione. Si può creare una **Raccolta di aree di lavoro** in due modi:

   -	Uso manuale del portale di Azure
   -	Uso delle API di Azure Resource Manager a livello di codice

Ecco la procedura per compilare una **Raccolta di aree di lavoro** tramite il portale di Azure.

   1.	Aprire il **portale di Azure** ed eseguire l'accesso: [http://portal.azure.com](http://portal.azure.com).

   2.	Fare clic su **+ Nuovo** nel pannello in alto.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.	In **Dati e analisi** fare clic su **Power BI Embedded**.
   4.	Nel **pannello di creazione** immettere le informazioni necessarie. Per **Prezzi**, vedere [Prezzi di Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Fare clic su **Crea**.

Il provisioning della **Raccolta di aree di lavoro** richiederà alcuni istanti. Al termine verrà visualizzato il pannello della **Raccolta di aree di lavoro**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

Questo **pannello di creazione** contiene le informazioni necessarie chiamare le API che creano le aree di lavoro e vi distribuiscono il contenuto.

La sezione successiva descrive come usare le **Chiavi di accesso** per generare **token dell'app** che saranno usati per autenticare le richieste delle API.

<a name="view-access-keys"/>
## Visualizzare le chiavi di accesso dell'API Power BI

Le **Chiavi di accesso** sono tra le informazioni più importanti necessarie per chiamare le API REST Power BI. Vengono usate per generare i **token dell'app** che servono per autenticare le richieste delle API. Per visualizzare le **Chiavi di accesso**, fare clic su **Chiavi di accesso** nel pannello **Impostazioni**. Per altre informazioni sui **token delle app**, vedere l'articolo relativo al [funzionamento dei token delle app](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Come si noterà, sono disponibili due chiavi.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Copiarle e archiviarle in modo sicuro nell'applicazione. È molto importante gestire queste chiavi come una password, perché forniscono l'accesso a tutto il contenuto della **Raccolta di aree di lavoro**.

Anche se sono disponibili due chiavi, è necessaria una sola chiave alla volta. La seconda chiave viene fornita per consentire di rigenerare periodicamente le chiavi senza interrompere l'accesso al servizio.

Dopo aver creato un'istanza di Power BI per l'applicazione e le **Chiavi di accesso**, è possibile importare un report nell'app in uso. Prima di apprendere come importare un report, la sezione successiva descrive come creare set di dati e report di Power BI da incorporare in un'app.

## Creare set di dati e report di Power BI da incorporare in un'app

Dopo avere creato un'istanza di Power BI per l'applicazione e avere preso nota delle **Chiavi di accesso**, è necessario creare i set di dati e i report di Power BI da incorporare. I set di dati e i report possono essere creati con **Power BI Desktop**. È possibile scaricare [Power BI Desktop gratuitamente](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). In alternativa, per iniziare rapidamente, è possibile scaricare l'[esempio Retail Analysis PBIX](http://go.microsoft.com/fwlink/?LinkID=780547). Per altre informazioni su come usare **Power BI Desktop**, vedere l'articolo di [introduzione a Power BI Desktop](https://powerbi.microsoft.com/it-IT/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

**Power BI Desktop** consente di connettersi all'origine dati mediante l'importazione di una copia dei dati in **Power BI Desktop** o tramite la connessione diretta all'origine dati con **DirectQuery**.

Ecco le differenze tra l'**importazione** e la modalità **DirectQuery**.

|Importazione | DirectQuery
|---|---
|Tabelle, colonne, *e dati* vengono importati o copiati in **Power BI Desktop**. Mentre si utilizzano le visualizzazioni, **Power BI Desktop** esegue query su una copia dei dati. Per visualizzare le eventuali modifiche apportate ai dati sottostanti, è necessario aggiornare o importare di nuovo un set di dati completo e aggiornato.|Solo *tabelle e colonne * vengono importate o copiate in **Power BI Desktop**. Mentre si utilizzano le visualizzazioni, **Power BI Desktop** esegue query sull'origine dati sottostante, quindi vengono sempre visualizzati dati aggiornati.

Per altre informazioni sulla connessione a un'origine dati, vedere [Connessione a un'origine dati](power-bi-embedded-connect-datasource.md).

Dopo aver salvato il lavoro in **Power BI Desktop**, viene creato un file PBIX. Questo file contiene il report. Se si importano dati, il PBIX include il set di dati completo, mentre se si usa **DirectQuery**, il PBIX contiene solo uno schema del set di dati. Distribuire a livello di codice il PBIX nell'area di lavoro con l'[API di importazione di Power BI](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI Embedded** include altre API per modificare il server e il database a cui punta il set di dati e impostare le credenziali dell'account del servizio che saranno usate dal set di dati per la connessione al database. Vedere gli articoli relativi a [POST di SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) e [PATCH dell'origine dati del gateway](https://msdn.microsoft.com/library/mt711498.aspx).

## Passaggi successivi
Nei passaggi precedenti è stata creata una raccolta di aree di lavoro e i primi report e set di dati. A questo punto è possibile apprendere come scrivere codice per **Power BI Embedded**. Per iniziare, è stata creata un'app Web da usare come [esempio introduttivo](power-bi-embedded-get-started-sample.md). L'esempio illustra come:

  -	Effettuare il provisioning del contenuto
      - Creare un'area di lavoro
      - Importare un file PBIX
      - Aggiornare le stringhe di connessione e impostare le credenziali per i set di dati.

  -	Incorporare un report in modo sicuro

## Vedere anche
- [Esempio introduttivo](power-bi-embedded-get-started-sample.md)
- [Informazioni su Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Introduzione a Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
- [Prezzi di Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527)

<!---HONumber=AcomDC_0629_2016-->