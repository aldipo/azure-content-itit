<properties 
	pageTitle="Scalabilità distribuita a livello geografico con ambienti del servizio app" 
	description="Informazioni sulla scalabilità orizzontale delle app mediante la distribuzione a livello geografico con Gestione traffico e ambienti del servizio app" 
	services="app-service" 
	documentationCenter="" 
	authors="stefsch" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/21/2016" 
	ms.author="stefsch"/>

# Scalabilità distribuita a livello geografico con ambienti del servizio app

## Panoramica ##
Gli scenari di applicazioni che richiedono una scalabilità molto elevata possono superare la capacità delle risorse di calcolo disponibili per una singola distribuzione di un'app. Applicazioni di voto, eventi sportivi ed eventi di intrattenimento televisivi sono tutti esempi di scenari che richiedono una scalabilità estremamente elevata. I requisiti di scalabilità elevata possono essere soddisfatti con la scalabilità orizzontale delle applicazioni, con più distribuzioni di app eseguite in una singola area geografica, oltre che in diverse aree, per gestire i requisiti di carico particolarmente intenso.

Gli ambienti del servizio app sono una piattaforma ideale per la scalabilità orizzontale. Una volta selezionata una configurazione di Ambiente del servizio app che possa supportare una frequenza di richieste nota, gli sviluppatori possono distribuire altre istanze di ambienti del servizio app usando modelli di progetto (o "cookiecutter") per ottenere una capacità di carico di picco desiderata.

Si supponga ad esempio che un'app eseguita in una configurazione di Ambiente del servizio app sia stata testata per la gestione di 20 KB di richieste al secondo (RPS). Se la capacità di carico di picco desiderata è pari a 100 KB RPS, è possibile creare cinque (5) ambienti del servizio app per assicurare che l'applicazione possa gestire il carico massimo previsto.

Poiché in genere i clienti accedono alle app usando un dominio personalizzato, o personale, gli sviluppatori hanno bisogno di un modo per distribuire le richieste di app tra tutte le istanze di Ambiente del servizio app. Un modo ottimale per ottenere questo risultato consiste nel risolvere il dominio personalizzato tramite un [profilo di Gestione traffico di Azure][AzureTrafficManagerProfile]. Il profilo di Gestione traffico può essere configurato in modo che punti a tutti i singoli ambienti del servizio app. Gestione traffico gestirà automaticamente la distribuzione dei clienti tra tutti gli ambienti del servizio app in base alle impostazioni di bilanciamento del carico nel profilo di Gestione traffico. Questo approccio funziona indipendentemente dal fatto che tutti gli ambienti del servizio app siano distribuiti in una singola area di Azure o a livello mondiale tra più aree di Azure.

Inoltre, poiché i clienti accedono alle app tramite il dominio personale, non conoscono il numero di ambienti del servizio app che eseguono un'app. Di conseguenza, gli sviluppatori possono aggiungere e rimuovere in modo rapido e facile ambienti del servizio app in base al carico di traffico osservato.

Il diagramma concettuale seguente illustra un'app con scalabilità orizzontale fra tre ambienti del servizio app in una singola area geografica.

![Architettura concettuale][ConceptualArchitecture]

Il resto di questo argomento illustra in dettaglio la procedura relativa alla configurazione di una topologia distribuita per l'app di esempio usando più ambienti del servizio app.

## Pianificazione della topologia ##
Prima di compilare il footprint di un'app distribuita, è utile avere anticipatamente alcune informazioni.

- **Dominio personalizzato per l'app**: definire qual è il nome del dominio personalizzato che i clienti useranno per accedere all'app. Per l'app di esempio il nome di dominio personalizzato è *www.scalableasedemo.com*
- **Dominio di Gestione traffico**: è necessario scegliere un nome di dominio quando si crea un [profilo di gestione traffico di Azure][AzureTrafficManagerProfile]. Questo nome sarà combinato con il suffisso *trafficmanager.net* per registrare una voce di dominio gestita da Gestione traffico. Per l'app di esempio il nome scelto è *scalable-ase-demo*. Il nome di dominio completo gestito da Gestione traffico sarà quindi *scalable-ase-demo.trafficmanager.net*.
- **Strategia per la scalabilità del footprint dell'app**: definire se il footprint dell'applicazione sarà distribuito tra più ambienti del servizio app in una singola area geografica, in più aree geografiche o con una combinazione di entrambi gli approcci. La decisione dovrà essere basata sulla previsione dell'origine del traffico dei clienti, oltre che sull'efficacia della scalabilità del resto dell'infrastruttura di back-end di supporto di un'app. Ad esempio, un'applicazione al 100% senza stato può essere configurata in modo altamente scalabile usando una combinazione di più ambienti del servizio app per area di Azure. moltiplicati per gli ambienti del servizio app distribuiti tra più aree di Azure. Con la disponibilità di oltre 15 aree di Azure pubbliche da cui scegliere, i clienti possono veramente creare un footprint dell'applicazione iperscalabile in tutto il mondo. Per l'app di esempio usata per questo articolo sono stati creati tre ambienti del servizio app in una singola area di Azure (Stati Uniti centro-meridionali).
- **Convenzione di denominazione per gli ambienti del servizio app**: ogni ambiente del servizio app richiede un nome univoco. Oltre a uno o due ambienti del servizio app, è utile avere una convenzione di denominazione per facilitare l'identificazione di ogni ambiente del servizio app. Per l'app di esempio è stata usata una convenzione di denominazione semplice. I nomi dei tre ambienti del servizio app sono *fe1ase*, *fe2ase* e *fe3ase*.
- **Convenzione di denominazione per le app**: poiché saranno distribuite più istanze dell'app, è necessario un nome per ogni istanza dell'app distribuita. Una funzionalità poco conosciuta ma molto pratica degli ambienti del servizio app è la possibilità di usare lo stesso nome tra più ambienti. Poiché ogni ambiente del servizio app ha un suffisso univoco, gli sviluppatori possono scegliere di riutilizzare esattamente lo stesso nome dell'app in ogni ambiente. Ad esempio, uno sviluppatore può avere app denominate come segue: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net* e così via. Per l'app di esempio ogni istanza dell'app ha tuttavia anche un nome univoco. I nomi delle istanze dell'app usati sono *webfrontend1*, *webfrontend2* e *webfrontend3*.


## Configurazione del profilo di Gestione traffico ##
Dopo la distribuzione di più istanze di un'app in più ambienti del servizio app, è possibile registrare le singole istanze con Gestione traffico. Per l'app di esempio è necessario un profilo di Gestione traffico per *scalable-ase-demo.trafficmanager.net* che possa indirizzare i clienti a una qualsiasi delle istanze seguenti dell'app distribuita:

- **webfrontend1.fe1ase.p.azurewebsites.net**: istanza dell'app di esempio distribuita nel primo ambiente del servizio app.
- **webfrontend2.fe2ase.p.azurewebsites.net**: istanza dell'app di esempio distribuita nel secondo ambiente del servizio app.
- **webfrontend3.fe3ase.p.azurewebsites.net**: istanza dell'app di esempio distribuita nel terzo ambiente del servizio app.

Il modo più semplice per registrare più endpoint del Servizio app di Azure, tutti in esecuzione nella **stessa** area di Azure, consiste nell'usare [Supporto di Gestione risorse di Azure per la versione di anteprima di Gestione traffico di Azure][ARMTrafficManager] con PowerShell.

Come primo passaggio creare un profilo di Gestione traffico di Azure. Il codice seguente illustra come è stato creato il profilo per l'app di esempio:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Si noti che il parametro *RelativeDnsName* è stato impostato su *scalable-ase-demo*. È così che il nome di dominio *scalable-ase-demo.trafficmanager.net* viene creato e associato al profilo di Gestione traffico.

Il parametro *TrafficRoutingMethod* definisce i criteri di bilanciamento del carico che saranno usati da Gestione traffico per determinare come distribuire il carico del cliente tra tutti gli endpoint disponibili. In questo esempio è stato usato il metodo *Weighted*. Le richieste del cliente saranno quindi distribuite tra tutti gli endpoint dell'applicazione registrata in base ai pesi associati a ogni endpoint.

Una volta creato il profilo, ogni istanza dell'app viene aggiunta al profilo come *endpoint esterno*. Il codice seguente illustra gli URL per ognuna delle tre istanze dell'app aggiunte al profilo.

    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type ExternalEndpoints –Target webfrontend1.fe1ase.p.azurewebsites.net –EndpointStatus Enabled –Weight 10
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type ExternalEndpoints –Target webfrontend2.fe2ase.p.azurewebsites.net –EndpointStatus Enabled –Weight 10
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type ExternalEndpoints –Target webfrontend3.fe3ase.p.azurewebsites.net –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

Come si noterà, è presente una chiamata a *Add-AzureTrafficManagerEndpointConfig* per ogni singola istanza dell'app. Il parametro *Target* in ogni comando di PowerShell punta al nome di dominio completo (FQDN) di ognuna delle tre istanze dell'app distribuita. I diversi FQDN sono i valori che saranno usati per esaminare la catena di CNAME DNS e individuare *scalable-ase-demo.trafficmanager.net*, in modo da distribuire il carico di traffico tra tutti gli endpoint registrati nel profilo di Gestione traffico.

Tutti e tre gli endpoint usano lo stesso valore (10) per il parametro *Weight*. Gestione traffico distribuirà quindi le richieste dei clienti tra tutte e tre le istanze dell'app in modo relativamente uniforme.

*Nota:* poiché il supporto di Gestione risorse di Azure per Gestione traffico è attualmente in versione di anteprima, è necessario che per gli endpoint del Servizio app di Azure il parametro *Type* sia impostato su *ExternalEndpoints*. In futuro gli endpoint del Servizio app di Azure saranno supportati in modalità nativa come tipo di endpoint dalla variante Gestione risorse di Azure di Gestione traffico.

## Indirizzamento del dominio personalizzato dell'app al dominio di Gestione traffico ##
Come passaggio finale è necessario che il dominio personalizzato dell'app punti al dominio di Gestione traffico. Per l'app di esempio *www.scalableasedemo.com* deve puntare a *scalable-ase-demo.trafficmanager.net*. Questo passaggio deve essere completato con il registrar che gestisce il dominio personalizzato.

Usando gli strumenti di gestione del dominio del registrar, è necessario creare un record CNAME che dal dominio personalizzato punti al dominio di Gestione traffico. L'immagine seguente mostra un esempio di come appare la configurazione di questo CNAME:

![CNAME per il dominio personalizzato][CNAMEforCustomDomain]

Anche se questo aspetto non viene trattato in questo argomento, tenere presente che per ogni singola istanza dell'app deve essere registrato anche il dominio personalizzato. In caso contrario, se una richiesta raggiunge un'istanza dell'app e per l'app non è stato registrato il dominio personalizzato, la richiesta non riuscirà.

In questo esempio il dominio personalizzato è *www.scalableasedemo.com* e a ogni istanza dell'applicazione è associato il dominio personalizzato.

![Dominio personalizzato][CustomDomain]

Per un riepilogo della procedura di registrazione di un dominio personalizzato con le app del Servizio app di Azure, vedere l'articolo seguente relativo alla [registrazione di domini personalizzati][RegisterCustomDomain].

## Prova della topologia distribuita ##
Come risultato finale della configurazione di Gestione traffico e del DNS, il flusso delle richieste per *www.scalableasedemo.com* avviene con la sequenza seguente:

1. Un browser o un dispositivo esegue una ricerca DNS di *www.scalableasedemo.com*
2. La voce CNAME del registrar causa il reindirizzamento della ricerca DNS a Gestione traffico di Azure.
3. Viene eseguita una ricerca DNS di *scalable-ase-demo.trafficmanager.net* in uno dei server DNS di Gestione traffico di Azure.
4. In base ai criteri di bilanciamento del carico, ovvero il parametro *TrafficRoutingMethod* usato in precedenza durante la creazione del profilo di Gestione traffico, Gestione traffico sceglierà uno degli endpoint configurati e restituirà l'FQDN dell'endpoint al browser o al dispositivo.
5.  Poiché l'FQDN dell'endpoint è l'URL di un'istanza dell'app in esecuzione in un ambiente del servizio app, il browser o il dispositivo chiederà a un server DNS di Microsoft Azure di risolvere l'FQDN in un indirizzo IP. 
6. Il browser o il dispositivo invierà la richiesta HTTP/S all'indirizzo IP.  
7. La richiesta arriverà a una delle istanze dell'app in esecuzione in uno degli ambienti del servizio app.

L'immagine della console seguente mostra una ricerca DNS del dominio personalizzato dell'app di esempio risolta correttamente in un'istanza dell'app eseguita in uno dei tre ambienti del servizio app di esempio, in questo caso il secondo dei tre ambienti del servizio app:

![Ricerca DNS][DNSLookup]

## Informazioni e collegamenti aggiuntivi ##
Tutti gli articoli e le procedure sugli ambienti del servizio app sono disponibili nel [File LEGGIMI per gli ambienti di servizio dell'applicazione](../app-service/app-service-app-service-environments-readme.md).

Documentazione sul [Supporto di Gestione risorse di Azure per la versione di anteprima di Gestione traffico di Azure][ARMTrafficManager] con PowerShell.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]: https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]: https://azure.microsoft.com/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]: ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]: ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]: ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png

<!---HONumber=AcomDC_0622_2016-->