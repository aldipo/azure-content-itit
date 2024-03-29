<properties
   pageTitle="Modelli collegati con Resource Manager | Microsoft Azure"
   description="Descrive come usare i modelli collegati in un modello di Azure Resource Manager per creare una soluzione basata su un modello modulare. Mostra come passare i valori dei parametri, specificare un file di parametri e gli URL creati in modo dinamico."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/08/2016"
   ms.author="tomfitz"/>

# Uso di modelli collegati con Azure Resource Manager

Dall'interno di un modello di Azure Resource Manager, è possibile collegarsi a un altro modello che consente di scomporre la distribuzione in un set di modelli di destinazione specifici. In modo analogo alla scomposizione di un'applicazione in un numero di classi di codice, la scomposizione offre vantaggi in termini di test, riuso e leggibilità.

È possibile passare parametri da un modello principale a un modello collegato. Tali parametri possono venire associati direttamente ai parametri e alle variabili esposti dal modello chiamante. Il modello collegato può inoltre passare una variabile di output al modello di origine, consentendo un scambio bidirezionale di dati tra modelli.

## Collegamento a un modello

Per creare un collegamento tra due modelli, aggiungere una risorsa di distribuzione all'interno del modello principale che punta al modello collegato. Impostare la proprietà **templateLink** sull'URI del modello collegato. È possibile fornire i valori dei parametri per il modello nidificato specificando i valori direttamente nel modello o mediante il collegamento a un file di parametri. Nel seguente esempio viene utilizzata la proprietà **parameters** per specificare direttamente un valore di parametro.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Il servizio di Resource Manager deve essere in grado di accedere al modello collegato, il che significa che non è possibile specificare un file locale per il modello collegato che sia disponibile solo nella rete locale. È possibile fornire solo un valore URI che includa **http** o **https**. Una possibilità è quella di collocare il modello collegato in un account di archiviazione e utilizzare l'URI per tale elemento, come illustrato di seguito.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Anche se il modello collegato deve essere disponibile esternamente, non è necessario che sia pubblicamente disponibile. È possibile aggiungere il modello a un account di archiviazione privato accessibile solo da parte del proprietario dell'account di archiviazione e quindi creare un token di firma di accesso condiviso (SAS) per consentire l'accesso durante la distribuzione. Aggiungere il token con firma di accesso condiviso all'URI del modello collegato. Per conoscere la procedura per la configurazione di un modello in un account di archiviazione e per la generazione di un token con firma di accesso condiviso, consultare [Distribuire le risorse con i modelli di Resource Manager e Azure PowerShell](resource-group-template-deploy.md) o [Distribuire le risorse con i modelli di Azure Resource Manager e l'interfaccia della riga di comando di Azure](resource-group-template-deploy-cli.md).

Nell'esempio seguente viene illustrato un modello padre che si collega a un altro modello. È possibile accedere al modello nidificato con un token con firma di accesso condiviso passato come parametro.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Anche se il token viene passato come stringa sicura, l'URI del modello collegato, incluso il token di firma di accesso condiviso, è registrato nelle operazioni di distribuzione per il gruppo di risorse in questione. Per limitare l'esposizione, impostare una scadenza per il token.

## Collegamento a un file di parametri

Nell'esempio successivo viene utilizzata la proprietà **parametersLink** per il collegamento a un file di parametri.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Il valore URI per il file di parametro collegato non può essere un file locale e deve includere o **http** o **https**. Naturalmente, è possibile limitare l'accesso al file dei parametri solo tramite un token con firma di accesso condiviso.

## Uso di variabili per collegare i modelli

Negli esempi precedenti sono illustrati i valori di URL hard-coded per i collegamenti del modello. Questo approccio potrebbe funzionare per un modello semplice ma non funziona correttamente in caso di uso di un ampio set di modelli modulari. In alternativa, è possibile creare una variabile statica contenente un URL di base per il modello principale e quindi creare dinamicamente gli URL per i modelli collegati da tale URL di base. Il vantaggio di questo approccio è rappresentato dal fatto che risulta più semplice spostare o scomporre il modello perché è sufficiente modificare la variabile statica nel modello principale. Il modello principale passa gli URI corretti a tutto il modello scomposto.

Nell'esempio seguente viene illustrato come usare un URL di base per creare due URL per i modelli collegati (**sharedTemplateUrl** e **vmTemplate**).

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

È inoltre possibile usare [deployment ()](resource-group-template-functions.md#deployment) per ottenere l'URL di base per il modello corrente e usarlo per ottenere l'URL per altri modelli nello stesso percorso. Ciò è utile se il percorso del modello cambia (forse a causa del controllo delle versioni) o si desidera evitare la codifica URL nel file del modello.

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## Esempio completo

I seguenti modelli di esempio mostrano una disposizione semplificata di modelli collegati, allo scopo di illustrare alcuni dei concetti in questo articolo. supponendo che i modelli siano stati aggiunti nello stesso contenitore in un account di archiviazione con accesso pubblico disattivato. Il modello collegato restituisce un valore al modello principale nella sezione degli **output**.

Il file **parent.json** è costituito da:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('nestedTemplate').outputs.result]"
        }
      }
    }

Il file **helloworld.json** è costituito da:

    {
	  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
	  "contentVersion": "1.0.0.0",
	  "parameters": {},
	  "variables": {},
	  "resources": [],
	  "outputs": {
		"result": {
			"value": "Hello World",
			"type" : "string"
		}
	  }
    }
    
Nella PowerShell, ottenere un token per il contenitore e distribuire i modelli con:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

Nell'interfaccia della riga di comando di Azure, ottenere un token per il contenitore e distribuire i modelli con il seguente codice. Attualmente, è necessario specificare un nome da assegnare alla distribuzione quando si utilizza l'URI di un modello che include un token con firma di accesso condiviso.

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Verrà richiesto di fornire il token con firma di accesso condiviso come parametro. È necessario che il token sia preceduto da **?**.

## Passaggi successivi
- Per informazioni sulla definizione dell'ordine di distribuzione per le risorse, vedere [Definizione delle dipendenze nei modelli di Azure Resource Manager](resource-group-define-dependencies.md)
- Per informazioni su come definire una sola risorsa ma creare molte istanze di essa, vedere [Creare più istanze di risorse in Azure Resource Manager](resource-group-create-multiple.md)

<!---HONumber=AcomDC_0615_2016-->