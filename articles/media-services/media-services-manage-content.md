<properties 
	pageTitle="Come gestire i contenuti multimediali con Servizi multimediali di Azure tramite il portale di Azure classico" 
	description="Informazioni su come gestire i contenuti multimediali in Servizi multimediali di Azure. Sono inclusi: caricamento, l'indicizzazione, codifica, crittografia e pubblicazione." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2016"  
	ms.author="juliako"/>


# Gestire i contenuti con Servizi multimediali di Azure tramite il portale di Azure classico


Questo argomento descrive come usare il portale di Azure classico per gestire i contenuti multimediali nell'account di Servizi multimediali.

Questo argomento illustra come eseguire le seguenti operazioni sul contenuto direttamente dal portale:

- Visualizzare informazioni sul contenuto, ad esempio stato di pubblicazione, URL di pubblicazione, dimensione, data e ora dell'ultimo aggiornamento e se l'asset è crittografato.
- Caricare nuovo contenuto
- Indicizzazione dei contenuti
- Codificare contenuti
- Crittografare il contenuto
- Pubblicare/annullare la pubblicazione di contenuto
- Riprodurre il contenuto


##<a id="upload"></a>Procedura: Caricare contenuto


[AZURE.INCLUDE [media-services-selector-upload-files](../../includes/media-services-selector-upload-files.md)]


1. Nel [portale di Azure classico](http://go.microsoft.com/fwlink/?LinkID=256666&clcid=0x409) fare clic su **Servizi multimediali** e quindi sul nome dell'account di Servizi multimediali.
2. Selezionare la pagina CONTENT.
3. Fare clic sul pulsante **Upload** nella pagina o nella parte inferiore del portale.
4. Nella finestra di dialogo **Upload content** selezionare il file di asset desiderato. Fare clic sul file e quindi su **Open** oppure premere **INVIO**.

	![Finestra di dialogo Carica contenuto][uploadcontent]

5. Nella finestra di dialogo Carica contenuto fare clic sul pulsante con il segno di spunta per accettare il file e il nome del contenuto.
6. Verrà avviato il caricamento e sarà possibile visualizzarne lo stato nella parte inferiore del portale.

	![Stato processo][status]

Al termine del caricamento, il nuovo asset verrà visualizzato nell'elenco dei contenuti. Per convenzione, al nome verrà aggiunto il suffisso "**-Source**" per consentire di tenere traccia del nuovo contenuto come contenuto di origine per le attività di codifica.

![ContentPage][contentpage]

Se il valore delle dimensioni del file non viene aggiornato al termine del processo di caricamento, premere il pulsante **Sync Metadata**. Le dimensioni del file di asset verranno sincronizzate con le dimensioni effettive del file nell'archivio e verrà aggiornato il valore nella pagina dei contenuti.

##<a id="index"></a>Procedura: Indicizzare i contenuti.

> [AZURE.SELECTOR]
- [.NET](media-services-index-content.md)
- [Portale](media-services-manage-content.md#index)

Azure Media Indexer consente di rendere disponibile per la ricerca il contenuto dei file multimediali e di generare una trascrizione full-text per i sottotitoli codificati e le parole chiave. L'indicizzazione del contenuto può essere eseguita dal portale di Azure classico con la procedura riportata di seguito. Tuttavia, per avere più controllo sui file e sulle modalità del processo di indicizzazione, è possibile usare l'SDK di Servizi multimediali per .NET o le API REST. Per altre informazioni, vedere [Indicizzazione di file multimediali con Azure Media Indexer](media-services-index-content.md).

I seguenti passaggi mostrano come usare il portale di Azure classico per indicizzare il contenuto.

1. Selezionare il file da indicizzare. Se l'indicizzazione è supportata per questo tipo di file, il pulsante ELABORA viene abilitato nella parte inferiore della pagina CONTENUTO.
1. Premere il pulsante ELABORA.
2. Nella finestra di dialogo **Elabora** scegliere il processore **Azure Media Indexer**.
3. Quindi, inserire nella finestra di dialogo Elabora le informazioni dettagliate su **titolo** e **descrizione** relative al file multimediale di input.

![Process][process]

##<a id="encode"></a>Procedura: Codificare contenuti

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-asset.md)
- [REST](media-services-rest-encode-asset.md)
- [Portale](media-services-manage-content.md#encode)

Per distribuire un video digitale tramite Internet è necessario comprimere il file multimediale. Servizi multimediali fornisce un codificatore multimediale che consente di specificare la modalità di codifica dei contenuti (ad esempio, i codec da usare, il formato di file, la risoluzione e la velocità in bit).

Quando si usa Servizi multimediali di Azure, uno degli scenari più frequenti consiste nella distribuzione di contenuti in streaming a velocità in bit adattiva ai client. Con lo streaming a velocità in bit adattiva, il client può passare a un flusso con velocità in bit maggiore o minore durante la visualizzazione del video, in base alla larghezza di banda attuale della rete, all'utilizzo della CPU e ad altri fattori. Servizi multimediali supporta le seguenti tecnologie di streaming a velocità in bit adattiva: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH e HDS (solo per i titolari di licenza Adobe PrimeTime/Access).

Servizi multimediali fornisce il servizio di creazione dinamica dei pacchetti, che consente di distribuire i contenuti codificati in formato MP4 o Smooth Streaming con velocità in bit adattiva in formati di streaming supportati da Servizi multimediali (MPEG DASH, HLS, Smooth Streaming, HDS), senza dover ricreare i pacchetti con questi formati di streaming.

Per sfruttare i vantaggi del servizio di creazione dinamica dei pacchetti, è necessario seguire questa procedura:

- Codificare il file in formato intermedio (di origine) in un set di file MP4 o Smooth Streaming a velocità in bit adattiva (i passaggi per la codifica sono descritti più avanti in questa esercitazione).
- Ottenere almeno un'unità di streaming on demand per l'endpoint di streaming da cui si pianifica la distribuzione dei contenuti. Per altre informazioni, vedere l'articolo sulla [procedura per scalare unità riservate di streaming on demand](media-services-manage-origins.md#scale_streaming_endpoints/).

Con la creazione dinamica dei pacchetti si archiviano e si pagano solo i file in un unico formato di archiviazione e Servizi multimediali crea e fornisce la risposta appropriata in base alle richieste di un client.

Oltre alle funzionalità di creazione dinamica dei pacchetti, le unità riservate di streaming on demand offrono capacità in uscita dedicate, acquistabili in incrementi di 200 Mbps. Per impostazione predefinita, lo streaming on demand è configurato in un modello di istanza condivisa in base al quale le risorse del server (ad esempio, calcolo, capacità in uscita e così via) vengono condivise con tutti gli altri utenti. Per migliorare la velocità effettiva dello streaming on demand, si consiglia di acquistare unità riservate di streaming on demand.

Questa sezione descrive la procedura per la codifica di contenuti con Media Encoder Standard tramite il portale di Azure classico.

1.  Selezionare il file da codificare.

  Se la codifica è supportata per questo tipo di file, il pulsante ELABORA viene abilitato nella parte inferiore della pagina CONTENUTO.
4. Nella finestra di dialogo **Elabora** selezionare il processore **Media Encoder Standard**.
5. Scegliere una delle **configurazioni di codifica**.

![Process2][process2]


L'argomento relativo alle [stringhe di set di impostazioni di attività per Media Encoder Standard](https://msdn.microsoft.com/library/mt269960) illustra il significato di ogni set di impostazioni.

5. Quindi, immettere il nome del contenuto di output descrittivo desiderato oppure accettare il valore predefinito. Fare quindi clic sul pulsante con il segno di spunta per avviare l'operazione di codifica. Nella parte inferiore del portale sarà possibile monitorare l'avanzamento dell'operazione.
6. Premere OK.

Al termine della codifica, la pagina CONTENUTO includerà il file codificato.

Per visualizzare l'avanzamento del processo di codifica, passare alla pagina **PROCESSI**.

Se il valore delle dimensioni del file non viene aggiornato al termine del processo di codifica, premere il pulsante **Sync Metadata**. Le dimensioni del file di asset di output verranno sincronizzate con le dimensioni effettive del file nell'archivio e verrà aggiornato il valore nella pagina Content.

##<a id="encrypt"></a>Procedura: Crittografare il contenuto


Se si desidera crittografare in modo dinamico l'asset in Servizi multimediali con una chiave AES o PlayReady DRM, assicurarsi di seguire questa procedura:

- Codificare il file in formato intermedio (di origine) in un set di file MP4 o Smooth Streaming a velocità in bit adattiva (i passaggi per la codifica sono descritti nella sezione [Codificare il contenuto](#encode)).
- Ottenere almeno un'unità di streaming on demand per l'endpoint di streaming da cui si pianifica la distribuzione dei contenuti. Per altre informazioni, vedere l'articolo sulla [procedura per scalare unità riservate di streaming on demand](media-services-manage-origins.md#scale_streaming_endpoints/).
- Configurare "i criteri di servizio predefiniti relativi alla chiave AES" o "i criteri di servizio predefiniti relativi alla licenza". Per altre informazioni, vedere l'argomento [Configurare i criteri di autorizzazione della chiave simmetrica](media-services-portal-configure-content-key-auth-policy.md).


	Quando si è pronti ad abilitare la crittografia, premere il pulsante **CRITTOGRAFIA** nella parte inferiore della pagina **CONTENUTO**.

	![Crittografare il contenuto][encrypt]

	Dopo aver abilitato la crittografia, ogni volta che un flusso viene richiesto da un lettore, Servizi multimediali usa la chiave specificata per crittografare dinamicamente i contenuti mediante AES o PlayReady. Per decrittografare il flusso, il lettore richiederà la chiave dal servizio di distribuzione delle chiavi. Per decidere se l'utente è autorizzato a ottenere la chiave, il servizio valuta i criteri di autorizzazione specificati.

Vedere anche:

- [Protezione con PlayReady DRM](media-services-rest-deliver-streaming-content.md)
- [Protezione con AES-128](media-services-protect-with-aes128.md)

##<a id="publish"></a>Procedura: Pubblicare contenuti

> [AZURE.SELECTOR]
- [.NET](media-services-deliver-streaming-content.md)
- [REST](media-services-rest-deliver-streaming-content.md)
- [Portale](media-services-manage-content.md#publish)

###Panoramica

Per poter fornire all'utente un URL da usare per scaricare o trasmettere in streaming i contenuti distribuiti, è prima necessario "pubblicare" la risorsa creando un localizzatore. I localizzatori forniscono l'accesso ai file contenuti nell'asset. Servizi multimediali supporta due tipi di localizzatori: localizzatori OnDemandOrigin, usati per lo streaming dei file multimediali (ad esempio, MPEG DASH, HLS o Smooth Streaming) e localizzatori di firma di accesso condiviso, usati per scaricare i file multimediali.

Se per pubblicare gli asset si usa il portale di Azure classico, i localizzatori vengono creati automaticamente e viene fornito un URL basato su OnDemandOrigin (se l'asset contiene un file .ism) o un URL della firma di accesso condiviso.

Un URL di firma di accesso condiviso ha il seguente formato:

	{blob container name}/{asset name}/{file name}/{SAS signature}

Un URL di streaming presenta il seguente formato e può essere usato per riprodurre asset Smooth Streaming:

	{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Per creare un URL di streaming HLS, aggiungere (format=m3u8-aapl) all'URL.

	{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Per creare un URL di streaming MPEG DASH, aggiungere (format=mpd-time-csf) all'URL.

	{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Per i localizzatori viene definita una data di scadenza. Quando si pubblicano asset attraverso il portale, vengono creati localizzatori con scadenza a 100 anni.

>[AZURE.NOTE] I localizzatori creati attraverso il portale prima del mese di marzo 2015 hanno una data di scadenza di due anni.

Per aggiornare la data di scadenza di un localizzatore, è possibile usare [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator) o [.NET](http://go.microsoft.com/fwlink/?LinkID=533259). Si noti che quando si aggiorna la data di scadenza di un localizzatore di firma di accesso condiviso, l'URL viene modificato.

###Publish

Per pubblicare un asset tramite il portale, seguire questa procedura:

1. Selezionare l'asset.
2. Quindi, fare clic sul pulsante Pubblica.
	
 ![Contenuto pubblicato][publishedcontent]


## Procedura: Riprodurre contenuti dal portale

Il **portale di Azure classico** fornisce un lettore di contenuti che può essere usato per testare il proprio video.

Fare clic sul contenuto video desiderato e quindi sul pulsante **Riproduci** nella parte inferiore del portale.
 
Considerazioni applicabili:

- Verificare che il video sia stato pubblicato.
- Il **LETTORE DI CONTENUTI DI SERVIZI MULTIMEDIALI** esegue la riproduzione dall'endpoint di streaming predefinito. Se si vuole eseguire la riproduzione da un endpoint di streaming diverso, usare un altro lettore, ad esempio [Lettore di Servizi multimediali di Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

##Percorsi di apprendimento di Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornire commenti e suggerimenti

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!-- Images -->
[portaloverview]: ./media/media-services-manage-content/media-services-content-page.png
[publishedcontent]: ./media/media-services-manage-content/media-services-upload-content-published.png
[uploadcontent]: ./media/media-services-manage-content/UploadContent.png
[status]: ./media/media-services-manage-content/Status.png
[encoder]: ./media/media-services-manage-content/EncoderDialog2.png
[branding]: ./media/branding-reporting.png
[contentpage]: ./media/media-services-manage-content/media-services-content-page.png
[process]: ./media/media-services-manage-content/media-services-process-video.png
[process2]: ./media/media-services-manage-content/media-services-process-video2.png
[encrypt]: ./media/media-services-manage-content/media-services-encrypt-content.png
[AMSPlayer]: ./media/media-services-manage-content/media-services-portal-player.png

<!---HONumber=AcomDC_0629_2016-->