<properties 
   pageTitle="Specifica le impostazioni DNS in un File di configurazione del servizio | Microsoft Azure"
   description="Specificare le impostazioni DNS personalizzate utilizzando il file di configurazione del servizio per la rete virtuale"
   services="virtual-network"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/24/2016"
   ms.author="joaoma" />

# Specifica le impostazioni DNS in un File di configurazione del servizio

## Elemento DNS

Un file di configurazione del servizio può contenere un elemento DnsServers con un elenco di indirizzi IPv4 per i server Domain Name System (DNS) che verrà utilizzato dal servizio. Le impostazioni nel file di configurazione del servizio hanno la precedenza sulle impostazioni nel file di configurazione di rete. Per altre informazioni, vedere [Schema di configurazione dei servizi di Azure (con estensione .cscfg)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**Elemento NetworkConfiguration**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Il**nome**dell’attributo nell’elemento **DnsServer**viene utilizzato solo come nome di riferimento. Non rappresenta il nome host per il server DNS. Ogni valore dell’attributo**DnsServer**deve essere univoco nell'intera sottoscrizione Microsoft Azure

## Vedere anche

[Schema di configurazione dei servizi di Azure (file .cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Attività di configurazione di Rete virtuale di Azure](http://go.microsoft.com/fwlink/?LinkId=248093)

[Configurare una rete virtuale usando un file di configurazione di rete](http://go.microsoft.com/fwlink/?LinkId=248094)

[Informazioni sulle impostazioni della rete virtuale nel portale di gestione](http://go.microsoft.com/fwlink/?LinkId=248092)

<!---HONumber=AcomDC_0302_2016-->