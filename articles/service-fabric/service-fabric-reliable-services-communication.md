<properties
   pageTitle="Panoramica sulla comunicazione di Reliable Services | Microsoft Azure"
   description="Panoramica sul modello di comunicazione di Reliable Services, compresi i listener di apertura nei servizi, gli endpoint di risoluzione e la comunicazione tra servizi."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="03/25/2016"
   ms.author="vturecek"/>

# Come usare le API di comunicazione di Reliable Services

Azure Service Fabric è una piattaforma completamente indipendente dalle comunicazioni tra servizi. Sono accettabili tutti i protocolli e gli stack, da UDP a HTTP. Dipende dallo sviluppatore del servizio scegliere la modalità di comunicazione tra servizi. Il framework applicazione di Reliable Services offre gli stack di comunicazione predefiniti nonché le API che è possibile usare per creare componenti di comunicazione personalizzati.

## Impostare la comunicazione tra servizi

L'API di Reliable Services usa una semplice interfaccia per la comunicazione tra servizi. Per aprire un endpoint del servizio, è sufficiente implementare questa interfaccia:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Aggiungere quindi l'implementazione del listener di comunicazione restituendola in un override del metodo della classe di base del servizio.

Per i servizi senza stato:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Per i servizi con stato:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In entrambi i casi restituire una raccolta di listener. Ciò consente al servizio di rimanere in ascolto su più endpoint, potenzialmente tramite protocolli diversi, usando più listener. Può essere ad esempio presente un listener HTTP e un listener WebSocket separato. Ogni listener riceve un nome e la raccolta risultante di coppie *nome : indirizzo* è rappresentata come oggetto JSON quando un client richiede gli indirizzi di ascolto per un'istanza o una partizione del servizio.

In un servizio senza stato l'override restituisce una raccolta di ServiceInstanceListener. L'oggetto ServiceInstanceListener contiene una funzione per creare l'interfaccia ICommunicationListener e assegnargli un nome. Per i servizi con stato l'override restituisce una raccolta di ServiceReplicaListener. Questo è un comportamento leggermente diverso rispetto a quello del servizio senza stato, perché l'oggetto ServiceReplicaListener dispone di un'opzione per aprire l'interfaccia ICommunicationListener nelle repliche secondarie. Ciò consente non solo di usare più listener di comunicazione in un servizio, ma anche di specificare quali listener accettano le richieste nelle repliche secondarie e quali si limitano a rimanere in ascolto sulle repliche primarie.

Ad esempio un oggetto ServiceRemotingListener può gestire le chiamate RPC solo sulle repliche primarie, mentre un secondo listener personalizzato può accettare le richieste di lettura sulle repliche secondarie:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomListener(context),
            "customReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

Infine, descrivere gli endpoint necessari per il servizio tramite il [manifesto del servizio](service-fabric-application-model.md) nella sezione relativa agli endpoint.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Il listener di comunicazione può accedere alle risorse di endpoint allocate da `CodePackageActivationContext` in `ServiceContext`. Quando viene aperto, il listener può avviare l'ascolto delle richieste.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Le risorse di endpoint sono comuni all'intero pacchetto del servizio e vengono allocate da Service Fabric quando viene attivato il pacchetto del servizio. Più repliche di servizio ospitate nella stessa istanza di ServiceHost possono condividere la stessa porta. Ciò significa che il listener di comunicazione deve supportare la condivisione delle porte. Il metodo consigliato per eseguire questa operazione consiste nell'uso dell'ID partizione e dell'ID replica/istanza da parte del listener di comunicazione durante la generazione dell'indirizzo di ascolto.

### Registrazione dell'indirizzo del servizio

Un servizio di sistema chiamato *servizio Naming* viene eseguito nei cluster di Service Fabric. Il servizio Naming è un registrar per i servizi e i relativi indirizzi su cui è in ascolto ogni istanza o replica del servizio. Quando il metodo `OpenAsync` di un oggetto `ICommunicationListener` viene completato, il relativo valore restituito viene registrato nel servizio Naming. Questo valore restituito che viene pubblicato nel servizio Naming è una stringa il cui valore può essere costituito da qualsiasi valore. Questo valore stringa è il valore che verrà visualizzato dai client quando richiedono al servizio Naming un indirizzo per il servizio.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Service Fabric fornisce le API che consentono ai client e ad altri servizi di richiedere questo indirizzo in base al nome del servizio. Questo aspetto è importante perché l'indirizzo del servizio non è statico. I servizi vengono spostati nel cluster ai fini del bilanciamento del carico e della disponibilità delle risorse. Questo meccanismo consente ai client di risolvere l'indirizzo di ascolto per un servizio.

> [AZURE.NOTE] Per una procedura completa su come scrivere un oggetto `ICommunicationListener`, vedere l'articolo relativo ai [servizi API Web di Service Fabric con self-hosting OWIN](service-fabric-reliable-services-communication-webapi.md).

## Comunicazione con un servizio
L'API di Reliable Services offre le librerie seguenti per la scrittura di client che comunicano con i servizi.

### Risoluzione degli endpoint di servizio
Il primo passaggio per la comunicazione con un servizio è risolvere un indirizzo di endpoint della partizione o dell'istanza del servizio con cui si vuole comunicare. La classe di utilità `ServicePartitionResolver` è una classe primitiva di base che consente ai client di determinare l'endpoint di un servizio in fase di esecuzione. Nella terminologia di Service Fabric il processo di determinazione dell'endpoint di un servizio è denominato *risoluzione degli endpoint del servizio*.

Per connettersi ai servizi all'interno di un cluster, è possibile creare la classe `ServicePartitionResolver` senza parametri:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver();
```

Per connettersi ai servizi in un cluster diverso, è possibile creare la classe `ServicePartitionResolver` con un set di endpoint del gateway del cluster. Si noti che gli endpoint del gateway sono solo endpoint diversi per la connessione allo stesso cluster. Ad esempio:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alla classe `ServicePartitionResolver` è possibile assegnare la funzione per la creazione di un oggetto `FabricClient` da usare internamente.
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient` è l'oggetto usato per comunicare con il cluster di Service Fabric per varie operazioni di gestione nel cluster. Ciò è utile quando si vuole avere un maggiore controllo sul modo in cui `ServicePartitionClient` interagisce con il cluster. `FabricClient` esegue la memorizzazione nella cache internamente ed è in genere dispendioso da creare, quindi è importante riusare le istanze di `FabricClient` quanto più possibile.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Un metodo di risoluzione viene quindi usato per recuperare l'indirizzo di un servizio o una partizione del servizio per i servizi partizionati.

```csharp
ServicePartitionResolver resolver = new ServicePartitionResolver();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

L'indirizzo di un servizio può essere risolto facilmente con `ServicePartitionResolver`, ma per garantire il corretto utilizzo dell'indirizzo risolto è necessaria una procedura più complessa. Il client dovrà rilevare se il tentativo di connessione non è riuscito a causa di un errore temporaneo (ad esempio, nel caso in cui il servizio è stato spostato o è temporaneamente non disponibile) ed è quindi possibile eseguire un nuovo tentativo o a causa di un errore permanente (ad esempio, nel caso in cui il servizio è stato eliminato o la risorsa richiesta non esiste più). Le istanze del servizio o le repliche possono essere spostate da un nodo all'altro in qualsiasi momento per vari motivi. L'indirizzo del servizio risolto tramite `ServicePartitionResolver` potrebbe essere non aggiornato quando il codice client prova a connettersi. In tal caso il client dovrà risolvere di nuovo l'indirizzo. Fornendo l'oggetto `ResolvedServicePartition` precedente si indica che il resolver deve riprovare anziché recuperare semplicemente un indirizzo memorizzato nella cache.

In genere, non è necessario che il codice client funzioni direttamente con l'oggetto `ServicePartitionResolver`. Il codice viene creato e passato alle factory di client di comunicazione nell'API di Reliable Services. Le factory usano il resolver internamente per generare un oggetto client da poter usare per comunicare con i servizi.

### Client e factory di comunicazione

La libreria di factory di comunicazione implementa un tipico schema di ripetizione dei tentativi per la gestione degli errori che semplifica la ripetizione dei tentativi di connessioni agli endpoint del servizio risolti. La libreria di factory fornisce il meccanismo di ripetizione dei tentativi mentre l'utente fornisce i gestori di errori.

`ICommunicationClientFactory` definisce l'interfaccia di base implementata da una factory di client di comunicazione che produce client in grado di comunicare con un servizio di Service Fabric. L'implementazione dell'oggetto CommunicationClientFactory dipende dallo stack di comunicazione usato dal servizio di Service Fabric con cui il client vuole comunicare. L'API di Reliable Services include un oggetto `CommunicationClientFactoryBase<TCommunicationClient>`, che offre un'implementazione di base dell'interfaccia `ICommunicationClientFactory` e consente di eseguire attività comuni a tutti gli stack di comunicazione. Tali attività includono l'uso di un resolver `ServicePartitionResolver` per determinare l'endpoint del servizio. I client in genere implementano la classe astratta CommunicationClientFactoryBase per gestire la logica specifica dello stack di comunicazione.

Il client di comunicazione riceve solo un indirizzo e lo usa per connettersi a un servizio. Il client può usare qualsiasi protocollo desidera.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

La factory del client è responsabile principalmente della creazione dei client di comunicazione. Per i client che non mantengono una connessione permanente, ad esempio un client HTTP, la factory deve solo creare e restituire il client. Gli altri protocolli che mantengono una connessione permanente, ad esempio alcuni protocolli binari, devono essere anche convalidati dalla factory per determinare se la connessione deve essere ricreata.

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Infine, un gestore di eccezioni è responsabile per determinare l'azione da intraprendere quando si verifica un'eccezione. Le eccezioni vengono suddivise in due categorie: **con possibilità di ritentare** e **senza possibilità di ritentare**.

 - Le eccezioni **senza possibilità di ritentare** vengono restituite semplicemente al chiamante. 
 - Le eccezioni **con possibilità di ritentare** vengono a loro volta suddivise in due categorie: **temporanee** e **non temporanee**.
  - Le eccezioni **temporanee** sono quelle che possono essere semplicemente ritentate senza risolvere di nuovo l'indirizzo dell'endpoint del servizio. Includono problemi di rete temporanei o risposte di errore del servizio diversi da quelli che indicano che l'indirizzo dell'endpoint del servizio non esiste. 
  - Le eccezioni **non temporanee** sono quelle per cui è necessario risolvere di nuovo l'indirizzo dell'endpoint del servizio. Includono eccezioni che indicano che l'endpoint del servizio non è raggiungibile, ad esempio perché il servizio è stato spostato in un altro nodo. 

L'oggetto `TryHandleException` prende una decisione su una determinata eccezione. Se **non sa** le decisioni da prendere in merito a un'eccezione, restituirà **false**. Se **sa** la decisione da prendere, imposterà il risultato di conseguenza e restituirà **true**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### Riassumendo
Con un oggetto `ICommunicationClient`, `ICommunicationClientFactory` e `IExceptionHandler` basato su un protocollo di comunicazione, un oggetto `ServicePartitionClient` può includere tutti gli elementi e fornire il ciclo di risoluzione degli indirizzi della partizione del servizio e di gestione degli errori in base a tali componenti.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## Passaggi successivi
 - Vedere un esempio di comunicazione HTTP tra i servizi in un [progetto di esempio in GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Chiamate di procedura remota con i Reliable Services remoti](service-fabric-reliable-services-communication-remoting.md)

 - [Web API che usa OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)

 - [Comunicazione di WCF tramite Reliable Services](service-fabric-reliable-services-communication-wcf.md)

<!---HONumber=AcomDC_0608_2016-->