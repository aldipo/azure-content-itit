<properties 
	pageTitle="Creazione di filtri con il .NET SDK di Servizi multimediali di Azure" 
	description="Questo argomento descrive come creare filtri che il client può usare per trasmettere in streaming sezioni specifiche di un flusso. Servizi multimediali crea manifesti dinamici per consentire questo streaming selettivo." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="ne" 
	ms.topic="article" 
	ms.date="04/18/2016"
	ms.author="juliako;cenkdin"/>


#Creazione di filtri con il .NET SDK di Servizi multimediali di Azure

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)

A partire dalla versione 2.11, Servizi multimediali consente di definire filtri per i propri asset. I filtri sono costituiti da regole lato server che consentono ai clienti di eseguire operazioni particolari, come riprodurre solo una sezione di un video (anziché il video intero) oppure specificare solo un sottoinsieme di rendering audio e video, in modo che possa essere gestito dal dispositivo del cliente (anziché tutti i rendering associati all'asset). Il filtro degli asset viene eseguito attraverso **manifesti dinamici** creati su richiesta del cliente per trasmettere un video in streaming in base ai filtri specificati.

Per altre informazioni sui filtri e sul manifesto dinamico, vedere [Filtri e manifesti dinamici](media-services-dynamic-manifest-overview.md).

Questo argomento illustra come usare l’SDK .NET Servizi multimediali per creare, aggiornare ed eliminare filtri.


Si noti che si aggiorna un filtro, l'endpoint di streaming può impiegare fino a due minuti per aggiornare le regole. Se il contenuto è stato trasmesso usando dei filtri (e memorizzato nelle cache dei proxy e delle reti CDN), l'aggiornamento del filtro può determinare un errore del lettore. È consigliabile quindi cancellare la cache dopo aver aggiornato il filtro. Se questa operazione non è consentita, prendere in considerazione la possibilità di usare un filtro diverso.

##Tipi usati per la creazione dei filtri

Durante la creazione dei filtri vengono usati i tipi seguenti:

- **IStreamingFilter**. Questo tipo è basato sulla seguente API REST [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Questo tipo è basato sulla seguente API REST [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Questo tipo è basato sulla seguente API REST [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** e **IFilterTrackPropertyCondition**. Questi tipi sono basati sulle seguenti API REST [FilterTrackSelect e FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##Creare/aggiornare/leggere/eliminare filtri globali

Il seguente codice illustra come utilizzare .NET per creare, aggiornare, leggere ed eliminare filtri di asset.
	
	string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
	            
	List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
	
	FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
	filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
	filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
	filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
	filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
	filterTrackSelectStatements.Add(filterTrackSelectStatement);
	
	// Create
	IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
	
	// Update
	filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
	filter.Update();
	
	// Read
	var filterUpdated = _context.Filters.FirstOrDefault();
	Console.WriteLine(filterUpdated.Name);

	// Delete
	filter.Delete();


##Creare/aggiornare/leggere/eliminare filtri di asset

Il seguente codice illustra come utilizzare .NET per creare, aggiornare, leggere ed eliminare filtri di asset.

	
	string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
	var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
	
	string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
	
	    
	// Create
	IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
	                                    new PresentationTimeRange(), 
	                                    new List<FilterTrackSelectStatement>());
	
	// Update
	filter.PresentationTimeRange = 
	        new PresentationTimeRange(start: 6000000000, end: 72000000000);
	
	filter.Update();
	
	// Read
	asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
	var filterUpdated = asset.AssetFilters.FirstOrDefault();
	Console.WriteLine(filterUpdated.Name);
	
	// Delete
	filterUpdated.Delete();
	



##Creare URL di streaming basati su filtri

Per informazioni su come pubblicare e distribuire asset, vedere [Informazioni generali sulla distribuzione di contenuti ai clienti](media-services-deliver-content-overview.md).


Gli esempi seguenti illustrano come aggiungere filtri agli URL di streaming.

**MPEG DASH**

	http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

	http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

	http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

	http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

	http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##Percorsi di apprendimento di Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornire commenti e suggerimenti

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##Vedere anche 

[Filtri e manifesti dinamici](media-services-dynamic-manifest-overview.md)
 

<!---HONumber=AcomDC_0420_2016-->