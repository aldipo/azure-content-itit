<properties 
   pageTitle="Gestione delle risorse di Azure con Cloud Explorer | Microsoft Azure"
   description="Informazioni su come usare Cloud Explorer per esplorare e gestire le risorse di Azure in Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="05/08/2016"
   ms.author="tarcher" />

# Gestione delle risorse di Azure con Cloud Explorer

##Panoramica

Cloud Explorer è stato progettato per consentire di esplorare e gestire in modo più semplice e rapido le risorse di Azure nell'IDE di Visual Studio. È ad esempio possibile usarlo per aprire un'app Web nel [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) o in un browser oppure per collegare un debugger. È anche possibile visualizzare le proprietà di un contenitore BLOB e aprirlo nell'editor dei contenitori BLOB.

Cloud Explorer è basato sullo stack di Azure Resource Manager, proprio come il [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). Riconosce risorse quali i gruppi di risorse di Azure e servizi di Azure quali le app per la logica e le app per le API. Supporta infine il [controllo degli accessi in base al ruolo](../role-based-access-control-configure/). Per visualizzare le risorse di Azure che sono state aggiunte o modificate, fare clic su **Refresh** sulla barra degli strumenti di Cloud Explorer.

Cloud Explorer viene installato come parte di Visual Studio Tools per Azure SDK 2.7.

## Prerequisiti

- Visual Studio 2015 RTM.

- Visual Studio Tools per Azure SDK.
- Per visualizzare le risorse di Azure in Cloud Explorer, è necessario avere un account di Azure e avere effettuato l'accesso. Se non si dispone di un account, è possibile crearne uno in un paio di minuti. Se si dispone di un abbonamento MSDN, vedere [Vantaggi di Azure per gli abbonati MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). In caso contrario, vedere [creare un account di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/).

- Se Cloud Explorer non è visibile, è possibile visualizzarlo scegliendo **Visualizzazione**, **Altre finestre,** **Cloud Explorer** sulla barra dei menu.

## Gestire gli account e le sottoscrizioni di Azure

Per visualizzare le risorse di Azure in Cloud Explorer, è necessario accedere a un account Azure con una o più sottoscrizioni attive. Se sono disponibili più account Azure, sarà possibile aggiungerli in Cloud Explorer, quindi scegliere le sottoscrizioni da includere nella visualizzazione delle risorse in Cloud Explorer.

Se si usa Azure per la prima volta o non sono stati aggiunti gli account necessari a Visual Studio, sarà necessario aggiungerli.

## Per aggiungere account Azure a Cloud Explorer

1. Fare clic sull'icona Settings sulla barra degli strumenti di Cloud Explorer.

1. Selezionare il collegamento **Add an account**. Accedere all'account Azure di cui si vogliono esplorare le risorse. L'account appena aggiunto dovrebbe essere selezionato nell'elenco a discesa di selezione dell'account. Le sottoscrizioni associate all'account vengono visualizzate sotto la voce relativa all'account.

    ![Aggiunta di sottoscrizioni di Azure](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Selezione di sottoscrizioni di Azure](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Selezionare le caselle di controllo relative alle sottoscrizioni di account da esplorare, quindi fare clic su **Apply**.

    Le risorse di Azure per le sottoscrizioni selezionate verranno visualizzate in Cloud Explorer.

## Per rimuovere un account Azure

1. Scegliere **File**, **Impostazioni dell'Account** sulla barra dei menu.

1. Nella sezione **Tutti gli account** della finestra di dialogo **Impostazioni dell'Account**, scegliere il comando **Rimuovere** accanto all'account che si desidera rimuovere. Si noti che questo comando rimuove l'account solo da Visual Studio. Non influisce sull'account di Azure stesso.

## Visualizzare i tipi o i gruppi di risorse

Per visualizzare le risorse di Azure, è possibile scegliere la visualizzazione **Tipi di risorsa** o **Gruppi di risorse**.

![Elenco a discesa della visualizzazione delle risorse](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- La visualizzazione **Tipi di risorsa**, che è anche la visualizzazione comune usata nel [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), mostra le risorse di Azure classificate in base al tipo, ad esempio app Web, account di archiviazione e macchine virtuali. È simile alla visualizzazione delle risorse di Azure in Esplora server.

- La visualizzazione Gruppi di risorse classifica le risorse di Azure in base al gruppo di risorse di Azure a cui sono associate.

 
	Un gruppo di risorse è un bundle di risorse di Azure, in genere usate da un'applicazione specifica. Per altre informazioni sui gruppi di risorse di Azure, vedere [Panoramica di Gestione risorse di Microsoft Azure](./resource-group-overview.md).

## Visualizzare ed esplorare le risorse

Per passare a una risorsa di Azure e visualizzare le informazioni corrispondenti in Cloud Explorer, espandere il tipo o il gruppo di risorse associato dell'elemento, quindi selezionare la risorsa. Quando si seleziona una risorsa, le informazioni corrispondenti vengono visualizzate nelle due schede nella parte inferiore di Cloud Explorer.

![Selezione di una visualizzazione delle risorse](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- La scheda **Azioni** mostra le azioni da intraprendere in Cloud per la risorsa selezionata. È anche possibile visualizzare le azioni disponibili nel menu di scelta rapida della risorsa.

- La scheda **Properties** mostra le proprietà della risorsa, ad esempio il tipo, le impostazioni locali e il gruppo di risorse a cui è associata.

L'azione **Open in portal** è disponibile per ogni risorsa. Quando si sceglie questa azione, Cloud Explorer mostra la risorsa selezionata nel [portale di Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). Questa funzionalità è particolarmente utile per l'esplorazione di risorse con molti livelli di nidificazione.

È possibile che vengano visualizzate azioni aggiuntive e altri valori di proprietà, in base alla risorsa di Azure. Ad esempio, per le app Web e le app per la logica sono disponibili anche le azioni **Open in browser** e **Attach debugger**, oltre a **Open in portal**. Quando si sceglie un BLOB, una coda o una tabella dell'account di archiviazione, vengono visualizzate le azioni per l'apertura di editor. Per le app Azure sono disponibili le proprietà relative a **URL** e **stato**, mentre per le risorse di archiviazione sono disponibili le proprietà relative alle chiavi e alle stringhe di connessione.

## Cercare risorse

Per trovare risorse con un nome specifico nelle sottoscrizioni dell'account Azure, immettere il nome nella casella Search in Cloud Explorer.

![Ricerca di risorse in Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Quando si immettono caratteri nella casella Search, nell'albero delle risorse vengono visualizzate solo le risorse corrispondenti ai caratteri immessi.

<!---HONumber=AcomDC_0511_2016-->