<properties
    pageTitle="Creare uno spazio dei nomi del bus di servizio con Hub eventi e un gruppo di consumer usando un modello di Azure Resource Manager | Microsoft Azure"
    description="Creare uno spazio dei nomi del bus di servizio con Hub eventi e un gruppo di consumer usando un modello di Azure Resource Manager"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="04/15/2016"
    ms.author="sethm;shvija"/>

# Creare uno spazio dei nomi del bus di servizio con Hub eventi e un gruppo di consumer usando un modello di Azure Resource Manager

Questo articolo illustra come usare un modello di Azure Resource Manager per creare uno spazio dei nomi del bus di servizio con Hub eventi e un gruppo di consumer. Verrà illustrato come definire le risorse da distribuire e i parametri specificati quando viene eseguita la distribuzione. È possibile usare questo modello per la distribuzione o personalizzarlo in base alle esigenze.

Per altre informazioni sulla creazione dei modelli, vedere [Creazione di modelli di Azure Resource Manager][].

Per il modello completo, vedere il [modello di Hub eventi e del gruppo di consumer del bus di servizio][] su GitHub.

>[AZURE.NOTE] Questi modelli di Azure Resource Manager sono disponibili per il download e la distribuzione.
>
>-    [Creare uno spazio dei nomi del bus di servizio con coda e regola di autorizzazione](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Creare uno spazio dei nomi del bus di servizio con coda](service-bus-resource-manager-namespace-queue.md)
>-    [Creare uno spazio dei nomi del bus di servizio con argomento e sottoscrizione](service-bus-resource-manager-namespace-topic.md)
>-    [Creare uno spazio dei nomi del bus di servizio](service-bus-resource-manager-namespace.md)
>
>Per verificare gli ultimi modelli, vedere i [modelli di avvio rapido di Azure][] e cercare il bus di servizio.

## Distribuzione

Questo modello consente di distribuire uno spazio dei nomi del bus di servizio con Hub eventi e un gruppo di consumer.

Hub eventi è un servizio di elaborazione eventi e viene usato per offrire ingresso a eventi e dati di telemetria in Azure su larga scala, con bassa latenza e affidabilità elevata.

Altre informazioni su [Hub eventi di Azure](../event-hubs/event-hubs-what-is-event-hubs.md).

Per eseguire automaticamente la distribuzione, fare clic sul pulsante seguente:

[![Distribuzione in Azure](./media/service-bus-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-eventhub-and-consumergroup%2Fazuredeploy.json)

## Parametri

Gestione risorse di Azure permette di definire i parametri per i valori da specificare durante la distribuzione del modello. Il modello include una sezione denominata `Parameters` che contiene tutti i valori dei parametri. È necessario definire un parametro per i valori che variano in base al progetto distribuito o all'ambiente in cui viene distribuito il progetto. Non definire i parametri per i valori che rimangono invariati. Ogni valore di parametro nel modello viene usato per definire le risorse distribuite.

Di seguito viene fornita la descrizione di ogni parametro del modello.

### serviceBusNamespaceName

Nome dello spazio dei nomi del bus di servizio da creare.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### serviceBusEventHubName

Nome dell'hub eventi creato nello spazio dei nomi del bus di servizio.

```
"serviceBusEventHubName": {
"type": "string"
}
```

### serviceBusConsumerGroupName

Nome del gruppo di consumer creato per l'hub eventi nello spazio dei nomi del bus di servizio.

```
"serviceBusConsumerGroupName": {
"type": "string"
}
```

### serviceBusApiVersion

Versione API del bus di servizio del modello.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## Risorse da distribuire

Crea uno spazio dei nomi del bus di servizio di tipo **Hub eventi** con Hub eventi e un gruppo di consumer.

```
"resources": [
        {
            "apiVersion": "[variables('ehVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/Namespaces",
            "location": "[variables('location')]",
            "kind": "EventHub",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('ehVersion')]",
                    "name": "[parameters('serviceBusEventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusEventHubName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('ehVersion')]",
                            "name": "[parameters('serviceBusConsumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('serviceBusEventHubName')]"
                            ],
                            "properties": {
                            }
                        }
                    ]
                }
            ]
        }
    ]
```

## Comandi per eseguire la distribuzione

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-eventhub-and-consumergroup/azuredeploy.json
```

## Interfaccia della riga di comando di Azure

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-eventhub-and-consumergroup/azuredeploy.json][]
```

## Passaggi successivi

Dopo aver creato e distribuito le risorse con Azure Resource Manager, è possibile imparare a gestire queste risorse. Leggere gli articoli seguenti:

- [Gestire il bus di servizio di Azure mediante Automazione di Azure](service-bus-automation-manage.md)
- [Gestire Hub eventi con PowerShell](service-bus-powershell-how-to-provision.md)
- [Gestire le risorse del bus di Hub eventi con Service Bus Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Creazione di modelli di Azure Resource Manager]: ../resource-group-authoring-templates.md
  [modelli di avvio rapido di Azure]: https://azure.microsoft.com/documentation/templates/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [modello di Hub eventi e del gruppo di consumer del bus di servizio]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-eventhub-and-consumergroup/

<!---HONumber=AcomDC_0518_2016-->