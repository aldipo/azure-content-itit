<properties
   pageTitle="Spostare i circuiti ExpressRoute dal modello di distribuzione classica a quello Resource Manager | Microsoft Azure"
   description="Questa pagina illustra come spostare un circuito classico nel modello di distribuzione Resource Manager."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/05/2016"
   ms.author="ganesr"/>


# Spostare i circuiti ExpressRoute dal modello di distribuzione classica a quello Resource Manager

## Prerequisiti di configurazione

- È necessaria la versione più recente dei moduli di Azure PowerShell (almeno la versione 1.0).
- Prima di procedere con la configurazione, assicurarsi di avere verificato i [prerequisiti](expressroute-prerequisites.md), i [requisiti di routing](expressroute-routing.md) e i [flussi di lavoro](expressroute-workflows.md).
- Prima di continuare, vedere le informazioni disponibili in [Spostamento di un circuito ExpressRoute dal modello di distribuzione classica a quello Resource Manager](expressroute-move.md). Assicurarsi di avere compreso le limitazioni e i limiti relativi alle operazioni consentite.
- Se si vuole spostare un circuito Azure ExpressRoute dal modello di distribuzione classica al modello di distribuzione Azure Resource Manager, è necessario che il circuito sia completamente configurato e operativo nel modello di distribuzione classica.
- Assicurarsi che sia disponibile un gruppo di risorse creato nel modello di distribuzione Resource Manager.

## Spostare il circuito ExpressRoute nel modello di distribuzione Resource Manager

È necessario spostare un circuito ExpressRoute nel modello di distribuzione Resource Manager, in modo che sia possibile usarlo nei modelli di distribuzione classica e Resource Manager. A questo scopo è possibile eseguire i comandi di PowerShell seguenti.

### Passaggio 1: Raccogliere informazioni dettagliate sul circuito dal modello di distribuzione classica

È necessario raccogliere prima di tutto le informazioni sul circuito ExpressRoute.

Accedere all'ambiente Azure classico e quindi ottenere la chiave servizio. È possibile usare il frammento di codice di PowerShell seguente per raccogliere le informazioni:

	# Sign in to your Azure account
	Add-AzureAccount

	# Select the appropriate Azure subscription
	Select-AzureSubscription "<Enter Subscription Name here>"

	# Import the PowerShell modules for Azure and ExpressRoute
	Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
	Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

	# Get the service keys of all your ExpressRoute circuits
	Get-AzureDedicatedCircuit

Copiare la **chiave servizio** del circuito da spostare nel modello di distribuzione Resource Manager.

### Passaggio 2: Accedere all'ambiente Resource Manager e creare un nuovo gruppo di risorse

È possibile creare un nuovo gruppo di risorse usando il frammento di codice seguente:

	# Sign in to your Azure Resource Manager environment
	Login-AzureRmAccount

	# Select the appropriate Azure subscription
	Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

	#Create a new resource group if you don't already have one
	New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

È anche possibile usare un gruppo di risorse esistente, se è già disponibile.

### Passaggio 3: Spostare il circuito ExpressRoute nel modello di distribuzione Resource Manager

È ora possibile spostare il circuito ExpressRoute dal modello di distribuzione classica al modello Resource Manager. Prima di continuare, verificare le informazioni disponibili in [Spostamento di un circuito ExpressRoute dal modello di distribuzione classica a quello Resource Manager](expressroute-move.md).

Per completare questa operazione, eseguire il frammento di codice seguente:

	Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Al termine dello spostamento, il nuovo nome elencato nel cmdlet precedente verrà usato per fare riferimento alla risorsa. Il circuito verrà essenzialmente rinominato.

## Abilitare un circuito ExpressRoute per entrambi i modelli di distribuzione

È necessario spostare il circuito ExpressRoute nel modello di distribuzione Resource Manager prima di controllare l'accesso al modello di distribuzione.

Eseguire il cmdlet seguente per abilitare l'accesso a entrambi i modelli di distribuzione:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Al termine di questa operazione, sarà possibile visualizzare il circuito nel modello di distribuzione classica.

Eseguire il comando seguente per ottenere i dettagli del circuito ExpressRoute:

    get-azurededicatedcircuit

Si dovrebbe essere in grado di vedere la chiave servizio elencata. È ora possibile gestire i collegamenti al circuito ExpressRoute usando i comandi del modello di distribuzione classico standard per le reti virtuali classiche e i comandi standard di ARM per le reti virtuali ARM. Gli articoli seguenti illustrano in modo dettagliato come gestire i collegamenti al circuito ExpressRoute:

- [Collegare la rete virtuale al circuito ExpressRoute nel modello di distribuzione Resource Manager](expressroute-howto-linkvnet-arm.md)
- [Collegare la rete virtuale al circuito ExpressRoute nel modello di distribuzione classica](expressroute-howto-linkvnet-classic.md)


## Disabilitare il circuito ExpressRoute nel modello di distribuzione classica

Eseguire il cmdlet seguente per disabilitare l'accesso al modello di distribuzione classica:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Al termine di questa operazione, non sarà possibile visualizzare il circuito nel modello di distribuzione classica.

## Passaggi successivi

Dopo aver creato il circuito, verificare di eseguire le operazioni seguenti:

- [Creare e modificare il routing per un circuito ExpressRoute](expressroute-howto-routing-arm.md)
- [Collegare la rete virtuale al circuito ExpressRoute](expressroute-howto-linkvnet-arm.md)

<!---HONumber=AcomDC_0518_2016-->