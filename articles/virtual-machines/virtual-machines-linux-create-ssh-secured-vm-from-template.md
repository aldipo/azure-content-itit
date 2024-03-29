<properties
	pageTitle="Creare una VM Linux protetta usando un modello di Azure | Microsoft Azure"
	description="Creare una VM Linux protetta in Azure con un modello di Azure Resource Manager."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="vlivech"
	manager="timlt"
	editor=""
	tags="azure-service-management,azure-resource-manager" />

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="04/27/2016"
	ms.author="v-livech"/>

# Creare una VM Linux protetta usando un modello di Azure

Per creare una VM Linux da un modello, è necessaria l'[interfaccia della riga di comando di Azure](../xplat-cli-install.md) in modalità Resource Manager (`azure config mode arm`).

## Breve riepilogo del comando

```bash
chrisl@fedora$ azure group create -n <exampleRGname> -l <exampleAzureRegion> [--template-uri <URL> | --template-file <path> | <template-file> -e <parameters.json file>]
```

## Procedura dettagliata

I modelli consentono di creare VM in Azure con le impostazioni da personalizzare durante l'avvio, ad esempio i nomi utente e i nomi host. Questo articolo è incentrato sull'avvio di una VM Ubuntu con un modello di Azure che crea un gruppo di sicurezza di rete con una sola porta aperta (22 per SSH) e che richiede chiavi SSH per l'accesso.

I modelli di Azure Resource Manager sono file JSON che possono essere usati per semplici attività occasionali, ad esempio l'avvio di una VM Ubuntu come descritto in questo articolo, o per creare complesse configurazioni di Azure di interi ambienti, quale come una distribuzione di test, sviluppo o produzione dalla rete al sistema operativo fino alla distribuzione di stack di applicazioni.

## Creare la VM Linux

L'esempio di codice seguente illustra come chiamare `azure group create` per creare un gruppo di risorse e distribuire una VM Linux protetta con SSH, usando contemporaneamente [questo modello di Azure Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Tenere presente che nell'esempio è necessario usare nomi univoci per l'ambiente locale. Questo esempio usa `quicksecuretemplate` come nome del gruppo di risorse, `securelinux` come nome della VM e `quicksecurelinux` come nome del sottodominio.

```bash
chrisl@fedora$ azure group create -n quicksecuretemplate -l eastus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
info:    Executing command group create
+ Getting resource group quicksecuretemplate
+ Creating resource group quicksecuretemplate
info:    Created resource group quicksecuretemplate
info:    Supply values for the following parameters
adminUserName: ops
sshKeyData: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDRZ/XB8p8uXMqgI8EoN3dWQw... user@contoso.com
dnsLabelPrefix: quicksecurelinux
vmName: securelinux
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<guid>/resourceGroups/quicksecuretemplate
data:    Name:                quicksecuretemplate
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

È possibile creare un nuovo gruppo di risorse e distribuire una VM usando il parametro `--template-uri` oppure è possibile scaricare o creare un modello in locale e passare il modello usando il parametro `--template-file` con un percorso del file modello come argomento. L'interfaccia della riga di comando di Azure richiede i parametri necessari per il modello.

## Passaggi successivi

Dopo aver creato le VM Linux con i modelli, è possibile vedere quali altri framework per app sono disponibili per l'uso con i modelli. Eseguire una ricerca nella [raccolta di modelli](https://azure.microsoft.com/documentation/templates/) per scoprire quali framework per app si possono distribuire successivamente.

<!---HONumber=AcomDC_0504_2016-->