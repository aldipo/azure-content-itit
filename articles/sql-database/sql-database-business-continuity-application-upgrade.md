<properties 
   pageTitle="Continuità aziendale del database SQL durante l'aggiornamento dell'applicazione" 
   description="Questa sezione fornisce informazioni su come evitare tempo di inattività durante l'aggiornamento di un'applicazione." 
   services="sql-database"
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="05/27/2016"
   ms.author="carlrab"/>

#Aggiornare un'applicazione senza tempo di inattività

Nel contesto di Microsoft Azure, il termine "applicazione" fa riferimento a componenti come i front-end, i servizi distribuiti in un servizio cloud e il livello dati usato per rendere persistenti i metadati o i dati delle applicazioni. Le applicazioni cloud sono spesso progettate in modo da garantire un servizio ininterrotto 24 ore su 24, 7 giorni su 7. L'implementazione di una nuova versione dell'applicazione, quando nel sito attivo vengono applicate modifiche al livello dati, può causare alcuni problemi, ad esempio la disponibilità limitata di alcune funzionalità o addirittura uno stato completo di inattività.

Quando si progetta il processo di aggiornamento di un'applicazione, l'obiettivo principale deve essere quello di eliminare o ridurre drasticamente l'intervallo di tempo in cui la capacità dell'applicazione risulta limitata. A questo scopo, il processo comporta in genere la creazione di una copia temporanea dell'applicazione da usare come backup nel caso in cui l'aggiornamento non riesca. Durante la progettazione e la pianificazione dell'aggiornamento, è necessario considerare i fattori seguenti:

1.	Tempo massimo accettabile in cui l'applicazione avrà una capacità limitata. 
2.	Set minimo di funzionalità che saranno disponibili durante il processo di aggiornamento.
3.	Possibilità di eseguire il rollback in caso di errori durante l'aggiornamento.
4.	Costo totale. Sono inclusi il costo dei componenti aggiuntivi dell’applicazione necessari per creare una copia temporanea, ad esempio database aggiuntivi per la replica geografica attiva) e i costi incrementali per le distribuzioni temporanee utilizzate dal processo di aggiornamento. 

Se l'applicazione può funzionare temporaneamente in modalità di sola lettura, il flusso di lavoro di aggiornamento potrebbe essere progettato in modo da eliminare completamente il tempo di inattività. Per comprendere come implementare il flusso di lavoro di aggiornamento per la specifica topologia dell'applicazione, vedere [Gestione degli aggiornamenti in sequenza delle applicazioni cloud con la replica geografica attiva del database SQL](sql-database-manage-application-rolling-upgrade.md).
 
 
 

<!---HONumber=AcomDC_0615_2016-->