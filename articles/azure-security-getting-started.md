<properties
   pageTitle="Introduzione alla sicurezza in Microsoft Azure | Microsoft Azure"
   description="Questo articolo offre una panoramica delle funzionalità di sicurezza di Microsoft Azure e considerazioni generali per le organizzazioni che intendono eseguire la migrazione degli asset a un provider di servizi cloud."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="yuridio"/>

#Introduzione alla sicurezza in Microsoft Azure

Quando si compilano asset IT o se ne esegue la migrazione in un provider di servizi cloud, si dipende dalla capacità di tale organizzazione di proteggere le applicazioni e i dati affidati ai suoi servizi e dai controlli di sicurezza forniti per controllare la sicurezza degli asset basati sul cloud.

L'infrastruttura di Azure è stata progettata, dalla struttura fino alle applicazioni, per ospitare milioni di clienti contemporaneamente e fornire alle aziende una solida base per poter soddisfare le esigenze di sicurezza. Azure offre anche un'ampia gamma di opzioni di sicurezza configurabili, con la possibilità di controllarle per poter personalizzare la sicurezza e soddisfare così i requisiti univoci di ogni distribuzione.

In questa panoramica sulla sicurezza in Azure si esamineranno:

-   Servizi e funzionalità di Azure che è possibile usare per proteggere i servizi e i dati in Azure

-   Difesa dell'infrastruttura di Azure da parte di Microsoft per proteggere i dati e le applicazioni

##Gestione delle identità e dell'accesso


Controllare l'accesso all'infrastruttura IT, alle applicazioni e ai dati è fondamentale. In Microsoft Azure queste funzionalità sono offerte da servizi come Azure Active Directory, Archiviazione di Azure e il supporto per svariati standard e API.

[Azure Active Directory](./active-directory/active-directory-whatis.md) (Azure AD) è un repository di identità e un motore che fornisce l'autenticazione, l'autorizzazione e il controllo di accesso per gli utenti, i gruppi e gli oggetti di un'organizzazione. Azure AD offre agli sviluppatori anche un modo efficace per l'integrazione della gestione delle identità nelle applicazioni. I protocolli standard di settore come [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx) e [OpenID Connect](http://openid.net/connect/) rendono l'accesso possibile in un'ampia varietà di piattaforme quali .NET, Java, Node.js e PHP.

L'API Graph basata su REST consente agli sviluppatori di leggere e scrivere nella directory da qualsiasi piattaforma. Grazie al supporto per [OAuth 2.0](http://oauth.net/2/), gli sviluppatori possono creare applicazioni Web e per dispositivi mobili che possano integrarsi con API Web Microsoft e di terze parti, per creare API Web personalizzate e sicure. Sono disponibili librerie client open source per .NET, Windows Store, iOS e Android, con altre librerie in fase di sviluppo.

### Come Azure abilita la gestione delle identità e dell'accesso

Azure AD può essere usato come directory cloud autonoma dell'organizzazione oppure come soluzione integrata con l'implementazione di Active Directory locale esistente. Alcune funzionalità di integrazione includono la sincronizzazione della directory e Single Sign-On (SSO), che estendono l'ambito delle identità locali esistenti al cloud e migliorano l'esperienza amministratore e utente finale.

Alcune delle altre funzionalità di gestione delle identità e dell'accesso includono:

-   Azure AD abilita [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) nelle applicazioni SaaS, indipendentemente da dove vengono ospitate. Alcune applicazioni sono federate con Azure AD e altre usano SSO basato su password. Le applicazioni federate possono anche supportare il provisioning utenti e l'insieme di credenziali delle password.

-   L'accesso ai dati in [Archiviazione di Azure](https://azure.microsoft.com/services/storage/) è controllato dall'autenticazione. Ogni account di archiviazione ha una chiave primaria ([chiave dell'account di archiviazione](https://msdn.microsoft.com/library/azure/ee460785.aspx)) e una chiave privata secondaria (firma di accesso condiviso).

-   Azure AD fornisce l'identità come servizio tramite la federazione (con [Active Directory Federation Services](./active-directory/fundamentals-identity.md)), la sincronizzazione e la replica con le directory locali.

-   [Azure Multi-Factor Authentication (MFA)](./multi-factor-authentication/multi-factor-authentication.md) è il servizio di autenticazione a più fattori che richiede agli utenti l'ulteriore verifica dell'accesso tramite un'app per dispositivi mobili, una chiamata telefonica o un SMS. È disponibile per l'uso con Azure AD per la protezione delle risorse locali con il server Azure MFA e con applicazioni e directory personalizzate che usano l'SDK.

-   [Servizi di dominio Azure AD](https://azure.microsoft.com/services/active-directory-ds/) consente di aggiungere le macchine virtuali di Azure a un dominio senza bisogno di distribuire controller di dominio. Gli utenti possono accedere a queste macchine virtuali con le credenziali di Active Directory aziendali e amministrare le macchine virtuali aggiunte a un dominio con Criteri di gruppo per applicare le baseline della sicurezza in tutte le macchine virtuali di Azure.

-   [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) offre un servizio di gestione delle identità globale a disponibilità elevata per le applicazioni rivolte agli utenti, adattabile a centinaia di milioni di identità. Il servizio può essere integrato tra piattaforme mobili e Web. Gli utenti possono accedere a tutte le applicazioni attraverso esperienze personalizzabili usando gli account dei propri social network esistenti o creando nuove credenziali.

##Controllo di accesso ai dati e crittografia

Microsoft adotta i principi di separazione dei compiti e dei [privilegi minimi](https://en.wikipedia.org/wiki/Principle_of_least_privilege) in tutte le operazioni di Azure. L'accesso ai dati da parte del personale di supporto di Azure richiede un'autorizzazione esplicita che viene concessa su base JIT e viene registrata e controllata e infine revocata al termine dell'impegno.

Azure offre anche diverse funzionalità per la protezione dei dati in transito e inattivi, inclusa la crittografia per dati, file, applicazioni, servizi, comunicazioni e unità. È possibile crittografare le informazioni prima di inserirle in Azure, oltre che archiviare le chiavi nei data center locali.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started\sec-azgsfig1.PNG)

### Tecnologie di crittografia di Azure

Per raccogliere dettagli sull'accesso come amministratore all'ambiente della sottoscrizione, è possibile usare la [creazione report di Azure AD](./active-directory/active-directory-reporting-audit-events.md). È possibile configurare [Crittografia unità BitLocker](https://technet.microsoft.com/library/cc732774.aspx) nei VHD contenenti informazioni riservate in Azure.

Le altre funzionalità di Azure che facilitano la protezione dei dati includono:

-   Gli sviluppatori di applicazioni possono integrare la crittografia nelle applicazioni distribuite in Azure usando [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) di Windows e .NET Framework.

- La crittografia lato client per l'archivio BLOB Microsoft consente di controllare completamente le chiavi. Il servizio di archiviazione non vede mai le chiavi e non può decrittografare i dati.

-   [Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) (con [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) fornisce la crittografia a livello di file e di dati e la prevenzione della perdita di dati nella gestione dell'accesso basato su policy.

-   Azure supporta la [crittografia a livello di tabella e a livello di colonna (TDE/CLE)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) nelle macchine virtuali di SQL Server e supporta i server di gestione delle chiavi locali di terze parti nei data center dei clienti.

-   Le chiavi dell'account di archiviazione, le firme di accesso condiviso, i certificati di gestione e altre chiavi sono univoci per ogni tenant di Azure.

-   La risorsa di archiviazione ibrida Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) crittografa i dati con una coppia di chiavi pubbliche/private a 128 bit prima di caricarli in Archiviazione di Azure.

-   Azure supporta e usa diversi meccanismi di crittografia, tra cui SSL/TLS, IPsec e AES, in base ai tipi di dati, ai contenitori e a trasporti.

##Virtualizzazione

La piattaforma Azure usa un ambiente virtualizzato. Le istanze utente funzionano come macchine virtuali autonome prive di accesso a un server host fisico e questo isolamento viene applicato usando i [livelli di privilegi del processore (ring-0/ring-3)](https://en.wikipedia.org/wiki/Protection_ring) fisico.

Ring 0 è il livello con più privilegi e 3 quello con meno privilegi. Il sistema operativo guest viene eseguito in un livello Ring 1 con meno privilegi e le applicazioni nel livello con privilegi più basso Ring 3. Questa virtualizzazione delle risorse fisiche porta a una netta separazione tra il sistema operativo guest e l'hypervisor, con un'ulteriore separazione della sicurezza tra i due.

L'hypervisor di Azure funge da micro-kernel e passa tutte le richieste di accesso all'hardware dalle VM guest all'host per elaborarle con un'interfaccia di memoria condivisa denominata VMBus. Questo impedisce agli utenti di ottenere l'accesso in lettura/scrittura/esecuzione non elaborato al sistema e riduce il rischio di condividere le risorse di sistema.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started\sec-azgsfig2.PNG)

### Come Azure implementa la virtualizzazione

Azure usa un firewall hypervisor (filtro di pacchetti), implementato nell'hypervisor e configurato da un agente controller di infrastruttura. Ciò consente di proteggere i tenant da accesso non autorizzato. Per impostazione predefinita, quando viene creata una VM, tutto il traffico viene bloccato e quindi l'agente controller di infrastruttura configura il filtro di pacchetti per aggiungere *regole ed eccezioni* per consentire il traffico autorizzato.

In questo caso sono previste due categorie di regole:

-   **Regole di configurazione macchina virtuale o di infrastruttura**: per impostazione predefinita, vengono bloccate tutte le comunicazioni. Alcune eccezioni consentono a una VM di inviare e ricevere il traffico DHCP e DNS. Le VM possono anche inviare il traffico a Internet "pubblico" e inviare il traffico ad altre VM nel cluster e nel server di attivazione del sistema operativo. L'elenco di destinazioni in uscita consentite delle VM non include subnet di router di Azure, back-end di gestione di Azure e altre proprietà Microsoft.

-   **File di configurazione dei ruoli**: definisce gli ACL in ingresso in base al modello di servizio dei tenant. Se, ad esempio, un tenant ha un front-end Web sulla porta 80 in una VM, Azure apre la porta TCP 80 a tutti gli indirizzi IP se si sta configurando un endpoint nel modello di [gestione dei servizi di Azure](resource-manager-deployment-model.md). Se la VM ha un ruolo di lavoro o back-end in esecuzione, il ruolo di lavoro viene aperto solo per la VM nello stesso tenant.

##Isolamento

Garantire la separazione per impedire il trasferimento non autorizzato e non intenzionale di informazioni tra le distribuzioni in un'architettura multi-tenant condivisa è un altro importante requisito di sicurezza del cloud.

Azure implementa il [controllo di accesso alla rete](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) e la separazione tramite l'isolamento VLAN, gli ACL, i servizi di bilanciamento del carico e i filtri IP. Il traffico esterno in ingresso verso le macchine virtuali è limitato alle porte e ai protocolli definiti. L'applicazione di filtri alla rete viene implementata per impedire il traffico falsificato e limita il traffico in ingresso e in uscita ai componenti attendibili della piattaforma. Nei dispositivi di protezione perimetrale vengono implementati criteri di flusso che rifiutano il traffico per impostazione predefinita.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started\sec-azgsfig3.PNG)

Network Address Translation (NAT) viene usato per separare il traffico di rete interno dal traffico esterno. Il traffico interno non è instradabile all'esterno. Gli [indirizzi IP virtuali](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) instradabili all'esterno vengono convertiti in indirizzi [IP dinamici interni](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) instradabili solo in Azure.

Il traffico esterno verso le macchine virtuali di Azure è protetto con firewall dagli elenchi di controllo di accesso (ACL) su router, servizi di bilanciamento del carico e commutatori di livello 3. Sono consentiti solo protocolli noti specifici. Gli ACL vengono applicati per limitare il traffico originato dalle VM guest verso altre VLAN usate per la gestione. Inoltre il traffico filtrato con filtri IP nel sistema operativo host viene ulteriormente limitato a livello rete e di collegamento dati.

### Come Azure implementa l'isolamento

Il controller di infrastruttura di Azure è responsabile dell'allocazione delle risorse dell'infrastruttura nei carichi di lavoro dei tenant e gestisce le comunicazioni unidirezionali dall'host alle VM. L'hypervisor di Azure impone la separazione di memoria e processi tra le VM e instrada in modo sicuro il traffico di rete ai tenant del sistema operativo guest. Azure implementa l'isolamento anche per tenant, archiviazione e reti virtuali:

-   Ogni tenant di Azure AD viene isolato in modo logico usando i limiti di sicurezza.

-   Gli account di archiviazione di Azure sono univoci per ogni sottoscrizione e l'accesso deve essere autenticato con una chiave dell'account di archiviazione.

-   Le reti virtuali vengono isolate in modo logico con una combinazione di indirizzi IP privati univoci, firewall e ACL IP. I servizi di bilanciamento del carico instradano il traffico ai tenant appropriati in base alle definizioni degli endpoint.

##Rete virtuale e firewall

Le [reti distribuite e virtuali](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) in Azure consentono di assicurarsi che il traffico di rete privato venga isolato in modo logico dal traffico su altre reti virtuali di Azure.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started\sec-azgsfig4.PNG)

La sottoscrizione può contenere più reti private isolate (e includere un firewall, il bilanciamento del carico e Network Address Translation).

Azure offre tre livelli principali di separazione delle reti in ogni cluster di Azure per separare il traffico in modo logico. Le [reti locali virtuali](https://azure.microsoft.com/services/virtual-network/) (VLAN) vengono usate per separare il traffico dei clienti dal resto della rete di Azure. L'accesso alle rete di Azure dall'esterno del cluster è limitato dai servizi di bilanciamento del carico.

Il traffico di rete verso e dalle VM deve passare attraverso il commutatore virtuale dell'hypervisor. Il componente del filtro IP nel sistema operativo radice isola la VM radice dalle VM guest e le VM guest l'una dall'altra. Applica filtri al traffico per limitare la comunicazione tra i nodi del tenant e Internet pubblico (in base alla configurazione del servizio del cliente), separandoli dagli altri tenant.

Il filtro IP impedisce alle VM guest di:

- Generare traffico falsificato

- Ricevere traffico non indirizzato a esse

- Indirizzare il traffico agli endpoint dell'infrastruttura protetti

- Inviare o ricevere traffico broadcast non appropriato

È possibile inserire le macchine virtuali nelle [reti virtuali di Azure](https://azure.microsoft.com/documentation/services/virtual-network/). Queste reti virtuali sono simili alle reti configurate negli ambienti locali, dove vengono in genere associate a un commutatore virtuale. Le macchine virtuali connesse alla stessa rete virtuale di Azure possono comunicare tra loro senza alcuna configurazione aggiuntiva. È anche possibile configurare subnet diverse nella rete virtuale di Azure.

Per proteggere le comunicazioni sulla rete virtuale di Azure, è possibile usare le tecnologie di Rete virtuale di Azure seguenti:

-   [**Gruppi di sicurezza di rete**](./virtual-network/virtual-networks-nsg.md). È possibile usare un gruppo di sicurezza di rete per controllare il traffico verso una o più istanze di macchina virtuale (VM) in una rete virtuale. Un NSG contiene le regole di controllo di accesso che consentono o negano il traffico in base alla direzione del traffico, al protocollo, all’indirizzo e alla porta di origine e all’indirizzo e alla porta di destinazione.

-   [**Routing definito dall'utente**](./virtual-network/virtual-networks-udr-overview.md). È possibile controllare il routing dei pacchetti attraverso un'appliance virtuale creando route definite dall'utente che specifichino l'hop successivo per i pacchetti che passano a una subnet specifica per accedere all'appliance di sicurezza di rete virtuale.

-   [**Inoltro IP**](./virtual-network/virtual-networks-udr-overview.md). Un'appliance di sicurezza di rete virtuale deve poter ricevere traffico in ingresso non indirizzato a se stessa. Per consentire a una macchina virtuale di ricevere il traffico indirizzato ad altre destinazioni, abilitare l'inoltro IP per la macchina virtuale.

-   [**Tunneling forzato**](./vpn-gateway/vpn-gateway-about-forced-tunneling.md). Il tunneling forzato consente di reindirizzare o "forzare" tutto il traffico associato a Internet generato dalle macchine virtuali in una rete virtuale di Azure verso la posizione locale tramite un tunnel VPN da sito a sito per l'ispezione e il controllo.

-   [ACL **endpoint**](./virtual-machines/virtual-machines-windows-classic-setup-endpoints.md). È possibile controllare a quali computer sono consentite le connessioni in ingresso da Internet a una macchina virtuale nella rete virtuale di Azure definendo gli ACL endpoint.

-   [**Soluzioni di sicurezza di rete per i partner**](https://azure.microsoft.com/marketplace/). Sono disponibili numerose soluzioni di sicurezza di rete per i partner accessibili da Azure Marketplace.

### Come Azure implementa le reti virtuali e il firewall

Per impostazione predefinita, Azure implementa firewall con filtro dei pacchetti in tutte le VM host e guest. Anche nelle immagini dei sistemi operativi Windows di Raccolta di Azure, Windows Firewall è abilitato per impostazione predefinita. I servizi di bilanciamento del carico sul perimetro delle reti pubbliche di Azure controllano le comunicazioni in base agli ACL IP gestiti dagli amministratori dei clienti.

Se Azure sposta i dati di un cliente durante le normali operazioni o durante un'emergenza, l'azione viene eseguita su canali di comunicazioni crittografati privati. Le altre funzionalità sfruttate da Azure da usare nelle reti virtuali e nel firewall sono:

-   **Firewall host nativo**: l'infrastruttura e la risorsa di archiviazione di Azure vengono eseguite su un sistema operativo nativo senza hypervisor e quindi Windows Firewall viene configurato con i due precedenti set di regole. La risorsa di archiviazione viene eseguita come nativa per ottimizzare le prestazioni.

-   **Firewall host**: il firewall host protegge il sistema operativo host che esegue l'hypervisor. Le regole vengono programmate per consentire solo al controller di infrastruttura e ai jumpbox di comunicare con il sistema operativo host su una porta specifica. Le altre eccezioni consentono la risposta DHCP e le risposte DNS. Azure usa un file di configurazione computer che include il modello delle regole del firewall per il sistema operativo host. L'host stesso è protetto contro gli attacchi esterni da Windows Firewall configurato in modo da consentire solo le comunicazioni da origini note autenticate.

-   **Firewall guest**: replica le regole presenti nel filtro pacchetti del commutatore VM, ma programmate in un altro software (ad esempio, il componente Windows Firewall del sistema operativo guest). Il firewall VM guest può essere configurato per limitare le comunicazioni verso o dalla VM guest, anche se la comunicazione è consentita dalle configurazioni nel filtro IP host. Ad esempio, è possibile scegliere di usare il firewall VM guest per limitare la comunicazione tra due reti virtuali configurate per connettersi l'una all'altra.

-   **Firewall di archiviazione**: il firewall sul front-end di archiviazione filtra il traffico indirizzandolo solo alle porte 80/443 e ad altre porte di utilità necessarie. Il firewall sul back-end di archiviazione limita le comunicazioni solo a quelle provenienti dai server front-end di archiviazione.

-   **Gateway di rete virtuale**: i [gateway di rete virtuale di Azure](./vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) fungono da gateway cross-premise che connettono i carichi di lavoro nella rete virtuale di Azure ai siti locali. È necessario connettersi ai siti locali con [tunnel VPN da sito a sito IPsec](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) o con circuiti [ExpressRoute](./expressroute/expressroute-introduction.md). Per i tunnel VPN IPsec/IKE, i gateway eseguono handshake IKE e stabiliscono i tunnel VPN S2S IPsec tra le reti virtuali e i siti locali. I gateway di rete virtuale terminano anche le [VPN da punto a sito](./vpn-gateway/vpn-gateway-point-to-site-create.md).

##Accesso remoto sicuro

Per i dati archiviati nel cloud devono essere abilitate misure di sicurezza sufficienti per impedire gli exploit e garantire la riservatezza e l'integrità durante il transito. Queste misure includono controlli di rete associati ai meccanismi di gestione delle identità e dell'accesso controllabile e basata su criteri di un'organizzazione.

La tecnologia di crittografia predefinita consente di crittografare le comunicazioni all'interno e tra distribuzioni, tra aree di Azure e da Azure ai data center locali. L'accesso di amministratore alle macchine virtuali tramite [sessioni Desktop remoto](./virtual-machines/virtual-machines-windows-classic-connect-logon.md), [Windows PowerShell remoto](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx) e il portale di gestione di Azure è sempre crittografato.

Per estendere in modo sicuro il data center locale al cloud, Azure fornisce sia [VPN da sito a sito](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) che [VPN da punto a sito](./vpn-gateway/vpn-gateway-point-to-site-create.md), oltre a collegamenti dedicati con [ExpressRoute](./expressroute/expressroute-introduction.md) (le connessioni alle reti virtuali di Azure su VPN vengono crittografate).

### Come Azure implementa l'accesso remoto sicuro

Le connessioni al portale di Azure devono essere sempre autenticate e richiedono SSL/TLS. È possibile configurare certificati di gestione per abilitare la gestione sicura. I protocolli sicuri standard del settore, ad esempio [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) e [IPsec](https://en.wikipedia.org/wiki/IPsec) sono completamente supportati.

[Azure ExpressRoute](./expressroute/expressroute-introduction.md) permette di creare connessioni private tra i data center di Azure e l'infrastruttura disponibile localmente o in un ambiente con percorso condiviso. Le connessioni ExpressRoute non sfruttano la rete Internet pubblica. Offrono più affidabilità, maggiore velocità, latenze più basse e maggiore sicurezza dei normali collegamenti basati su Internet. In alcuni casi, l'uso di connessioni ExpressRoute per trasferire dati tra dispositivi locali e Azure può produrre vantaggi significativi in termini di costi.

##Registrazione e monitoraggio

Azure fornisce la registrazione autenticata degli eventi relativi alla sicurezza che generano un audit trail ed è progettato per resistere alle manomissioni. Sono incluse le informazioni sul sistema, ad esempio i registri eventi di sicurezza nelle VM dell'infrastruttura di Azure e in Azure AD. Il monitoraggio degli eventi di sicurezza include la raccolta di eventi quali modifiche agli indirizzi IP del server DHCP o DNS, tentativi di accesso alle porte, protocolli o indirizzi IP bloccati da progettazione, modifiche ai criteri di sicurezza o alle impostazioni del firewall, creazione di account o gruppi, processi imprevisti o installazione di driver.

![Microsoft Antimalware in Azure](./media/azure-security-getting-started\sec-azgsfig5.PNG)

I log di controllo che registrano le attività e l'accesso utente con privilegi, i tentativi di accesso autorizzato e non autorizzato, le eccezioni di sistema e gli eventi di sicurezza delle informazioni vengono conservati per un periodo di tempo stabilito. Il periodo di conservazione dei log viene deciso dall'utente che configura la raccolta e la conservazione dei log in base a requisiti specifici.

### Come Azure implementa la registrazione e il monitoraggio

Azure distribuisce agenti di gestione e agenti ASM (Azure Security Monitor) in ogni nodo di calcolo, di archiviazione o di infrastruttura sottoposto a gestione, nativo o virtuale. Ogni agente di gestione è configurato per eseguire l'autenticazione a un account di archiviazione del team del servizio con un certificato ottenuto dall'archivio certificati di Azure e inoltrare i dati eventi e di diagnostica preconfigurati all'account di archiviazione. Questi agenti non vengono distribuiti nelle macchine virtuali dei clienti.

Gli amministratori di Azure accedono ai log da un portale Web per l'accesso autenticato e controllato ai log. Un amministratore può analizzare, filtrare e correlare i log. Gli account di archiviazione del team dei servizi di Azure per i log sono protetti dall'accesso di amministratore diretto per impedire la manomissione dei log.

Microsoft raccoglie i log dai dispositivi di rete con il protocollo Syslog e dai server host con Microsoft Audit Collection Services (ACS). Questi log vengono inseriti in un database di log dal quale vengono generati avvisi relativi a eventi sospetti direttamente per un amministratore Microsoft. L'amministratore può accedere a questi log e analizzarli.

[Diagnostica di Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx) è una funzionalità che consente di raccogliere dati di diagnostica da un'applicazione in esecuzione in Azure. Si tratta dei dati di diagnostica per il debug, la risoluzione dei problemi, la valutazione delle prestazioni, il monitoraggio dell'utilizzo delle risorse, l'analisi del traffico, la pianificazione della capacità e il controllo. Una volta raccolti i dati di diagnostica, è possibile trasferirli in modo permanente in un account di archiviazione di Azure. I trasferimenti possono essere pianificati o su richiesta.

##Prevenzione delle minacce

Oltre all'isolamento, alla crittografia e all'applicazione di filtri, Azure usa diversi meccanismi e processi di prevenzione delle minacce per proteggere l'infrastruttura e i servizi, tra cui controlli interni e tecnologie usate per rilevare e prevenire minacce avanzate, ad esempio DDoS, escalation dei privilegi e i [10 rischi principali identificati da OWASP](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

I controlli di sicurezza e i processi di gestione dei rischi adottati da Microsoft per proteggere l'infrastruttura cloud riducono il rischio di eventi di sicurezza imprevisti. Ma, in caso di evento imprevisto, il team SIM (Security Incident Management), che fa parte del team Microsoft OSSC (Online Security Services & Compliance), è pronto a rispondere 24 ore su 24, 7 giorni su 7.

### Come Azure implementa la prevenzione delle minacce

Azure usa controlli di sicurezza per implementare la prevenzione delle minacce e per aiutare i clienti a prevenire le potenziali minacce nei propri ambienti. L'elenco seguente riepiloga le funzionalità di prevenzione delle minacce offerte da Azure:

-   [Azure Antimalware](azure-security-antimalware.md) è abilitato per impostazione predefinita in tutti i server dell'infrastruttura. Facoltativamente è possibile abilitarlo nelle VM.

-   Microsoft esegue un monitoraggio continuo nei server, nelle reti e nelle applicazioni per rilevare le minacce e prevenire gli exploit. Gli avvisi automatizzati notificano agli amministratori i comportamenti anomali, consentendo loro di applicare un'azione correttiva alle minacce sia interne che esterne.

-   È possibile distribuire soluzioni di sicurezza di terze parti nelle sottoscrizioni, ad esempio Web application firewall di [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).

-   L'approccio di Microsoft al test di penetrazione include il "[Red Team](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)", costituito da professionisti della sicurezza Microsoft che attaccano sistemi di produzione attivi (non di clienti) in Azure per testare le difese contro minacce avanzate reali e persistenti.

-   Sistemi di distribuzione integrati gestiscono la distribuzione e l'installazione delle patch di sicurezza nella piattaforma Azure.

##Passaggi successivi

[Centro protezione Azure](https://azure.microsoft.com/support/trust-center/)

[Blog del team di sicurezza di Azure](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Blog di Active Directory](http://blogs.technet.com/b/ad/)

<!---HONumber=AcomDC_0525_2016-->