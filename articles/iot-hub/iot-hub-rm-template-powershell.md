<properties
	pageTitle="Creare un hub IoT usando un modello di Gestione risorse di Azure e PowerShell | Microsoft Azure"
	description="Seguire questa esercitazione per iniziare a usare modelli di Gestione risorse per creare un hub IoT con PowerShell."
	services="iot-hub"
	documentationCenter=".net"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="05/03/2016"
     ms.author="dobett"/>

# Creare un hub IoT tramite PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## Introduzione

È possibile usare Gestione risorse di Azure per creare e gestire hub IoT di Azure a livello di codice. In questa esercitazione viene illustrato come usare un modello di Resource Manager per creare un hub IoT con PowerShell.

> [AZURE.NOTE] Azure offre due diversi modelli di distribuzione per creare e usare le risorse: [Gestione risorse e la distribuzione classica](../resource-manager-deployment-model.md). Questo articolo illustra l’utilizzo del modello di distribuzione Gestione risorse.

Per completare questa esercitazione, sono necessari gli elementi seguenti:

- Un account Azure attivo. <br/>Se non si dispone di un account, è possibile creare un account di valutazione gratuita in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure][lnk-free-trial].
- [Microsoft Azure PowerShell 1.0][lnk-powershell-install] o versione successiva.

> [AZURE.TIP] L'articolo [Uso di Azure PowerShell con Gestione risorse di Azure][lnk-powershell-arm] fornisce altre informazioni su come usare gli script di PowerShell e i modelli di Gestione risorse di Azure per creare risorse di Azure.

## Connettersi alla sottoscrizione di Azure

In un prompt dei comandi di PowerShell, immettere il comando seguente per accedere alla sottoscrizione di Azure:

```
Login-AzureRmAccount
```

È possibile usare i comandi seguenti per individuare dove è possibile distribuire un hub IoT e le versioni API attualmente supportate:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Creare un gruppo di risorse per contenere l'hub IoT usando il comando seguente in una delle località supportate per l'hub IoT. In questo esempio viene creato un gruppo di risorse denominato **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## Inviare un modello per creare un hub IoT

Utilizzare un modello JSON per creare un nuovo hub IoT nel gruppo di risorse. È anche possibile utilizzare un modello per apportare modifiche a un hub IoT esistente.

1. Usare un editor di testo per creare un modello di Gestione risorse di Azure denominato **template.json** con la definizione di risorsa seguente per creare un nuovo hub IoT standard. In questo esempio l'hub IoT viene aggiunto all'area **Stati Uniti orientali**, vengono creati due gruppi di consumer (**cg1** e **cg2**) sull'endpoint compatibile con Hub eventi e viene usata la versione **2016-02-03** dell'API. Questo modello prevede anche che il nome dell'hub IoT venga passato come un parametro denominato **hubName**.

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Salvare il file di modello nel computer locale. Questo esempio presuppone che il file venga salvato in una cartella denominata **c:\\templates**.

3. Eseguire il comando seguente per distribuire il nuovo hub IoT, passando il nome dell'hub IoT come parametro. In questo esempio, il nome dell'hub IoT è **abcmyiothub** (notare che questo nome deve essere globalmente univoco, quindi deve includere il nome o le iniziali):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. L'output visualizza le chiavi per l'hub IoT che è stato creato.

5. È possibile verificare che l'applicazione abbia aggiunto il nuovo hub IoT visitando il [portale][lnk-azure-portal] e visualizzare l'elenco di risorse, oppure utilizzando il cmdlet PowerShell **Get-AzureRmResource**.

> [AZURE.NOTE] Questa applicazione di esempio aggiunge un hub IoT Standard S1 che verrà addebitato. È possibile eliminare l'hub IoT tramite il [portale][lnk-azure-portal] o utilizzando il cmdlet PowerShell **Remove AzureRmResource** al termine.

## Passaggi successivi

Dopo aver distribuito un hub IoT usando un modello di Gestione risorse di Azure con PowerShell, può essere opportuno ottenere informazioni più dettagliate:

- Informazioni sulle funzionalità dell'[API REST del provider di risorse dell'hub IoT][lnk-rest-api].
- Per ulteriori informazioni sulle funzionalità di Gestione risorse di Azure, leggere la [Panoramica su Gestione risorse di Azure][lnk-azure-rm-overview].

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

<!---HONumber=AcomDC_0504_2016-->