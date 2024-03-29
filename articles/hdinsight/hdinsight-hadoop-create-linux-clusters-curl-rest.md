<properties
   	pageTitle="Creare cluster Hadoop, HBase o Storm su Linux in HDInsight tramite cURL e l'API REST di Azure | Microsoft Azure"
   	description="Informazioni su come creare cluster HDInsight basati su Linux tramite cURL, i modelli di gestione risorse di Azure e l'API REST di Azure. È possibile specificare il tipo di cluster (Hadoop, HBase o Storm) o utilizzare gli script per installare i componenti personalizzati."
   	services="hdinsight"
   	documentationCenter=""
   	authors="Blackmist"
   	manager="paulettm"
   	editor="cgronlun"
	tags="azure-portal"/>

<tags
   	ms.service="hdinsight"
   	ms.devlang="na"
   	ms.topic="article"
   	ms.tgt_pltfrm="na"
   	ms.workload="big-data"
   	ms.date="05/16/2016"
   	ms.author="larryfr"/>

#Creare cluster basati su Linux in HDInsight tramite cURL e l’API REST di Azure

[AZURE.INCLUDE [selettore](../../includes/hdinsight-selector-create-clusters.md)]

L'API REST di Azure consente di eseguire operazioni di gestione su servizi ospitati nella piattaforma Azure, inclusa la creazione di nuove risorse, ad esempio cluster HDInsight basati su Linux. In questo documento verrà descritto come creare modelli di Gestione risorse di Azure per configurare un cluster di HDInsight e l’archiviazione associata, quindi utilizzare cURL per distribuire il modello per l'API REST di Azure per creare un nuovo cluster HDInsight.

> [AZURE.IMPORTANT] I passaggi descritti in questo documento utilizzano il numero di nodi di lavoro predefinito (4) per un cluster HDInsight. Se si prevedono più di 32 nodi di lavoro, al momento della creazione del cluster o con il ridimensionamento del cluster dopo la creazione, è necessario selezionare una dimensione del nodo head con almeno 8 core e 14 GB di RAM.
>
> Per altre informazioni sulle dimensioni di nodo e i costi associati, vedere [Prezzi di HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

##Prerequisiti

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Una sottoscrizione di Azure**. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Interfaccia della riga di comando di Azure__. L’interfaccia della riga di comando di Azure viene utilizzata per creare un’entità servizio, quindi viene utilizzata per generare i token di autenticazione per le richieste all'API REST di Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __cURL__. Questa utilità è disponibile tramite il sistema di gestione del pacchetto o può essere scaricata da [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Se si utilizza PowerShell per eseguire i comandi in questo documento, è necessario innanzitutto rimuovere l’alias `curl` creato per impostazione predefinita. Questo alias utilizza Invoke-WebRequest, un cmdlet di PowerShell, anziché cURL quando si utilizza il comando `curl` da un prompt di PowerShell e restituisce errori per molti dei comandi utilizzati in questo documento.
    > 
    > Per rimuovere l'alias, utilizzare la seguente procedura dal prompt di PowerShell:
    >
    > `Remove-item alias:curl`
    >
    > Una volta rimosso l'alias, sarà possibile utilizzare la versione di cURL installata nel sistema.

##Creare un modello

I modelli di Gestione risorse di Azure sono documenti JSON che descrivono un __gruppo di risorse__ e tutte le risorse in esso contenute, ad esempio HDInsight. Questo approccio basato sui modelli consente di definire tutte le risorse necessarie per HDInsight in un modello e di gestire le modifiche apportate all'intero gruppo mediante __distribuzioni__ che applicano modifiche al gruppo.

In genere i modelli vengono forniti modelli in due parti; il modello stesso e un file di parametri che viene popolato con valori specifici per la configurazione. Per esempio, nome del cluster, nome amministratore e password. Quando si utilizza direttamente l'API REST, è necessario combinarli in un file. Il formato di questo documento JSON è:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Ad esempio, di seguito è riportata una fusione dei file di modello e di parametri ricavati da [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), che consente di creare un cluster basato su Linux usando una password per proteggere l'account utente SSH.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

Questo esempio verrà utilizzato nei passaggi di questo documento. È necessario sostituire il segnaposto _valori_ nella sezione __Parametri__ alla fine del documento con i valori che si desidera utilizzare per il cluster.

##Effettuare l'accesso alla sottoscrizione di Azure

Seguire i passaggi descritti in [Connettersi a una sottoscrizione Azure dall'interfaccia della riga di comando di Azure](../xplat-cli-connect.md) e connettersi alla sottoscrizione usando il comando `azure login`.

##Creare un’entità servizio

> [AZURE.NOTE] Questa procedura costituisce una sintesi delle informazioni contenute nella sezione _Autenticazione con password - Interfaccia della riga di comando di Azure_ del documento [Autenticazione di un'entità servizio con Gestione risorse di Azure](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli). Consente di creare una nuova entità servizio necessaria per autenticare le richieste dell'API REST usate per la creazione di risorse di Azure, ad esempio un cluster HDInsight.

1. Al prompt dei comandi, in una sessione terminal o in una shell usare il comando seguente per elencare le sottoscrizioni di Azure.

        azure account list
        
    Nell'elenco selezionare la sottoscrizione che si vuole usare e annotare il valore della colonna __Id__, che costituisce l'__ID sottoscrizione__ e dovrà essere usato nella maggior parte dei passaggi descritti in questo documento.

2. Creare una nuova applicazione in Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Sostituire i valori di `--name`, `--home-page` e `--identifier-uris` con valori personalizzati. Specificare una password per la nuova voce di Active Directory.
    
    > [AZURE.NOTE] Poiché si sta creando questa applicazione per l'autenticazione tramite un'entità servizio, non è necessario che i valori `--home-page` e `--identifier-uris` facciano riferimento a una pagina Web effettiva ospitata in Internet, ma possono essere anche semplici URI univoci.
    
    Dall'elenco di dati restituiti salvare il valore __AppId__.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Creare un'entità servizio usando il valore __AppId__ precedentemente restituito.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Dall'elenco di dati restituiti salvare il valore __ID oggetto__.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Assegnare il ruolo __Proprietario__ all'entità servizio usando il valore __ID oggetto__ precedentemente restituito. È necessario usare anche l'__ID sottoscrizione__ ottenuto in precedenza.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Dopo il completamento del comando, l'entità servizio disporrà dell'accesso all'ID sottoscrizione specificato in qualità di Proprietario.

##Ottenere un token di autenticazione

1. Usare il comando seguente per trovare l'__ID tenant__ relativo alla sottoscrizione.

        azure account show -s <subscription ID>
        
    Nell'elenco dei dati restituiti trovare la voce __ID tenant__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Generare un nuovo token usando l'API REST di Azure.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Sostituire __TenantID__, __AppID__ e __password__ con i valori ottenuti o usati in precedenza.

    Se la richiesta ha esito positivo, si riceverà una risposta serie 200 e il corpo della risposta conterrà un documento JSON.

    Il documento JSON restituito da questa richiesta contiene un elemento denominato __access\_token__; il valore di questo elemento è il token di accesso da utilizzare per l'autenticazione delle richieste utilizzate nelle sezioni successive di questo documento.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##Creare un gruppo di risorse

Per creare un nuovo gruppo di risorse, seguire questa procedura. È necessario creare innanzitutto il gruppo prima di poter creare le risorse, ad esempio il cluster HDInsight.

* Sostituire __SubscriptionID__ con l'ID sottoscrizione ricevuto durante la creazione dell'entità servizio.
* Sostituire __AccessToken__ con il token di accesso ricevuto nel passaggio precedente.
* Sostituire __DataCenterLocation__ con il data center in cui si vuole creare il gruppo di risorse e le risorse. Ad esempio "Stati Uniti centrali del sud". 
* Sostituire __ResourceGroupName__ con il nome che si vuole usare per questo gruppo:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Se la richiesta ha esito positivo, si riceverà una risposta serie 200 e il corpo della risposta conterrà un documento JSON che include le informazioni del gruppo. L’elemento `"provisioningState"` conterrà un valore di `"Succeeded"`.

##Creare una distribuzione

Utilizzare le operazioni seguenti per distribuire la configurazione del cluster (valori di modello e di parametri,) nel gruppo di risorse.

* Sostituire __SubscriptionID__ e __AccessToken__ con i valori usati in precedenza. 
* Sostituire __ResourceGroupName__ con il nome del gruppo di risorse creato nella sezione precedente.
* Sostituire __DeploymentName__ con il nome che si vuole usare per questa distribuzione.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Se il documento JSON contenente il modello e i parametri è stato salvato in un file, è possibile usare il codice seguente anziché `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Se la richiesta ha esito positivo, si riceverà una risposta serie 200 e il corpo della risposta conterrà un documento JSON che include le informazioni dell’operazione di distribuzione.

> [AZURE.IMPORTANT] Si noti che la distribuzione è stata inviata, ma non è stata completata in questo momento. Possono essere necessari diversi minuti, in genere circa 15, per completare la distribuzione.

##Controllare lo stato di una distribuzione

Per verificare lo stato della distribuzione, usare il comando seguente:

* Sostituire __SubscriptionID__ e __AccessToken__ con i valori usati in precedenza. 
* Sostituire __ResourceGroupName__ con il nome del gruppo di risorse creato nella sezione precedente.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Restituirà informazioni di un documento JSON che contiene informazioni sull'operazione di distribuzione. L’elemento `"provisioningState"` conterrà lo stato della distribuzione; se contiene un valore di `"Succeeded"`, quindi la distribuzione è stata completata correttamente. A questo punto, il cluster deve essere disponibile per l'utilizzo.

##Passaggi successivi

Dopo aver creato un cluster HDInsight, usare le informazioni seguenti per acquisire familiarità con il cluster.

###Cluster Hadoop

* [Usare Hive con HDInsight](hdinsight-use-hive.md)
* [Usare Pig con HDInsight](hdinsight-use-pig.md)
* [Usare MapReduce con HDInsight](hdinsight-use-mapreduce.md)

###Cluster HBase

* [Introduzione a HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Sviluppare applicazioni Java per HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###Cluster Storm

* [Sviluppare topologie Java per Storm in HDInsight](hdinsight-storm-develop-java-topology.md)
* [Usare i componenti di Python in Storm in HDInsight](hdinsight-storm-develop-python-topology.md)
* [Distribuire e monitorare le topologie con Storm in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

<!---HONumber=AcomDC_0518_2016-->