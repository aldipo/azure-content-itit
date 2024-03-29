<properties
	pageTitle="Configurare gli endpoint in una VM Windows classica | Microsoft Azure"
	description="Informazioni su come configurare gli endpoint nel portale di Azure classico per consentire la comunicazione con una macchina virtuale Windows in Azure."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/19/2016"
	ms.author="cynthn"/>

# Come configurare gli endpoint in una macchina virtuale classica in Azure


Tutte le macchine virtuali Windows create in Azure con il modello di distribuzione classico possono comunicare automaticamente mediante un canale di rete privato con altre macchine virtuali dello stesso servizio cloud o nella stessa rete virtuale. Tuttavia, i computer in Internet o altre reti virtuali richiedono gli endpoint per indirizzare il traffico di rete in ingresso a una macchina virtuale. Questo articolo è disponibile anche per le [macchine virtuali Linux](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Informazioni su come [eseguire questa procedura con il modello di Resource Manager](virtual-machines-windows-nsg-quickstart-portal.md). Nel modello di distribuzione di **Resource Manager** gli endpoint vengono configurati tramite i **gruppi di sicurezza di rete (NGS)**. Per altre informazioni, vedere [Consentire l'accesso esterno alla VM mediante il portale di Azure](virtual-machines-windows-nsg-quickstart-portal.md).

Quando si crea una macchina virtuale Windows nel portale di Azure classico, gli endpoint comuni, ad esempio quelli di Desktop remoto e Comunicazione remota di Windows PowerShell, sono in genere creati automaticamente. È possibile configurare altri endpoint durante la creazione della macchina virtuale o successivamente all'occorrenza.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## Passaggi successivi

* Per usare un cmdlet di Azure PowerShell per impostare un endpoint di VM, vedere [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Per usare un cmdlet di Azure PowerShell per gestire un ACL in un endpoint, vedere [Come gestire gli elenchi di controllo di accesso per gli endpoint tramite PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Se è stata creata una macchina virtuale nel modello di distribuzione di Resource Manager, è possibile usare Azure PowerShell per [creare gruppi di sicurezza di rete](../virtual-network/virtual-networks-create-nsg-arm-ps.md) per controllare il traffico verso la VM.

<!---HONumber=AcomDC_0629_2016-->