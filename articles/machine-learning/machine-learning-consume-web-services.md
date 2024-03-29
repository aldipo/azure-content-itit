<properties
	pageTitle="Utilizzare un servizio Web di Machine Learning | Microsoft Azure"
	description="Dopo la pubblicazione di un servizio di apprendimento automatico, può essere utilizzato il servizio Web RESTFul che viene reso disponibile come servizio di richiesta-risposta o come un servizio di esecuzione del batch."
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="paulettm"
	editor="cgronlun" />

<tags
	ms.service="machine-learning"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="tbd"
	ms.date="05/22/2016"
	ms.author="garye" />


# Come utilizzare un servizio Web di Azure Machine Learning pubblicato da un esperimento di Machine Learning

## Introduzione

Quando viene pubblicato come servizio Web, gli esperimenti Azure Machine Learning forniscono un'API REST che può essere utilizzata da un'ampia gamma di dispositivi e piattaforme. Infatti, la semplice API REST accetta e risponde con messaggi in formato JSON. Il portale di Azure Machine Learning fornisce il codice che può essere utilizzato per chiamare il servizio Web in R, c# e Python. Ma questi servizi possono essere chiamati con qualsiasi linguaggio di programmazione e da qualsiasi dispositivo che soddisfi i tre criteri:

* Dispone di una connessione di rete
* Dispone di funzionalità di SSL per eseguire le richieste HTTPS
* È in grado di analizzare JSON (dalle librerie disponibili o supporto)

Ciò significa che i servizi possono essere utilizzati da applicazioni Web, applicazioni per dispositivi mobili, applicazioni desktop personalizzate e persino all'interno di Excel.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Un servizio Web di Azure Machine Learning può essere utilizzato in due modi diversi, come un servizio di richiesta-risposta o come un servizio di esecuzione batch. In ogni scenario, la funzionalità viene fornita tramite il servizio Web RESTFul reso disponibile per l'utilizzo dopo la pubblicazione dell'esperimento. La distribuzione di un servizio Web di apprendimento automatico in Azure con un endpoint del servizio Web di Azure, dove il servizio viene ridimensionato automaticamente in base all'utilizzo, è possibile evitare i costi iniziali e successivi per le risorse hardware.

> [AZURE.TIP] Per informazioni su un modo semplice per creare un'app Web per l'accesso al servizio Web predittivo, vedere l'articolo relativo all'[utilizzare di un servizio Web di Azure Machine Learning con un modello di app Web](machine-learning-consume-web-service-with-web-app-template.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

Per informazioni su come creare e pubblicare un servizio Web di Azure Machine Learning, vedere [Pubblicare un servizio Web di Azure Machine Learning][publish]. Per una procedura dettagliata di creazione e pubblicazione di un esperimento di Machine Learning, vedere [Sviluppare una soluzione predittiva mediante Azure Machine Learning][walkthrough].

[publish]: machine-learning-publish-a-machine-learning-web-service.md
[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md


## Servizio di richiesta-risposta (RRS)

Un servizio di richiesta-risposta (RRS) è un servizio Web a bassa latenza e altamente scalabile utilizzato per fornire un'interfaccia ai modelli senza stato,creati e pubblicati da un esperimento Azure Machine Learning Studio. In questo modo abilita scenari in cui l'applicazione del consumo attende una risposta in tempo reale.

RRS accetta una singola riga o più righe di parametri di input e può generare una singola riga o più righe come output. La riga di output può contenere più colonne.

Un esempio per RRS convalida l'autenticità di un'applicazione. In questo caso, è possibile prevedere centinaia di milioni di installazioni di un'applicazione. All'avvio dell'applicazione, effettua una chiamata al servizio RRS con input pertinente. Quindi, l'applicazione riceve una risposta di convalida dal servizio che consente o blocca le esecuzioni da parte dell'applicazione.


## Servizio Esecuzione batch (BES)

Un servizio di esecuzione del batch (BES) è un servizio che gestisce volumi elevati, asincroni e valutazioni di un batch di record di dati. L'input per il BES contiene un batch di record da varie origini, ad esempio blob, tabelle di Azure, SQL Azure, HDInsight (i risultati di una Query Hive, ad esempio) e origini HTTP. L'output per il BES contiene i risultati della valutazione. I risultati vengono inseriti su un file nell'archiviazione blob di Azure e i dati dall'endpoint di archiviazione vengono restituiti nella risposta.

Un BES potrebbe essere utile quando le risposte non sono necessarie immediatamente, ad esempio per valutazioni regolarmente pianificate per singoli utenti o dispositivi IOT (Internet of things).

## esempi
Per visualizzare il funzionamento di RRS e BES, viene utilizzato un servizio Web di Azure di esempio. Questo servizio potrebbe essere utilizzato in uno scenario IOT (Internet Of Things). In breve, il dispositivo invia solo un valore, `cog_speed`, e riceve una sola risposta.

Esistono quattro tipi di informazioni necessari per chiamare il servizio RRS o BES. Queste informazioni sono disponibili nelle pagine del servizio [Azure Machine Learning Studio](https://studio.azureml.net) dopo la distribuzione dell'esperimento. Fare clic sulla scheda SERVIZI WEB sulla sinistra dello schermo per visualizzare i servizi distribuiti. Fare clic su un servizio per trovare i collegamenti e le informazioni seguenti per RRS e BES:

1.	La **chiave API** del servizio, disponibile nel dashboard dei servizi
2.	L'**URI della richiesta** del servizio, disponibile nella pagina della Guida dell'API del servizio scelto
3.	Le **intestazioni ** e il **corpo della richiesta** API previsti, disponibili nella pagina della Guida dell'API del servizio scelto
4.	Le **intestazioni ** e il **corpo della risposta** API previsti, disponibili nella pagina della Guida dell'API del servizio scelto

Nei due esempi seguenti, il linguaggio c# viene utilizzato per illustrare che il codice necessario e la piattaforma di destinazione è un desktop di Windows 8.

### Esempio RRS
Fare clic su **RICHIESTA/RISPOSTA** sotto la **PAGINA DELLA GUIDA DELL'API** nel dashboard del servizio per visualizzare la pagina della Guida dell'API. In questa pagina, a parte l'URI, sono presenti le definizioni di input e output ed esempi di codice. In particolare per questo servizio, l'input API viene visualizzato di seguito e rappresenta il carico utile della chiamata API.

**Richiesta di esempio**

	{
	  "Inputs": {
	    "input1": {
	      "ColumnNames": [
	        "cog_speed"
	      ],
	      "Values": [
	        [
	          "0"
	        ],
	        [
	          "1"
	        ]
	      ]
	    }
	  },
	  "GlobalParameters": {}
	}


Analogamente, la risposta dell'API per questo servizio viene ugualmente visualizzata di seguito.

**Risposta di esempio**

	{
	  "Results": {
	    "output1": {
	      "type": "DataTable",
	      "value": {
	        "ColumnNames": [
	          "cog_speed"
	        ],
	        "ColumnTypes": [
	          "Numeric"
	        ].
	      "Values": [
	        [
	          "0"
	        ],
	        [
	          "1"
	        ]
	      ]
	    }
	  },
	  "GlobalParameters": {}
	}

Nella parte inferiore della pagina della guida si trovano gli esempi di codice. Di seguito si trova il codice di esempio per l'implementazione C#.

**Codice di esempio in C#**

	using System;
	using System.Collections.Generic;
	using System.IO;
	using System.Net.Http;
	using System.Net.Http.Formatting;
	using System.Net.Http.Headers;
	using System.Text;
	using System.Threading.Tasks;

	namespace CallRequestResponseService
	{
	    public class StringTable
	    {
	        public string[] ColumnNames { get; set; }
	        public string[,] Values { get; set; }
	    }

	    class Program
	    {
	        static void Main(string[] args)
	        {
	            InvokeRequestResponseService().Wait();
	        }

	        static async Task InvokeRequestResponseService()
	        {
	            using (var client = new HttpClient())
	            {
	                var scoreRequest = new
	                {
	                    Inputs = new Dictionary<string, StringTable> () {
	                        {
	                            "input1",
	                            new StringTable()
	                            {
	                                ColumnNames = new string[] {"cog_speed"},
	                                Values = new string[,] {  { "0"},  { "1"}  }
	                            }
	                        },
	                    GlobalParameters = new Dictionary<string, string>() { }
	                };

	                const string apiKey = "abc123"; // Replace this with the API key for the web service
	                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);

	                client.BaseAddress = new Uri("https://ussouthcentral.services.azureml.net/workspaces/<workspace id>/services/<service id>/execute?api-version=2.0&details=true");

	                // WARNING: The 'await' statement below can result in a deadlock if you are calling this code from the UI thread of an ASP.Net application.
	                // One way to address this would be to call ConfigureAwait(false) so that the execution does not attempt to resume on the original context.
	                // For instance, replace code such as:
	                //      result = await DoSomeTask()
	                // with the following:
	                //      result = await DoSomeTask().ConfigureAwait(false)

	                HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

	                if (response.IsSuccessStatusCode)
	                {
	                    string result = await response.Content.ReadAsStringAsync();
	                    Console.WriteLine("Result: {0}", result);
	                }
	                else
	                {
	                    Console.WriteLine("Failed with status code: {0}", response.StatusCode);
	                }
	            }
	        }
	    }
	}

**Codice di esempio in Java**

Il codice di esempio seguente mostra come costruire una richiesta di API REST in Java. Si presuppone che le variabili (apikey e apiurl) includano i dettagli dell'API necessari e che la variabile jsonBody includa un oggetto JSON appropriato, come richiesto dall'API REST per effettuare una stima corretta. È possibile scaricare il codice completo dal sito Web di GitHub all'indirizzo [https://github.com/nk773/AzureML\_RRSApp](https://github.com/nk773/AzureML_RRSApp). Questo esempio Java richiede la [libreria Apache HTTP Client](https://hc.apache.org/downloads.cgi).

	/**
	 * Download full code from github - [https://github.com/nk773/AzureML_RRSApp](https://github.com/nk773/AzureML_RRSApp)
 	 */
    	/**
     	  * Call REST API for retrieving prediction from Azure ML 
     	  * @return response from the REST API
     	  */	
    	public static String rrsHttpPost() {
        
        	HttpPost post;
        	HttpClient client;
        	StringEntity entity;
        
        	try {
            		// create HttpPost and HttpClient object
            		post = new HttpPost(apiurl);
            		client = HttpClientBuilder.create().build();
            
            		// setup output message by copying JSON body into 
            		// apache StringEntity object along with content type
            		entity = new StringEntity(jsonBody, HTTP.UTF_8);
            		entity.setContentEncoding(HTTP.UTF_8);
            		entity.setContentType("text/json");

            		// add HTTP headers
            		post.setHeader("Accept", "text/json");
            		post.setHeader("Accept-Charset", "UTF-8");
        
            		// set Authorization header based on the API key
            		post.setHeader("Authorization", ("Bearer "+apikey));
            		post.setEntity(entity);

            		// Call REST API and retrieve response content
            		HttpResponse authResponse = client.execute(post);
            
            		return EntityUtils.toString(authResponse.getEntity());
            
        	}
        	catch (Exception e) {
            
            		return e.toString();
        	}
    
    	}
    
    	
 

### Esempio BES
A differenza del servizio RRS, il servizio BES è asincrono. Ciò significa che l'API BES accoda semplicemente un processo da eseguire e il chiamante esegue il polling dello stato del processo per verificarne il completamento. Ecco le operazioni attualmente supportate per i processi batch:

1. Creare (inviare) un processo batch.
1. Avviare questo processo batch.
1. Ottenere lo stato o il risultato di un processo batch.
1. Annullare un processo batch in esecuzione.

**1. Creare un processo di esecuzione batch**

Quando si crea un processo batch per l'endpoint di servizio di Azure Machine Learning, è possibile specificare diversi parametri che definiscono l'esecuzione del batch:

* **Input**: rappresenta un riferimento al BLOB in cui viene archiviato il processo batch di input.
* **GlobalParameters**: rappresenta il set di parametri globali che è possibile definire per l'esperimento. Un esperimento di Azure Machine Learning può avere contemporaneamente parametri obbligatori e facoltativi che personalizzano l'esecuzione del servizio e tutti i parametri obbligatori, se disponibili, devono essere forniti dal chiamante. Questi parametri vengono specificati come una raccolta di coppie chiave-valore.
* **Outputs**: se il servizio ha definito uno o più output, il chiamante può reindirizzarli verso un percorso BLOB di Azure. Ciò consente di salvare l'output del servizio in un percorso preferito e con un nome prevedibile. In caso contrario, il nome del BLOB di output viene generato casualmente. 

    Si noti che il servizio prevede che l'output del contenuto, in base al tipo, venga salvato in formati supportati:
  - Output di set di dati: possono essere salvati come file con estensione **csv, tsv, arff**.
  - Output di modelli sottoposti a training: possono essere salvati come file con estensione **ilearner**.

  Gli override del percorso di output vengono specificati come una raccolta di coppie *<output name  blob reference>*, dove il *nome di output* è il nome definito dall'utente per un nodo di output specifico (visualizzato anche nella pagina della Guida dell'API del servizio) e un *riferimento al BLOB* è un riferimento al percorso di un BLOB di Azure verso cui l'output deve essere reindirizzato.

Tutti questi parametri di creazione del processo possono essere facoltativi, a seconda della natura del servizio. I servizi per cui non sono definiti nodi di input, ad esempio, non richiedono il passaggio di un parametro *Input*. Analogamente, la funzionalità di override del percorso di output è completamente facoltativa. In caso contrario, gli output verranno archiviati nell'account di archiviazione predefinito configurato per l'area di lavoro di Azure Machine Learning. Di seguito, viene illustrato un payload di richiesta di esempio passato all'API REST, per un servizio in cui vengono fornite solo le informazioni di input:

**Richiesta di esempio**

	{
	  "Input": {
	    "ConnectionString":     
	    "DefaultEndpointsProtocol=https;AccountName=mystorageacct;AccountKey=mystorageacctKey",
	    "RelativeLocation": "/mycontainer/mydatablob.csv",
	    "BaseLocation": null,
	    "SasBlobToken": null
	  },
	  "Outputs": null,
	  "GlobalParameters": null
	}

La risposta all'API di creazione del processo batch è l'ID processo univoco associato al processo. Questo ID è molto importante perché rappresenta l'unico mezzo per poter fare riferimento al processo nel sistema per altre operazioni.

**Risposta di esempio**

	"539d0bc2fde945b6ac986b851d0000f0" // The JOB_ID

**2. Avviare un processo di esecuzione batch**

La creazione di un processo batch ne comporta la registrazione nel sistema e l'inserimento con stato *Not started*. Per pianificare effettivamente il processo per l'esecuzione, chiamare l'API di **avvio** descritta nella pagina della Guida dell'API dell'endpoint di servizio e fornire l'ID processo ottenuto durante la creazione del processo stesso.

**3. Ottenere lo stato di un processo di esecuzione batch**

È possibile eseguire il polling dello stato del processo batch asincrono in qualsiasi momento passando l'ID del processo all'API GetJobStatus. Se l'operazione è stata completata correttamente, la risposta dell'API conterrà un indicatore dello stato corrente del processo, nonché i risultati effettivi del processo batch. In caso di errore, vengono restituite altre informazioni sui motivi effettivi che hanno causato l'errore nella proprietà *Details*, come illustrato di seguito:

**Payload della risposta**

	{
	    "StatusCode": STATUS_CODE,
	    "Results": RESULTS,
	    "Details": DETAILS
	}

*StatusCode* può avere uno dei valori seguenti:

* Not started
* Running
* Failed
* Cancelled
* Finished

La proprietà *Results* viene popolata solo se il processo è stato completato correttamente; in caso contrario, è **null**. Al completamento del processo e se il servizio contiene almeno un nodo di output definito, i risultati verranno restituiti come una raccolta di coppie *[nome output, riferimento al BLOB]*, dove il riferimento al BLOB è un riferimento di sola lettura di firma di accesso condiviso al BLOB contenente il risultato effettivo.

**Risposta di esempio**

	{
	    "Status Code": "Finished",
	    "Results":
	    {
	        "dataOutput":
	        {              
	            "ConnectionString": null,
	            "RelativeLocation": "outputs/dataOutput.csv",
	            "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
	            "SasBlobToken": "?sv=2013-08-15&sr=b&sig=ABCD&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
	        },
	        "trainedModelOutput":
	        {              
	            "ConnectionString": null,
	            "RelativeLocation": "models/trainedModel.ilearner",
	            "BaseLocation": "https://mystorageaccount.blob.core.windows.net/",
	            "SasBlobToken": "?sv=2013-08-15&sr=b&sig=EFGH%3D&st=2015-04-04T05%3A39%3A55Z&se=2015-04-05T05%3A44%3A55Z&sp=r"              
	        },           
	    },
	    "Details": null
	}

**4. Annullare un processo di esecuzione batch**

È possibile annullare un processo batch in esecuzione in qualsiasi momento chiamando l'API CancelJob designata e passando l'ID del processo. Ciò può essere effettuato per vari motivi, ad esempio se il processo impiega troppo tempo per il completamento.



#### Uso dell'SDK BES

Il [pacchetto NuGet BES SDK](http://www.nuget.org/packages/Microsoft.Azure.MachineLearning/) fornisce funzioni che semplificano la chiamata a BES per assegnare un punteggio in modalità batch. Per installare il pacchetto NuGet in Visual Studio, nel menu **Strumenti** selezionare **Gestione pacchetti NuGet** e quindi fare clic su **Console di Gestione pacchetti**.

Gli esperimenti Azure Machine Learning distribuiti come servizi Web possono includere moduli di input del servizio Web. Ciò significa che prevedono che l'input sia fornito mediante la chiamata al servizio Web sotto forma di riferimento a un percorso BLOB. È inoltre possibile non utilizzare un modulo di input del servizio Web e utilizzare invece un modulo **Importazione dei dati**. In questo caso, per ottenere i dati il modulo **Importazione dei dati** in genere legge da un database SQL usando una query in fase di esecuzione. È possibile usare parametri del servizio Web per scegliere in modo dinamico altri server o tabelle e così via. L'SDK supporta entrambi questi modelli.

L'esempio di codice riportato di seguito illustra come inviare e monitorare un processo batch su un endpoint di servizio di Azure Machine Learning tramite BES SDK. Per informazioni dettagliate su impostazioni e chiamate, osservare i commenti.

#### **Codice di esempio**

	// This code requires the Nuget package Microsoft.Azure.MachineLearning to be installed.
	// Instructions for doing this in Visual Studio:
	// Tools -> Nuget Package Manager -> Package Manager Console
	// Install-Package Microsoft.Azure.MachineLearning

	  using System;
	  using System.Collections.Generic;
	  using System.Threading.Tasks;

	  using Microsoft.Azure.MachineLearning;
	  using Microsoft.Azure.MachineLearning.Contracts;
	  using Microsoft.Azure.MachineLearning.Exceptions;

	namespace CallBatchExecutionService
	{
	    class Program
	    {
	        static void Main(string[] args)
	        {	            
	            InvokeBatchExecutionService().Wait();
	        }

	        static async Task InvokeBatchExecutionService()
	        {
	            // First collect and fill in the URI and access key for your web service endpoint.
	            // These are available on your service's API help page.
	            var endpointUri = "https://ussouthcentral.services.azureml.net/workspaces/YOUR_WORKSPACE_ID/services/YOUR_SERVICE_ENDPOINT_ID/";
	            string accessKey = "YOUR_SERVICE_ENDPOINT_ACCESS_KEY";

	            // Create an Azure Machine Learning runtime client for this endpoint
	            var runtimeClient = new RuntimeClient(endpointUri, accessKey);

	            // Define the request information for your batch job. This information can contain:
	            // -- A reference to the AzureBlob containing the input for your job run
	            // -- A set of values for global parameters defined as part of your experiment and service
	            // -- A set of output blob locations that allow you to redirect the job's results

	            // NOTE: This sample is applicable, as is, for a service with explicit input port and
	            // potential global parameters. Also, we choose to also demo how you could override the
	            // location of one of the output blobs that could be generated by your service. You might
	            // need to tweak these features to adjust the sample to your service.
	            //
	            // All of these properties of a BatchJobRequest shown below can be optional, depending on
	            // your service, so it is not required to specify all with any request.  If you do not want to
	            // use any of the parameters, a null value should be passed in its place.

	            // Define the reference to the blob containing your input data. You can refer to this blob by its
                    // connection string / container / blob name values; alternatively, we also support references
                    // based on a blob SAS URI

                    BlobReference inputBlob = BlobReference.CreateFromConnectionStringData(connectionString:                                         "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                        containerName: "YOUR_CONTAINER_NAME",
                        blobName: "YOUR_INPUT_BLOB_NAME");

                    // If desired, one can override the location where the job outputs are to be stored, by passing in
                    // the storage account details and name of the blob where we want the output to be redirected to.

                    var outputLocations = new Dictionary<string, BlobReference>
                        {
                          {
                           "YOUR_OUTPUT_NODE_NAME",
                           BlobReference.CreateFromConnectionStringData(                                     connectionString: "DefaultEndpointsProtocol=https;AccountName=YOUR_ACCOUNT_NAME;AccountKey=YOUR_ACCOUNT_KEY",
                                containerName: "YOUR_CONTAINER_NAME",
                                blobName: "YOUR_DESIRED_OUTPUT_BLOB_NAME")
                           }
                        };

	            // If applicable, you can also set the global parameters for your service
	            var globalParameters = new Dictionary<string, string>
	            {
	                { "YOUR_GLOBAL_PARAMETER", "PARAMETER_VALUE" }
	            };

	            var jobRequest = new BatchJobRequest
	            {
	                Input = inputBlob,
	                GlobalParameters = globalParameters,
	                Outputs = outputLocations
	            };

	            try
	            {
	                // Register the batch job with the system, which will grant you access to a job object
	                BatchJob job = await runtimeClient.RegisterBatchJobAsync(jobRequest);

	                // Start the job to allow it to be scheduled in the running queue
	                await job.StartAsync();

	                // Wait for the job's completion and handle the output
	                BatchJobStatus jobStatus = await job.WaitForCompletionAsync();
	                if (jobStatus.JobState == JobState.Finished)
	                {
	                    // Process job outputs
	                    Console.WriteLine(@"Job {0} has completed successfully and returned {1} outputs", job.Id, jobStatus.Results.Count);
	                    foreach (var output in jobStatus.Results)
	                    {
	                        Console.WriteLine(@"\t{0}: {1}", output.Key, output.Value.AbsoluteUri);
	                    }
	                }
	                else if (jobStatus.JobState == JobState.Failed)
	                {
	                    // Handle job failure
	                    Console.WriteLine(@"Job {0} has failed with this error: {1}", job.Id, jobStatus.Details);
	                }
	            }
	            catch (ArgumentException aex)
	            {
	                Console.WriteLine("Argument {0} is invalid: {1}", aex.ParamName, aex.Message);
	            }
	            catch (RuntimeException runtimeError)
	            {
	                Console.WriteLine("Runtime error occurred: {0} - {1}", runtimeError.ErrorCode, runtimeError.Message);
	                Console.WriteLine("Error details:");
	                foreach (var errorDetails in runtimeError.Details)
	                {
	                    Console.WriteLine("\t{0} - {1}", errorDetails.Code, errorDetails.Message);
	                }
	            }
	            catch (Exception ex)
	            {
	                Console.WriteLine("Unexpected error occurred: {0} - {1}", ex.GetType().Name, ex.Message);
	            }
	        }
	    }
	}

#### Codice di esempio Java per BES
Come mostrato di seguito, l'API REST del servizio di esecuzione batch accetta l'elemento JSON costituito da un riferimento a un file con estensione csv di esempio di input e a un file con estensione csv di esempio di output e crea un processo in Azure ML per eseguire le stime batch. È possibile visualizzare il codice completo in [GitHub](https://github.com/nk773/AzureML_BESApp/tree/master/src/azureml_besapp). Questo esempio Java richiede la [libreria Apache HTTP Client](https://hc.apache.org/downloads.cgi).


	{ "GlobalParameters": {}, 
    	"Inputs": { "input1": { "ConnectionString": 	"DefaultEndpointsProtocol=https;
			AccountName=myAcctName; AccountKey=Q8kkieg==", 
        	"RelativeLocation": "myContainer/sampleinput.csv" } }, 
    	"Outputs": { "output1": { "ConnectionString": 	"DefaultEndpointsProtocol=https;
			AccountName=myAcctName; AccountKey=kjC12xQ8kkieg==", 
        	"RelativeLocation": "myContainer/sampleoutput.csv" } } 
	} 


#####Creare un processo BES	
	    
	    /**
	     * Call REST API to create a job to Azure ML 
	     * for batch predictions
	     * @return response from the REST API
	     */	
	    public static String besCreateJob() {
	        
	        HttpPost post;
	        HttpClient client;
	        StringEntity entity;
	        
	        try {
	            // create HttpPost and HttpClient object
	            post = new HttpPost(apiurl);
	            client = HttpClientBuilder.create().build();
	            
	            // setup output message by copying JSON body into 
	            // apache StringEntity object along with content type
	            entity = new StringEntity(jsonBody, HTTP.UTF_8);
	            entity.setContentEncoding(HTTP.UTF_8);
	            entity.setContentType("text/json");
	
	            // add HTTP headers
	            post.setHeader("Accept", "text/json");
	            post.setHeader("Accept-Charset", "UTF-8");
	        
	            // set Authorization header based on the API key
				// note a space after the word "Bearer " - don't miss that
	            post.setHeader("Authorization", ("Bearer "+apikey));
	            post.setEntity(entity);
	
	            // Call REST API and retrieve response content
	            HttpResponse authResponse = client.execute(post);
	            
	            jobId = EntityUtils.toString(authResponse.getEntity()).replaceAll(""", "");
	            
	            
	            return jobId;
	            
	        }
	        catch (Exception e) {
	            
	            return e.toString();
	        }
	    
	    }
	    
#####Avviare un processo BES creato in precedenza	        
	    /**
	     * Call REST API for starting prediction job previously submitted 
	     * 
	     * @param job job to be started 
	     * @return response from the REST API
	     */	
	    public static String besStartJob(String job){
	        HttpPost post;
	        HttpClient client;
	        StringEntity entity;
	        
	        try {
	            // create HttpPost and HttpClient object
	            post = new HttpPost(startJobUrl+"/"+job+"/start?api-version=2.0");
	            client = HttpClientBuilder.create().build();
	         
	            // add HTTP headers
	            post.setHeader("Accept", "text/json");
	            post.setHeader("Accept-Charset", "UTF-8");
	        
	            // set Authorization header based on the API key
	            post.setHeader("Authorization", ("Bearer "+apikey));
	
	            // Call REST API and retrieve response content
	            HttpResponse authResponse = client.execute(post);
	            
	            if (authResponse.getEntity()==null)
	            {
	                return authResponse.getStatusLine().toString();
	            }
	            
	            return EntityUtils.toString(authResponse.getEntity());
	            
	        }
	        catch (Exception e) {
	            
	            return e.toString();
	        }
	    }
#####Annullare un processo BES creato in precedenza
	    
	    /**
	     * Call REST API for canceling the batch job 
	     * 
	     * @param job job to be started 
	     * @return response from the REST API
	     */	
	    public static String besCancelJob(String job) {
	        HttpDelete post;
	        HttpClient client;
	        StringEntity entity;
	        
	        try {
	            // create HttpPost and HttpClient object
	            post = new HttpDelete(startJobUrl+job);
	            client = HttpClientBuilder.create().build();
	         
	            // add HTTP headers
	            post.setHeader("Accept", "text/json");
	            post.setHeader("Accept-Charset", "UTF-8");
	        
	            // set Authorization header based on the API key
	            post.setHeader("Authorization", ("Bearer "+apikey));
	
	            // Call REST API and retrieve response content
	            HttpResponse authResponse = client.execute(post);
	         
	            if (authResponse.getEntity()==null)
	            {
	                return authResponse.getStatusLine().toString();
	            }
	            return EntityUtils.toString(authResponse.getEntity());
	            
	        }
	        catch (Exception e) {
	            
	            return e.toString();
	        }
	    }
	    
###Altri ambienti di programmazione
È anche possibile generare il codice in diversi altri linguaggi usando un documento Swagger disponibile nella pagina della Guida dell'API e seguendo le istruzioni fornite nel sito [swagger.io](http://swagger.io/). Accedere a [swagger.io](http://swagger.io/swagger-codegen/) e seguire le istruzioni per scaricare il codice Swagger, Java e Apache Maven. Di seguito è riportato un riepilogo delle istruzioni relative alla configurazione di Swagger per altri ambienti di programmazione.

* Verificare che sia installato Java 7 o versione successiva.
* Installare Apache Maven (in Ubuntu è possibile usare *apt-get install mvn*).
* Accedere a GitHub e scaricare il progetto Swagger come file con estensione zip.
* Decomprimere il progetto Swagger.
* Compilare gli strumenti Swagger eseguendo *mvn package* dalla directory di origine di Swagger.

A questo punto è possibile usare uno qualsiasi degli strumenti Swagger. Di seguito sono riportate le istruzioni per generare codice del client Java.

* Passare alla pagina della Guida dell'API di Azure ML (fare clic [qui](https://studio.azureml.net/apihelp/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/jobs) per un esempio).
* Trovare l'URL per swagger.json per le API REST di Azure ML (penultimo punto nella parte superiore della pagina della Guida dell'API).
* Fare clic sul collegamento del documento Swagger (fare clic [qui](https://management.azureml.net/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/apidocument) per un esempio).
* Usare il comando seguente come mostrato nel [file Readme di Swagger](https://github.com/swagger-api/swagger-codegen/blob/master/README.md) per generare il codice client.

**Riga di comando di esempio per la generazione del codice client**

	java -jar swagger-codegen-cli.jar generate\
	 -i https://ussouthcentral.services.azureml.net:443/workspaces/\
	fb62b56f29fc4ba4b8a8f900c9b89584/services/26a3afce1767461ab6e73d5a206fbd62/swagger.json\
	 -l java -o /home/username/sample

* Combinare i valori host, basePath e "/swagger.json" dei campi in una [pagina della Guida dell'API](https://management.azureml.net/workspaces/afbd553b9bac4c95be3d040998943a4f/webservices/4dfadc62adcc485eb0cf162397fb5682/endpoints/26a3afce1767461ab6e73d5a206fbd62/apidocument) Swagger di esempio mostrata di seguito per creare l'URL Swagger usato nella riga di comando precedente.

**Pagina di esempio della Guida dell'API**


	{
	  "swagger": "2.0",
	  "info": {
	    "version": "2.0",
	    "title": "Sample 5: Binary Classification with Web Service: Adult Dataset [Predictive Exp.]",
	    "description": "No description provided for this web service.",
	    "x-endpoint-name": "default"
	  },
	  "host": "ussouthcentral.services.azureml.net:443",
	  "basePath": "/workspaces/afbd553b9bac4c95be3d040998943a4f/services/26a3afce1767461ab6e73d5a206fbd62",
	  "schemes": [
	    "https"
	  ],
	  "consumes": [
	    "application/json"
	  ],
	  "produces": [
	    "application/json"
	  ],
	  "paths": {
	    "/swagger.json": {
	      "get": {
	        "summary": "Get swagger API document for the web service",
	        "operationId": "getSwaggerDocument",
	        

<!---HONumber=AcomDC_0608_2016-->