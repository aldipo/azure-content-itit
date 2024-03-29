<properties
    pageTitle="Uso di Outlook in Azure RemoteApp | Microsoft Azure" 
    description="Informazioni su come configurare e usare Outlook in Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/18/2016"
    ms.author="elizapo" />

# Uso di Microsoft Outlook in Azure RemoteApp

Azure RemoteApp supporta Microsoft Outlook O365. Altre informazioni sul [funzionamento di Office in Azure RemoteApp](remoteapp-officesubscription.md). Per l'uso di Outlook in Azure RemoteApp è opportuno adottare alcune impostazioni consigliate.

## Modalità cache
La modalità cache è una configurazione consigliata per l'uso di Outlook in Azure RemoteApp. Quando si configura un account di Outlook 2013 per l'uso della modalità cache, Outlook 2013 funziona da una copia locale della cassetta postale di Microsoft Exchange dell'utente archiviata in un file di dati offline (file OST) nel computer dell'utente, insieme alla Rubrica offline. La cassetta postale nella cache e la Rubrica offline vengono aggiornate periodicamente dal servizio di O365. Altre informazioni sulle [differenze tra la modalità cache e la modalità online](https://technet.microsoft.com/library/jj683103.aspx).

L'utente può selezionare la **modalità cache** o la **modalità online** durante la configurazione dell'account o modificando le impostazioni dell'account. È anche possibile distribuire una modalità o l'altra usando lo Strumento di personalizzazione di Office o i Criteri di gruppo.

Vedere le [istruzioni dettagliate su come abilitare la modalità cache](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## Ricerca
In Azure RemoteApp, l'uso della ricerca all'interno di Outlook presenta alcune limitazioni. Azure RemoteApp usa macchine virtuali in pool per supportare le sessioni utente. L'indicizzazione della ricerca dipende dall'ID computer, che varia a seconda della macchina virtuale. È possibile che ogni volta che un utente accede ad Azure RemoteApp venga indirizzato a una nuova macchina virtuale. Ciò significa che, se si abilita la ricerca locale, l'indicizzatore viene eseguito ogni volta che l'ID computer cambia (quando l'utente è in una macchina virtuale diversa). A seconda delle dimensioni del file OST, l'esecuzione dell'indicizzatore può richiedere molto tempo e consumare risorse necessarie per altre applicazioni. La ricerca, quindi, può risultare lenta o non dare alcun risultato. Una soluzione al problema consiste nell'abilitare la ricerca online per impostazione predefinita. Purtroppo, in Outlook 2013 non è possibile disabilitare la ricerca locale/indicizzata e abilitare la ricerca online per impostazione predefinita.

Outlook 2016 offre una soluzione al problema con una nuova esperienza di ricerca online nelle cassette postali ospitate su Exchange 2016 (o in Office 365). I risultati della ricerca server vengono analizzati rispetto alla cache locale (OST). In alcuni scenari Outlook può tornare a usare l'indicizzatore della ricerca, ma la maggior parte delle ricerche viene eseguita in modalità online. Se la ricerca nella posta elettronica è molto importante, è consigliabile usare Azure RemoteApp con Outlook 2016.

<!---HONumber=AcomDC_0525_2016-->