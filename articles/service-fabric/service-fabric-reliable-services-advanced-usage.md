<properties
   pageTitle="Uso avanzato dei servizi Reliable Services | Microsoft Azure"
   description="Informazioni sull'uso avanzato dei servizi Reliable Services di Service Fabric per una maggiore flessibilità nei servizi."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="vturecek"/>

# Uso avanzato del modello di programmazione Reliable Services
Service Fabric di Azure semplifica la scrittura e la gestione di servizi affidabili con e senza stato. Questa guida illustra l'uso avanzato dei servizi Reliable Services per ottenere più controllo e flessibilità sui servizi. Prima di leggere questa guida, acquisire familiarità con il [modello di programmazione Reliable Services](service-fabric-reliable-services-introduction.md).

Sia i servizi con stato che i servizi senza stato hanno due punti di ingresso principali per il codice utente:

 - `RunAsync` è un punto di ingresso generico per il codice del servizio.
 - `CreateServiceReplicaListeners` e `CreateServiceInstanceListeners` vengono usati per l'apertura di listener di comunicazione per le richieste client.
 
Per la maggior parte dei servizi questi due punti di ingresso sono sufficienti. In rari casi, quando è necessario un maggiore controllo sul ciclo di vita del servizio, sono disponibili eventi del ciclo di vita aggiuntivi.

## Ciclo di vita dell'istanza di servizio senza stato

Il ciclo di vita di un servizio senza stato è molto semplice. Un servizio senza stato può solo essere aperto, chiuso o interrotto. `RunAsync` viene eseguito in un servizio senza stato quando viene aperta un'istanza del servizio e viene annullato quando viene chiusa o interrotta un'istanza del servizio.

Anche se `RunAsync` dovrebbe essere sufficiente nella maggior parte dei casi, in un servizio senza stato sono disponibili anche eventi di apertura, chiusura e interruzione:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`: OnOpenAsync viene chiamato quando l'istanza del servizio senza stato sta per essere usata. In questo momento è possibile avviare attività estese di inizializzazione del servizio.

- `Task OnCloseAsync(CancellationToken)`: OnCloseAsync viene chiamato quando l'istanza del servizio senza stato sta per essere arrestata normalmente. Ciò può verificarsi quando il codice del servizio viene aggiornato, l'istanza del servizio viene spostata a causa del bilanciamento del carico o viene rilevato un errore temporaneo. OnCloseAsync può essere usato per chiudere in modo sicuro tutte le risorse, arrestare qualsiasi elaborazione in background, completare il salvataggio dello stato esterno o chiudere le connessioni esistenti.

- `void OnAbort()`: OnAbort viene chiamato quando l'istanza del servizio senza stato viene arrestata in modo forzato. Questo metodo in genere viene chiamato quando viene rilevato un errore permanente sul nodo o quando Service Fabric non è in grado di gestire in modo affidabile il ciclo di vita dell'istanza del servizio a causa di errori interni.

## Ciclo di vita della replica del servizio con stato

Il ciclo di vita di una replica del servizio con stato è molto più complesso rispetto a quello di un'istanza del servizio senza stato. Oltre agli eventi di apertura, chiusura e interruzione, una replica del servizio con stato è soggetta a modifiche dei ruoli durante il ciclo di vita. Quando una replica del servizio con stato cambia ruolo, viene attivato l'evento `OnChangeRoleAsync`:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`: OnChangeRoleAsync viene chiamato quando il servizio con stato cambia ruolo, ad esempio primario o secondario. Alle repliche primarie viene assegnato lo stato di scrittura (sono autorizzate a creare le raccolte Reliable Collections e a scriverci). Alle repliche secondarie viene assegnato lo stato di lettura (possono solo leggere dalle raccolte Reliable Collections esistenti). È possibile avviare o aggiornare attività in background in risposta alle modifiche dei ruoli, come l'esecuzione di convalida di sola lettura, la generazione di report o il data mining in una replica secondaria.

In un servizio con stato solo la replica primaria ha accesso in scrittura allo stato ed è in genere il momento in cui il servizio esegue il lavoro effettivo. Il metodo `RunAsync` in un servizio con stato viene eseguito solo quando la replica del servizio con stato è primaria. Il metodo `RunAsync` viene annullato quando il ruolo della replica primaria non è più primario, nonché durante gli eventi di chiusura e interruzione.

L'uso dell'evento `OnChangeRoleAsync` permette di eseguire un lavoro a seconda del ruolo della replica e in risposta alla modifica del ruolo.

Un servizio con stato garantisce anche gli stessi quattro eventi del ciclo di vita di un servizio senza stato, con la stessa semantica e gli stessi casi d'uso:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## Passaggi successivi
Per argomenti più avanzati relativi a Service Fabric, vedere gli articoli seguenti:

- [Configurazione di Reliable Services con stato](service-fabric-reliable-services-configuration.md)

- [Introduzione all'integrità di Service Fabric](service-fabric-health-introduction.md)

- [Utilizzo dei report di integrità del sistema per la risoluzione dei problemi](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Configurazione dei servizi con Cluster Resource Manager di Service Fabric](service-fabric-cluster-resource-manager-configure-services.md)

<!---HONumber=AcomDC_0406_2016-->