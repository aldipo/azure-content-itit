<properties
   pageTitle="Configurare connessioni di ExpressRoute e VPN da sito a sito coesistenti | Microsoft Azure"
   description="Questo articolo illustra come configurare connessioni ExpressRoute e VPN da sito a sito ai fini della coesistenza per il modello di distribuzione classica."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/06/2016"
   ms.author="charwen"/>

# Configurare connessioni coesistenti da sito a sito ed ExpressRoute per il modello di distribuzione classica


> [AZURE.SELECTOR]
- [PowerShell - Gestione risorse](expressroute-howto-coexist-resource-manager.md)
- [PowerShell - Classico](expressroute-howto-coexist-classic.md)

La possibilità di configurare una VPN da sito a sito ed ExpressRoute offre diversi vantaggi. È possibile configurare una VPN da sito a sito come percorso di failover sicuro per ExressRoute oppure usare VPN da sito a sito per connettersi a siti che non sono connessi tramite ExpressRoute. In questo articolo verranno illustrati i passaggi per configurare entrambi questi scenari. Questo articolo si applica al modello di distribuzione classica. Questa configurazione non è disponibile nel portale.

**Informazioni sui modelli di distribuzione di Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

>[AZURE.IMPORTANT] Per eseguire le istruzioni riportate di seguito devono essere presenti circuiti ExpressRoute preconfigurati. Assicurarsi di aver seguito le guide per la [creazione di un circuito ExpressRoute](expressroute-howto-circuit-classic.md) e per la [configurazione del routing](expressroute-howto-routing-classic.md) prima di seguire questa procedura.

## Limiti e limitazioni

- **Il routing di transito non è supportato:** Azure non consente il routing tra la rete locale connessa tramite VPN da sito a sito e la rete locale connessa tramite ExpressRoute.
- **Le connessioni da punto a sito non sono supportate:** non è possibile abilitare connessioni VPN da punto a sito alla stessa rete virtuale connessa a ExpressRoute. Non possono coesistere connessioni VPN da punto a sito ed ExpressRoute per la stessa rete virtuale.
- **Non è possibile abilitare il tunneling forzato sul gateway VPN da sito a sito:** è possibile solo "forzare" tutto il traffico associato a Internet verso la rete locale tramite ExpressRoute. 
- **Solo gateway standard o ad alte prestazioni:** è necessario usare un gateway standard o ad alte prestazioni sia per il gateway ExpressRoute che per il gateway VPN da sito a sito. Per informazioni sugli SKU del gateway, vedere [SKU del gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Solo gateway VPN basato su route:** è necessario usare un gateway VPN basato su route. Per informazioni sul gateway VPN basato su route, vedere [Informazioni sui gateway VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Requisito di route statica:** se la rete locale è connessa sia a ExpressRoute che a una VPN da sito a sito, per il routing della connessione VPN da sito a sito alla rete Internet pubblica è necessario che nella rete locale sia configurata una route statica.
- **Il gateway ExpressRoute deve essere configurato per primo:** è necessario creare il gateway ExpressRoute prima di aggiungere il gateway VPN da sito a sito.


## Progetti di configurazione

### Configurare una VPN da sito a sito come percorso di failover per ExpressRoute

È possibile configurare una connessione VPN da sito a sito come backup per ExpressRoute. Questo si applica solo alle reti virtuali collegate al percorso di peering privato di Azure. Non esiste alcuna soluzione di failover basato su VPN per i servizi accessibili tramite i peering pubblico di Azure e Microsoft. Il circuito ExpressRoute è sempre il collegamento principale. Il flusso dei dati attraverserà il percorso VPN da sito a sito solo se il circuito ExpressRoute ha esito negativo.

![Coesistenza](media/expressroute-howto-coexist-classic/scenario1.jpg)

### Configurare una VPN da sito a sito per la connessione a siti non connessi tramite ExpressRoute

È possibile configurare una rete in cui alcuni siti si connettono direttamente ad Azure tramite VPN da sito a sito e altri si connettono tramite ExpressRoute.

![Coesistenza](media/expressroute-howto-coexist-classic/scenario2.jpg)

>[AZURE.NOTE] Non è possibile configurare una rete virtuale come router di transito.

## Selezione dei passaggi da usare

Esistono due diverse serie di procedure disponibili per configurare le connessioni che possono coesistere. La procedura di configurazione scelta dipende dalla disponibilità o meno di una rete virtuale esistente a cui stabilire la connessione oppure dall'esigenza di creare una nuova rete virtuale.


- Non è disponibile una rete virtuale ed è necessario crearne una.
	
	Se non si ha ancora una rete virtuale, questa procedura consentirà la creazione di una nuova rete virtuale e di nuove connessioni ExpressRoute e VPN da sito a sito usando il modello di distribuzione classica. Per la configurazione, eseguire i passaggi nella sezione [Per creare una nuova rete virtuale con connessioni coesistenti](#new).

- È già disponibile una rete virtuale con modello di distribuzione classica.

	E’ possibile che esista già una rete virtuale con una connessione VPN da sito a sito o una connessione ExpressRoute. La sezione [Per configurare connessioni coesistenti per una rete virtuale esistente](#add) illustra come eliminare il gateway e quindi come creare nuove connessioni ExpressRoute e VPN da sito a sito. Durante la creazione di nuove connessioni, è necessario completare i passaggi in un ordine molto specifico. Non usare le istruzioni in altri articoli per creare gateway e connessioni.

	In questa procedura, la creazione di connessioni che possono coesistere richiederà l’eliminazione del gateway e la configurazione di nuovi gateway. Mentre si elimina e si ricrea il gateway e le connessioni, si avrà un tempo di inattività per le connessioni cross-premise, ma non sarà necessario eseguire la migrazione delle macchine virtuali o dei servizi a una nuova rete virtuale. Le macchine virtuali e i servizi saranno comunque in grado di comunicare tramite il servizio di bilanciamento del carico mentre si configura il gateway, se sono configurati in questo senso.


## <a name="new"></a>Per creare una nuova rete virtuale con connessioni coesistenti

Questa procedura illustra come creare una rete virtuale e connessioni da sito a sito ed ExpressRoute coesistenti.

1. È necessario installare l'ultima versione dei cmdlet di Azure PowerShell. Per altre informazioni sull'installazione dei cmdlet di PowerShell, vedere [Come installare e configurare Azure PowerShell](../powershell-install-configure.md). Si noti che i cmdlet usati per questa configurazione possono essere leggermente diversi da quelli con cui si ha familiarità. Assicurarsi di usare i cmdlet specificati in queste istruzioni. 

2. Creare uno schema per la rete virtuale. Per altre informazioni sullo schema di configurazione, vedere [Schema di configurazione della rete virtuale di Azure](https://msdn.microsoft.com/library/azure/jj157100.aspx).

	Quando si crea lo schema, assicurarsi di usare i valori seguenti:

	- La subnet del gateway per la rete virtuale deve essere /27 o un prefisso più breve (ad esempio /26 o /25).
	- Il tipo di connessione del gateway è "Dedicato".

		      <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
		        <AddressSpace>
		          <AddressPrefix>10.17.159.192/26</AddressPrefix>
		        </AddressSpace>
		        <Subnets>
		          <Subnet name="Subnet-1">
		            <AddressPrefix>10.17.159.192/27</AddressPrefix>
		          </Subnet>
		          <Subnet name="GatewaySubnet">
		            <AddressPrefix>10.17.159.224/27</AddressPrefix>
		          </Subnet>
		        </Subnets>
		        <Gateway>
		          <ConnectionsToLocalNetwork>
		            <LocalNetworkSiteRef name="MyLocalNetwork">
		              <Connection type="Dedicated" />
		            </LocalNetworkSiteRef>
		          </ConnectionsToLocalNetwork>
		        </Gateway>
		      </VirtualNetworkSite>

3. Dopo la creazione e configurazione del file di schema xml, caricare il file. Verrà creata la rete virtuale.

	Usare il cmdlet seguente per caricare il file, sostituendo il valore con uno personalizzato.

		Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'

4. <a name="gw"></a>Creare un gateway ExpressRoute. Specificare *Standard* o *HighPerformance* per GatewaySKU e *DynamicRouting* per GatewayType.

	Usare l'esempio di seguito, sostituendo i valori personalizzati.

		New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance

5. Collegare il gateway ExpressRoute al circuito ExpressRoute. Dopo aver completato questo passaggio, viene stabilita la connessione tra la rete locale e Azure tramite ExpressRoute.

		New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET

6. <a name="vpngw"></a>Creare quindi il gateway VPN da sito a sito. GatewaySKU deve essere *Standard* o *HighPerformance* e GatewayType deve essere *DynamicRouting*.

		New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance

	Per recuperare le impostazioni del gateway di rete virtuale, inclusi l'ID del gateway e l'indirizzo IP pubblico, usare il cmdlet `Get-AzureVirtualNetworkGateway`.

		Get-AzureVirtualNetworkGateway

		GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
		GatewayName          : S2SVPN
		LastEventData        :
		GatewayType          : DynamicRouting
		LastEventTimeStamp   : 5/29/2015 4:41:41 PM
		LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
		LastEventID          : 23002
		State                : Provisioned
		VIPAddress           : 104.43.x.y
		DefaultSite          :
		GatewaySKU           : HighPerformance
		Location             :
		VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
		SubnetId             :
		EnableBgp            : False
		OperationDescription : Get-AzureVirtualNetworkGateway
		OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
		OperationStatus      : Succeeded

7. Creare un'entità gateway VPN del sito locale. Questo comando non configura il gateway VPN locale. Consente invece di fornire le impostazioni del gateway locale, ad esempio l'indirizzo IP pubblico e lo spazio indirizzi locale, in modo che il gateway VPN di Azure possa connettersi.

	>[AZURE.IMPORTANT] Il sito locale per la connessione VPN da sito a sito non è definito nel file netcfg. È invece necessario usare questo cmdlet per specificare i parametri del sito locale. Non è possibile definirlo tramite il portale o il file netcfg.

	Usare l'esempio seguente, sostituendo i valori con quelli personalizzati.

		New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>

	> [AZURE.NOTE] Se la rete locale include più route, è possibile passarle tutte come una matrice. $MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")


	Per recuperare le impostazioni del gateway di rete virtuale, inclusi l'ID del gateway e l'indirizzo IP pubblico, usare il cmdlet `Get-AzureVirtualNetworkGateway`. Vedere l'esempio seguente.

		Get-AzureLocalNetworkGateway

		GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
		GatewayName          : MyLocalNetwork
		IpAddress            : 23.39.x.y
		AddressSpace         : {10.1.2.0/24}
		OperationDescription : Get-AzureLocalNetworkGateway
		OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
		OperationStatus      : Succeeded


8. Configurare il dispositivo VPN locale per la connessione al nuovo gateway. Quando si configura il dispositivo VPN, usare le informazioni recuperate nel passaggio 6. Per altre informazioni sulla configurazione del dispositivo VPN, vedere la pagina relativa alla [configurazione del dispositivo VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

9. Collegare il gateway VPN da sito a sito in Azure al gateway locale.

	In questo esempio connectedEntityId è l'ID del gateway locale, che è possibile trovare eseguendo `Get-AzureLocalNetworkGateway`. È possibile trovare virtualNetworkGatewayId usando il cmdlet `Get-AzureVirtualNetworkGateway`. Dopo questo passaggio, viene stabilita la connessione tra la rete locale e Azure tramite la connessione VPN da sito a sito.


		New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <a name="add"></a>Per configurare connessioni coesistenti per una rete virtuale esistente

Se esiste già una rete virtuale, controllare le dimensioni della subnet del gateway. Se la subnet del gateway è pari a/29 o /28, è necessario eliminare prima di tutto il gateway di rete virtuale e aumentare le dimensioni della subnet del gateway. I passaggi descritti in questa sezione illustrano come eseguire questa operazione.

Se la subnet del gateway è /27 o superiore e la rete virtuale è connessa tramite ExpressRoute, è possibile ignorare i passaggi seguenti e andare al ["Passaggio 6: Creare un gateway VPN da sito a sito"](#vpngw) nella sezione precedente.

>[AZURE.NOTE] Quando si elimina il gateway esistente, gli ambienti locali perderanno la connessione alla rete virtuale mentre si lavora a questa configurazione.

1. È necessario installare la versione più recente dei cmdlet di PowerShell per Gestione risorse di Azure. Per altre informazioni sull'installazione dei cmdlet di PowerShell, vedere [Come installare e configurare Azure PowerShell](../powershell-install-configure.md). Si noti che i cmdlet usati per questa configurazione possono essere leggermente diversi da quelli con cui si ha familiarità. Assicurarsi di usare i cmdlet specificati in queste istruzioni. 

2. Eliminare il gateway ExpressRoute o VPN da sito a sito esistente. Usare il cmdlet seguente, sostituendo i valori con quelli personalizzati.

		Remove-AzureVNetGateway –VnetName MyAzureVNET

3. Esportare lo schema della rete virtuale. Usare il cmdlet PowerShell seguente, sostituendo i valori con quelli personalizzati.

		Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”

4. Modificare lo schema del file di configurazione di rete in modo che la subnet del gateway sia /27 o un prefisso più breve (ad esempio /26 o /25). Vedere l'esempio seguente.
>[AZURE.NOTE] Se non sono rimasti indirizzi IP sufficienti nella rete virtuale per aumentare le dimensioni della subnet del gateway, è necessario aggiungere altro spazio di indirizzi IP. Per altre informazioni sullo schema di configurazione, vedere [Schema di configurazione della rete virtuale di Azure](https://msdn.microsoft.com/library/azure/jj157100.aspx).

          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>

5. Se il gateway precedente era una VPN da sito a sito, è necessario modificare anche il tipo di connessione su **Dedicato**.

		         <Gateway>
		          <ConnectionsToLocalNetwork>
		            <LocalNetworkSiteRef name="MyLocalNetwork">
		              <Connection type="Dedicated" />
		            </LocalNetworkSiteRef>
		          </ConnectionsToLocalNetwork>
		        </Gateway>

6. A questo punto, si avrà una rete virtuale senza gateway. Per creare nuovi gateway e completare le connessioni, è possibile procedere con il [Passaggio 4: Creare un gateway ExpressRoute](#gw), disponibile nel set di passaggi precedente.

## Passaggi successivi

Per altre informazioni su ExpressRoute, vedere le [Domande frequenti su ExpressRoute](expressroute-faqs.md).

<!---HONumber=AcomDC_0511_2016-->