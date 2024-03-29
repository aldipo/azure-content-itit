<properties
   pageTitle="Creare uno spazio dei nomi del bus di servizio usando il portale di Azure | Microsoft Azure"
   description="Per iniziare a usare il bus di servizio, è necessario uno spazio dei nomi. Ecco come crearne uno tramite il portale di Azure."
   services="service-bus"
   documentationCenter=".net"
   authors="jtaubensee"
   manager="timlt"
   editor="sethmanheim"/>

<tags
   ms.service="service-bus"
   ms.devlang="tbd"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="dotnet"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="jotaub@microsoft.com"/>

#Creare uno spazio dei nomi del bus di servizio usando il portale di Azure
Lo spazio dei nomi è un contenitore comune per tutti i componenti di messaggistica. Più code e argomenti possono risiedere in un unico spazio dei nomi e gli spazi dei nomi vengono spesso usati come contenitori di applicazioni. Esistono attualmente 2 diversi modi per creare uno spazio dei nomi del bus di servizio.

1.	Portale di Azure (in questo articolo)

2.	[Modelli di Gestione risorse di Azure][create-namespace-using-arm]

##Creazione di uno spazio dei nomi nel portale di Azure
[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Congratulazioni. È stato creato uno spazio dei nomi del bus di servizio.

##Passaggi successivi

Per esempi che illustrano alcune delle funzionalità più avanzate di messaggistica del bus di servizio di Azure, vedere il repository GitHub. [https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples]

[create-namespace-using-arm]: ./service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

<!---HONumber=AcomDC_0608_2016-->