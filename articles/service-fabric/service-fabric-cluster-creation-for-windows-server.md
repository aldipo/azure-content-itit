<properties
   pageTitle="Creare un cluster di Azure Service Fabric locale o nel cloud | Microsoft Azure"
   description="Informazioni su come creare un cluster di Azure Service Fabric su qualsiasi macchina (fisica o virtuale) che esegue Windows Server, sia locale o nel cloud."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="chackdan"/>


# Creare un cluster di Azure Service Fabric locale o nel cloud

Azure Service Fabric permette di creare cluster Service Fabric su qualsiasi macchina virtuale o computer con Windows Server in esecuzione. In questo modo, è possibile distribuire ed eseguire applicazioni di Service Fabric in qualsiasi ambiente in cui è presente un set interconnesso di computer Windows Server, in locale o con qualsiasi provider cloud. Service Fabric offre un pacchetto di installazione per la creazione di cluster Service Fabric.

Questo articolo illustra i passaggi per creare un cluster tramite il pacchetto autonomo per il Service Fabric locale, anche se può essere facilmente adattato a qualsiasi altro ambiente, ad esempio cloud.

>[AZURE.NOTE] Questa offerta autonoma è attualmente disponibile in versione di anteprima. [Fare clic qui](http://go.microsoft.com/fwlink/?LinkID=733084) per scaricare una copia del contratto di licenza.

<a id="downloadpackage"></a>
## Download del pacchetto autonomo Service Fabric


[Scaricare il pacchetto autonomo per Service Fabric per Windows Server 2012 R2](http://go.microsoft.com/fwlink/?LinkId=730690), denominato *Microsoft.Azure.ServiceFabric.WindowsServer.&lt;versione&gt;.zip*.

Nel pacchetto di download sono disponibili i seguenti file:

|**Nome file**|**Descrizione breve**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|Il file CAB che contiene i file binari da distribuire su ogni computer del cluster.|
|ClusterConfig.Unsecure.DevCluster.JSON|Esempio di file di configurazione del cluster che contiene tutte le impostazioni per un cluster di sviluppo non sicuro con una sola VM/computer e tre nodi, incluse le informazioni per ogni nodo appartenente al cluster. |
|ClusterConfig.Unsecure.MultiMachine.JSON|Esempio di file di configurazione del cluster che contiene tutte le impostazioni per il cluster, incluse le informazioni per ogni computer appartenente a un cluster non sicuro con più VM/computer.|
|ClusterConfig.Windows.DevCluster.JSON|Esempio di file di configurazione del cluster che contiene tutte le impostazioni per un cluster di sviluppo sicuro con una sola VM/computer e tre nodi, incluse le informazioni per ogni nodo appartenente al cluster. Il cluster è protetto tramite [identità di Windows](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.Windows.MultiMachine.JSON|Esempio di file di configurazione del cluster che contiene tutte le impostazioni per il cluster sicuro, incluse le informazioni per ogni computer appartenente a un cluster sicuro con più VM/computer. Il cluster è protetto tramite [identità di Windows](https://msdn.microsoft.com/library/ff649396.aspx).|
|ClusterConfig.x509.DevCluster.JSON|Esempio di file di configurazione del cluster che contiene tutte le impostazioni per un cluster di sviluppo sicuro con una sola VM/computer e tre nodi, incluse le informazioni per ogni nodo appartenente al cluster. Il cluster è protetto tramite certificati X.509 di Windows.|
|ClusterConfig.x509.MultiMachine.JSON|Esempio di file di configurazione del cluster che contiene tutte le impostazioni per il cluster sicuro, incluse le informazioni per ogni computer appartenente a un cluster sicuro con più VM/computer. Il cluster è protetto tramite certificati X.509 di Windows.|
|EULA.txt|Le condizioni di licenza per l'utilizzo del pacchetto autonomo Service Fabric di Microsoft Azure Service Fabric. Per scaricare una copia del contratto di licenza [fare clic qui](http://go.microsoft.com/fwlink/?LinkID=733084).|
|Readme.txt|Collegamento alle note sulla versione e alle istruzioni di installazione base. Si tratta di un piccolo sottoinsieme delle istruzioni illustrate su questa pagina.|
|CreateServiceFabricCluster.ps1|Script di PowerShell per la rimozione del cluster tramite le impostazioni nel file ClusterConfig.JSON.|
|RemoveServiceFabricCluster.ps1|Lo script di PowerShell per la rimozione del cluster utilizzando le impostazioni nel file ClusterConfig.JSON.|
|AddNode.ps1|Script di PowerShell per cluster che aggiunge un nodo a un cluster esistente|
|RemoveNode.ps1|Script di PowerShell per cluster che rimuove un nodo da un cluster|


## Pianificazione e preparazione per la distribuzione del cluster
I passaggi seguenti devono essere eseguiti prima di creare il cluster.

### Passaggio 1: pianificazione dell’infrastruttura del cluster
Si sta per creare un cluster Service Fabric sulle proprie macchine, quindi occorre stabilire a quali tipi di errore il cluster deve sopravvivere. Ad esempio, sono necessarie linee di alimentazione o connessioni Internet separate per queste macchine? Inoltre, occorre considerare anche la sicurezza fisica di tali macchine. Dove si trova fisicamente la macchina e chi deve accedervi? Dopo aver preso queste decisioni, eseguire il mapping logico delle macchine ai vari domini di errore (vedere di seguito per altre informazioni). L'infrastruttura di pianificazione per i cluster di produzione sarà molto più coinvolta rispetto a quella dei cluster di test.

### Passaggio 2: preparazione delle macchine per soddisfare i prerequisiti
Prerequisiti per ogni macchina da aggiungere al cluster:

- Consigliati minimo 2 GB di memoria
- Connettività di rete: verificare che le macchine siano su una rete o su più reti protette
- Windows Server 2012 R2 o Windows Server 2012 (è necessaria l’installazione di KB2858668 per questo componente).
- .NET Framework 4.5.1 o versione successiva, installazione completa
- Windows PowerShell 3.0
- L'amministratore del cluster che distribuisce e configura il cluster deve avere privilegi da amministratore su ogni computer.
- Il servizio RemoteRegistry deve essere in esecuzione su tutti i computer.

### Passaggio 3: determinare le dimensioni iniziali del cluster
Ogni nodo è costituito da uno stack Service Fabric completo ed è un singolo membro del cluster Service Fabric. In una tipica distribuzione di Service Fabric è presente un nodo per ogni istanza del sistema operativo (fisica o virtuale). La dimensione del cluster è determinata dalle esigenze aziendali, anche se è necessario disporre di una dimensione minima del cluster di tre nodi (macchine/VM). A scopi di sviluppo, è possibile configurare più di un nodo in una macchina specifica. In un ambiente di produzione, il Service Fabric supporta solo un nodo per ogni macchina virtuale o fisica.

### Passaggio 4: identificazione del numero di domini di errore e di aggiornamento
Un **dominio di errore** è un'unità fisica di errore ed è direttamente correlato all'infrastruttura fisica nei data center. Un dominio di errore comprende componenti hardware (computer, commutatori e altro) che condividono un singolo punto di errore. Sebbene non sia presente una mappatura 1:1 tra domini di errore e rack, ogni rack può essere considerato in senso lato un dominio di errore. Quando si esaminano i nodi nel cluster, è consigliabile distribuire i nodi tra gli ultimi tre domini di errore.

Quando si specificano domini di errore nel file *ClusterConfig.JSON* è possibile scegliere il nome del dominio di errore. Il Service Fabric supporta i domini di errore gerarchici, in modo che possano rispecchiare la topologia infrastrutturale. Ad esempio, è consentito quanto segue:

- "faultDomain": "fd:/Room1/Rack1/Machine1"
- "faultDomain": "fd:/FD1"
- "faultDomain": "fd:/Room1/Rack1/PDU1/M1"


Un **dominio di aggiornamento** è un'unità logica di nodi. Durante un aggiornamento orchestrato di Service Fabric (aggiornamento di un'applicazione o del cluster), tutti i nodi in un dominio di aggiornamento vengono resi inattivi per eseguire l'aggiornamento, mentre i nodi in altri domini di aggiornamento rimangono disponibili per soddisfare le richieste. L'aggiornamento del firmware sulle macchine non rispetta i domini di aggiornamento ed è pertanto necessario eseguirli su una macchina alla volta.

Il metodo più semplice per descrivere questi concetti è considerare i domini di errore come unità degli errori imprevisti e i domini di aggiornamento come unità della manutenzione pianificata.

Quando si specificano domini di aggiornamento nel file *ClusterConfig.JSON* è possibile scegliere il nome del dominio di aggiornamento. Ad esempio, sono ammessi tutti i seguenti domini:

- "upgradeDomain": "UD0"
- "upgradeDomain": "UD1A"
- "upgradeDomain": "DomainRed"
- "upgradeDomain": "Blue"

### Passaggio 5: download del pacchetto autonomo per il Service Fabric per Windows Server
[Scaricare il pacchetto autonomo per Service Fabric per Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) e decomprimere il pacchetto in una macchina di distribuzione non appartenente al cluster o in una delle macchine che faranno parte del cluster.

<a id="createcluster"></a>
## Creazione del cluster

Dopo aver seguito la procedura della sezione precedente di pianificazione e preparazione, è possibile creare il cluster.

### Passaggio 1: modificare la configurazione del cluster
Aprire il file *ClusterConfig.JSON* dal pacchetto scaricato. È possibile utilizzare qualsiasi editor per modificare le impostazioni seguenti:

|**Impostazioni di configurazione**|**Descrizione**|
|-----------------------|--------------------------|
|NodeTypes|I tipi di nodo permettono di separare i nodi del cluster in diversi gruppi. Un cluster deve avere almeno un NodeType. Tutti i nodi in un gruppo possiedono le seguenti caratteristiche comuni. <br> *Name*: nome del tipo di nodo. <br>*EndPoints*: endpoint con nomi diversi (porte) associati a questo tipo di nodo. È possibile usare qualsiasi numero di porta, purché non sia in conflitto con altri elementi di questo manifesto e non sia già usato da altri programmi nel computer o nella macchina virtuale <br> *PlacementProperties*: descrivono le proprietà per questo tipo di nodo che verranno poi usate come vincoli di posizionamento per i servizi di sistema o i propri servizi. Queste proprietà sono coppie chiave/valore definite dall'utente che forniscono metadati aggiuntivi per un determinato nodo. Tra le proprietà del nodo è possibile elencare la presenza o meno di un'unità disco rigido o di una scheda grafica, il numero di spindle del disco rigido, i core e altre proprietà fisiche. <br> *Capacità*: le capacità del nodo definiscono il nome e la quantità di una particolare risorsa utilizzabile da parte di un determinato nodo. Ad esempio, un nodo può definire la propria capacità per una metrica denominata "MemoryInMb" con un valore predefinito di 2048 MB di memoria disponibile. Queste capacità sono utilizzate in fase di esecuzione per garantire che i servizi che richiedono una determinata quantità di risorse vengano inseriti nei nodi con tali risorse a disposizione.|
|Nodi|I dettagli per ciascun nodo che farà parte del cluster (tipo di nodo, nome del nodo, indirizzo IP, dominio di errore e dominio di aggiornamento del nodo). Le macchine sulle quali deve essere creato il cluster devono essere elencate qui con il rispettivo indirizzo IP. <br> Se si usano gli stessi indirizzi IP per tutti i nodi, verrà creato un cluster di una casella che potrà essere usato per scopi di test. Non utilizzare cluster di una casella per la distribuzione dei carichi di lavoro di produzione.|

### Passaggio 2: esecuzione dello script di creazione del cluster
Dopo aver modificato la configurazione del cluster nel documento JSON e aggiunto tutte le relative le informazioni sul nodo, eseguire lo script PowerShell di creazione del cluster dalla cartella del pacchetto e passare il percorso al file di configurazione e il percorso della radice del pacchetto.

Questo script può essere eseguito su qualsiasi macchina con accesso da amministratore a tutte le macchine elencate come nodi nel file di configurazione del cluster. La macchina sulla quale viene eseguito questo script può far parte o meno del cluster.

```
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.JSON -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
```

>[AZURE.NOTE] I log di distribuzione sono disponibili localmente sulla VM/computer su cui si esegue il comando PowerShell CreateServiceFabricCluster. Si trovano in una sottocartella denominata "DeploymentTraces" all'interno della cartella da cui è stato eseguito il comando PS.

## Aggiungere nodi al cluster Service Fabric 

1. Preparare la VM/computer da aggiungere al cluster (vedere il passaggio 2 della procedura nella precedente sezione Pianificazione e preparazione per la distribuzione del cluster). 
2. Pianificare a quale dominio di errore e dominio di aggiornamento aggiungere questa VM/ computer.
3. [Scaricare il pacchetto autonomo per Service Fabric per Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) e decomprimerlo sulla VM/computer che si pensa di aggiungere al cluster. 
4. Aprire un prompt di amministrazione di PowerShell e passare al percorso del pacchetto decompresso
5. Eseguire AddNode.PS1

```
.\AddNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClusterConnectionEndPoint 182.17.34.52:19000 -UpgradeDomain UD1 -FaultDomain FD1
```

## Rimuovere nodi dal cluster Service Fabric 

1. Eseguire TS sulla VM/computer da rimuovere dal cluster
2. Aprire un prompt di amministrazione di PowerShell e passare al percorso del pacchetto decompresso
5. Eseguire RemoveNode.PS1

```
.\RemoveNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -ExistingClusterConnectionEndPoint 182.17.34.52:19000
```

## Eliminare il cluster Service Fabric 
1. Eseguire TS su una delle VM/computer appartenenti al cluster
2. Aprire un prompt di amministrazione di PowerShell e passare al percorso del pacchetto decompresso
5. Eseguire RemoveNode.PS1

```
.\RemoveNode.ps1 -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab -ExistingClusterConnectionEndPoint 182.17.34.52:19000
```


## Passaggi successivi
- [Informazioni sulla sicurezza del cluster](service-fabric-cluster-security.md)
- [Service Fabric SDK e introduzione](service-fabric-get-started.md)
- [Gestione delle applicazioni di Infrastruttura di servizi in Visual Studio](service-fabric-manage-application-in-visual-studio.md).
- [Panoramica della funzionalità di creazione di cluster autonomi e confronto con i cluster gestiti da Azure](service-fabric-deploy-anywhere.md)

<!---HONumber=AcomDC_0622_2016-->