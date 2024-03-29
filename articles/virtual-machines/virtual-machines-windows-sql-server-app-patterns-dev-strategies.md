<properties
	pageTitle="Modelli di applicazione di SQL Server in macchine virtuali | Microsoft Azure"
	description="Questo articolo descrive i modelli di applicazione per SQL Server in macchine virtuali di Azure. Offre agli architetti e agli sviluppatori di soluzioni una base solida per l'architettura e il design efficace delle applicazioni."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management,azure-resource-manager" />
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="05/02/2016"
	ms.author="jroth" />

# Modelli di applicazione e strategie di sviluppo per SQL Server in Macchine virtuali di Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## Riepilogo:
Determinare quale modello o quali modelli dell'applicazione usare per le applicazioni basate su SQL Server in ambiente Azure è un'importante decisione di progettazione e richiede una conoscenza approfondita del funzionamento congiunto di SQL Server con ciascun componente dell'infrastruttura. Con i servizi di infrastruttura di Azure è possibile eseguire facilmente la migrazione, la manutenzione e il monitoraggio delle applicazioni SQL Server esistenti compilate nella piattaforma Windows Server in Macchine virtuali di Azure.

Lo scopo di questo articolo è di fornire agli architetti e agli sviluppatori soluzioni una base solida per l'architettura e il design efficace delle applicazioni, che potranno seguire nella migrazione di applicazioni esistenti in Azure oltre che nello sviluppo di nuove applicazioni in Azure.

Per ogni modello di applicazione sono disponibili uno scenario locale, la rispettiva soluzione abilitata per il cloud e i consigli tecnici correlati. L'articolo discute inoltre strategie di sviluppo specifiche di Azure che consentono di progettare correttamente le applicazioni. A causa dell'elevato numero di possibili modelli di applicazione, è consigliabile che gli architetti e gli sviluppatori scelgano i modelli più appropriati per i relativi utenti e applicazioni.

**Collaboratori tecnici:** Luis Carlos Vargas Herring, Madhan Arumugam Ramakrishnan

**Revisori tecnici:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## Introduzione

È possibile sviluppare numerosi tipi di applicazioni di più livelli separando i componenti dei diversi livelli di applicazione in computer diversi e in componenti separati. Ad esempio, è possibile inserire i componenti dell'applicazione client e le regole di business in una macchina, i componenti del livello Web front-end e del livello di accesso ai dati in un'altra macchina e un livello di database back-end in una macchina diversa. Questo tipo di strutturazione contribuisce a isolare i diversi livelli. Se si cambia la provenienza dei dati, non sarà necessario modificare l'applicazione client o Web, ma solo i componenti del livello di accesso ai dati.

Una tipica applicazione *a più livelli* include il livello presentazione, il livello business e il livello dati:


| Livello | Descrizione |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Presentazione** | Il *livello presentazione* (livello Web, livello front-end) è il livello in cui gli utenti interagiscono con un'applicazione. |
| **Business** | Il *livello business* (livello intermedio) è quello usato dal livello presentazione e dal livello dati per comunicare tra loro e include la funzionalità centrale del sistema. |
| **Dati** | Il *livello dati* è fondamentalmente il server su cui vengono archiviati i dati di un'applicazione (ad esempio, un server che esegue SQL Server). |

I livelli di applicazione rappresentano i raggruppamenti logici della funzionalità e dei componenti in un'applicazione, mentre i livelli rappresentano la distribuzione fisica delle funzionalità e dei componenti su server fisici, computer, reti o posizioni remote separate. I livelli di un'applicazione possono risiedere sullo stesso computer fisico (lo stesso livello) o possono essere distribuiti su computer separati (più livelli) e i componenti in ogni livello comunicano con i componenti in altri livelli attraverso interfacce ben definite. Si pensi al termine "livello" come a modelli di distribuzione fisica, ad esempio due livelli, tre livelli e più livelli. Un **modello di applicazione a 2 livelli** contiene due livelli di applicazione: server applicazioni e server di database. La comunicazione diretta avviene tra il server applicazioni e il server di database. Il server applicazioni contiene componenti sia del livello Web sia del livello business. Un **modello di applicazione a 3 livelli** contiene tre livelli di applicazione: server Web, server applicazioni, che contiene il livello di logica di business e/o i componenti di accesso ai dati del livello business, e infine il server di database. La comunicazione tra il server Web e il server di database avviene tramite il server applicazioni. Per informazioni approfondite sui livelli di applicazione e sui livelli, vedere la [Guida all'architettura delle applicazioni Microsoft](https://msdn.microsoft.com/library/ff650706.aspx).

Prima di iniziare a leggere questo articolo occorre conoscere i concetti fondamentali di SQL Server e di Azure. Per altre informazioni, vedere la [documentazione online di SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-server-iaas-overview.md) e [Azure.com](https://azure.microsoft.com/).

Questo articolo descrive diversi modelli di applicazione che possono essere idonei alle applicazioni semplici nonché alle applicazioni aziendali altamente complesse. Prima di descrivere nel dettaglio ogni modello, si consiglia di acquisire familiarità con i servizi di archiviazione dei dati disponibili in Azure, ad esempio [Archiviazione di Azure](../storage/storage-introduction.md), [Database SQL di Azure](../sql-database/sql-database-technical-overview.md) e [SQL Server in una macchina virtuale di Azure](virtual-machines-windows-sql-server-iaas-overview.md). Per prendere le migliori decisioni in termini di progettazione delle applicazioni, è necessario comprendere chiaramente quale servizio di archiviazione dei dati usare e quando usarlo.

### È consigliabile scegliere SQL Server in una macchina virtuale di Azure nelle condizioni seguenti:

- È necessario controllare SQL Server e Windows, ad esempio la versione, determinati aggiornamenti rapidi e la configurazione delle prestazioni di SQL Server.

- È necessario disporre di una piena compatibilità con SQL Server installato in locale e si desidera spostare le applicazioni esistenti in Azure senza doverle convertire.

- Si desidera sfruttare le funzionalità dell'ambiente Azure, ma il database SQL di Azure non supporta tutte le funzionalità richieste dall'applicazione. Queste funzionalità possono includere gli elementi seguenti:

	- **Dimensioni del database**: alla data di aggiornamento del presente articolo, il database SQL supporta un database con dimensioni massime pari a 500 GB di dati. Se l'applicazione in uso richiede oltre 500 GB di dati e non si desidera implementare soluzioni di partizionamento orizzontale personalizzate, si consiglia di usare SQL Server in una macchina virtuale di Azure. Per informazioni aggiornate, vedere [Scalabilità orizzontale di database SQL di Azure](https://msdn.microsoft.com/library/azure/dn495641.aspx) e [Livelli di servizio e livelli di prestazioni del database SQL di Azure](../sql-database/sql-database-service-tiers.md).
	- **Conformità HIPAA**: si consiglia ai clienti che operano nel settore sanitario e ai fornitori di software indipendenti (ISV) di scegliere [SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-server-iaas-overview.md) invece del [database SQL di Azure](../sql-database/sql-database-technical-overview.md) in quanto SQL Server in una macchina virtuale di Azure è protetto dal Contratto di società in affari HIPAA (BAA). Per informazioni sulla conformità, vedere [Centro protezione di Microsoft Azure: conformità](https://azure.microsoft.com/support/trust-center/compliance/).
	- **Funzionalità a livello di istanza**: al momento il database SQL non supporta funzionalità esterne al database, ad esempio i server collegati, i processi di SQL Agent, FileStream, Service Broker e così via. Per altre informazioni, vedere [Linee guida e limitazioni per il database SQL di Azure](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## 1 livello (semplice): singola macchina virtuale

In questo modello di applicazione l'applicazione e il database SQL Server vengono distribuiti a una macchina virtuale autonoma in Azure. La stessa macchina virtuale contiene l'applicazione client o Web, i componenti business, il livello di accesso ai dati e il server di database. I codici di presentazione, business e accesso ai dati sono logicamente separati, ma fisicamente situati in un singolo server. Gran parte dei clienti inizia con questo modello di applicazione per poi scalare aggiungendo altri ruoli Web o macchine virtuali al proprio sistema.

Questo modello di applicazione è utile nei casi seguenti:

- Si desidera eseguire una semplice migrazione alla piattaforma Azure per valutare se questa risponde o meno ai requisiti dell'applicazione.

- Si desidera conservare tutti i livelli di applicazione ospitati sulla stessa macchina virtuale nello stesso data center di Azure allo scopo di ridurre la latenza tra livelli.

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- Si desidera eseguire test di stress per diversi livelli di carico di lavoro ma, allo stesso tempo, non si desidera essere proprietari di molte macchine fisiche di cui eseguire la manutenzione per tutto il tempo.

Il diagramma seguente illustra un semplice scenario locale e il modo in cui è possibile distribuire la soluzione abilitata per il cloud su una singola macchina virtuale in Azure.

![Modello dell'applicazione a 1 livello](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

La distribuzione del livello business (componenti logica di business e accesso ai dati) sullo stesso livello fisico del livello presentazione può massimizzare le prestazioni dell'applicazione, a meno che non si ricorra a un livello separato per evitare eventuali problemi relativi alla scalabilità o alla sicurezza.

Poiché è molto comune iniziare con questo modello, può essere utile consultare l'articolo seguente per informazioni sulla migrazione dei dati in una macchina virtuale di SQL Server: [Migrazione di un database a SQL Server su una macchina virtuale di Azure](virtual-machines-windows-migrate-sql.md).

## 3 livelli (semplice): più macchine virtuali

In questo modello di applicazione un'applicazione a 3 livelli viene distribuita in Azure inserendo ogni livello dell'applicazione in una diversa macchina virtuale. Questo modello fornisce un ambiente flessibile per semplici scenari di scalabilità verticale e orizzontale. Quando una macchina virtuale contiene l'applicazione client o Web, un'altra ospita i componenti business e un'altra ospita il server di database.

Questo modello di applicazione è utile nei casi seguenti:

- Si desidera eseguire la migrazione di applicazioni di database complesse in Macchine virtuali di Azure.

- Si desidera che livelli di applicazioni diversi siano ospitati in aree diverse. Ad esempio, è possibile che siano presenti database condivisi e distribuiti a più aree a scopo di creazione di report.

- Si desidera spostare le applicazioni aziendali dalle piattaforme virtualizzate locali in Macchine virtuali di Azure. Per una discussione dettagliata sulle applicazioni aziendali, vedere il [relativo argomento](https://msdn.microsoft.com/library/aa267045.aspx).

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- Si desidera eseguire test di stress per diversi livelli di carico di lavoro ma, allo stesso tempo, non si desidera essere proprietari di molte macchine fisiche di cui eseguire la manutenzione per tutto il tempo.

Il diagramma seguente illustra come distribuire una semplice applicazione a 3 livelli in Azure inserendo ogni livello di applicazione in una diversa macchina virtuale.

![Modello dell'applicazione a 3 livelli](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

In questo modello di applicazione è presente una sola macchina virtuale in ogni livello. Se si dispone di più macchine virtuali in Azure, si consiglia di configurare una rete virtuale. La [rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) crea un limite di sicurezza attendibile e consente alle macchine virtuali di comunicare tra loro tramite un indirizzo IP privato. A tale scopo, assicurarsi sempre che tutte le connessioni Internet siano indirizzate solo al livello presentazione. È dunque consigliabile aprire un endpoint pubblico sul livello presentazione, ma non sugli altri livelli. Nel seguire questo modello di applicazione è inoltre necessario configurare l'elenco di controllo di accesso di rete su tale porta pubblica per autorizzare l'accesso di determinati indirizzi IP. Per altre informazioni, vedere [Gestire l'elenco di controllo di accesso su un endpoint](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

Nel diagramma i protocolli Internet possono essere TCP, UDP, HTTP o HTTPS.

>[AZURE.NOTE] La configurazione di una rete virtuale in Azure è gratuita. Verrà tuttavia addebitata una tariffa per il gateway VPN a cui si connette localmente. Questo addebito dipende dalla lunghezza del periodo di tempo in cui viene fornita e resa disponibile la connessione.

## 2 e 3 livelli con scalabilità orizzontale del livello presentazione

In questo modello di applicazione un'applicazione a 2 o a 3 livelli viene distribuita in Macchine virtuali di Azure inserendo ogni livello di applicazione in una diversa macchina virtuale. È anche possibile scalare orizzontalmente il livello presentazione a causa del maggior volume di richieste client in entrata.

Questo modello di applicazione è utile nei casi seguenti:

- Si desidera spostare le applicazioni aziendali dalle piattaforme virtualizzate locali in Macchine virtuali di Azure.

- È opportuno scalare orizzontalmente il livello presentazione a causa del maggior volume di richieste client in entrata.

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- Si desidera eseguire test di stress per diversi livelli di carico di lavoro ma, allo stesso tempo, non si desidera essere proprietari di molte macchine fisiche di cui eseguire la manutenzione per tutto il tempo.

- Si desidera essere proprietari di un ambiente di infrastrutture che sia possibile scalare verticalmente su richiesta.

Il diagramma seguente illustra come inserire i livelli di applicazione in più macchine virtuali in Azure scalando orizzontalmente il livello presentazione a causa del maggior volume di richieste client in entrata. Come osservato nel diagramma, il servizio di bilanciamento del carico di Azure è responsabile della distribuzione del traffico tra più macchine virtuali e determina inoltre il server Web a cui connettersi. La presenza di più istanze di server Web dietro il servizio di bilanciamento del carico assicura la disponibilità elevata del livello presentazione.

![Modello dell'applicazione: scalabilità orizzontale del livello presentazione](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### Procedure consigliate per i modelli a 2, 3 o più livelli con più macchine virtuali in un solo livello

Si consiglia di collocare le macchine virtuali appartenenti allo stesso livello nello stesso servizio cloud e nello stesso set di disponibilità. Ad esempio, inserire un set di server Web in **CloudService1** e **AvailabilitySet1** e un set di server di database in **CloudService2** e **AvailabilitySet2**. Un set di disponibilità in Azure consente di collocare i nodi a disponibilità elevata in domini di errore e domini di aggiornamento separati.

Per sfruttare più istanze di macchine virtuali di un livello è necessario configurare il servizio di bilanciamento del carico di Azure tra livelli di applicazione. Per configurare il servizio di bilanciamento del carico in ogni livello, creare un endpoint con carico bilanciato separatamente sulle macchine virtuali di ogni livello. Per un livello specifico, creare prima le macchine virtuali nello stesso servizio cloud: in tal modo, ci si assicurerà che abbiano tutte lo stesso indirizzo IP virtuale pubblico. Creare quindi un endpoint su una delle macchine virtuali su tale livello e assegnarlo ad altre macchine virtuali sullo stesso livello per il servizio di bilanciamento del carico. Creando un set con bilanciamento del carico si distribuisce il traffico tra più macchine virtuali e si consente al servizio di bilanciamento del carico di determinare a quale nodo connettersi in caso di errore del nodo della macchina virtuale back-end. Ad esempio, la presenza di più istanze di server Web dietro il servizio di bilanciamento del carico assicura la disponibilità elevata del livello presentazione.

Come procedura consigliata, assicurarsi sempre che tutte le connessioni Internet siano indirizzate prima al livello presentazione. Il livello presentazione accede al livello business, che a sua volta accede al livello dati. Ad esempio, aprire un endpoint sul livello presentazione. Ogni endpoint dispone di una porta pubblica e di una porta privata. La porta privata viene usata internamente dalla macchina virtuale per l'ascolto del traffico sull'endpoint. La porta pubblica è il punto di ingresso per la comunicazione dall'esterno di Azure e viene usata dal servizio di bilanciamento del carico di Azure. Si consiglia di configurare un elenco di controllo di accesso di rete per definire regole che consentano di isolare e controllare il traffico in ingresso sulla porta pubblica di qualsiasi endpoint pubblico in qualsiasi livello di applicazione. Per altre informazioni, vedere l'argomento relativo alla [gestione dell'elenco di controllo di accesso su un endpoint](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

Si noti che il servizio di bilanciamento del carico in Azure ha un funzionamento simile agli stessi servizi in un ambiente locale. Per altre informazioni, vedere [Bilanciamento del carico per i servizi di infrastruttura di Azure](virtual-machines-windows-load-balance.md).

Si consiglia inoltre di configurare una rete privata per le macchine virtuali mediante la rete virtuale di Azure, che consente la comunicazione tra le due reti tramite l'indirizzo IP privato. Per altre informazioni, vedere [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md).

## 2 e 3 livelli con scalabilità orizzontale del livello business

In questo modello di applicazione un'applicazione a 2 o a 3 livelli viene distribuita in Macchine virtuali di Azure inserendo ogni livello di applicazione in una diversa macchina virtuale. Data la complessità dell'applicazione, può essere opportuno distribuire i componenti del server applicazioni a più macchine virtuali.

Questo modello di applicazione è utile nei casi seguenti:

- Si desidera spostare le applicazioni aziendali dalle piattaforme virtualizzate locali in Macchine virtuali di Azure.

- Si desidera distribuire i componenti del server applicazioni a più macchine virtuali data la complessità dell'applicazione.

- Si desidera spostare le applicazioni line-of-business locali con elevato contenuto di logica di business in Macchine virtuali di Azure. Le applicazioni line-of-business sono un set di applicazioni per computer di importanza critica per la gestione di un'azienda, ad esempio applicazioni di contabilità, risorse umane, retribuzione, gestione della catena di fornitura e pianificazione delle risorse.

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- Si desidera eseguire test di stress per diversi livelli di carico di lavoro ma, allo stesso tempo, non si desidera essere proprietari di molte macchine fisiche di cui eseguire la manutenzione per tutto il tempo.

- Si desidera essere proprietari di un ambiente di infrastrutture che sia possibile scalare verticalmente su richiesta.

Il diagramma seguente illustra uno scenario locale e la relativa soluzione abilitata per il cloud. In questo scenario i livelli di applicazione vengono inseriti in più macchine virtuali in Azure scalando orizzontalmente il livello business, che contiene i componenti del livello di logica di business e di accesso ai dati. Come osservato nel diagramma, il servizio di bilanciamento del carico di Azure è responsabile della distribuzione del traffico tra più macchine virtuali e determina inoltre il server Web a cui connettersi. La presenza di più istanze di server applicazioni dietro il servizio di bilanciamento del carico assicura la disponibilità elevata del livello business. Per altre informazioni, vedere [Procedure consigliate per i modelli a 2, 3 o più livelli con più macchine virtuali in un solo livello](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Modello dell'applicazione con scalabilità orizzontale del livello business](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## 2 e 3 livelli con scalabilità orizzontale e disponibilità elevata e ripristino di emergenza (HADR) dei livelli presentazione e business

In questo modello di applicazione un'applicazione a 2 o a 3 livelli viene distribuita in Macchine virtuali di Azure distribuendo i componenti del livello presentazione (server Web) e del livello business (server applicazioni) a più macchine virtuali. Vengono inoltre implementate soluzioni a disponibilità elevata e di ripristino di emergenza per i propri database in Macchine virtuali di Azure.

Questo modello di applicazione è utile nei casi seguenti:

- Si desidera spostare applicazioni aziendali dalle piattaforme virtualizzate locali in Azure mediante l'implementazione delle capacità di disponibilità elevata e ripristino di emergenza di SQL Server.

- Si desidera scalare orizzontalmente il livello presentazione e il livello business a causa del maggior volume di richieste client in entrata e della complessità dell'applicazione.

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- Si desidera eseguire test di stress per diversi livelli di carico di lavoro ma, allo stesso tempo, non si desidera essere proprietari di molte macchine fisiche di cui eseguire la manutenzione per tutto il tempo.

- Si desidera essere proprietari di un ambiente di infrastrutture che sia possibile scalare verticalmente su richiesta.

Il diagramma seguente illustra uno scenario locale e la relativa soluzione abilitata per il cloud. In questo scenario i componenti del livello presentazione e del livello business vengono scalati orizzontalmente in più macchine virtuali di Azure. Vengono inoltre implementate tecniche a disponibilità elevata e di ripristino di emergenza (HADR) per i database SQL in Azure.

L'esecuzione di più copie di un'applicazione in macchine virtuali diverse assicura che si eseguirà il bilanciamento del carico delle richieste tra le diverse macchine virtuali. Quando si hanno più macchine virtuali è necessario assicurarsi che siano tutte accessibili e in esecuzione nello stesso momento. Se si configura il bilanciamento del carico, l'apposito servizio terrà traccia dell'integrità delle macchine virtuali, dirigendo correttamente le chiamate in entrata ai nodi funzionanti della macchina virtuale. Per informazioni su come configurare il bilanciamento del carico delle macchine virtuali, vedere [Bilanciamento del carico per i servizi di infrastruttura di Azure](virtual-machines-windows-load-balance.md). La presenza di più istanze di server Web e applicazioni dietro il servizio di bilanciamento del carico assicura la disponibilità elevata del livello presentazione e business.

![Scalabilità orizzontale e disponibilità elevata](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### Procedure consigliate per i modelli di applicazione che richiedono disponibilità elevata e ripristino di emergenza (HADR) per SQL Server

Quando si configurano le soluzioni a disponibilità elevata e di ripristino di emergenza per SQL Server in Macchine virtuali di Azure, la configurazione di una rete virtuale con [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) è obbligatoria. Le macchine virtuali nell'ambito di una rete virtuale disporranno di un indirizzo IP privato stabile anche dopo un'interruzione del servizio. In tal modo sarà possibile evitare i tempi di aggiornamento richiesti per la risoluzione dei nomi DNS. La rete virtuale consente inoltre di estendere la propria rete locale ad Azure, creando un limite di sicurezza attendibile. Se l'applicazione è caratterizzata da limitazioni di dominio aziendale, ad esempio l'autenticazione Windows o Active Directory, la configurazione della [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) è obbligatoria.

La maggior parte dei clienti che esegue codice di produzione su Azure mantiene repliche sia primarie sia secondarie in Azure.

Per informazioni approfondite ed esercitazioni sulle tecniche di disponibilità elevata e di ripristino di emergenza, vedere [Disponibilità elevata e ripristino di emergenza di SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-high-availability-dr.md).

## 2 e 3 livelli con Macchine virtuali di Azure e Servizi cloud

In questo modello di applicazione un'applicazione a 2 o a 3 livelli viene distribuita in Azure usando sia i [Servizi cloud di Azure](../cloud-services/cloud-services-choose-me.md#tellmecs) (ruoli Web e di lavoro - Platform as a Service (PaaS)) sia [Macchine virtuali di Azure](virtual-machines-windows-about.md) (Infrastructure as a Service (IaaS)). L'uso dei [Servizi cloud di Azure](https://azure.microsoft.com/documentation/services/cloud-services/) per il livello presentazione o business e di SQL Server in [Macchine virtuali di Azure](virtual-machines-windows-about.md) per il livello dati è vantaggioso per gran parte delle applicazioni in esecuzione su Azure. Il motivo è che l'esecuzione di un'istanza di calcolo su Servizi cloud consente di eseguire facilmente la gestione, la distribuzione, il monitoraggio e la scalabilità orizzontale.

Grazie a Servizi cloud, Azure gestisce automaticamente l'infrastruttura, esegue la manutenzione di routine, applica patch ai sistemi operativi e tenta di correggere gli errori hardware e del servizio. Quando è necessario scalare orizzontalmente l'applicazione, sono disponibili opzioni di scalabilità orizzontale manuali e automatiche per il progetto di servizio cloud semplicemente aumentando o riducendo il numero di istanze o di macchine virtuali in uso da parte dell'applicazione. È anche possibile usare Visual Studio localmente per distribuire l'applicazione a un progetto di servizio cloud in Azure.

Riepilogando, se non si desidera gestire attività amministrative prolungate per il livello presentazione o business e l'applicazione in uso non richiede alcuna configurazione complessa del software o del sistema operativo, usare i Servizi cloud di Azure. Se il database SQL di Azure non supporta tutte le funzionalità che si stanno cercando, usare SQL Server in una macchina virtuale di Azure per il livello dati. L'esecuzione di un'applicazione sui Servizi cloud di Azure e l'archiviazione di dati in Macchine virtuali di Azure combinano i vantaggi di entrambi i servizi. Per un confronto dettagliato, vedere la sezione [Confronto delle strategie di sviluppo Web in Azure](#comparing-web-development-strategies-in-azure) in questo argomento.

In questo modello di applicazione il livello presentazione include un ruolo Web, cioè un componente dei Servizi cloud in esecuzione nell'ambiente Azure personalizzato per la programmazione delle applicazioni Web supportata da IIS e ASP.NET. Il livello business o back-end include un ruolo di lavoro, cioè un componente dei Servizi cloud in esecuzione nell'ambiente di Azure, utile per lo sviluppo generalizzato, che potrebbe eseguire l'elaborazione in background di un ruolo Web. Il livello di database risiede in una macchina virtuale SQL Server in Azure. La comunicazione tra il livello presentazione e il livello di database avviene direttamente oppure attraverso il livello business (componenti del ruolo di lavoro).

Questo modello di applicazione è utile nei casi seguenti:

- Si desidera spostare applicazioni aziendali dalle piattaforme virtualizzate locali in Azure mediante l'implementazione delle capacità di disponibilità elevata e ripristino di emergenza di SQL Server.

- Si desidera essere proprietari di un ambiente di infrastrutture che sia possibile scalare verticalmente su richiesta.

- Il database SQL di Azure non supporta tutte le funzionalità di cui necessita l'applicazione in uso.

- Si desidera eseguire test di stress per diversi livelli di carico di lavoro ma, allo stesso tempo, non si desidera essere proprietari di molte macchine fisiche di cui eseguire la manutenzione per tutto il tempo.

Il diagramma seguente illustra uno scenario locale e la relativa soluzione abilitata per il cloud. In questo scenario il livello presentazione viene inserito in ruoli Web, il livello business in ruoli di lavoro e il livello dati in macchine virtuali in Azure. L'esecuzione di più copie del livello presentazione in diversi ruoli di lavoro assicura che si eseguirà il bilanciamento del carico delle richieste tra di essi. Quando si combinano i Servizi cloud di Azure con Macchine virtuali di Azure si consiglia di configurare anche [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md). Con la [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) è possibile disporre di indirizzi IP privati stabili e persistenti nell'ambito dello stesso servizio cloud. Dopo avere definito una rete virtuale per le macchine virtuali e i servizi cloud, questi potranno cominciare a comunicare tra loro attraverso l'indirizzo IP privato. In aggiunta, se le macchine virtuali e i ruoli Web o di lavoro di Azure si trovano nella stessa [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) si ottengono una latenza minima e una connettività più sicura. Per altre informazioni, vedere [Informazioni sul servizio cloud](../cloud-services/cloud-services-choose-me.md).

Come osservato nel diagramma, il servizio di bilanciamento del carico di Azure distribuisce il traffico tra più macchine virtuali e inoltre determina a quale server Web o server applicazioni connettersi. La presenza di più istanze di server Web e applicazioni dietro il servizio di bilanciamento del carico assicura la disponibilità elevata dei livelli presentazione e business. Per altre informazioni, vedere [Procedure consigliate per i modelli di applicazione che richiedono disponibilità elevata e ripristino di emergenza (HADR) per SQL Server](#best-practices-for-application-patterns-requiring-sql-hadr).

![Modelli dell'applicazione con Servizi cloud](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Un altro approccio all'implementazione di questo modello di applicazione consiste nell'usare un ruolo Web consolidato che contenga componenti del livello presentazione e del livello business, come illustrato nel diagramma seguente. Questo modello di applicazione è utile per le applicazioni che richiedono un design con stato. Poiché Azure fornisce nodi di calcolo senza stato su ruoli Web e di lavoro, si consiglia di implementare una logica per memorizzare lo stato della sessione mediante una delle tecnologie seguenti: [Cache di Azure](https://azure.microsoft.com/documentation/services/redis-cache/), [Archiviazione tabelle di Azure](../storage/storage-dotnet-how-to-use-tables.md) o [Database SQL di Azure](../sql-database/sql-database-technical-overview.md).

![Modelli dell'applicazione con Servizi cloud](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## Modello con macchine virtuali di Azure, database SQL di Azure e servizio app di Azure (app Web)

L'obiettivo principale di questo modello di applicazione è mostrare come combinare i componenti IaaS (Infrastructure as a Service) con i componenti PaaS (Platform as a Service) di Azure nella propria soluzione. Il modello è incentrato sul database SQL di Azure per l'archiviazione di dati relazionali e non include SQL Server in una macchina virtuale di Azure, che invece fa parte dell'offerta IaaS di Azure.

In questo modello di applicazione, un'applicazione di database viene distribuita in Azure mediante l'inserimento dei livelli presentazione e business nella stessa macchina virtuale e tramite l'accesso a un database nei server di database SQL (Database SQL) di Azure. È possibile implementare il livello presentazione usando le tradizionali soluzioni Web basate su IIS oppure è possibile implementare un livello presentazione/business combinato usando le [App Web di Azure](https://azure.microsoft.com/documentation/services/app-service/web/).

Questo modello di applicazione è utile nei casi seguenti:

- È già stato configurato un server di database SQL esistente in Azure e si desidera testare rapidamente l'applicazione.

- Si desidera testare le capacità dell'ambiente Azure.

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- I componenti della logica di business e dell'accesso ai dati possono già essere inclusi in un'applicazione Web completa.

Il diagramma seguente illustra uno scenario locale e la relativa soluzione abilitata per il cloud. In questo scenario si collocano i livelli di applicazione in un'unica macchina virtuale in Azure e si accede ai dati nel database SQL di Azure.

![Modello dell'applicazione misto](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Se si sceglie di implementare un livello Web/applicazione combinato mediante App Web di Azure, si consiglia di mantenere il livello intermedio o di applicazione come librerie DLL nel contesto di un'applicazione Web.

Per altre informazioni sulle tecniche di programmazione, leggere i consigli offerti nella sezione [Confronto delle strategie di sviluppo Web in Azure](#comparing-web-development-strategies-in-azure) al termine di questo argomento.

## Modello dell'applicazione ibrido a più livelli

Nel modello dell'applicazione ibrido a più livelli l'applicazione viene implementata in più livelli distribuiti localmente e su Azure. Di conseguenza, viene creato un sistema ibrido flessibile e riutilizzabile, che sarà possibile modificare o al quale si potrà aggiungere uno specifico livello senza modificare gli altri. Per estendere la propria rete aziendale al cloud, viene usato il servizio [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md).

Questo modello di applicazione ibrido è utile nei casi seguenti:

- Si desidera creare applicazioni da eseguire in parte sul cloud e in parte localmente.

- Si desidera eseguire la migrazione sul cloud di tutti o di alcuni elementi di un'applicazione locale esistente.

- Si desidera spostare le applicazioni aziendali dalle piattaforme virtualizzate locali su Azure.

- Si desidera essere proprietari di un ambiente di infrastrutture che sia possibile scalare verticalmente su richiesta.

- Si desidera eseguire rapidamente il provisioning degli ambienti di sviluppo e test per brevi periodi di tempo.

- Si desidera trovare un modo efficace in termini di costi per eseguire i backup delle applicazioni di database aziendali.

Il diagramma seguente illustra un modello dell'applicazione ibrido a più livelli distribuito localmente e su Azure. Come illustrato nel diagramma, l'infrastruttura locale include il controller di dominio dei [Servizi di dominio Active Directory](https://technet.microsoft.com/library/hh831484.aspx) per supportare l'autenticazione e l'autorizzazione degli utenti. Si noti che il diagramma illustra uno scenario in cui alcune parti del livello dati risiedono in un data center locale, mentre altre parti risiedono in Azure. A seconda delle esigenze applicative, è possibile implementare molti altri scenari ibridi. Ad esempio, è possibile mantenere il livello presentazione e il livello business in un ambiente locale, mantenendo però il livello dati in Azure.

![Modello dell'applicazione a più livelli](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

In Azure è possibile usare Active Directory come directory cloud autonoma dell'organizzazione oppure integrare l'istanza locale esistente di Active Directory con [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Come illustrato nel diagramma, i componenti del livello business possono accedere a più origini dati, ad esempio a [SQL Server in Azure](virtual-machines-windows-sql-server-iaas-overview.md) attraverso un indirizzo IP interno privato, a un'istanza di SQL Server locale tramite la [rete virtuale di Azure](../virtual-network/virtual-networks-overview.md) oppure al [database SQL](../sql-database/sql-database-technical-overview.md) usando le tecnologie del provider di dati .NET Framework. In questo diagramma il database SQL di Azure è un servizio di archiviazione dati opzionale.

Nel modello di applicazione ibrido a più livelli è possibile implementare il seguente flusso di lavoro nell'ordine specificato:

1. Identificare le applicazioni di database aziendali che devono essere spostate nel cloud usando [Microsoft Assessment and Planning (MAP) Toolkit](http://microsoft.com/map). MAP Toolkit raccoglie i dati di inventario e di prestazioni dai computer che si desidera destinare alla virtualizzazione e fornisce consigli sulla capacità e sulla pianificazione.

1. Pianificare le risorse e la configurazione necessarie nella piattaforma Azure, ad esempio gli account di archiviazione e le macchine virtuali.

1. Configurare la connettività tra la rete aziendale locale e la [Rete virtuale di Azure](../virtual-network/virtual-networks-overview.md). Per configurare la connessione tra la rete aziendale locale e una macchina virtuale in Azure, usare uno dei due metodi seguenti:

	1. Stabilire una connessione tra la rete locale e Azure tramite endpoint pubblici su una macchina virtuale in Azure. Questo metodo consente di configurare facilmente e usare l'autenticazione di SQL Server sulla macchina virtuale. Configurare inoltre l'elenco di controllo di accesso di rete sulle porte pubbliche per autorizzare l'accesso di determinati indirizzi IP. Per altre informazioni, vedere [Gestire l'elenco di controllo di accesso su un endpoint](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

	1. Stabilire una connessione tra la rete locale e Azure tramite il tunnel VPN di Azure. Questo metodo consente di estendere i criteri di dominio a una macchina virtuale in Azure, nonché di configurare le regole del firewall e usare l'autenticazione di Windows nella macchina virtuale. Attualmente, Azure supporta le connessioni sicure VPN da sito a sito e VPN da punto a sito:

		- Con la connessione da sito a sito sicura è possibile stabilire la connettività di rete tra la rete locale e la rete virtuale in Azure. Questo tipo di connessione è consigliato per connettere l'ambiente del data center locale ad Azure.

		- Con la connessione da punto a sito sicura è possibile stabilire la connettività di rete tra la rete virtuale in Azure e i singoli computer in esecuzione ovunque. Questo tipo di connessione è consigliato principalmente a scopo di sviluppo e test.

	Per informazioni sulle modalità di connessione a SQL Server in Azure, vedere [Connettersi a una macchina virtuale di SQL Server in Azure](virtual-machines-windows-classic-sql-connect.md).

1. Configurare i lavori e gli avvisi pianificati per il backup dei dati locali sul disco di una macchina virtuale in Azure. Per altre informazioni, vedere [Backup e ripristino di SQL Server con il servizio di archiviazione BLOB di Azure](https://msdn.microsoft.com/library/jj919148.aspx) e [Backup e ripristino per SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-backup-recovery.md).

1. A seconda delle esigenze applicative è possibile implementare uno tra i tre scenari comuni seguenti:

	1. È possibile mantenere il server Web, il server applicazioni e i dati non sensibili in un server di database in Azure, mentre è possibile mantenere i dati sensibili in locale.

	1. È possibile mantenere il server Web e il server applicazioni in locale, mentre è possibile mantenere il server di database su una macchina virtuale in Azure.

	1. È possibile mantenere il server di database, il server Web e il server applicazioni in locale, mentre è possibile mantenere le repliche di database su macchine virtuali in Azure. Questa impostazione consente ai server Web o alle applicazioni di accedere alle repliche del database in Azure. Di conseguenza, è possibile ridurre il carico di lavoro in un database locale. Si consiglia di implementare questo scenario per i carichi di lavoro a elevato utilizzo di lettura e a scopo di sviluppo. Per informazioni sulla creazione di repliche di database in Azure, vedere i Gruppi di disponibilità AlwaysOn in [Disponibilità elevata e ripristino di emergenza di SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-high-availability-dr.md).

## Confronto delle strategie di sviluppo Web in Azure

Per implementare e distribuire un'applicazione basata su SQL Server a più livelli in Azure, è possibile usare uno dei due metodi di programmazione seguenti:

- Configurare un server Web tradizionale (IIS - Internet Information Services) in Azure e accedere ai database in SQL Server in Macchine virtuali di Azure.

- Implementare e distribuire un servizio cloud in Azure. Assicurarsi quindi che questo servizio cloud possa accedere ai database in SQL Server in Macchine virtuali di Azure. Un servizio cloud può includere più ruoli Web e di lavoro.

La tabella seguente illustra un confronto tra lo sviluppo Web tradizionale con Servizi cloud di Azure e App Web di Azure per quanto riguarda SQL Server in Macchine virtuali di Azure. La tabella include App Web di Azure, in quanto è possibile usare SQL Server nella macchina virtuale di Azure come origine dati per App Web di Azure tramite l'indirizzo IP virtuale pubblico o il nome DNS.

||Sviluppo Web tradizionale nelle macchine virtuali di Azure|Servizi cloud in Azure|Hosting Web con app Web di Azure|
|---|---|---|---|
|**Migrazione delle applicazioni in locale**|Applicazioni esistenti così come sono.|Le applicazioni devono disporre di ruoli Web e di ruoli di lavoro.|Applicazioni esistenti così come sono, ma adatte per applicazioni e servizi Web indipendenti che richiedono una rapida scalabilità.|
|**Sviluppo e distribuzione**|Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, Gestione IIS, PowerShell.|Visual Studio, Azure SDK, TFS, PowerShell. Ogni servizio cloud dispone di due ambienti in cui è possibile distribuire il pacchetto e la configurazione del servizio: gestione temporanea e produzione. È possibile distribuire un servizio cloud nell'ambiente di gestione temporanea per testarlo prima di promuoverlo alla produzione.|Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, Mercurial, TFS, Web Deploy, PowerShell.|
|**Amministrazione e installazione**|Si è responsabili delle attività amministrative relative ad applicazione, dati, regole del firewall, rete virtuale e sistema operativo.|Si è responsabili delle attività amministrative relative ad applicazione, dati, regole del firewall e rete virtuale.|Si è responsabili delle attività amministrative relative solo ad applicazione e dati.|
|**Disponibilità elevata e ripristino di emergenza (HADR, High Availability and Disaster Recovery)**|Si consiglia di posizionare le macchine virtuali nello stesso set di disponibilità e nello stesso servizio cloud. L'inserimento delle macchine virtuali nello stesso set di disponibilità consente ad Azure di collocare i nodi a disponibilità elevata in domini di errore e domini di aggiornamento separati. In maniera analoga, l'inserimento delle macchine virtuali nello stesso servizio cloud consente al servizio di bilanciamento del carico e alle macchine virtuali di comunicare direttamente tra loro attraverso la rete locale nell'ambito di un data center di Azure.<br/><br/>L'utente è responsabile dell'implementazione di una soluzione di disponibilità elevata e ripristino di emergenza per SQL Server in Macchine virtuali di Azure per evitare qualsiasi tempo di inattività. Per le tecnologie HADR supportate, vedere l'argomento relativo alla [disponibilità elevata e ripristino di emergenza di SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>L'utente è responsabile del backup dei propri dati e dell'applicazione.<br/><br/>Azure può spostare le macchine virtuali in caso di esito negativo della macchina host nel data center a causa di problemi hardware. Potrebbero anche verificarsi tempi di inattività pianificati della macchina virtuale quando si eseguono aggiornamenti di sicurezza o software della macchina host. Di conseguenza, si consiglia di mantenere almeno due macchine virtuali in ogni livello di applicazione per garantirne una disponibilità continua. Azure non fornisce contratti di servizio per una singola macchina virtuale. Per altre informazioni, vedere [Informazioni tecniche sulla resilienza di Azure](../resiliency/resiliency-technical-guidance.md).|Azure gestisce gli errori risultanti dall'hardware sottostante o dal software del sistema operativo. Si consiglia di implementare più istanze di un ruolo Web o di lavoro per garantire la disponibilità elevata dell'applicazione. Per altre informazioni, vedere il [Contratto di servizio per servizi cloud, macchine virtuali e rete virtuale](http://www.microsoft.com/download/details.aspx?id=38427) e [Ripristino di emergenza e disponibilità elevata per applicazioni Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>L'utente è responsabile del backup dei propri dati e dell'applicazione.<br/><br/>Per i database che risiedono in un database SQL Server in una macchina virtuale di Azure, l'utente è responsabile dell'implementazione di una soluzione di disponibilità elevata e ripristino di emergenza per evitare qualsiasi tempo di inattività. Per le tecnologie HADR supportate, vedere Disponibilità elevata e ripristino di emergenza di SQL Server in Macchine virtuali di Azure.<br/><br/>**Mirroring del database in SQL Server**: uso con i servizi cloud di Azure (ruoli Web/di lavoro). Le macchine virtuali di SQL Server e un progetto di servizio cloud possono risiedere nella stessa rete virtuale di Azure. Se la macchina virtuale di SQL Server non si trova nella stessa rete virtuale, è necessario creare un alias di SQL Server per indirizzare la comunicazione all'istanza di SQL Server. Inoltre, il nome di alias deve corrispondere al nome di SQL Server.|La disponibilità elevata viene ereditata da ruoli di lavoro di Azure, archiviazione BLOB di Azure e database SQL di Azure. Il servizio di archiviazione di Azure gestisce ad esempio tre repliche di tutti i dati di BLOB, tabelle e code. In qualsiasi momento, il database SQL di Azure mantiene tre repliche di dati in esecuzione: una replica primaria e due repliche secondarie. Per altre informazioni, vedere [Documentazione di Archiviazione di Azure](https://azure.microsoft.com/documentation/services/storage/) e [Panoramica del database SQL](../sql-database/sql-database-technical-overview.md).<br/><br/>Quando si usa SQL Server nella macchina virtuale di Azure come origine dati per App Web di Azure, tenere presente che App Web di Azure non supporta la rete virtuale di Azure. In altri termini, tutte le connessioni da App Web di Azure alle macchine virtuali di SQL Server in Azure devono attraversare gli endpoint pubblici delle macchine virtuali. Questo potrebbe causare alcune limitazioni per gli scenari di disponibilità elevata e ripristino di emergenza. Ad esempio, l'applicazione client su App Web di Azure che si connette alla macchina virtuale di SQL Server con il mirroring del database non potrebbe connettersi al nuovo server primario, in quanto il mirroring richiede la configurazione della rete virtuale di Azure tra le macchine virtuali host di SQL Server in Azure. L'uso del **mirroring dei database SQL Server** con App Web di Azure non è quindi attualmente supportato.<br/><br/>**Gruppi di disponibilità di SQL Server AlwaysOn**: è possibile configurare i gruppi di disponibilità di SQL Server AlwaysOn quando si usa App Web di Azure con le macchine virtuali di SQL Server in Azure. Tuttavia, è necessario configurare il listener del gruppo di disponibilità AlwaysOn per indirizzare la comunicazione alla replica primaria tramite endpoint pubblici con bilanciamento del carico.|
|**Connettività cross-premise**|È possibile usare la rete virtuale di Azure per la connessione in locale.|È possibile usare la rete virtuale di Azure per la connessione in locale.|La rete virtuale di Azure è supportata. Per altre informazioni, vedere l'articolo relativo all'[integrazione della rete virtuale delle app Web](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Scalabilità**|L'aumento delle prestazioni del servizio è possibile incrementando le dimensioni della macchina virtuale o aggiungendo più dischi. Per altre informazioni sulle dimensioni delle macchine virtuali, vedere [Dimensioni delle macchine virtuali](virtual-machines-windows-sizes.md).<br/><br/>**Per il server di database**: la scalabilità orizzontale è disponibile tramite le tecniche di partizionamento del database e i gruppi di disponibilità di SQL Server AlwaysOn.<br/><br/>Per i carichi di lavoro a elevato utilizzo di lettura è possibile usare i [gruppi di disponibilità AlwaysOn](https://msdn.microsoft.com/library/hh510230.aspx) su più nodi secondari, nonché la replica di SQL Server.<br/><br/>Per i carichi di lavoro a elevato utilizzo di scrittura è possibile implementare il partizionamento orizzontale di database tra più server fisici per creare applicazioni con scalabilità orizzontale.<br/><br/>È anche possibile implementare una scalabilità orizzontale usando [SQL Server con il routing dipendente dai dati](https://technet.microsoft.com/library/cc966448.aspx). Con il routing dipendente dai dati è necessario implementare il meccanismo di partizionamento nell'applicazione client, in genere nel livello business, per indirizzare le richieste del database a più nodi SQL Server. Il livello business contiene mapping alla modalità di partizionamento dei dati e al nodo che contiene i dati.<br/><br/>È possibile applicare la scalabilità alle applicazioni in cui vengono eseguite le macchine virtuali. Per altre informazioni, vedere l'articolo su [come ridimensionare un'applicazione](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Nota importante**: la funzionalità di **scalabilità automatica** di Azure consente di aumentare o ridurre automaticamente il numero di macchine virtuali usate dall'applicazione. In questo modo l'esperienza degli utenti finali non è compromessa nei periodi di massima attività e le macchine virtuali sono spente quando la domanda è bassa. È consigliabile non impostare l'opzione di scalabilità automatica per il servizio cloud se include macchine virtuali di SQL Server. La funzionalità di scalabilità automatica, infatti, consente ad Azure di avviare una macchina virtuale quando l'utilizzo della CPU in tale macchina virtuale è superiore rispetto a una certa soglia e di spegnerla quando l'utilizzo della CPU scende al di sotto di tale soglia. Si tratta di una funzionalità utile per le applicazioni senza stato, ad esempio i server Web, in cui tutte le macchine virtuali possono gestire il carico di lavoro senza riferimenti allo stato precedente. Tuttavia, la funzionalità di scalabilità automatica non è utile per le applicazioni con stato come SQL Server in cui solo un'istanza consente la scrittura nel database.|L'aumento delle prestazioni del servizio è possibile tramite l'uso di più ruoli Web e più ruoli di lavoro. Per altre informazioni sulle dimensioni delle macchine virtuali per ruoli Web e di lavoro, vedere [Dimensioni dei servizi cloud](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Quando si usano i **servizi cloud** è possibile definire più ruoli per distribuire l'elaborazione e conseguire la scalabilità flessibile dell'applicazione. Ogni servizio cloud include uno o più ruoli Web e/o di lavoro, ognuno con i rispettivi file e configurazione dell'applicazione. È possibile scalare verticalmente un servizio cloud aumentando il numero di istanze del ruolo (macchine virtuali) distribuite per un ruolo o riducendo il numero di istanze del ruolo. Per informazioni dettagliate, vedere [Opzioni di hosting di calcolo fornite da Azure](../cloud-services/cloud-services-choose-me.md).<br/><br/>La scalabilità orizzontale è disponile per mezzo del supporto a disponibilità elevata incorporato in Azure tramite il [contratto di servizio per servizi cloud, macchine virtuali e rete virtuale](http://www.microsoft.com/download/details.aspx?id=38427) e il servizio di bilanciamento del carico.<br/><br/>Per un'applicazione a più livelli ,si consiglia di connettere l'applicazione dei ruoli Web o di lavoro alle macchine virtuali del server di database tramite la rete virtuale di Azure. Azure offre inoltre il bilanciamento del carico per macchine virtuali nello stesso servizio cloud, distribuendo le richieste degli utenti tra le macchine virtuali. Le macchine virtuali connesse in tale modo saranno in grado di comunicare direttamente tra loro sulla rete locale in un data center di Azure.<br/><br/>È possibile impostare la **scalabilità automatica** sul portale di Azure classico, oltre alle ore di pianificazione. Per altre informazioni, vedere l'articolo su [come ridimensionare un'applicazione](../cloud-services/cloud-services-how-to-scale.md).|**Aumentare e ridurre le prestazioni del servizio**: è possibile aumentare o ridurre la dimensione dell'istanza (VM) riservata al sito Web.<br/><br/>Aumentare il numero di istanze del servizio: è possibile aggiungere più istanze riservate (VM) al sito Web.<br/><br/>È possibile impostare la **Scalabilità automatica** nel portale, oltre alle ore di pianificazione. Per altre informazioni, vedere l'articolo su [come ridimensionare le app Web](../app-service-web/web-sites-scale.md).|

Per altre informazioni sulla scelta tra i metodi di programmazione disponibili, vedere [Confronto tra App Web di Azure, Servizi cloud e Macchine virtuali di Azure](../app-service-web/choose-web-site-cloud-service-vm.md).

## Passaggi successivi

Per altre informazioni sull'esecuzione di SQL Server in Macchine virtuali di Azure, vedere [SQL Server in Macchine virtuali di Azure](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0629_2016-->