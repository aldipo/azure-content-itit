<properties
   pageTitle="Collegamento di una rete virtuale a un circuito ExpressRoute mediante il modello di distribuzione di Gestione risorse e il portale di Azure| Microsoft Azure"
   description="Questo documento fornisce una panoramica delle operazioni per il collegamento di reti virtuali (VNet) a circuiti ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/14/2016"
   ms.author="cherylmc" />

# Collegare una rete virtuale a un circuito ExpressRoute

> [AZURE.SELECTOR]
- [Portale di Azure - Gestione risorse](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShell - Gestione risorse](expressroute-howto-linkvnet-arm.md)
- [PowerShell - Classico](expressroute-howto-linkvnet-classic.md)



Questo articolo spiega come collegare le reti virtuali (Vnet) ai circuiti ExpressRoute di Azure usando il modello di distribuzione di Gestione risorse e il portale di Azure. Le reti virtuali possono trovarsi nella stessa sottoscrizione o appartenere a un'altra sottoscrizione.


**Informazioni sui modelli di distribuzione di Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Prerequisiti di configurazione

- Prima di procedere con la configurazione, assicurarsi di avere verificato i [prerequisiti](expressroute-prerequisites.md), i [requisiti di routing](expressroute-routing.md) e i [flussi di lavoro](expressroute-workflows.md).
- È necessario avere un circuito ExpressRoute attivo.
	- Seguire le istruzioni per [creare un circuito ExpressRoute](expressroute-howto-circuit-arm.md) e fare in modo che venga abilitato dal provider di connettività.

	- Assicurarsi di disporre del peering privato di Azure configurato per il circuito. Vedere l'articolo relativo alla [configurazione del routing](expressroute-howto-routing-portal-resource-manager.md) per istruzioni relative al routing.

	- Per abilitare la connettività end-to-end è necessario assicurarsi che sia configurato il peering privato di Azure e sia attivo il peering BGP tra la rete e Microsoft.

	- Assicurarsi di disporre di una rete virtuale e di un gateway di rete virtuale creati e con provisioning completo. Seguire le istruzioni relative alla creazione di un [gateway VPN](../articles/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) (solo i passaggi da 1 a 5).

È possibile collegare fino a 10 reti virtuali a un circuito ExpressRoute. Tutti i circuiti ExpressRoute devono trovarsi nella stessa area geopolitica. È possibile collegare numerose reti virtuali al circuito ExpressRoute se è stato abilitato il componente aggiuntivo ExpressRoute Premium. Per altre informazioni sul componente aggiuntivo Premium, vedere le [domande frequenti](expressroute-faqs.md).

## Collegare una rete virtuale della stessa sottoscrizione a un circuito


### Per creare una connessione

1. Verificare che il circuito ExpressRoute e il peering privato di Azure siano configurati correttamente. Seguire le istruzioni in [Creare un circuito ExpressRoute](expressroute-howto-circuit-arm.md) e [Configurare il routing](expressroute-howto-routing-arm.md). Il circuito ExpressRoute deve essere simile a quello della figura seguente.

	![Schermata del circuito ExpressRoute](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)

	>[AZURE.NOTE] Le informazioni sulla configurazione BGP non verranno visualizzate se i peering sono stati configurati dal provider di livello 3. Se è stato eseguito il provisioning del circuito, deve essere possibile creare le connessioni.

2. È ora possibile avviare il provisioning di una connessione per collegare il gateway della rete virtuale al circuito ExpressRoute. Fare clic su **Connessione** > **Aggiungi** per aprire il pannello **Aggiungi connessione** e configurare i valori. Vedere l'esempio di riferimento seguente.


	![Aggiungere la schermata della connessione](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)


3. Al termine della configurazione, l'oggetto connessione visualizzerà le informazioni per la connessione.

	![Schermata dell'oggetto connessione](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)


### Per eliminare una connessione

È possibile eliminare una connessione selezionando l'icona **Elimina** nel pannello della connessione.

## Collegare una rete virtuale di un'altra sottoscrizione a un circuito

A questo punto, non è possibile collegare le reti virtuali di diverse sottoscrizioni usando il portale di Azure. È possibile tuttavia eseguire questa operazione con PowerShell. Per altre informazioni, vedere l'articolo relativo a [PowerShell](expressroute-howto-linkvnet-arm.md).

## Passaggi successivi

Per altre informazioni su ExpressRoute, vedere le [Domande frequenti su ExpressRoute](expressroute-faqs.md).

<!---HONumber=AcomDC_0518_2016-->