<properties
	pageTitle="Gestire i cluster Hadoop tramite l’interfaccia della riga di comando di Azure | Microsoft Azure"
	description="Come utilizzare l’interfaccia della riga di comando di Azure per gestire i cluster Hadoop in HDInsight"
	services="hdinsight"
	editor="cgronlun"
	manager="paulettm"
	authors="mumian"
	tags="azure-portal"
	documentationCenter=""/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/24/2016"
	ms.author="jgao"/>

# Gestire cluster Hadoop in HDInsight tramite la CLI di Azure

[AZURE.INCLUDE [selettore](../../includes/hdinsight-portal-management-selector.md)]

Informazioni su come usare l'[Interfaccia della riga di comando di Azure](../xplat-cli-install.md) per gestire i cluster Hadoop in Azure HDInsight. L'interfaccia della riga di comando di Azure viene implementata in Node.js. Può essere usato in tutte le piattaforme che supportano Node.js, incluse Windows, Mac e Linux.

In questo articolo viene illustrato solo l’uso dell’interfaccia della riga di comando di Azure con HDInsight. Per una guida generale sull'uso dell’interfaccia della riga di comando di Azure vedere [Installare e configurare l’interfaccia della riga di comando di Azure][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##Prerequisiti

Per eseguire le procedure descritte nell'articolo è necessario:

- **Una sottoscrizione di Azure**. Vedere [Ottenere una versione di prova gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Interfaccia della riga di comando di Azure**: per informazioni sull’installazione e la configurazione vedere [Installare e configurare l’interfaccia della riga di comando di Azure](../xplat-cli-install.md).
- **Collegarsi ad Azure**, usando il comando seguente:

		azure login

	Per altre informazioni sull'autenticazione con un account aziendale o dell'istituto di istruzione, vedere l'argomento relativo alla [connessione a una sottoscrizione di Azure dall'interfaccia della riga di comando di Azure](xplat-cli-connect.md).
	
- **Passare alla modalità Gestione risorse di Azure**, usando il comando seguente:

		azure config mode arm

Per ottenere assistenza, utilizzare l’opzione **-h**. Ad esempio:

	azure hdinsight cluster create -h
	
##Creare i cluster

Vedere [Creare cluster basati su Linux in HDInsight tramite l'interfaccia della riga di comando di Azure](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##Elencare i cluster e visualizzarne i dettagli
Utilizzare i comandi seguenti per elencare e mostrare i dettagli dei cluster:

	azure hdinsight cluster list
	azure hdinsight cluster show <Cluster Name>

![HDI.CLIListCluster][image-cli-clusterlisting]


##Eliminare cluster

Utilizzare il comando seguente per eliminare un cluster:

	azure hdinsight cluster delete <Cluster Name>

È inoltre possibile eliminare un cluster eliminando il gruppo di risorse che lo contiene. Tenere presente che questa operazione eliminerà tutte le risorse nel gruppo, compreso l’account di archiviazione predefinito.

	azure group delete <Resource Group Name>

##Ridimensionare i cluster

Per modificare le dimensioni del cluster Hadoop:

	azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## Abilitare/disabilitare l'accesso HTTP per un cluster

	azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
	azure hdinsight cluster disable-http-access [options] <Cluster Name>

## Abilitare/disabilitare l'accesso RDP per un cluster

  	azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
  	azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##Passaggi successivi
In questo articolo si è appreso come eseguire diverse attività amministrative relative ai cluster HDInsight. Per altre informazioni, vedere gli articoli seguenti:

* [Amministrare cluster Hadoop in HDInsight tramite il portale di Azure][hdinsight-admin-portal]
* [Gestire cluster Hadoop in HDInsight tramite Azure PowerShell][hdinsight-admin-powershell]
* [Introduzione all'uso di Azure HDInsight][hdinsight-get-started]
* [Come utilizzare l’interfaccia della riga di comando di Azure][azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Elenco e visualizzazione di cluster"

<!---HONumber=AcomDC_0608_2016-->