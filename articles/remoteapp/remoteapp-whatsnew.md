
<properties
    pageTitle="Novità di Azure RemoteApp | Microsoft Azure"
    description="Informazioni sulle modifiche e i miglioramenti apportati a RemoteApp di Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="elizapo" />



# Novità di RemoteApp di Azure

Uno dei vantaggi di Azure RemoteApp è che viene costantemente migliorato. Ogni modifica apportata viene comunicata qui.

## Aggiornamenti futuri
Il team di RemoteApp di Azure pubblica mensilmente aggiornamenti nel blog di Servizi Desktop remoto. In tale area è possibile trovare non solo le modifiche apportate a RemoteApp di Azure, ma anche altre informazioni sull'uso di Servizi Desktop remoto. Per informazioni, consultare i [blog di Servizi Desktop remoto](https://blogs.msdn.microsoft.com/rds/). Ad esempio, alcune settimane fa, è stata pubblicata una voce su [sollevamento e spostamento dei carichi di lavoro con RemoteApp di Azure e Azure AD](https://blogs.msdn.microsoft.com/rds/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services/).
 
## Settembre 2015
- Aggiunta di Infopath all'immagine delle raccolte e dei modelli di Microsoft Office 365. Per condividere Infopath, assicurarsi di aggiornare le raccolte con l'immagine più recente.
- Aggiornamenti client:
	- Aggiornamento dei client Windows per consentire agli utenti di condividere commenti e suggerimenti, soprattutto sui problemi di connessione.
	- Aggiornamento dei client iOS per correggere un errore di messaggistica e per risolvere un problema in cui le credenziali scadevano prima del previsto.
- Microsoft sta lavorando al test del supporto per Office 2016. Una volta completato, cercare le immagini aggiornate.
- Pubblicazione di un nuovo articolo sulle [differenze tra le raccolte cloud e ibride](remoteapp-collections.md), che aiuta a scegliere il tipo di raccolta più adatto alle proprie app: solo cloud, cloud + rete virtuale o ibrido.
- Se si vuole condividere QuickBooks con Azure RemoteApp, ma non si è certi della procedura da seguire, vedere il [nuovo articolo di Eric Orman](remoteapp-quickbooks.md) che illustra tutti i passaggi.

## Agosto 2015
In agosto sono state apportate notevoli modifiche, tra cui:

- Possibilità di usare una rete virtuale di Azure con una raccolta nel cloud. Per la nuova procedura, vedere le [istruzioni per la creazione cloud](remoteapp-create-cloud-deployment.md).
- Possibilità di aggiungere app al menu **Start ** per il nuovo client Windows RemoteApp. Le app verranno visualizzate nell'elenco di applicazioni ed è possibile aggiungerle al menu **Start ** di Windows.
- Aggiunta di una nuova immagine alla raccolta di VM di Azure: Host sessione Desktop remoto di Windows Server con Microsoft Office 365 ProPlus.
- Correzione del client Mac per evitare il blocco delle app con finestre modali.
- Documentazione sull'uso della [sottoscrizione di Office 365 ProPlus](remoteapp-officesubscription.md) con Azure RemoteApp.
- Informazioni dettagliate su come [proteggere le app e i dati](remoteapp-secure.md) nella raccolta di Azure RemoteApp.

## 2015 luglio 2014

A luglio vengono portati in scena le novità in arrivo nel mese di agosto, pertanto non c’è molto di cui parlare di ora, principalmente aggiornamenti dei documenti. Ecco le modifiche più recenti:

- Aggiunta della scheda **Supporto** al portale, in modo da poter accedere più facilmente alle risorse di supporto, ad esempio i forum.
- Rielaborazione delle informazioni sulla risoluzione dei problemi per la creazione di una raccolta ibrida. Vedere [le ultimissime novità](remoteapp-hybridtrouble.md) per suggerimenti sulla risoluzione dei problemi, ad esempio come identificare le porte appropriate per configurare la rete virtuale.
- Documentato come i [dati utente](remoteapp-upd.md) vengono creati e salvati in Azure RemoteApp.
- Documentato come [bloccare le applicazioni](remoteapp-secure.md).
- Pubblicati i [cmdlet di Azure RemoteApp](https://msdn.microsoft.com/library/mt428031.aspx).
- Infine, abbiamo iniziato una conversazione con alcuni utenti di Azure RemoteApp sulla terminologia. Cercare le modifiche al modo in cui si fa riferimento alle diverse opzioni della raccolta.

## Giugno 2015

Sono state introdotte numerose modifiche. Il team è stato molto occupato nel mese di giugno:

- La [pagina di destinazione](https://www.remoteapp.windowsazure.com/) di RemoteApp di Azure è stata riprogettata.
- Il software di tutte le immagini disponibili come parte della sottoscrizione è stato aggiornato.
- Sono stati apportati miglioramenti alle raccolte ibride, tra cui il supporto del tunneling forzato e il controllo della dimensione della subnet IP prima di tentare di creare la raccolta.
- Rilevato che il carattere jolly * non funziona per le webcam. In alternativa, è necessario specificare l'ID di istanza o GUID. Microsoft aggiornerà le informazioni sul reindirizzamento di conseguenza
- per consentire l’aggiunta di software antivirus all’immagine quando si crea un'immagine modello dalla raccolta di Azure.

Altre modifiche saranno distribuite nel mese di luglio, per cui sarà presto disponibile un nuovo aggiornamento.

## Maggio 2015

Sono state introdotte diverse aggiunte e sono trascorsi diversi mesi dopo la creazione iniziale di questo argomento, pertanto questo elenco non è esaustivo. Provare queste nuove funzionalità:

- Automatizzare tutto - RemoteApp di Azure ora ha [cmdlet nel modulo Azure PowerShell](remoteapp-tutorial-arawithpowershell.md).
- [Creare un'immagine di RemoteApp di Azure da una macchina virtuale di Azure](remoteapp-image-on-azurevm.md). Consente di caricare l'immagine personalizzata in Azure molto più rapidamente.
- Utilizzare una rete virtuale di Azure invece di una rete virtuale RemoteApp per connettersi alle risorse di rete aziendale in Azure. Sono state aggiornate le [istruzioni sulle raccolte ibride](remoteapp-create-hybrid-deployment.md) per illustrare nei dettagli la creazione di una rete virtuale di Azure (Passaggio 1).
- Parlando di reti virtuali, consultare le [nuove informazioni](remoteapp-vnetsizing.md) su limiti e limitazioni delle dimensioni delle reti virtuali.
- Parlando di limiti, quali sono i [limiti dei servizi e le impostazioni predefinite](../azure-subscription-service-limits.md)?

Per altre informazioni su Azure RemoteApp Il team di RemoteApp era al lavoro presso Ignite alcune settimane fa. Guardare il video di Eric, [The Fundamentals of Microsoft Azure RemoteApp Management and Administration](http://channel9.msdn.com/Events/Ignite/2015/BRK3868).

Si desidera vedere RemoteApp di Azure nel mondo reale? Consultare l’esercitazione [Eseguire qualsiasi applicazione su qualsiasi dispositivo in qualsiasi luogo](remoteapp-anyapp.md), che illustra come condividere l'accesso con gli utenti, inclusa la condivisione di file di database. È disponibile anche un’esercitazione su come fare in modo che [Office 365](remoteapp-tutorial-o365anywhere.md) funzioni allo stesso modo su ogni dispositivo.

Grazie per l’attenzione. Ulteriori aggiornamenti saranno comunicati il mese prossimo.


### Come contribuire al miglioramento
Non tutti sanno che oltre alla classificazione di questo articolo e all'aggiunta di commenti di seguito, è possibile apportare modifiche all'articolo stesso. Mancano informazioni? Alcune informazioni non sono corrette? Qualcosa non è abbastanza chiaro? Scorrere verso l'alto e fare clic su **Modifica in GitHub** per apportare modifiche. Dopo che saranno state esaminate e approvate, le modifiche e i miglioramenti suggeriti dagli utenti saranno applicati all'articolo.

<!---HONumber=AcomDC_0629_2016-->