<properties
	pageTitle="Creare una macchina virtuale SQL Server in Azure PowerShell (distribuzione classica) | Microsoft Azure"
	description="Fornisce procedure e script di PowerShell per la creazione di una macchina virtuale di Azure con le immagini della galleria di macchine virtuali SQL Server. In questo argomento viene usata la modalità di distribuzione classica."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management" />
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="04/20/2016"
	ms.author="jroth" />

# Effettuare il provisioning di una macchina virtuale di SQL Server con Azure PowerShell (classico)

## Panoramica

In questo articolo viene descritta la procedura per creare una macchina virtuale SQL Server in Azure usando i cmdlet di PowerShell.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
Se è necessaria la versione relativa a Resource Manager di questo argomento, vedere [Effettuare il provisioning di una macchina virtuale di SQL Server con Azure PowerShell (Resource Manager)](virtual-machines-windows-ps-sql-create.md).

## Installare e configurare PowerShell

1. Se non si dispone di un account Azure, provare la [versione di valutazione gratuita di Azure](https://azure.microsoft.com/pricing/free-trial/).

2. [Installare i cmdlet di Azure PowerShell più recenti](../powershell-install-configure.md).

3. Dopo l'installazione, avviare Windows PowerShell.

4. Connettere PowerShell alla sottoscrizione di Azure con il comando Add-AzureAccount.

		Add-AzureAccount

## Determinare l'area di destinazione di Azure

La macchina virtuale di SQL Server verrà ospitata in un servizio cloud che si trova un'area di Azure specifica. I passaggi seguenti consentono di determinare l'area, l'account di archiviazione e il servizio cloud che verranno usati nel resto dell'esercitazione.

1. Determinare il data center che si desidera usare per ospitare la macchina virtuale di SQL Server. I seguenti comandi di PowerShell visualizzano in dettaglio le aree disponibili con un elenco di riepilogo finale.

		Get-AzureLocation
		(Get-AzureLocation).Name

2.  Dopo che è stata identificata la località preferita, impostare una variabile denominata **$dcLocation** su quell'area.

		$dcLocation = "<region name>"

## Impostare l'account di archiviazione e la sottoscrizione

1. Determinare la sottoscrizione di Azure da usare per la nuova macchina virtuale.

		(Get-AzureSubscription).SubscriptionName

1. Assegnare la sottoscrizione di Azure di destinazione alla variabile **$subscr**. Quindi, impostarla come la sottoscrizione di Azure corrente.

		$subscr="<subscription name>"
		Select-AzureSubscription -SubscriptionName $subscr –Current

1. Verificare la presenza di account di archiviazione esistenti. Lo script seguente consente di visualizzare tutti gli account di archiviazione presenti nell'area selezionata:

		(Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

	>[AZURE.NOTE] Se è necessario un nuovo account di archiviazione, creare prima un nome account di archiviazione tutto in minuscolo con il comando New-AzureStorageAccount come nell'esempio seguente: **New-AzureStorageAccount -StorageAccountName "<nome dell'account di archiviazione>" -Location $dcLocation**.

1. Assegnare il nome dell'account di archiviazione di destinazione a **$staccount**. Usare quindi **Set-AzureSubscription** per impostare la sottoscrizione e l'account di archiviazione corrente.

		$staccount="<storage account name>"
		Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## Selezionare un'immagine di macchina virtuale di SQL Server

1. Trovare l'elenco delle immagini delle macchine virtuali di SQL Server disponibili nella raccolta. Queste immagini hanno la proprietà **ImageFamily** che inizia con "SQL". La query seguente consente di visualizzare la famiglia di immagini disponibile che include SQL Server preinstallato.

		Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Nella famiglia di immagini di macchina virtuale trovata potrebbero esserci più immagini pubblicate. Usare lo script seguente per trovare il nome dell'immagine di macchina virtuale pubblicata più recente per la famiglia di immagini selezionata (ad esempio **SQL Server 2014 SP1 Enterprise in Windows Server 2012 R2**):

		$family="<ImageFamily value>"
		$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

		echo "Selected SQL Server image name:"
		echo "   $image"

## Creare la macchina virtuale

Infine, creare la macchina virtuale con PowerShell.

1. Creare un servizio cloud per ospitare la nuova macchina virtuale. Si noti che è anche possibile usare un servizio cloud esistente. Creare una nuova variabile **$svcname** con il nome breve del servizio cloud.

		$svcname = "<cloud service name>"
		New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Specificare il nome della macchina virtuale e le dimensioni. Per altre informazioni sulle dimensioni della macchina virtuale, vedere [Dimensioni delle macchine virtuali per Azure](virtual-machines-linux-sizes.md).

		$vmname="<machine name>"
		$vmsize="<Specify a valid machine size>" # see the link to virtual machine sizes
		$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Specificare l'account amministratore locale e la password.

		$cred=Get-Credential -Message "Type the name and password of the local administrator account."
		$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Eseguire lo script seguente per creare la macchina virtuale.

		New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Per le opzioni di configurazione e le spiegazioni aggiuntive, vedere la sezione **Compilare il set di comandi** in [Uso di Azure PowerShell per creare e preconfigurare macchine virtuali basate su Windows](virtual-machines-windows-classic-create-powershell.md).

## Script di PowerShell di esempio

Lo script seguente costituisce un esempio di uno script completo che crea una macchina virtuale **SQL Server 2014 SP1 Enterprise in Windows Server 2012 R2**. Se si usa questo script, è necessario personalizzare le variabili iniziali sulla base dei passaggi precedenti di questo argomento.

	# Customize these variables based on your settings and requirements:
	$dcLocation = "East US"
	$subscr="mysubscription"
	$staccount="mystorageaccount"
	$family="SQL Server 2014 SP1 Enterprise on Windows Server 2012 R2"
	$svcname = "mycloudservice"
	$vmname="myvirtualmachine"
	$vmsize="A5"

	# Set the current subscription and storage account
	# Comment out the New-AzureStorageAccount line if the account already exists
	Select-AzureSubscription -SubscriptionName $subscr –Current
	New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
	Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

	# Select the most recent VM image in this image family:
	$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

	# Create the new cloud service; comment out this line if cloud service exists already:
	New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

	# Create the VM config:
	$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

	# Set administrator credentials:
	$cred=Get-Credential -Message "Type the name and password of the local administrator account."
	$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

	# Create the SQL Server VM:
	New-AzureVM –ServiceName $svcname -VMs $vm1


## Connettersi a Desktop remoto

1. Creare il file con estensione rdp nella cartella documenti dell'utente corrente per avviare queste macchine virtuali e completare l'installazione:

		$documentspath = [environment]::getfolderpath("mydocuments")
		Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Nella directory dei documenti avviare il file con estensione rdp. Connettersi con il nome utente e la password dell'amministratore specificati in precedenza (ad esempio, se è stato specificato il nome utente VMAdmin, specificare "\\VMAdmin" come utente e fornire la password).

		.\vm1.rdp

## Completare la configurazione del computer SQL Server per l'accesso remoto

Dopo aver eseguito l'accesso al computer con desktop remoto, configurare SQL Server in base alle istruzioni presenti in [Procedura per la configurazione della connettività di SQL Server in una VM di Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## Passaggi successivi

Per istruzioni aggiuntive sul provisioning delle macchine virtuali con PowerShell, vedere la [documentazione delle macchine virtuali](virtual-machines-windows-classic-create-powershell.md). Per altri script relativi a SQL Server e Archiviazione Premium, vedere [Utilizzare Archiviazione Premium di Azure con SQL Server in macchine virtuali](virtual-machines-windows-classic-sql-server-premium-storage.md).

In molti casi, il passaggio successivo consiste nella migrazione dei database in questa nuova macchina virtuale di SQL Server. Per linee guida sulla migrazione dei database, vedere [Migrazione di un database a SQL Server in una VM di Azure](virtual-machines-windows-migrate-sql.md).

Se si è interessati anche all'uso del portale di Azure per creare macchine virtuali di SQL Server, vedere [Effettuare il provisioning di una macchina virtuale di SQL Server nel portale di Azure](virtual-machines-windows-portal-sql-server-provision.md). Si noti che l'esercitazione che illustra il portale descrive la creazione di macchine virtuali mediante il modello consigliato di Gestione risorse anziché il modello classico impiegato in questo argomento di PowerShell.

Oltre a queste risorse, è consigliabile esaminare [altri argomenti relativi all'esecuzione di SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0629_2016-->