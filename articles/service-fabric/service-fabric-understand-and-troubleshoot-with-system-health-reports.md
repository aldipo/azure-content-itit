<properties
   pageTitle="Risoluzione dei problemi con i report sull'integrità del sistema | Microsoft Azure"
   description="Descrive i report di integrità inviati dai componenti dell’Infrastruttura dei servizi di Azure e il relativo utilizzo per la risoluzione dei problemi del cluster o dei problemi delle applicazioni."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/25/2016"
   ms.author="oanapl"/>

# Usare i report sull'integrità del sistema per la risoluzione dei problemi

I componenti di Azure Service Fabric forniscono report predefiniti su tutte le entità del cluster. L'[archivio integrità](service-fabric-health-introduction.md#health-store) crea ed elimina le entità in base ai report di sistema. Le organizza anche in una gerarchia che acquisisce le interazioni delle entità.

> [AZURE.NOTE] Per comprendere i concetti correlati all'integrità, vedere altre informazioni sul [modello di integrità di Service Fabric](service-fabric-health-introduction.md).

I report sull'integrità del sistema forniscono la visibilità delle funzionalità del cluster e dell'applicazione e contrassegnano i problemi riscontrati tramite a livello di integrità. Per le applicazioni e i servizi i report sull'integrità del sistema verificano che le entità siano implementate e si comportino correttamente dal punto di vista di Service Fabric. I report non forniscono il monitoraggio dell'integrità della logica di business del servizio o il rilevamento dei processi bloccati. I servizi utente possono arricchire i dati di integrità con informazioni specifiche per la logica.

> [AZURE.NOTE] I report sull'integrità dei watchdog sono visibili solo *dopo* che i componenti di sistemala hanno creato un'entità. Quando si elimina un'entità, l'archivio integrità elimina automaticamente tutti i report sull'integrità ad essa associati. Lo stesso vale quando si crea una nuova istanza dell'entità, ad esempio, viene creata una nuova istanza di replica del servizio. Tutti i report associati all'istanza precedente vengono eliminati e rimossi dall'archivio.

I report sui componenti di sistema vengono identificati dall'origine, che inizia con il prefisso "**System**". I watchdog non possono usare lo stesso prefisso per le proprie origini, perché i report con parametri non validi saranno rifiutati. Si osserveranno alcuni report di sistema per capire da quali eventi vengono attivati e come risolvere gli eventuali problemi che rappresentano.

> [AZURE.NOTE] Service Fabric continua ad aggiungere report sulle condizioni di interesse che possono migliorare la visibilità degli eventi che si verificano nel cluster o nell'applicazione.

## Report sull'integrità del sistema cluster
L'entità integrità del cluster viene creata automaticamente nell'archivio integrità, quindi se tutto funziona correttamente non ha un report di sistema.

### Perdita di nodi vicini
**System.Federation** segnala un errore quando rileva una perdita di nodi vicini. Il report è relativo a singoli nodi e l'ID del nodo è incluso nel nome della proprietà. Se si verifica la perdita di un nodo vicino nell'intero anello di Service Fabric, in genere è possibile prevedere due eventi, ovvero il report viene generato da entrambi i lati del gap. In caso di perdita di più nodi vicini, si verificheranno più eventi.

Il report specifica il timeout di lease globale come durata (TTL). Il report viene inviato di nuovo ogni metà della durata TTL finché la condizione rimane attiva. L'evento viene rimosso automaticamente alla scadenza, quindi se il nodo a cui si riferisce il report è inattivo, viene comunque rimosso correttamente dall'archivio integrità.

- **SourceId**: System.Federation
- **Proprietà**: inizia con **Neighborhood** e include informazioni sul nodo.
- **Passaggi successivi**: analizzare il motivo della perdita del nodo vicino. Ad esempio, controllare la comunicazione tra i nodi del cluster.

## Report sull'integrità del sistema di nodi
**System.FM**, che rappresenta il servizio Gestione failover, è l'autorità che gestisce le informazioni sui nodi del cluster. Ogni nodo deve avere un report generato da System.FM che mostra il relativo stato. Le entità del nodo vengono rimosse quando viene rimosso lo stato del nodo. Ad esempio, vedere [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx).

### Nodo attivo/inattivo
System.FM restituisce OK quando il nodo viene aggiunto all'anello, ovvero è operativo. Segnala un errore quando il nodo non fa più parte dell'anello, ovvero è inattivo perché è in corso un aggiornamento o semplicemente perché si è verificato un errore. La gerarchia di integrità creata dall'archivio integrità agisce sulle entità distribuite in correlazione con i report sui nodi di System.FM. Considera il nodo un elemento padre virtuale di tutte le entità distribuite. Le entità distribuite in tale nodo non vengono esposte tramite query se il nodo è inattivo o non segnalato oppure se il nodo ha un'istanza diversa rispetto a quella associata alle entità. Quando System.FM segnala che il nodo è inattivo o riavviato (nuova istanza), l'archivio integrità elimina automaticamente le entità distribuite eventualmente esistenti solo nel nodo inattivo o nell'istanza precedente del nodo.

- **SourceId**: System.FM
- **Proprietà**: State
- **Passaggi successivi**: se il nodo è inattivo per un aggiornamento, deve tornare attivo dopo l'aggiornamento. In questo caso, lo stato di integrità deve essere riportato su OK. Se il nodo non ritorna attivo o in caso di errore, è necessario proseguire nell'analisi del problema.

Di seguito è illustrato l'evento System.FM con uno stato di integrità OK per il nodo attivo:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### Scadenza dei certificati
**System.FabricNode** segnala una condizione di avviso quando si avvicina la scadenza dei certificati usati dal nodo. Ogni nodo ha tre certificati: **Certificate\_cluster**, **Certificate\_server** e **Certificate\_default\_client**. Quando mancano almeno due settimane alla scadenza, lo stato di integrità del report è OK. Quando la scadenza è entro due settimane, il tipo di report è un avviso. Il valore TTL di questi eventi è infinito e vengono rimossi quando un nodo esce dal cluster.

- **SourceId**: System.FabricNode
- **Proprietà**: inizia con **Certificate** e contiene altre informazioni sul tipo di certificato.
- **Passaggi successivi**: aggiornare i certificati se sono prossimi alla scadenza.

### Violazione della capacità di carico
Il servizio di bilanciamento del carico di Service Fabric segnala una condizione di avviso se rileva una violazione della capacità del nodo.

 - **SourceId**: System.PLB
 - **Proprietà**: inizia con **Capacity**.
 - **Passaggi successivi**: controllare la metrica fornita e visualizzare la capacità corrente nel nodo.

## Report sull'integrità del sistema di applicazioni
**System.CM**, che rappresenta il servizio Cluster Manager, è l'autorità che gestisce le informazioni sull'applicazione.

### Stato
System.CM restituisce OK quando l'applicazione viene creata o aggiornata. Informa l'archivio integrità quando l'applicazione viene eliminata, in modo che possa essere rimossa dall'archivio.

- **SourceId**: System.CM
- **Proprietà**: State
- **Passaggi successivi**: se l'applicazione è stata creata, deve includere il report sull'integrità di Cluster Manager. In caso contrario, controllare lo stato dell'applicazione eseguendo una query, ad esempio con il cmdlet **Get-ServiceFabricApplication -ApplicationName *applicationName*** di PowerShell.

Di seguito è illustrato l'evento State nell'applicazione **fabric:/WordCount**:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## Report sull'integrità del sistema di servizi
**System.FM**, che rappresenta il servizio Gestione failover, è l'autorità che gestisce le informazioni sui servizi.

### Stato
System.FM restituisce OK quando il servizio viene creato. Elimina l'entità dall'archivio integrità quando il servizio viene eliminato.

- **SourceId**: System.FM
- **Proprietà**: State

Di seguito è illustrato l'evento State nel servizio **fabric:/WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### Violazione di repliche non collocate
**System.PLB** segnala una condizione di avviso se non riesce a trovare una posizione per una o più repliche del servizio. Il report viene rimosso alla scadenza.

- **SourceId**: System.FM
- **Proprietà**: State
- **Passaggi successivi**: controllare i vincoli del servizio e lo stato corrente della posizione.

Il codice seguente illustra una violazione per un servizio configurato con 7 repliche di destinazione in un cluster con 5 nodi:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## Report sull'integrità del sistema di partizioni
**System.FM**, che rappresenta il servizio Gestione failover, è l'autorità che gestisce le informazioni sulle partizioni del servizio.

### Stato
System.FM restituisce OK quando la partizione viene creata ed è integra. Elimina l'entità dall'archivio integrità quando la partizione viene eliminata.

Se il numero di repliche della partizione è inferiore al minimo, viene segnalata una condizione di errore. Se il numero di repliche della partizione non è inferiore al minimo, ma è al di sotto del numero di repliche di destinazione, viene segnalata una condizione di avviso. Se la partizione è in una condizione di perdita del quorum, System.FM segnala un errore.

Altri eventi importanti includono un avviso quando le operazioni di riconfigurazione e di compilazione richiedono più tempo del previsto. I tempi previsti per la compilazione e la riconfigurazione sono configurabili in base agli scenari del servizio. Ad esempio, se un servizio ha uno stato pari a un terabyte, ad esempio un database SQL, la compilazione richiederà più tempo rispetto a un servizio con una quantità di stato limitata.

- **SourceId**: System.FM
- **Proprietà**: State
- **Passaggi successivi**: se lo stato di integrità non è OK, è possibile che alcune repliche non vengano create, aperte o alzate di livello, primario o secondario, nel modo corretto. In molti casi, la causa radice è un bug del servizio nell'implementazione del ruolo di apertura o modifica.

Di seguito è illustrata una partizione integra.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Di seguito viene illustrata l’integrità di una partizione che è al di sotto del conteggio delle repliche di destinazione. Il passaggio successivo consiste nell'ottenere la descrizione della partizione, che mostra come è configurata: **MinReplicaSetSize** corrisponde a due e **TargetReplicaSetSize** a sette. Ottenere quindi il numero di nodi nel cluster: cinque. In questo caso, due repliche non possono essere posizionate.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### Violazione del vincolo di replica
**System.PLB** segnala una condizione di avviso se rileva una violazione del vincolo di replica e non può posizionare repliche della partizione.

- **SourceId**: System.PLB
- **Proprietà**: inizia con **ReplicaConstraintViolation**

## Report sull'integrità del sistema di repliche
**System.RA**, che rappresenta il componente agente di riconfigurazione, è l'autorità per lo stato della replica.

### Stato
**System.RA** restituisce OK quando viene creata la replica.

- **SourceId**: System.RA
- **Proprietà**: State

Di seguito viene illustrata una replica integra:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### Stato aperto della replica
La descrizione di questo report sull'integrità include l'ora (UTC) in cui è iniziata l'esecuzione della chiamata API.

**System.RA** segnala una condizione di avviso se l'apertura della replica richiede più tempo del periodo configurato (impostazione predefinita: 30 minuti). Se l'API influisce sulla disponibilità del servizio, il report viene eseguito molto più rapidamente. L'intervallo di tempo è configurabile, con valore predefinito di 30 secondi. Include il tempo impiegato per l'apertura del replicatore e del servizio. Se l'apertura viene completata, la proprietà restituisce OK.

- **SourceId**: System.RA
- **Proprietà**: **ReplicaOpenStatus**
- **Passaggi successivi**: se lo stato di integrità non è OK, verificare per quale motivo l'apertura della replica richiede più tempo del previsto.

### Chiamata API del servizio lenta
**System.RAP** e **System.Replicator** segnalano una condizione di avviso se una chiamata al codice del servizio utente richiede più tempo di quello configurato. L'avviso viene cancellato al completamento della chiamata.

- **SourceId**: System.RAP o System.Replicator
- **Proprietà**: nome dell'API lenta. La descrizione fornisce altri dettagli sull'ora in cui l'API è rimasta in sospeso.
- **Passaggi successivi**: esaminare il motivo per cui la chiamata richiede più tempo del previsto.

L'esempio seguente illustra una partizione con perdita di quorum e i passaggi di analisi eseguiti per capire il motivo. Una delle repliche presenta uno stato di integrità di avviso, che ne indica l'integrità. Mostra che l'operazione di servizio richiede più tempo del previsto, un evento segnalato da System.RAP. Dopo aver ottenuto queste informazioni, il passaggio successivo consiste nell'esaminare il codice del servizio e procedere con l'analisi. In questo caso, l'implementazione di **RunAsync** del servizio con stato genera un'eccezione non gestita. Si noti che viene eseguito il riciclo delle repliche, quindi è possibile che non vengano visualizzate repliche con stato di avviso. È possibile provare di nuovo a ottenere lo stato di integrità e cercare eventuali differenze nell'ID replica. In alcuni casi, si ottengono indicazioni utili.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Quando si avvia l'applicazione che presenta errori nel debugger, le finestre degli eventi di diagnostica mostrano l'eccezione generata da RunAsync:

![Eventi di diagnostica di Visual Studio 2015: errore RunAsync in fabric:/HelloWorldStatefulApplication.][1]

Eventi di diagnostica di Visual Studio 2015: errore RunAsync in **fabric:/HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### Coda di replica piena
**System.Replicator** segnala una condizione di avviso se la coda di replica è piena. Nel server primario ciò accade di solito perché una o più repliche secondarie sono lente nel riconoscere le operazioni. Nel server secondario ciò si verifica di solito quando il servizio è lento nell'applicare le operazioni. La condizione di avviso viene cancellata quando la coda non è più piena.

- **SourceId**: System.Replicator
- **Proprietà**: **PrimaryReplicationQueueStatus** o **SecondaryReplicationQueueStatus**, a seconda del ruolo della replica

## Report sull'integrità del sistema DeployedApplication
**System.Hosting** è l'autorità per le entità distribuite.

### Activation
System.Hosting restituisce OK quando un'applicazione viene attivata correttamente nel nodo. In caso contrario, restituisce un errore.

- **SourceId**: System.Hosting
- **Proprietà**: Activation, compresa la versione di implementazione
- **Passaggi successivi**: se l'applicazione non è integra, provare ad analizzare i motivi per cui l'attivazione non è riuscita.

Di seguito viene illustrata l'attivazione riuscita:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### Scaricare
**System.Hosting** segnala un errore se il download del pacchetto dell'applicazione non è riuscito.

- **SourceId**: System.Hosting
- **Proprietà**: **Download:*RolloutVersion***
- **Passaggi successivi**: ricercare la causa del download non riuscito nel nodo.

## Report sull'integrità del sistema DeployedServicePackage
**System.Hosting** è l'autorità per le entità distribuite.

### Attivazione del pacchetto di servizi
System.Hosting restituisce OK se l'attivazione del pacchetto di servizi nel nodo è riuscita. In caso contrario, restituisce un errore.

- **SourceId**: System.Hosting
- **Proprietà**: Activation
- **Passaggi successivi**: verificare per quale motivo l'attivazione non è riuscita.

### Attivazione del pacchetto di codice
**System.Hosting** restituisce OK per ogni pacchetto di codice se l'attivazione riesce. In caso contrario, restituisce l'avviso configurato. Se l'attivazione di **CodePackage** non riesce o termina con un errore superiore alla soglia configurata per **CodePackageHealthErrorThreshold**, viene restituito un errore. Se sono presenti più pacchetti di codice in un pacchetto del servizio, verrà generato un report sull'attivazione per ogni pacchetto.

- **SourceId**: System.Hosting
- **Proprietà**: usa il prefisso **CodePackageActivation** e contiene il nome del pacchetto di codice e il punto di ingresso come **CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint***, ad esempio **CodePackageActivation:Code:SetupEntryPoint**

### Registrazione del tipo di servizio
**System.Hosting** restituisce OK se il tipo di servizio è stato registrato correttamente. Viene restituito un errore se la registrazione non è stata eseguita in tempo, secondo quanto configurato con **ServiceTypeRegistrationTimeout**. L'annullamento della registrazione del tipo di servizio nel nodo è causato dalla chiusura del runtime. Hosting genera un avviso.

- **SourceId**: System.Hosting
- **Proprietà**: usa il prefisso **ServiceTypeRegistration** e contiene il nome del tipo di servizio, ad esempio **ServiceTypeRegistration:FileStoreServiceType**

Di seguito è illustrato un pacchetto del servizio distribuito integro.

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### Scaricare
**System.Hosting** segnala un errore se il download del pacchetto del servizio non è riuscito.

- **SourceId**: System.Hosting
- **Proprietà**: **Download:*RolloutVersion***
- **Passaggi successivi**: ricercare la causa del download non riuscito nel nodo.

### Convalida dell’aggiornamento
**System.Hosting** segnala un errore se la convalida durante l'aggiornamento non riesce o se l'aggiornamento nel nodo non riesce.

- **SourceId**: System.Hosting
- **Proprietà**: usa il prefisso **FabricUpgradeValidation** e contiene la versione dell'aggiornamento.
- **Descrizione**: punta all'errore che si è verificato.

## Passaggi successivi
[Come visualizzare i report sull'integrità di Service Fabric](service-fabric-view-entities-aggregated-health.md)

[Monitorare e diagnosticare servizi in locale](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Aggiornamento di un'applicazione di infrastruttura di servizi](service-fabric-application-upgrade.md)

<!---HONumber=AcomDC_0427_2016-->