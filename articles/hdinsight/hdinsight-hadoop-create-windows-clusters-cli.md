<properties
   pageTitle="Creare cluster Hadoop basati su Windows in HDInsight tramite l'interfaccia della riga di comando di Azure"
   	description="Informazioni su come creare cluster per Azure HDInsight usando l'interfaccia della riga di comando di Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/27/2016"
   ms.author="jgao"/>

# Creare cluster Hadoop basati su Windows in HDInsight tramite l'interfaccia della riga di comando di Azure

[AZURE.INCLUDE [selettore](../../includes/hdinsight-selector-create-clusters.md)]

Informazioni su come creare cluster HDInsight tramite l'interfaccia della riga di comando di Azure. Per altri strumenti e funzionalità per la creazione di cluster, fare clic sulla scheda Seleziona nella parte superiore di questa pagina o vedere [Metodi di creazione di cluster](hdinsight-provision-clusters.md#cluster-creation-methods).

##Prerequisiti:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Per poter eseguire le istruzioni descritte nell'articolo è necessario disporre di:

- **Sottoscrizione di Azure**. Vedere [Ottenere una versione di prova gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- L'**interfaccia della riga di comando di Azure**.

	[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##Connettersi ad Azure

Usare il comando seguente per collegarsi ad Azure:

	azure login

Per altre informazioni sull'autenticazione con un account aziendale o dell'istituto di istruzione, vedere l'argomento relativo alla [connessione a una sottoscrizione di Azure dall'interfaccia della riga di comando di Azure](../xplat-cli-connect.md).

Usare il comando seguente per passare alla modalità di Gestione risorse di Azure:

	azure config mode arm

Per ottenere assistenza, utilizzare l’opzione **-h**. Ad esempio:

	azure hdinsight cluster create -h

##Creare i cluster

È necessario disporre di Gestione risorse di Azure e di un account di archiviazione BLOB di Azure prima di poter creare un cluster HDInsight. Per creare un cluster HDInsight, è necessario specificare quanto segue:

- **Gruppo di risorse di Azure**: è necessario creare un account di Data Lake Analytics all'interno di un gruppo di risorse di Azure. Gestione risorse di Azure consente di lavorare con le risorse dell'applicazione come gruppo. È quindi possibile distribuire, aggiornare o eliminare tutte le risorse per l'applicazione mediante un'unica operazione coordinata.

	Per elencare i gruppi di risorse nella sottoscrizione:

		azure group list

	Per creare un nuovo gruppo di risorse:

		azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Nome del cluster HDInsight**

- **Posizione**: uno dei data center di Azure che supporta cluster HDInsight. Per un elenco di percorsi supportati, vedere [Prezzi di HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Account di archiviazione predefinito**: HDInsight usa un contenitore di archiviazione BLOB di Azure come file system predefinito. Per creare un cluster HDInsight, è necessario un account di archiviazione di Azure.

	Per creare un nuovo account di archiviazione di Azure:

		azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

	> [AZURE.NOTE] L'account di archiviazione deve trovarsi in HDInsight nel data center. Il tipo di account di archiviazione non può essere ZRS, perché ZRS non supporta la tabella.

	Per informazioni sulla creazione di un account di Archiviazione di Azure tramite il portale di Azure, vedere [Creare, gestire o eliminare un account di archiviazione][azure-create-storageaccount].

	Se si dispone già di un account di archiviazione, ma non se ne conosce né il nome né la chiave, è possibile usare i comandi seguenti per recuperare tali informazioni:

		-- Lists Storage accounts
		azure storage account list
		-- Shows a Storage account
		azure storage account show "<Storage Account Name>"
		-- Lists the keys for a Storage account
		azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

	Per dettagli sull'acquisizione delle informazioni dal portale di Azure, vedere la sezione relativa alla gestione dell'account di archiviazione di [Informazioni sugli account di archiviazione Azure](../storage/storage-create-storage-account#manage-your-storage-account).

- **(Facoltativo) Contenitore di BLOB predefinito**: il comando **azure hdinsight cluster create** crea il contenitore se non esiste. Se si sceglie di creare il contenitore prima, è possibile usare il comando seguente:

	azure storage container create --account-name "<Storage Account Name>" --account-key <Storage Account Key> [ContainerName]

Dopo aver preparato l'account di archiviazione, è possibile creare un cluster.


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##Creare cluster mediante i file di configurazione
In genere, si crea un cluster HDInsight, vi si eseguono i processi e quindi si elimina il cluster per ridurre il costo. L'interfaccia della riga di comando consente di salvare le configurazioni in un file, per poterle riutilizzare ogni volta che si crea un cluster.

	azure hdinsight config create [options ] <Config File Path> <overwirte>
	azure hdinsight config add-config-values [options] <Config File Path>
	azure hdinsight config add-script-action [options] <Config File Path>

Esempio: creare un file di configurazione che contiene un'azione script per l'esecuzione durante la creazione di un cluster.

	azure hdinsight config create "C:\myFiles\configFile.config"
	azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
	azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##Creare cluster con un'azione script

Creare un file di configurazione che contiene un'azione script da eseguire durante la creazione di un cluster.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Creare un cluster con un'azione script

	azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Per informazioni generali sull'azione di script, vedere [Personalizzare cluster HDInsight mediante l'azione script (Linux)](hdinsight-hadoop-customize-cluster.md).


## Creare cluster tramite modelli di Gestione risorse di Azure

È possibile usare l'interfaccia della riga di comando per creare cluster chiamando modelli di Gestione risorse di Azure. Vedere [Distribuire con l'interfaccia della riga di comando di Azure](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## Vedere anche

- [Introduzione all'uso di Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - Informazioni su come iniziare a usare il cluster HDInsight
- [Inviare processi Hadoop a livello di codice](hdinsight-submit-hadoop-jobs-programmatically.md) - Informazioni su come inviare processi a HDInsight a livello di codice
- [Gestire cluster Hadoop in HDInsight tramite la CLI di Azure](hdinsight-administer-use-command-line.md)
- [Uso dell’interfaccia della riga di comando di Azure per Mac, Linux e Windows con Gestione servizi di Azure](../virtual-machines-command-line-tools.md)

<!---HONumber=AcomDC_0601_2016-->