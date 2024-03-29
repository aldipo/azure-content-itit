<properties
	pageTitle="Concetti relativi a Mobile Engagement | Microsoft Azure"
	description="Concetti relativi ad Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="dwrede"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="02/26/2016"
	ms.author="piyushjo" />

# Concetti relativi ad Azure Mobile Engagement

Mobile Engagement definisce alcuni concetti comuni a tutte le piattaforme supportate, descritti brevemente in questo articolo.

Questo articolo è un buon punto di partenza per chi non ha familiarità con Mobile Engagement. Leggere anche la documentazione specifica per la piattaforma in uso, per informazioni più dettagliate sui concetti illustrati in questo articolo, con esempi e indicazioni relative alle possibili limitazioni.

## Dispositivi e utenti
Mobile Engagement identifica gli utenti generando un identificatore univoco per ogni dispositivo, detto identificatore di dispositivo o `deviceid`. L'identificatore viene generato in modo che tutte le applicazioni in esecuzione nello stesso dispositivo condividano lo stesso identificatore di dispositivo.

Implicitamente, ciò significa che Mobile Engagement considera che un dispositivo appartenga esattamente a un utente e, di conseguenza, utenti e dispositivi sono concetti equivalenti.

## Sessioni e attività
Una sessione indica un uso dell'applicazione da parte di un utente, dal momento in cui inizia a usarla fino a quando finisce.

Un'attività è un utilizzo di una determinata parte secondaria dell'applicazione da parte di un utente (in genere è una schermata, ma può essere qualunque elemento adeguato, in base all'applicazione).

Un utente può svolgere una sola attività per volta.

Un'attività è identificata da un nome (costituito da un massimo di 64 caratteri) e può incorporare alcuni dati aggiuntivi (con un limite di 1024 byte).

Le sessioni vengono calcolate automaticamente dalla sequenza di attività eseguite dagli utenti. Una sessione inizia quando l'utente inizia la prima attività e si arresta quando l'utente avrà terminato l'ultima attività. Ciò significa che una sessione non deve necessariamente essere avviata o arrestata in modo esplicito, al contrario, le attività vengono avviate o arrestate in modo esplicito. Se non viene segnalata alcuna attività, non viene segnalata alcuna sessione.

## Events
Gli eventi vengono usati per segnalare azioni istantanee (come la pressione di un pulsante o la lettura di articoli da parte di utenti).

Un evento può essere correlato alla sessione corrente o a un processo in esecuzione oppure può essere autonomo.

Un evento è identificato da un nome (costituito da un massimo di 64 caratteri) e può incorporare alcuni dati aggiuntivi (con un limite di 1024 byte).

## Errore
Gli errori vengono usati per segnalare problemi rilevati correttamente dall'applicazione, ad esempio azioni utente non corrette o errori delle chiamate API.

Un errore può essere correlato alla sessione corrente o a un processo in esecuzione oppure può essere autonomo.

Un errore è identificato da un nome (costituito da un massimo di 64 caratteri) e può incorporare alcuni dati aggiuntivi (con un limite di 1024 byte).

## Processo
I processi sono usati per segnalare azioni con una durata (come la durata delle chiamate API, il tempo di visualizzazione degli annunci, la durata delle attività in background o la durata delle azioni utente).

Un processo non è correlato a una sessione, perché l'attività può essere eseguita in background, senza alcuna interazione dell'utente.

Un processo è identificato da un nome (costituito da un massimo di 64 caratteri) e può incorporare alcuni dati aggiuntivi (con un limite di 1024 byte).

## Arresto anomalo
Gli arresti anomali vengono generati automaticamente da Mobile Engagement SDK per segnalare errori dell'applicazione in cui problemi non rilevati dall'applicazione causano l'arresto anomalo.

## Informazioni sull'applicazione
Le informazioni sull'applicazione (o info sull'app) vengono usate per aggiungere tag agli utenti, ovvero per associare dati agli utenti di un'applicazione. Questo comportamento è simile a quello dei cookie Web, tranne per il fatto che le info sull'app vengono archiviate sul lato server nella piattaforma Azure Mobile Engagement.

Le info sull'app possono essere registrate usando l'API di Mobile Engagement SDK o l'API dispositivo della piattaforma Mobile Engagement.

Le info sull'app sono costituite da una coppia chiave/valore associata a un dispositivo. La chiave è il nome delle informazioni sull'applicazione (costituito da un massimo di 64 lettere ASCII [a-zA-Z], numeri [0-9] e caratteri di sottolineatura [\_]). Il valore (costituito da un massimo di 1024 caratteri) può essere qualsiasi stringa, numero intero, data (aaaa-MM-gg) o valore booleano (true o false).

A ogni dispositivo può essere associato un numero qualsiasi di informazioni sull'applicazione, entro i limiti definiti dalle condizioni tariffarie di Mobile Engagement. Per una determinata chiave, Mobile Engagement tiene traccia solo dell'ultimo valore impostato (senza cronologia). Se si imposta o si modifica il valore delle informazioni sull'applicazione, Mobile Engagement valuta nuovamente i criteri dei destinatari impostati nelle informazioni sull'applicazione, se presenti, pertanto le informazioni sull'applicazione possono essere usate per attivare push in tempo reale.

## Dati aggiuntivi
I dati aggiuntivi (o extra) sono dati arbitrari che possono essere collegati a eventi, errori, attività e processi.

La loro struttura è simile a quella degli oggetti JSON: sono costituiti da un albero di coppie chiave/valore. Le chiavi sono costituite al massimo da 64 lettere ASCII [a-zA-Z], numeri [0-9] e caratteri di sottolineatura [\_] e le dimensioni totali dei dati aggiuntivi sono limitate a 1024 caratteri (dopo la codifica in JSON tramite Mobile Engagement SDK).

L'intero albero di coppie chiave/valore viene archiviato come oggetto JSON. Tuttavia, solo il primo livello di chiavi/valori viene scomposto per essere direttamente accessibile per alcune funzioni avanzate come i segmenti (ad esempio, è possibile definire in modo semplice un segmento denominato "SciFi fans" costituito da tutti gli utenti che hanno inviato almeno 10 volte l'evento denominato "content\_viewed" con la chiave aggiuntiva "content\_type" impostata sul valore "scifi" nell'ultimo mese). È pertanto consigliabile inviare solo dati aggiuntivi costituiti da semplici elenchi di coppie chiave/valore con valori scalari (ad esempio stringhe, date, numeri interi o valori booleani).

## Passaggi successivi

- [Panoramica di Windows Universal SDK per Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
- [Panoramica di Windows Phone Silverlight SDK per Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK per Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
- [Android SDK per Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

<!---HONumber=AcomDC_0302_2016-->