<properties 
   pageTitle="Configurare il tunneling forzato per connessioni da sito a sito con il modello di distribuzione Resource Manager | Microsoft Azure"
   description="Come reindirizzare o forzare tutto il traffico associato a Internet verso il percorso locale."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="cherylmc" />

# Configurare il tunneling forzato tramite il modello di distribuzione Azure Resource Manager

> [AZURE.SELECTOR]
- [PowerShell - Gestione servizi](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Gestione risorse](vpn-gateway-forced-tunneling-rm.md)

Il tunneling forzato consente di reindirizzare o "forzare" tutto il traffico associato a Internet verso la posizione locale tramite un tunnel VPN da sito a sito per l'ispezione e il controllo. Si tratta di un requisito di sicurezza critico per la maggior parte dei criteri IT aziendali.

Senza il tunneling forzato, il traffico associato a Internet dalle macchine virtuali in Azure raggiungerà direttamente Internet attraversando sempre l'infrastruttura di rete di Azure, senza l'opzione che consente di ispezionare o controllare il traffico. L'accesso a Internet non autorizzato potrebbe provocare la divulgazione di informazioni o altri tipi di violazioni della sicurezza

Questo articolo illustra la configurazione del tunneling forzato per le reti virtuali create usando il modello di distribuzione Resource Manager.

**Informazioni sui modelli di distribuzione di Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

**Strumenti e modelli di distribuzione per il tunneling forzato**

È possibile configurare una connessione di tunneling forzato in entrambi i modelli di distribuzione e tramite strumenti diversi. Per altre informazioni, vedere la tabella seguente. La tabella verrà aggiornata man mano che per questa configurazione risultano disponibili nuovi articoli, modelli di distribuzione e strumenti. Quando un articolo risulterà disponibile, nella tabella sarà presente un collegamento diretto.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)]


## Informazioni sul tunneling forzato


Nel diagramma seguente viene illustrato il funzionamento del tunneling forzato.

![Tunneling forzato](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Nell'esempio riportato in precedenza, il tunneling della subnet front-end non viene forzato. I carichi di lavoro nella subnet front-end possono continuare ad accettare e a rispondere alle richieste dei clienti direttamente da Internet. Il tunneling delle subnet di livello intermedio e back-end viene forzato. Tutte le connessioni in uscita da queste due subnet a Internet verranno forzate o reindirizzate verso un sito locale tramite uno dei tunnel VPN S2S.

Ciò consente di limitare e ispezionare l'accesso a Internet dalle macchine virtuali o dai servizi cloud in Azure, pur continuando ad abilitare l'architettura dei servizi multilivello richiesta. È inoltre possibile applicare il tunneling forzato a tutte le reti virtuali se non sono presenti carichi di lavoro con connessione Internet nelle reti virtuali.

## Problemi e considerazioni

Il tunneling forzato in Azure viene configurato tramite route di rete virtuale definite dall'utente. Il reindirizzamento del traffico a un sito locale viene espresso come route predefinita al gateway VPN di Azure. Per altre informazioni sulle route definite dall'utente e sulle reti virtuali, vedere [Cosa sono le route definite dall'utente e l'inoltro IP](../virtual-network/virtual-networks-udr-overview.md).

- Ciascuna subnet della rete virtuale dispone di una tabella di routing di sistema integrata. La tabella di routing di sistema include i 3 gruppi di route seguenti:

	- **Route della rete virtuale locale:** direttamente alle macchine virtuali di destinazione nella stessa rete virtuale
	
	- **Route locali:** al gateway VPN di Azure
	
	- **Route predefinita:** direttamente a Internet. I pacchetti destinati agli indirizzi IP privati non rientranti nelle due route precedenti verranno eliminati.

-  Questa procedura usa le route definite dall'utente per creare una tabella di routing per aggiungere una route predefinita, quindi associare la tabella di routing alle subnet della rete virtuale per abilitare il tunneling forzato in tali subnet.

- Il tunneling forzato deve essere associato a una rete virtuale con un gateway VPN basato su route. È necessario impostare un "sito predefinito" tra i siti locali cross-premise connessi alla rete virtuale.

- Il tunneling forzato ExpressRoute non viene configurato mediante questo meccanismo, ma è abilitato dai clienti annunciando una route predefinita tramite le sessioni di peering BGP ExpressRoute. Per altre informazioni, vedere la [documentazione di ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## Panoramica della configurazione

La procedura seguente consente di creare un gruppo di risorse e una rete virtuale. Si passerà quindi alla creazione di un gateway VPN e alla configurazione del tunneling forzato. In questa procedura la rete virtuale "MultiTier-VNet" include 3 subnet (*Frontend*, *Midtier* e *Backend*) con 4 connessioni di più sedi locali (*DefaultSiteHQ*) e 3 *Branch*.

La procedura illustrata consente di impostare *DefaultSiteHQ* come connessione predefinita del sito per il tunneling forzato e di configurare le subnet Midtier e Backend per l'uso del tunneling forzato.

	
## Prima di iniziare

Prima di iniziare la configurazione, verificare di essere in possesso dei requisiti seguenti.

- Una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi dell'abbonamento a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/pricing/free-trial/).

- È necessario installare la versione più recente dei cmdlet di PowerShell per Azure Resource Manager (versione 1.0 o successiva). Per altre informazioni sull'installazione dei cmdlet di PowerShell, vedere [Come installare e configurare Azure PowerShell](../powershell-install-configure.md).


## È possibile configurare il tunneling forzato?

1. Nella console di PowerShell accedere all'account Azure. Il cmdlet richiederà le credenziali di accesso per l'account Azure. Dopo l'accesso, vengono scaricate le impostazioni dell'account in modo che siano disponibili per Azure PowerShell.

		Login-AzureRmAccount 

2. Ottenere un elenco delle sottoscrizioni di Azure.

		Get-AzureRmSubscription

2. Specificare la sottoscrizione da usare.

		Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
		
3. Creare un gruppo di risorse.

		New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Creare una rete virtuale e specificare le subnet.

		$s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
		$s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
		$s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
		$s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
		$vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Creare i gateway di rete locale.

		$lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
		$lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
		$lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
		$lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
		
6. Creare la tabella di route e la regola di route.

		New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
		$rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
		Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
		Set-AzureRmRouteTable -RouteTable $rt


7. Associare la tabella di route alle subnet Midtier e Backend.

		$vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
		Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
		Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
		Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Creare il gateway con un sito predefinito. Questa procedura può richiedere 20 minuti o più, perché include le operazioni di creazione e configurazione del gateway. Anche se nella console sembrano solo pochi cmdlet, in background vengono eseguite numerose operazioni. Si noti che il parametro GatewayDefaultSite è quello che consente alla configurazione di routing forzato di funzionare. È quindi consigliabile non ignorare questo passaggio. Disponibile solo in PowerShell 1.0 o versioni successive.

		$pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
		$gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
		$ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
		New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Stabilire connessioni VPN da sito a sito

		$gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
		$lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
		$lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
		$lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
		$lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
		New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

		Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
		

<!---HONumber=AcomDC_0518_2016-->