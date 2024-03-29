<properties
   pageTitle="Scenari e progettazione di applicazioni | Microsoft Azure"
   description="Panoramica delle categorie di applicazioni cloud nell'infrastruttura di servizi. Illustra la progettazione di applicazioni con servizi con e senza stato."
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="02/12/2016"
   ms.author="mfussell"/>

# Scenari di applicazione di Service Fabric

Azure Service Fabric offre una piattaforma flessibile e affidabile che consente di scrivere ed eseguire molti tipi di applicazioni e servizi aziendali. Le applicazioni e i microservizi possono essere con o senza stato e implementano il bilanciamento delle risorse tra le macchine virtuali per ottimizzare l'efficienza. La particolare architettura di Service Fabric consente di eseguire operazioni di analisi dei dati e di calcolo in memoria, transazioni parallele ed elaborazione degli eventi quasi in tempo reale nelle applicazioni. È possibile aumentare o ridurre facilmente il numero di applicazioni a seconda dei requisiti di risorse.

La piattaforma Service Fabric in Azure è ideale per le categorie di applicazioni e servizi seguenti:

- **Servizi a disponibilità elevata**: i servizi di Service Fabric garantiscono un failover veloce tramite la creazione di più repliche secondarie del servizio. In caso di inattività di un nodo, di un processo o di un singolo servizio a causa di un errore hardware o di un'altra anomalia, una delle repliche secondarie viene innalzata di livello a replica primaria con una perdita trascurabile di servizio.

- **Servizi scalabili**: è possibile partizionare singoli servizi implementando la scalabilità orizzontale dello stato nel cluster. Inoltre, i singoli servizi possono essere creati e rimossi in tempo reale. I servizi possono essere aumentati facilmente e rapidamente da poche istanze su un numero ridotto di nodi a migliaia di istanze su molti nodi, e quindi ridotti nuovamente, a seconda delle esigenze delle risorse. È possibile usare Service Fabric per compilare questi servizi e gestire il relativo ciclo di vita completo.

- **Calcolo su dati non statici**: Service Fabric permette di compilare dati, I/O e applicazioni con stato e a elevato utilizzo di calcolo. Service Fabric consente la collocazione dell'elaborazione (calcolo) e dei dati nelle applicazioni. In genere, quando l'applicazione richiede l'accesso ai dati è presente una latenza di rete associata alla cache dei dati esterni o al livello della risorsa di archiviazione. Con i servizi con stato di Service Fabric la latenza viene eliminata e questo consente operazioni di lettura e scrittura più efficienti. Si supponga, ad esempio, di avere un'applicazione che esegue la selezione delle raccomandazioni quasi in tempo reale per i clienti, con un requisito di tempo di round trip inferiore a 100 millisecondi. Le caratteristiche di latenza e prestazioni di Service Fabric, in cui il calcolo della selezione delle raccomandazioni è collocato con i dati e le regole, offrono all'utente un'esperienza reattiva rispetto al modello di implementazione standard con cui si devono recuperare i dati necessari dall'archivio remoto.

- **Applicazioni interattive basate sulla sessione**: Service Fabric si rivela utile se le applicazioni, ad esempio un gioco online o la messaggistica istantanea, richiedono operazioni di lettura e scrittura con bassa latenza. Service Fabric permette di compilare queste applicazioni interattive con stato senza dover creare un archivio o una cache separata, necessaria invece per le app senza stato. Questo aumenta la latenza e può introdurre problemi di coerenza.

- **Elaborazione di grafici distribuiti**: con l'espansione dei social network è aumentata la necessità di analizzare grafici su larga scala in parallelo. Le funzionalità di scalabilità rapida e di elaborazione di carichi in parallelo rendono Service Fabric la piattaforma naturale per grafici su larga scala. Service Fabric consente di creare servizi a elevata scalabilità per gruppi, come ad esempio social networking, business intelligence e ricerche scientifiche.

- **Analisi dei dati e flussi di lavoro**: le operazioni di lettura/scrittura rapide di Service Fabric abilitano applicazioni che devono elaborare in modo affidabile eventi o flussi di dati. Service Fabric abilita anche applicazioni che descrivono le pipeline di elaborazione, in cui i risultati devono essere affidabili e passati alla fase di elaborazione successiva senza alcuna perdita. Sono inclusi sistemi transazionali e finanziari, in cui le garanzie di coerenza e calcolo dei dati sono essenziali.

## Progettare applicazioni costituite da microservizi con e senza stato
La compilazione di applicazioni con ruoli di lavoro del servizio cloud di Azure è un esempio di servizio senza stato. Al contrario, i microservizi con stato mantengono lo stato autorevole oltre alla richiesta e alla relativa risposta. In questo modo è possibile ottenere disponibilità elevata e coerenza dello stato attraverso semplici API che forniscono garanzie transazionali supportate dalla replica. I servizi con stato di Service Fabric portano la disponibilità elevata a tutti i tipi di applicazioni, non solo a database e altri archivi di dati. Si tratta di un progresso naturale. Le applicazioni sono già passate dall'uso di database puramente relazionali per la disponibilità elevata a database NoSQL. Ora le applicazioni stesse possono avere un proprio stato "attivo" e i dati possono essere gestiti al loro interno per migliorare ancora le prestazioni senza compromettere l'affidabilità, la coerenza o la disponibilità.

Quando si compilano applicazioni costituite da microservizi, si ha in genere una combinazione di app Web senza stato (ASP.NET, Node.js e così via) che chiamano servizi aziendali di livello intermedio con e senza stato, tutti distribuiti nello stesso cluster di Service Fabric con i comandi di distribuzione di Service Fabric. Ognuno di questi servizi è indipendente per quanto riguarda la scalabilità, l'affidabilità e l'uso delle risorse e questo migliora notevolmente la flessibilità dello sviluppo e della gestione del ciclo di vita.

I microservizi con stato semplificano la progettazione delle applicazioni perché eliminano la necessità di usare code e cache aggiuntive, che invece erano necessarie in passato per soddisfare i requisiti di disponibilità e latenza di applicazioni interamente senza stato. L'elevata disponibilità e la bassa latenza dei servizi con stato implicano una presenza ridotta di componenti mobili da gestire in un'applicazione nel suo insieme. I diagrammi seguenti illustrano le differenze tra la progettazione di un'applicazione senza stato e la progettazione di un'applicazione con stato. Sfruttando i modelli di programmazione [Reliable Services](service-fabric-reliable-services-introduction.md) e [Reliable Actors](service-fabric-reliable-actors-introduction.md), i servizi con stato semplificano la complessità delle applicazioni, pur garantendo un'elevata velocità effettiva e una bassa latenza.

## Applicazione creata con servizi senza stato##
![Applicazione che usa un servizio senza stato][Image1]

## Applicazione creata con servizi con stato##
![Applicazione che usa un servizio senza stato][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Passaggi successivi

* Per iniziare a creare servizi con e senza stato, usare i modelli di programmazione [Reliable Services](service-fabric-reliable-services-quick-start.md) e [Reliable Actors](service-fabric-reliable-actors-get-started.md) di Service Fabric.
* Vedere anche gli argomenti seguenti:
    * [Informazioni sui microservizi](service-fabric-overview-microservices.md)
    * [Definire e gestire lo stato del servizio](service-fabric-concepts-state.md)
    * [Disponibilità dei servizi di Service Fabric](service-fabric-availability-services.md)
    * [Scalabilità dei servizi di Service Fabric](service-fabric-concepts-scalability.md)
    * [Partizionare i servizi di Service Fabric](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg

<!---HONumber=AcomDC_0615_2016-->