<properties
	pageTitle="Creare farm di SharePoint Server | Microsoft Azure"
	description="È possibile creare rapidamente una nuova farm base o a disponibilità elevata di SharePoint Server 2013 con il marketplace nel portale di Azure."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="JoeDavies-MSFT"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="josephd"/>

# Creare farm di SharePoint Server

Con il marketplace del portale di Microsoft Azure, è possibile creare rapidamente delle farm pre-configurate di SharePoint Server 2013. Ciò consente di risparmiare una notevole quantità di tempo in caso si necessiti di una farm di SharePoint di base o a disponibilità elevata per un ambiente di sviluppo e test o in caso si stia valutando l'opportunità di usare SharePoint Server 2013 come soluzione per la collaborazione all'interno dell'organizzazione.

> [AZURE.NOTE] L'elemento **SharePoint Farm** nella sezione Marketplace del portale di Azure è stato rimosso. È stato sostituito dagli elementi **SharePoint 2013 non-HA Farm** e **SharePoint 2013 HA Farm**.

La farm di SharePoint di base è costituita da tre macchine virtuali in questa configurazione.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

È possibile usare questa configurazione di farm per un'installazione semplificata per lo sviluppo di app SharePoint o per la prima valutazione di SharePoint 2013.

Per creare la farm di SharePoint (tre server) di base:

1. Fare clic [qui](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/)
2. Fare clic su **Distribuisci**.
3. Nel riquadro **SharePoint 2013 non-HA Farm**, fare clic su **Crea**.
4. Specificare le impostazioni nei sette passaggi del riquadro di creazione di **SharePoint 2013 non-HA Farm** e quindi fare clic su **Crea**.

La farm di SharePoint a disponibilità elevata è costituita da nove macchine virtuali in questa configurazione.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

È possibile usare questa configurazione di farm per testare carichi client più elevati, la disponibilità elevata del sito di SharePoint esterno e i gruppi di disponibilità AlwaysOn per una farm di SharePoint. È anche possibile usare questa configurazione per lo sviluppo di app SharePoint in un ambiente a disponibilità elevata.

Per creare la farm di SharePoint a disponibilità elevata (nove server):

1. Fare clic [qui](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/)
2. Fare clic su **Distribuisci**.
3. Nel riquadro **SharePoint 2013 HA Farm**, fare clic su **Crea**.
4. Specificare le impostazioni nei sette passaggi del riquadro di creazione di **SharePoint 2013 HA Farm** e quindi fare clic su **Crea**.

> [AZURE.NOTE] Non è possibile creare l'elemento **SharePoint 2013 non-HA Farm** o **SharePoint 2013 HA Farm** con una versione di valutazione gratuita di Azure.

Il portale di Azure crea entrambe queste farm in una rete virtuale solo cloud con una presenza Web con connessione Internet. Non esiste alcuna connessione VPN o ExpressRoute da sito a sito alla rete dell'organizzazione.

## Gestire le farm di SharePoint

È possibile amministrare i server di queste farm tramite connessioni Desktop Remoto. Per ulteriori informazioni, vedere [Accedere alla macchina virtuale](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Dal sito Amministrazione centrale di SharePoint è possibile configurare i siti personali, le applicazioni di SharePoint e altre funzionalità. Per altre informazioni, vedere [Configurare SharePoint 2013](http://technet.microsoft.com/library/ee836142.aspx).

> [AZURE.NOTE] Con l'[immagine della versione di valutazione di SharePoint Server 2016](https://azure.microsoft.com/blog/test-sharepoint-server-2016/) è possibile creare una macchina virtuale che esegue SharePoint Server 2016.

## Passaggi successivi

- Altre configurazioni di [SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) nei servizi dell'infrastruttura di Azure.

<!---HONumber=AcomDC_0601_2016-->