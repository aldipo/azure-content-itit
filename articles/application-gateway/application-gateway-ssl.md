<properties
   pageTitle="Configurare un gateway applicazione per l'offload SSL tramite la distribuzione classica | Microsoft Azure"
   description="Questo articolo fornisce istruzioni per la creazione di un gateway applicazione con offload SSL tramite il modello di distribuzione classica di Azure."
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="joaoma"/>

# Configurare un gateway applicazione per l'offload SSL tramite il modello di distribuzione classica

> [AZURE.SELECTOR]
-[Azure Classic PowerShell](application-gateway-ssl.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)

Il gateway applicazione di Azure può essere configurato per terminare la sessione Secure Sockets Layer (SSL) nel gateway ed evitare costose attività di decrittografia SSL nella Web farm. L'offload SSL semplifica anche la configurazione e la gestione del server front-end dell'applicazione Web.


## Prima di iniziare

1. Installare la versione più recente dei cmdlet di Azure PowerShell usando l'Installazione guidata piattaforma Web. È possibile scaricare e installare la versione più recente dalla sezione **Windows PowerShell** della [Pagina di download](https://azure.microsoft.com/downloads/).
2. Assicurarsi di avere una rete virtuale funzionante con una subnet valida. Assicurarsi che nessuna macchina virtuale o distribuzione cloud stia usando la subnet. Il gateway applicazione deve essere da solo in una subnet di rete virtuale.
3. I server che verranno configurati per l'uso del gateway applicazione devono esistere. In alternativa, è necessario creare i relativi endpoint nella rete virtuale o assegnato loro un indirizzo IP/VIP pubblico.

Per configurare l'offload SSL in un gateway applicazione, eseguire i passaggi seguenti nell'ordine elencato.

1. [Creare un nuovo gateway applicazione](#create-a-new-application-gateway)
2. [Caricare i certificati SSL](#upload-ssl-certificates)
3. [Configurare il gateway](#configure-the-gateway)
4. [Definire la configurazione del gateway](#set-the-gateway-configuration)
5. [Avviare il gateway](#start-the-gateway)
6. [Verificare lo stato del gateway](#verify-the-gateway-status)


## Creare un nuovo gateway applicazione

Per creare il gateway, usare il cmdlet **New-AzureApplicationGateway**, sostituendo i valori esistenti con quelli personalizzati. Si noti che la fatturazione per il gateway non viene applicata a partire da questo punto. La fatturazione viene applicata a partire da un passaggio successivo, dopo l'avvio corretto del gateway.

Questo esempio illustra il cmdlet nella prima riga seguito dall'output.

	PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

	VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
	VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

Per convalidare la creazione del gateway, è possibile usare il cmdlet **Get-AzureApplicationGateway**.

In questo esempio, *Description*, *InstanceCount* e *GatewaySize* sono parametri facoltativi. Il valore predefinito per *InstanceCount* è 2, con un valore massimo di 10. Il valore predefinito per *GatewaySize* è Medium. Small e Large sono altri valori disponibili. *VirtualIPs* e *DnsName* vengono visualizzati vuoti perché il gateway non è stato ancora avviato. Questi valori verranno creati quando il gateway sarà in esecuzione.

Questo esempio illustra il cmdlet nella prima riga seguito dall'output.

	PS C:\> Get-AzureApplicationGateway AppGwTest

	VERBOSE: 4:39:39 PM - Begin Operation:
	Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed
	Operation: Get-AzureApplicationGateway
	Name: AppGwTest
	Description:
	VnetName: testvnet1
	Subnets: {Subnet-1}
	InstanceCount: 2
	GatewaySize: Medium
	State: Stopped
	VirtualIPs:
	DnsName:


## Caricare i certificati SSL

Usare **Add-AzureApplicationGatewaySslCertificate** per caricare il certificato del server in formato *pfx* nel gateway applicazione. Il nome del certificato è un nome scelto dall'utente e deve essere univoco all'interno del gateway applicazione. Il certificato viene identificato da questo nome in tutte le operazioni di gestione del certificato nel gateway applicazione.

Questo esempio illustra il cmdlet nella prima riga seguito dall'output. Sostituire i valori usati dell'esempio con quelli desiderati.

	PS C:\> Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

	VERBOSE: 5:05:23 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
	VERBOSE: 5:06:29 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   21fdc5a0-3bf7-2c12-ad98-192e0dd078ef

Successivamente, convalidare il caricamento del certificato. Usare il cmdlet **Get-AzureApplicationGatewayCertificate**.

Questo esempio illustra il cmdlet nella prima riga seguito dall'output.

	PS C:\> Get-AzureApplicationGatewaySslCertificate AppGwTest

	VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
	VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
	Name           : SslCert
	SubjectName    : CN=gwcert.app.test.contoso.com
	Thumbprint     : AF5ADD77E160A01A6......EE48D1A
	ThumbprintAlgo : sha1RSA
	State..........: Provisioned


## Configurare il gateway

La configurazione di un gateway applicazione è costituita da più valori. È possibile collegare tra loro tali valori per creare la configurazione.

I valori possibili sono:

- **Pool di server back-end:** elenco di indirizzi IP dei server back-end. Gli indirizzi IP elencati devono appartenere alla subnet della rete virtuale o devono essere indirizzi IP/VIP pubblici.
- **Impostazioni del pool di server back-end:** ogni pool ha impostazioni quali porta, protocollo e affinità basata sui cookie. Queste impostazioni sono associate a un pool e vengono applicate a tutti i server nel pool.
- **Porta front-end:** porta pubblica aperta sul gateway applicazione. Il traffico raggiunge questa porta e quindi viene reindirizzato a uno dei server back-end.
- **Listener**: ha una porta front-end, un protocollo (HTTP o HTTPS che fa distinzione tra maiuscole e minuscole) e il nome del certificato SSL (se si configura l'offload SSL).
- **Regola**: associa il listener e il pool di server back-end e definisce il pool di server back-end a cui deve essere indirizzato il traffico quando raggiunge un listener specifico. È attualmente supportata solo la regola *basic*. La regola *basic* è una distribuzione del carico di tipo round robin.

**Note aggiuntive sulla configurazione**

Per la configurazione dei certificati SSL, il protocollo in **HttpListener** deve essere sostituito con *Https* (distinzione tra maiuscole e minuscole). L'elemento **SslCert** deve essere aggiunto a **HttpListener** con il valore impostato sullo stesso nome usato nella sezione precedente sul caricamento dei certificati SSL. La porta front-end deve essere impostata su 443.

**Per abilitare l'affinità basata sui cookie**: è possibile configurare un gateway applicazione per fare in modo che una richiesta proveniente da una sessione client sia sempre diretta alla stessa macchina virtuale nella Web farm. A tale scopo viene aggiunto un cookie di sessione che consente al gateway di indirizzare il traffico in modo appropriato. Per abilitare l'affinità basata sui cookie, impostare **CookieBasedAffinity** su *Enabled* nell'elemento **BackendHttpSettings**.



È possibile definire la configurazione creando un oggetto di configurazione oppure usando un file XML di configurazione. Per definire la configurazione mediante un file XML di configurazione, usare l'esempio seguente.

**Esempio di file XML di configurazione**


	    <?xml version="1.0" encoding="utf-8"?>
	<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
	    <FrontendIPConfigurations />
	    <FrontendPorts>
	        <FrontendPort>
	            <Name>FrontendPort1</Name>
	            <Port>443</Port>
	        </FrontendPort>
	    </FrontendPorts>
	    <BackendAddressPools>
	        <BackendAddressPool>
	            <Name>BackendPool1</Name>
	            <IPAddresses>
	                <IPAddress>10.0.0.1</IPAddress>
	                <IPAddress>10.0.0.2</IPAddress>
	            </IPAddresses>
	        </BackendAddressPool>
	    </BackendAddressPools>
	    <BackendHttpSettingsList>
	        <BackendHttpSettings>
	            <Name>BackendSetting1</Name>
	            <Port>80</Port>
	            <Protocol>Http</Protocol>
	            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
	        </BackendHttpSettings>
	    </BackendHttpSettingsList>
	    <HttpListeners>
	        <HttpListener>
	            <Name>HTTPListener1</Name>
	            <FrontendPort>FrontendPort1</FrontendPort>
	            <Protocol>Https</Protocol>
	            <SslCert>GWCert</SslCert>
	        </HttpListener>
	    </HttpListeners>
	    <HttpLoadBalancingRules>
	        <HttpLoadBalancingRule>
	            <Name>HttpLBRule1</Name>
	            <Type>basic</Type>
	            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
	            <Listener>HTTPListener1</Listener>
	            <BackendAddressPool>BackendPool1</BackendAddressPool>
	        </HttpLoadBalancingRule>
	    </HttpLoadBalancingRules>
	</ApplicationGatewayConfiguration>


## Definire la configurazione del gateway

Verrà quindi configurato il gateway applicazione. È possibile usare il cmdlet **Set-AzureApplicationGatewayConfig** con un oggetto di configurazione o con un file XML di configurazione.


	PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

	VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
	VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## Avviare il gateway

Dopo aver configurato il gateway, usare il cmdlet **Start-AzureApplicationGateway** per avviarlo. La fatturazione per un gateway applicazione verrà applicata a partire dall'avvio corretto del gateway.


**Nota:** il cmdlet **Start-AzureApplicationGateway** potrebbe impiegare fino a 15-20 minuti.


	PS C:\> Start-AzureApplicationGateway AppGwTest

	VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
	VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
	Name       HTTP Status Code     Operation ID                             Error
	----       ----------------     ------------                             ----
	Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b


## Verificare lo stato del gateway

Usare il cmdlet **Get-AzureApplicationGateway** per controllare lo stato del gateway. Se nel passaggio precedente l'operazione **Start-AzureApplicationGateway** è riuscita, lo *stato* risulterà In esecuzione e i valori di *VirtualIPs* e *DnsName* dovrebbero essere validi.

Questo esempio illustra un gateway applicazione attivo, in esecuzione e pronto per accettare il traffico

	PS C:\> Get-AzureApplicationGateway AppGwTest

	Name          : AppGwTest2
	Description   :
	VnetName      : testvnet1
	Subnets       : {Subnet-1}
	InstanceCount : 2
	GatewaySize   : Medium
	State         : Running
	VirtualIPs    : {23.96.22.241}
	DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## Passaggi successivi


Per altre informazioni generali sulle opzioni di bilanciamento del carico, vedere:

- [Servizio di bilanciamento del carico di Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Gestione traffico di Azure](https://azure.microsoft.com/documentation/services/traffic-manager/)

<!---HONumber=AcomDC_0218_2016-->