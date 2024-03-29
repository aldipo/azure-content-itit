<properties
   pageTitle="Creare un probe personalizzato per il gateway applicazione usando PowerShell nel modello di distribuzione classica | Microsoft Azure"
   description="Informazioni su come creare un probe personalizzato per il gateway applicazione usando PowerShell nel modello di distribuzione classica"
   services="application-gateway"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="joaoma" />

# Creare un probe personalizzato per il gateway applicazione di Azure (classico) con PowerShell


[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)].

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)] Informazioni su come [eseguire questa procedura con il modello di Resource Manager](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## Creare un nuovo gateway applicazione

Per creare un gateway applicazione:

1. Creare una risorsa del gateway applicazione.
2. Creare un file XML di configurazione o un oggetto di configurazione.
3. Eseguire il commit della configurazione nella risorsa del gateway applicazione appena creata.

### Creare una risorsa del gateway applicazione

Per creare il gateway, usare il cmdlet **New-AzureApplicationGateway**, sostituendo i valori esistenti con quelli personalizzati. Si noti che la fatturazione per il gateway non viene applicata a partire da questo punto. La fatturazione viene applicata a partire da un passaggio successivo, dopo l'avvio corretto del gateway.

L'esempio seguente mostra come creare un nuovo gateway applicazione usando una rete virtuale denominata "testvnet1" e una subnet denominata "subnet-1".


	PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

	VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
	VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Description*, *InstanceCount* e *GatewaySize* sono parametri facoltativi.


Per convalidare la creazione del gateway, è possibile usare il cmdlet **Get-AzureApplicationGateway**.


	PS C:\> Get-AzureApplicationGateway AppGwTest
	Name          : AppGwTest
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Stopped
	VirtualIPs    : {}
	DnsName       :

>[AZURE.NOTE]  Il valore predefinito per *InstanceCount* è 2, con un valore massimo pari a 10. Il valore predefinito per *GatewaySize* è Medium. È possibile scegliere tra Small, Medium e Large.


 *VirtualIPs* e *DnsName* vengono visualizzati vuoti perché il gateway non è stato ancora avviato. Questi valori verranno creati quando il gateway sarà in esecuzione.

## Configurare un gateway applicazione

È possibile configurare il gateway applicazione usando XML o un oggetto di configurazione.

## Configurare un gateway applicazione usando XML

Nell'esempio seguente viene usato un file XML per configurare tutte le impostazioni del gateway applicazione ed eseguirne il commit nella risorsa del gateway applicazione.

### Passaggio 1  

Copiare il testo seguente in Blocco note.


	<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
	<FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
				<IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
	    <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
	</ApplicationGatewayConfiguration>


Modificare i valori tra parentesi per gli elementi di configurazione. Salvare il file con estensione XML.

L'esempio seguente mostra come usare un file di configurazione per impostare il gateway applicazione per il bilanciamento del carico del traffico HTTP sulla porta pubblica 80 e inviare il traffico di rete alla porta back-end 80 tra due indirizzi IP usando un probe personalizzato.

>[AZURE.IMPORTANT] L'elemento del protocollo HTTP o HTTPS deve rispettare la distinzione tra maiuscole e minuscole.


Viene aggiunto un nuovo elemento di configurazione <Probe> per configurare i probe personalizzati.

I parametri di configurazione sono:

- **Name**: nome di riferimento per il probe personalizzato.
- **Protocol**: protocollo usato (i valori possibili sono HTTP o HTTPS).
- **Host** e **Path**: percorso URL completo richiamato dal gateway applicazione per determinare l'integrità dell'istanza. Se, ad esempio, si ha un sito Web http://contoso.com/, il probe personalizzato può essere configurato per "http://contoso.com/path/custompath.htm" in modo che i controlli del probe ottengano una risposta HTTP corretta.
- **Interval**: configura i controlli dell'intervallo di probe in secondi.
- **Timeout**: definisce il timeout del probe per un controllo della risposta HTTP.
- **UnhealthyThreshold**: numero di risposte HTTP non riuscite necessario per contrassegnare l'istanza back-end come *non integra*.

Si fa riferimento al nome del probe nella configurazione <BackendHttpSettings> per assegnare il pool back-end che userà le impostazioni di probe personalizzato.

## Aggiungere una configurazione di probe personalizzata a un gateway applicazione esistente

Per modificare la configurazione corrente di un gateway applicazione sono necessari tre passaggi: ottenere il file di configurazione XML corrente, apportare modifiche per avere un probe personalizzato e configurare il gateway applicazione con le nuove impostazioni XML.

### Passaggio 1

Ottenere il file XML usando get-AzureApplicationGatewayConfig. Il file XML di configurazione verrà esportato e potrà essere modificato per aggiungere un'impostazione di probe.

	get-AzureApplicationGatewayConfig -Name <application gateway name> -Exporttofile "<path to file>"


### Passaggio 2

Aprire il file XML in un editor di testo. Aggiungere una sezione `<probe>` dopo `<frontendport>`.

	<Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>

Nella sezione backendHttpSettings del file XML aggiungere il nome del probe, come nell'esempio seguente:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Salvare il file XML.


### Passaggio 3

Aggiornare la configurazione del gateway applicazione con il nuovo file XML usando **Set-AzureApplicationGatewayConfig**. Il gateway applicazione verrà aggiornato con la nuova configurazione.

	set-AzureApplicationGatewayConfig -Name <application gateway name> -Configfile "<path to file>"


## Passaggi successivi

Per configurare l'offload SSL (Secure Sockets Layer), vedere [Configurare un gateway applicazione per l'offload SSL](application-gateway-ssl.md).

Per configurare un gateway applicazione da usare con il servizio di bilanciamento del carico interno, vedere [Creare un gateway applicazione con un servizio di bilanciamento del carico interno (ILB)](application-gateway-ilb.md).

<!---HONumber=AcomDC_0629_2016-->