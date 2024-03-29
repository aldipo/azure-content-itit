<properties
   pageTitle="Distribuire le risorse con il modello e l'interfaccia della riga di comando di Azure | Microsoft Azure"
   description="Utilizzare Azure Resource Manager e l'interfaccia della riga di comando di Azure per distribuire una risorsa in Azure. Le risorse sono definite in un modello di Resource Manager."
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

# Distribuire le risorse con i modelli di Azure Resource Manager e l'interfaccia della riga di comando di Azure

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Interfaccia della riga di comando di Azure](resource-group-template-deploy-cli.md)
- [Portale](resource-group-template-deploy-portal.md)
- [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [API REST](resource-group-template-deploy-rest.md)

In questo argomento viene illustrato come utilizzare l'interfaccia della riga di comando di Azure con i modelli di Azure Resource Manager per distribuire le risorse in Azure.

> [AZURE.TIP] Per informazioni su come eseguire il debug di un errore durante la distribuzione, vedere:
>
> - [Visualizzare le operazioni di distribuzione con l'interfaccia della riga di comando di Azure ](resource-manager-troubleshoot-deployments-cli.md) per informazioni su come risolvere l'errore.
> - [Risolvere errori comuni durante la distribuzione di risorse in Azure con Azure Resource Manager](resource-manager-common-deployment-errors.md) per informazioni sulla risoluzione degli errori di distribuzione più comuni.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## Distribuire con l'interfaccia della riga di comando di Azure

Se l’interfaccia della riga comando di Azure non è stata usata in precedenza con Resource Manager, vedere [Uso dell'interfaccia della riga di comando di Azure per Mac, Linux e Windows con Azure Resource Manager](xplat-cli-azure-resource-manager.md).

1. Accedere al proprio account Azure. Una volta specificate le credenziali, il comando restituisce il risultato dell'accesso.

        azure login
  
        ...
        info:    login command OK

2. Se si hanno più sottoscrizioni, specificare l'ID sottoscrizione che si desidera usare per la distribuzione.

        azure account set <YourSubscriptionNameOrId>

3. Passare al modulo Azure Resource Manager. Si riceverà la conferma della nuova modalità.

        azure config mode arm
   
        info:     New mode is arm

4. Se non è presente un gruppo di risorse, crearne uno nuovo. Specificare il nome del gruppo di risorse e il percorso per la soluzione. Viene restituito un riepilogo del nuovo gruppo di risorse.

        azure group create -n ExampleResourceGroup -l "West US"
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Convalidare la distribuzione prima dell'esecuzione eseguendo il comando **azure group template validate**. Durante il test della distribuzione, specificare i parametri esattamente come quando si esegue la distribuzione (illustrata nel passaggio successivo).

        azure group template validate -f <PathToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup

5. Per creare una nuova distribuzione per il gruppo di risorse, eseguire il comando seguente e specificare i parametri necessari. I parametri includeranno un nome per la distribuzione, il nome del gruppo di risorse, il percorso o l'URL per il modello creato e qualsiasi altro parametro necessario per lo scenario.
   
     Per specificare i valori dei parametri sono disponibili le tre opzioni seguenti:

     1. Usare i parametri inline e un modello locale. Ogni parametro è nel formato: `"ParameterName": { "value": "ParameterValue" }`. Nell'esempio seguente vengono illustrati i parametri con caratteri di escape.

            azure group deployment create -f <PathToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Usare i parametri inline e collegamento a un modello.

            azure group deployment create --template-uri <LinkToTemplate> -p "{"ParameterName":{"value":"ParameterValue"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Usare un file di parametri. Per informazioni sul file di modello, vedere [File di parametri](./#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Dopo aver distribuito le risorse con uno dei tre metodi appena illustrati, verrà visualizzato un riepilogo della distribuzione.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Per eseguire una distribuzione completa, impostare **Mode** su **Complete**. Quando si utilizza questa modalità, fare attenzione a non eliminare inavvertitamente le risorse non presenti nel modello.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Per registrare informazioni aggiuntive sulla distribuzione che potrebbero contribuire a risolvere eventuali errori di distribuzione, utilizzare il parametro **debug-setting**. È possibile specificare la registrazione del contenuto della richiesta, del contenuto della risposta o di entrambi con l'operazione di distribuzione.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## Distribuire modelli dall'archiviazione con token SAS

È possibile aggiungere i modelli a un account di archiviazione e collegarli durante la distribuzione con un token SAS.

> [AZURE.IMPORTANT] Attenendosi alla seguente procedura, il BLOB contenente il modello sarà accessibile solo da parte del proprietario dell'account. Tuttavia, quando si crea un token di firma di accesso condiviso per il BLOB, quest'ultimo sarà accessibile a tutti gli utenti con quell'URI. Se l'URI viene intercettato da un altro utente, quest'ultimo sarà in grado di accedere al modello. Utilizzare un token di firma di accesso condiviso è un buon metodo per limitare l'accesso ai modelli, ma è necessario non includere direttamente nel modello dati sensibili come le password.

### Aggiungere un modello privato all'account di archiviazione

Configurare un account di archiviazione per i modelli nel modo seguente:

1. Creare un nuovo gruppo di risorse.

        azure group create -n "ManageGroup" -l "westus"

2. Creare un nuovo account di archiviazione. Il nome dell'account di archiviazione deve essere univoco in tutto l'ambiente di Azure, pertanto assegnare all'account il proprio nome.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Impostare le variabili della chiave e dell'account di archiviazione.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Creare un nuovo contenitore. L'autorizzazione è impostata su **Off**, pertanto il contenitore è accessibile solo al proprietario.

        azure storage container create --container templates -p Off 
        
4. Aggiungere il proprio modello al contenitore.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### Fornire il token SAS in fase di distribuzione

Per distribuire un modello privato in un account di archiviazione, recuperare un token SAS e includerlo nell'URI del modello.

1. Creare un token di accesso condiviso con autorizzazioni di lettura e un'ora di scadenza per limitare l'accesso. Impostare l'ora di scadenza in modo da garantire un tempo sufficiente per completare la distribuzione. Recuperare l'URI completo del modello che include il token SAS.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Distribuire il modello fornendo l'URI che include il token SAS.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Per un esempio sull'utilizzo di un token SAS con i modelli collegati, vedere [Uso di modelli collegati con Gestione risorse di Azure](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## Passaggi successivi
- Per un esempio di distribuzione delle risorse con la libreria client .NET, vedere [Distribuire le risorse usando le librerie .NET e un modello](virtual-machines/virtual-machines-windows-csharp-template.md).
- Per definire i parametri nel modello, vedere [Creazione di modelli](resource-group-authoring-templates.md#parameters).
- Per indicazioni sulla distribuzione della soluzione in ambienti diversi, vedere [Ambienti di sviluppo e test in Microsoft Azure](solution-dev-test-environments.md).
- Per informazioni dettagliate sull'utilizzo di un riferimento KeyVault per passare valori protetti, vedere [Passare valori protetti durante la distribuzione](resource-manager-keyvault-parameter.md).

<!---HONumber=AcomDC_0615_2016-->