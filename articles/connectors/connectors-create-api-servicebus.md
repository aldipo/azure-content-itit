<properties
pageTitle="Usare il connettore del bus di servizio di Azure nelle app per la logica | Microsoft Azure"
description="Introduzione all'uso del connettore del bus di servizio di Azure (connettore) nelle app per la logica del Servizio app di Microsoft Azure"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/23/2016"
ms.author="deonhe"/>

# Introduzione al connettore del bus di servizio di Azure 

Connettersi al bus di servizio di Azure per inviare e ricevere messaggi. È possibile eseguire varie azioni, ad esempio inviare alla coda, inviare all'argomento, ricevere dalla coda, ricevere dalla sottoscrizione, e così via.

>[AZURE.NOTE] Questa versione dell'articolo si applica alla versione dello schema 2015-08-01-preview delle app per la logica.

Con il bus di servizio di Azure è possibile:

* Compilare app per la logica  

Per aggiungere un'operazione nelle app per la logica, vedere [Creare un'app per la logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Informazioni su trigger e azioni

Il connettore del bus di servizio di Azure include trigger e può essere usato come azione. Tutti i connettori supportano dati nei formati JSON e XML.

 Nel connettore del bus di servizio di Azure sono disponibili le azioni e/o i trigger seguenti:

### Azioni del bus di servizio di Azure
È possibile eseguire queste azioni:

|Azione|Descrizione|
|--- | ---|
|SendMessage|Invia un messaggio alla coda o all'argomento del bus di servizio di Azure.|
### Trigger del bus di servizio di Azure
È possibile ascoltare questi eventi:

|Trigger | Descrizione|
|--- | ---|
|GetMessageFromQueue|Recupera un nuovo messaggio dalla coda del bus di servizio di Azure.|
|GetMessageFromTopic|Recupera un nuovo messaggio dalla sottoscrizione dell'argomento del bus di servizio di Azure.|


## Creare una connessione al bus di servizio di Azure
Per usare il connettore del bus di servizio di Azure, creare prima una **connessione**, quindi indicare i dettagli di queste proprietà:

>[AZURE.INCLUDE [Passaggi per la creazione di una connessione al bus di servizio](../../includes/connectors-create-api-servicebus.md)]

>[AZURE.TIP] È possibile usare questa connessione in altre app per la logica.

## Informazioni di riferimento sulle API REST del bus di servizio di Azure
#### Questa documentazione è relativa alla versione 1.0


### Invia un messaggio alla coda o all'argomento del bus di servizio di Azure.
**```POST: /{entityName}/messages```**



| Nome| Tipo di dati|Obbligatorio|Posizione|Valore predefinito|Descrizione|
| ---|---|---|---|---|---|
|message| |yes|body|nessuno|Messaggio del bus di servizio|
|entityName|string|yes|path|nessuno|Nome della coda o dell'argomento|


### Ecco le risposte possibili:

|Nome|Descrizione|
|---|---|
|200|OK|
|default|Operazione non riuscita.|
------



### Recupera un nuovo messaggio dalla coda del bus di servizio di Azure.
**```GET: /{queueName}/messages/head```**



| Nome| Tipo di dati|Obbligatorio|Posizione|Valore predefinito|Descrizione|
| ---|---|---|---|---|---|
|queueName|string|yes|path|nessuno|Nome della coda.|


### Ecco le risposte possibili:

|Nome|Descrizione|
|---|---|
|200|OK|
|default|Operazione non riuscita.|
------



### Recupera un nuovo messaggio dalla sottoscrizione dell'argomento del bus di servizio di Azure.
**```GET: /{topicName}/subscriptions/{subscriptionName}/messages/head```**



| Nome| Tipo di dati|Obbligatorio|Posizione|Valore predefinito|Descrizione|
| ---|---|---|---|---|---|
|topicName|string|yes|path|nessuno|Nome dell'argomento.|
|subscriptionName|string|yes|path|nessuno|Nome della sottoscrizione dell'argomento.|


### Ecco le risposte possibili:

|Nome|Descrizione|
|---|---|
|200|OK|
|default|Operazione non riuscita.|
------



## Definizioni degli oggetti: 

 **ServiceBusMessage**: messaggio che include il contenuto e le proprietà

Proprietà obbligatorie per ServiceBusMessage:

ContentTransferEncoding

**Tutte le proprietà**:


| Nome | Tipo di dati |
|---|---|
|ContentData|string|
|ContentType|string|
|ContentTransferEncoding|string|
|Proprietà|oggetto|


## Passaggi successivi
[Creare un'app per la logica](../app-service-logic/app-service-logic-create-a-logic-app.md).

<!---HONumber=AcomDC_0525_2016-->