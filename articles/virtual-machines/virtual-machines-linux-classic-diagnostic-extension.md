
<properties
		pageTitle="Monitoraggio di una macchina virtuale Linux con un'estensione VM | Microsoft Azure"
		description="Informazioni su come usare l'estensione di diagnostica Linux per monitorare le prestazioni e i dati di diagnostica di una VM Linux in Azure."
		services="virtual-machines-linux"
		documentationCenter=""
  		authors="NingKuang"
		manager="timlt"
		editor=""
  		tags="azure-service-management"/>

<tags
		ms.service="virtual-machines-linux"
		ms.workload="infrastructure-services"
		ms.tgt_pltfrm="vm-linux"
		ms.devlang="na"
		ms.topic="article"
		ms.date="12/15/2015"
		ms.author="Ning"/>


# Uso dell'estensione di diagnostica Linux per monitorare le prestazioni e i dati di diagnostica di una macchina virtuale Linux

## Introduzione

L'estensione di diagnostica Linux consente all'utente di monitorare le VM Linux eseguite in Microsoft Azure. Questo servizio offre le funzionalità seguenti:

- Raccoglie e carica informazioni sulle prestazioni del sistema dalla VM Linux alla tabella di archiviazione dell'utente, incluse informazioni di diagnostica e di syslog.
- Consente agli utenti di personalizzare la metrica dei dati che verrà raccolta e caricata.
- Consente agli utenti di caricare in una tabella di archiviazione designata i file di log specificati.

Nella versione 2.0 i dati includono:

- Tutti i log Rsyslog Linux, inclusi i log di sistema, sicurezza e applicazioni.
- Tutti i dati di sistema specificati nel [sito System Center Cross Platform Solutions](https://scx.codeplex.com/wikipage?title=xplatproviders).
- I file di log specificati dall'utente.

Questa estensione funziona sia con i modelli classici che con i modelli di distribuzione Resource Manager.


## Abilitare l'estensione
È possibile abilitare questa estensione usando il [portale di Azure](https://portal.azure.com/#), Azure PowerShell o gli script dell'interfaccia della riga di comando di Azure.

Per visualizzare e configurare i dati di sistema e prestazioni direttamente dal portale di Azure, seguire [questi passaggi nel blog di Azure](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/ "URL del blog di Windows"/).


Questo articolo illustra come abilitare e configurare l'estensione usando i comandi dell'interfaccia della riga di comando di Azure. In questo modo è possibile leggere e visualizzare i dati direttamente dalla tabella di archiviazione.

Si noti che i metodi di configurazione descritti qui non funzioneranno per il portale di Azure. Per visualizzare e configurare i dati di sistema e prestazioni direttamente dal portale di Azure, l'estensione deve essere abilitata con il portale.


## Prerequisiti
- **Agente Linux di Azure 2.0.6 o versioni successive**. Si noti che la maggior parte delle immagini della raccolta Linux di macchine virtuali di Azure include la versione 2.0.6 o successive. È possibile eseguire **WAAgent -version** per verificare la versione installata nella VM. Se la VM esegue una versione precedente alla 2.0.6, è possibile seguire queste [istruzioni in GitHub](https://github.com/Azure/WALinuxAgent "istruzioni") per aggiornarla.

- L'**interfaccia della riga di comando di Azure**. Seguire [queste linee guida per l'installazione dell'interfaccia della riga di comando](../xplat-cli-install.md) per configurare l'ambiente dell'interfaccia della riga di comando di Azure nella macchina virtuale. Dopo l'installazione dell'interfaccia della riga di comando di Azure, sarà possibile usare il comando **azure** dall'interfaccia della riga di comando (Bash, terminale o prompt dei comandi) per accedere ai relativi comandi, ad esempio:
	- Eseguire **azure vm extension set --help** per informazioni della Guida dettagliate.
	- Eseguire **azure login** per accedere ad Azure.
	- Eseguire **azure vm list** per elencare tutte le macchine virtuali che si hanno in Azure.
- Un account di archiviazione per archiviare i dati. Saranno necessari un nome di account di archiviazione creato in precedenza e una chiave di accesso per caricare i dati nella risorsa di archiviazione.


## Usare l'interfaccia della riga di comando di Azure per abilitare l'estensione della diagnostica Linux

### Scenario 1. Abilitare l'estensione con il set di dati predefinito
Nella versione 2.0 e successive i dati predefiniti che verranno raccolti includono:

- Tutte le informazioni Rsyslog (inclusi i log di sistema, sicurezza e applicazioni).  
- Un set principale di dati di sistema di base. Si noti che il set di dati completo è descritto nel [sito System Center Cross Platform Solutions](https://scx.codeplex.com/wikipage?title=xplatproviders). Per abilitare dati aggiuntivi, proseguire con i passaggi degli scenari 2 e 3.

Passaggio 1. Creare un file denominato PrivateConfig.json con il contenuto seguente:

    {
        "storageAccountName" : "the storage account to receive data",
        "storageAccountKey" : "the key of the account"
    }

Passaggio 2. Eseguire **azure vm extension set vm\_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --private-config-path PrivateConfig.json**.


###   Scenario 2. Personalizzare le metriche di monitoraggio delle prestazioni  
In questa sezione viene descritto come personalizzare la tabella dati delle prestazioni e della diagnostica.

Passaggio 1. Creare un file denominato PrivateConfig.json con il contenuto descritto nello scenario 1. Creare anche un file denominato PublicConfig.json. Specificare i dati specifici che si desidera raccogliere.

Per tutti i provider e le variabili supportati, vedere il [sito System Center Cross Platform Solutions](https://scx.codeplex.com/wikipage?title=xplatproviders). È possibile disporre di più query e archiviarle in più tabelle aggiungendo altre query nello script.

Per impostazione predefinita i dati Rsyslog verranno sempre raccolti.

    {
      	"perfCfg":
      	[
      	    {
      	        "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
      	        "table" : "LinuxMemory"
      	    }
      	]
    }


Passaggio 2. Eseguire **azure vm extension set vm\_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.


###   Scenario 3. Caricamento dei propri file di log
Questa sezione descrive come raccogliere e caricare file di log specifici nell'account di archiviazione. È necessario specificare sia il percorso del file di log che il nome della tabella in cui si vuole archiviare il log. È possibile creare più file di log aggiungendo più voci di file/tabella nello script.

Passaggio 1. Creare un file denominato PrivateConfig.json con il contenuto descritto nello scenario 1. Creare quindi un altro file denominato PublicConfig.json con il contenuto seguente:

    {
        "fileCfg" :
        [
            {
                "file" : "/var/log/mysql.err",
                "table" : "mysqlerr"
             }
        ]
    }


Passaggio 2. Eseguire **azure vm extension set vm\_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.


###   Scenario 4. Arrestare la raccolta di log dell'estensione
Questa sezione descrive come arrestare la raccolta di log da parte dell'estensione. Si noti che il processo dell'agente di monitoraggio sarà operativo anche con questa riconfigurazione. Per arrestare completamente il processo dell'agente di monitoraggio, è possibile disabilitare l'estensione. Il comando per disabilitare l'estensione è **azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'**.

Passaggio 1. Creare un file denominato PrivateConfig.json con il contenuto descritto nello scenario 1. Creare un altro file denominato PublicConfig.json con il contenuto seguente:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


Passaggio 2. Eseguire **azure vm extension set vm\_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json**.


## Esaminare i dati
I dati delle prestazioni e della diagnostica vengono archiviati in una tabella di archiviazione di Azure. Vedere [Come usare l'archivio tabelle di Azure da Ruby](../storage/storage-ruby-how-to-use-table-storage.md) per informazioni su come accedere ai dati nella tabella di archiviazione usando gli script dell'interfaccia della riga di comando di Azure.

È anche possibile usare gli strumenti dell'interfaccia utente seguenti per accedere ai dati:

1. Esplora server di Visual Studio. Passare all'account di archiviazione. Dopo circa cinque minuti dall'esecuzione della VM, verranno visualizzate le quattro tabelle predefinite: "LinuxCpu", "LinuxDisk", "LinuxMemory" e "Linuxsyslog". Fare doppio clic sui nomi delle tabelle per visualizzare i dati.

2. [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Storage Explorer").

![immagine](./media/virtual-machines-linux-classic-diagnostic-extension/no1.png)

Se è stato abilitato fileCfg o perfCfg (come illustrato negli scenari 2 e 3), è possibile usare Esplora server di Visual Studio e Azure Storage Explorer per visualizzare i dati non predefiniti.

## Problemi noti
- Nella versione 2.0 dell'estensione di diagnostica Linux le informazioni di Rsyslog e il file di log specificato dal cliente sono accessibili solo tramite scripting.

<!---HONumber=AcomDC_0615_2016-->