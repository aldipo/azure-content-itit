<properties
	pageTitle="Distribuzioni approvate di Linux | Microsoft Azure"
	description="Informazioni sulle distribuzioni di Linux approvate in Azure, che includono le linee guida per Ubuntu, OpenLogic, Oracle e SUSE."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="szarkos"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management,azure-resource-manager"
	/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/25/2016"
	ms.author="szark"/>



#Distribuzioni di Linux supportate da Azure

Le immagini Linux disponibili nella raccolta di Azure o in Marketplace sono fornite da diversi partner e Microsoft collabora con diverse community Linux per aggiungere altre versioni all'elenco delle distribuzioni supportate. Nel frattempo, per le distribuzioni non disponibili nella raccolta, è possibile usare la propria distribuzione di Linux, attenendosi alle indicazioni disponibili in [questa pagina](virtual-machines-linux-classic-create-upload-vhd.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## Versioni e distribuzioni supportate ##

Nella tabella seguente sono elencate le distribuzioni e versioni di Linux supportate in Azure.

I driver di Linux Integration Services (LIS) per Hyper-V e Azure sono moduli kernel forniti da Microsoft direttamente nel kernel Linux upstream. I driver LIS sono compilati nel kernel della distribuzione per impostazione predefinita oppure, per le distribuzioni basate su RHEL/CentOS precedenti, sono disponibili come download separato [qui](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). Per ulteriori informazioni sui driver LIS, consultare [questo articolo](virtual-machines-linux-create-upload-generic.md#linux-kernel-requirements).

L'agente Linux di Azure è già preinstallato nelle immagini della raccolta di Azure ed è in genere disponibile dal repository del pacchetto della distribuzione. Il codice sorgente è disponibile su [GitHub](https://github.com/azure/walinuxagent).

Distribuzione|Versione|Driver|Agente
---|---|---|---
CentOS di OpenLogic |CentOS 6.3+, 7.0+| CentOS 6.3: [LIS Download](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: nel Kernel|Pacchetto: in <a href="http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/">OpenLogic repo sotto "WALinuxAgent"<p><p>Codice sorgente: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/)|494\.4.0+ |Nel kernel|Codice sorgente: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Debian da Credativ |Debian 7,9 +, 8.2 +|Nel kernel|Pacchetto: in repo sotto "waagent"<p><p>Codice sorgente: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux| 6\.4+, 7.0+|Nel kernel|Pacchetto: in repo sotto "WALinuxAgent"<p><p>Codice sorgente: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Red Hat Enterprise Linux. |RHEL 6.7+, 7.1+|Nel kernel|Pacchetto: in repo sotto "WALinuxAgent" <p><p>Codice sorgente: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise |SLES 11 SP3+, SLES 12+ e <p><p> SLES per SAP 11.3+ |Nel kernel|Pacchetto: in repo [Cloud:Strumenti](https://build.opensuse.org/project/show/Cloud:Tools) sotto "WALinuxAgent"<p><p>Codice sorgente: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE |openSUSE 13.1+|Nel kernel|Pacchetto: in repo [Cloud:Strumenti](https://build.opensuse.org/project/show/Cloud:Tools) sotto "WALinuxAgent" <p><p>Codice sorgente: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04, 14.04, 15.10 e 16.04|Nel kernel|Pacchetto: in repo sotto "walinuxagent" <p><p>Codice sorgente: [GitHub](https://github.com/Azure/WALinuxAgent)
## Partner

### OpenLogic
[http://www.openlogic.com/azure](http://www.openlogic.com/azure)

OpenLogic è uno dei principali fornitori di soluzioni aziendali open source per il cloud e per data center. OpenLogic aiuta centinaia di aziende leader di vari settori di mercato ad acquisire, supportare e controllare software open source. OpenLogic offre supporto tecnico a livello commerciale e risarcimenti per 600 pacchetti open supportati dalla community di esperti di OpenLogic, incluso supporto a livello aziendale per CentOS. È inoltre il partner di avvio per la fornitura di immagini basate su CentOS in Azure.

### CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Dal sito Web CoreOS:

*CoreOS è progettato per la protezione, l’uniformità e l’affidabilità. Anziché eseguire l'installazione dei pacchetti tramite yum o apt, CoreOS utilizza i contenitori di Linux per gestire i servizi a un livello superiore di astrazione. Il codice di un servizio singolo e tutte le dipendenze vengono inserite in un contenitore che può essere eseguito su uno o più macchine CoreOS.*


### Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ è una società indipendente di servizi e consulenza, specializzata nello sviluppo e nell'implementazione di soluzioni professionali tramite l'utilizzo di software gratuito. Come azienda leader nell'open source, Credativ ha ottenuto un riconoscimento internazionale, con molti reparti IT che ricorrono ai suoi servizi di assistenza. In collaborazione con Microsoft, Credativ sta attualmente preparando immagini Debian corrispondenti per Debian 8 (Jessie) e Debian precedente a 7 (Wheezy), appositamente progettate per l'esecuzione in Azure e facilmente gestibili tramite la piattaforma. Credativ supporta anche la manutenzione a lungo termine e l'aggiornamento delle immagini Debian per Azure tramite i centri di supporto open source.

### Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

La strategia di Oracle consiste nell'offrire una vasta gamma di soluzioni per cloud pubblici e privati, offrendo al tempo stesso ai clienti la possibilità di scelta e la flessibilità necessarie per determinare la modalità di distribuzione di software Oracle nel cloud Oracle e in cloud di altro tipo. La collaborazione di Oracle con Microsoft permette ai clienti di distribuire il software Oracle nei cloud Microsoft pubblici e privati, con la sicurezza offerta dalla certificazione e dal supporto Oracle. L'impegno e gli investimenti di Oracle in ambito di soluzioni per cloud Oracle pubblico e privato rimangono invariati.

### Red Hat
[http://www.redhat.com/en/partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Fornitore mondiale di soluzioni open source, Red Hat aiuta oltre il 90% delle società Fortune 500 a risolvere sfide aziendali, allineare le strategie IT a quelle aziendali e prepararsi per la tecnologia del futuro. Per raggiungere questi obiettivi, Red Hat fornisce soluzioni sicure tramite un modello di business aperto e uno schema di sottoscrizioni conveniente e prevedibile.

### SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server su Azure è una piattaforma collaudata che offre un'affidabilità e una sicurezza superiori per il cloud computing. La piattaforma Linux versatile di SUSE consente l'integrazione lineare con i servizi cloud di Azure per offrire un ambiente cloud facilmente gestibile. Grazie alle oltre 9.200 applicazioni certificate create da oltre 1.800 fornitori indipendenti di software per SUSE Linux Enterprise Server, SUSE è in grado di assicurare che i carichi di lavoro in esecuzione supportati nei data center possano essere distribuiti con sicurezza su Azure.

### Canonical
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

Le competenze di Canonical in ambito di engineering e le regole di governance della community aperta sono alla base del successo di Ubuntu nelle tecnologie per client, server e cloud computing, inclusi servizi cloud personali per i consumatori. La visione di Canonical per una piattaforma gratuita unificata in Ubuntu, dal telefono al cloud, con una serie di interfacce coerenti per telefono, tablet, TV e computer desktop, rende Ubuntu la scelta ottimale per enti di diverso tipo, dai provider di cloud pubblico ai produttori di dispositivi elettronici per consumatori, oltre ai singoli esperti di IT.

Grazie a centri per lo sviluppo e l'engineering in tutto il mondo, Canonical si trova in una posizione privilegiata ed è pertanto in grado di collaborare con produttori di hardware e sviluppatori di software, in modo da portare le soluzioni Ubuntu sul mercato, da PC a server e dispositivi palmari.

<!---HONumber=AcomDC_0330_2016-->