<properties
 pageTitle="Aggiungere nodi burst in un cluster HPC Pack | Microsoft Azure"
 description="Informazioni su come espandere la capacità del cluster HPC Pack su richiesta tramite l'aggiunta di istanze del ruolo di lavoro in esecuzione in un servizio cloud"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="04/13/2016"
 ms.author="danlep"/>

# Aggiungere nodi "burst" su richiesta (istanze del ruolo di lavoro) come risorse di calcolo a un cluster HPC Pack in Azure



In questo articolo viene illustrato come aggiungere nodi "burst" di Azure (istanze del ruolo di lavoro in esecuzione in un servizio cloud) su richiesta come risorse di calcolo in un nodo head HPC Pack esistente in Azure. Questo consente la scalabilità della capacità di calcolo del cluster HPC in Azure su richiesta, senza dover gestire un set di macchine virtuali dei nodi di calcolo preconfigurate.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Nodi burst][burst]

>[AZURE.TIP] Se si usa lo [script di distribuzione IaaS di HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) per creare il cluster in Azure, è possibile includere i nodi burst di Azure nella distribuzione automatizzata. Vedere gli esempi nell'articolo.

I passaggi descritti in questo articolo consentiranno di aggiungere nodi di Azure rapidamente a una macchina virtuale del nodo head HPC Pack basato su cloud per una distribuzione di test o con modello di verifica. La procedura è essenzialmente identica a quella per il "potenziamento in Azure" per aggiungere capacità di calcolo del cloud a un cluster HPC Pack locale. Per un'esercitazione, vedere [Configurazione di un cluster di calcolo ibrido con Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Per istruzioni dettagliate e considerazioni per le distribuzioni di produzione, vedere [Potenziamento in Azure con Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

Se si desidera usare le dimensioni di istanze a elevato utilizzo di calcolo A8 o A9, vedere [Informazioni sulle istanze A8, A9, A10 e A11 a elevato utilizzo di calcolo](virtual-machines-windows-a8-a9-a10-a11-specs.md).

## Prerequisiti

* **Nodo head HPC Pack distribuito in una macchina virtuale di Azure**: vedere [Distribuire un nodo head di HPC Pack in una VM di Azure](virtual-machines-windows-hpcpack-cluster-headnode.md) per la procedura di creazione di un nodo head del cluster nel modello di distribuzione classica.

* **Sottoscrizione di Azure** - Per aggiungere nodi di Azure, è possibile scegliere la stessa sottoscrizione usata per distribuire la macchina virtuale del nodo head oppure una o più sottoscrizioni diverse.

* **Quota di core** - Potrebbe essere necessario aumentare la quota di core, soprattutto se si sceglie di distribuire più nodi di Azure con dimensioni multicore. Per aumentare una quota, [aprire una richiesta di assistenza clienti online](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) gratuitamente.

## Passaggio 1: Creare un servizio cloud e un account di archiviazione per aggiungere nodi di Azure

Usare il Portale di Azure classico o strumenti equivalenti per configurare gli elementi seguenti, necessari per la distribuzione dei nodi di Azure:

* Un nuovo servizio cloud di Azure
* Un nuovo account di archiviazione di Azure

>[AZURE.NOTE] Non riutilizzare un servizio cloud esistente nella sottoscrizione.

**Considerazioni**

* Configurare un servizio cloud separato per ogni modello di nodo di Azure che si intende creare. È comunque possibile usare lo stesso account di archiviazione per più modelli di nodo.

* È in genere consigliabile posizionare il servizio cloud e l'account di archiviazione per la distribuzione nella stessa area di Azure.




## Passaggio 2: Configurare un certificato di gestione di Azure

Per aggiungere nodi di Azure come risorse di calcolo, sarà necessario disporre di un certificato di gestione nel nodo head e caricare un certificato corrispondente nella sottoscrizione di Azure usata per la distribuzione.

Per questo scenario, è possibile scegliere il **certificato di gestione di Azure HPC predefinito** installato e configurato automaticamente da HPC Pack nel nodo head. Questo certificato è utile a scopo di test e per distribuzioni con modello di verifica. Per usare questo certificato, caricare semplicemente il file C:\\Programmi\\Microsoft HPC Pack 2012\\Bin\\hpccert.cer dalla macchina virtuale del nodo head alla sottoscrizione.

Per altre opzioni per configurare il certificato di gestione, vedere gli [scenari per configurare il certificato di gestione di Azure per distribuzioni di potenziamento di Azure](http://technet.microsoft.com/library/gg481759.aspx).

## Passaggio 3: Distribuire nodi di Azure nel cluster



I passaggi per aggiungere e avviare i nodi di Azure in questo scenario sono in genere identici a quelli usati per un nodo head locale. Per altre informazioni, vedere le sezioni seguenti nella [procedura per distribuire nodi di Azure con Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Creare un modello di nodo di Azure

* Aggiungere nodi di Azure al cluster Windows HPC

* Avviare i nodi di Azure (eseguirne il provisioning)

Dopo aver aggiunto e avviato i nodi, questi saranno pronti per l'uso per l'esecuzione dei processi del cluster.

Se si verificano problemi durante la distribuzione di nodi di Azure, vedere [Risoluzione dei problemi delle distribuzioni di nodi di Azure con Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## Passaggi successivi

* Se si vogliono aumentare o ridurre automaticamente le risorse di calcolo di Azure in base al carico di lavoro corrente dei processi e delle attività del cluster, vedere [Aumentare e ridurre automaticamente le risorse di calcolo di Azure in un cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png

<!---HONumber=AcomDC_0629_2016-->