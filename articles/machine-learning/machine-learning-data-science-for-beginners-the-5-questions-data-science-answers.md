<properties
   pageTitle="Le 5 domande sull'analisi scientifica dei dati - Analisi scientifica dei dati per principianti | Microsoft Azure"
   description="È possibile ottenere una rapida introduzione all'analisi scientifica dei dati grazie alla serie Analisi scientifica dei dati per principianti, cinque brevi video che iniziano con le 5 domande a cui l'analisi scientifica dei dati può rispondere."
   keywords="effettuare l'analisi scientifica dei dati, introduzione all'analisi scientifica dei dati, analisi scientifica dei dati per principianti, tipi di domande, domande sull'analisi scientifica dei dati, algoritmi di analisi scientifica dei dati"
   services="machine-learning"
   documentationCenter="na"
   authors="brohrer-ms"
   manager="paulettm"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/24/2016"
   ms.author="cgronlun;brohrer;garye"/>

# Analisi scientifica dei dati per principianti, video 1: le 5 domande a cui l'analisi scientifica dei dati può rispondere

È possibile ottenere una rapida introduzione all'analisi scientifica dei dati grazie alla serie *Analisi scientifica dei dati per principianti* in cinque brevi video. Questa serie di video è utile se si è interessati ad analizzare scientificamente i dati, oppure a lavorare con persone che analizzano scientificamente i dati, e si desidera iniziare con i concetti più essenziali.

Per trarre il meglio dalla serie è consigliabile guardare i video in ordine. [L'elenco dei video è disponibile qui](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## Trascrizione: Le 5 domande a cui l'analisi scientifica dei dati può rispondere

Ciao! Benvenuti alla serie di video *Analisi scientifica dei dati per principianti*.

Il concetto di analisi scientifica dei dati può far paura, quindi in questa occasione illustrerò i concetti di base senza ricorrere a equazioni o al gergo informatico di programmazione.

Nel primo video si parlerà delle "5 domande a cui l'analisi scientifica dei dati può rispondere".

L'analisi scientifica dei dati usa numeri e nomi (anche noti come categorie o etichette) per prevedere le risposte alle domande.

Potrebbe essere sorprendente, ma *esistono solo cinque domande a cui l'analisi scientifica dei dati risponde*:

  * È A o B?
  * È strano?
  * In che quantità o in che numero?
  * In che modo sono organizzati i dati?
  * Qual è il prossimo passo da fare?

  La risposta a ciascuna di queste domande viene fornita da una famiglia a parte di metodi di apprendimento automatico, detti algoritmi.


È utile pensare a un algoritmo come una ricetta e ai dati come gli ingredienti. Un algoritmo specifica come combinare e mettere insieme i dati per ottenere una risposta. I computer sono simili ai frullatori. Svolgono la maggior parte del lavoro più impegnativo dell'algoritmo per l'utente e lo fanno in modo abbastanza veloce.

## Domanda 1: È A o B? usa gli algoritmi di classificazione

Ecco la prima domanda: È A o B?

![Algoritmi di classificazione: È A o B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Questa famiglia di algoritmi viene detta classificazione a due classi.

È utile per qualsiasi domanda che può avere solo due possibili risposte.

ad esempio:

  *	Sarà possibile percorrere le prossime 1.000 miglia con questi pneumatici: Sì o No?
  *	Cosa porta più clienti: un coupon da $ 5 o uno sconto del 25%?

Questa domanda può anche essere riformulata per includere più di due opzioni: È A o B o C o D, ecc.? Si tratta in questo caso della classificazione multiclasse ed è utile quando sono possibili più risposte o diverse migliaia di risposte. La classificazione multiclasse sceglie la più probabile.

## Domanda 2: È strano? usa gli algoritmi di rilevamento delle anomalie

La domanda successiva a cui l'analisi scientifica dei dati può rispondere è: È strano? A questa domanda risponde una famiglia di algoritmi detta rilevamento delle anomalie.

![Algoritmi di rilevamento delle anomalie: È strano?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Se si dispone di una carta di credito, si conoscono già i vantaggi del rilevamento delle anomalie. La società della carta di credito analizza i modelli di acquisto, in modo tale da avvisare gli utenti di possibili episodi di frodi. Addebiti "strani" potrebbero essere acquisti presso un negozio in cui non si effettuano compere abitualmente o acquisti di articoli eccezionalmente costosi.

Questa domanda può essere utile in molti modi. Ad esempio:

  *	Se si dispone di un'auto con manometri, ci si potrebbe chiedere: Questo manometro funziona normalmente?
  *	Se si tiene sotto controllo Internet ci si potrebbe chiedere: Questo messaggio è tipico di Internet?

Il rilevamento delle anomalie segnala eventi o comportamenti imprevisti o insoliti. Fornisce degli indizi su dove cercare per individuare i problemi.



## Domanda 3: In che quantità? o In che numero? usa algoritmi di regressione

L'apprendimento automatico può anche prevedere la risposta alla domanda In che quantità? o In che numero? La famiglia di algoritmi che risponde a questa domanda è detta regressione.

![Algoritmi di regressione: In che quantità? o In che numero?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Gli algoritmi di regressione effettuano previsioni numeriche, ad esempio:

  *	Quale sarà la temperatura martedì prossimo?
  *	Come andrà il quarto trimestre di vendite?

Aiutano a rispondere a qualsiasi domanda che richiede informazioni numeriche.

## Domanda 4: In che modo sono organizzati i dati? usa gli algoritmi di clustering

Le ultime due domande sono invece un po' più complesse.

Talvolta si desidera capire la struttura di un set di dati: In che modo sono organizzati i dati? Per questa domanda, non ci sono esempi per cui si conoscono già i risultati.

Esistono diversi modi per comprendere la struttura dei dati. Un approccio è il clustering. Separa i dati in "gruppi" naturali per un'interpretazione più semplice. Con il clustering non vi è alcuna risposta corretta.

![Algoritmi di clustering: In che modo sono organizzati i dati?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Esempi comuni di domande di clustering sono:

  *	A quali spettatori piacciono gli stessi tipi di film?
  *	Quali modelli di stampanti smettono di funzionare nello stesso modo?

Riuscendo a capire come sono organizzati i dati, è possibile comprendere meglio, e prevedere, comportamenti ed eventi.

## Domanda 5: Qual è il prossimo passo da fare? usa gli algoritmi di apprendimento per rinforzo

L'ultima domanda: Qual è il prossimo passo da fare? usa una famiglia di algoritmi detta apprendimento per rinforzo.

L'apprendimento per rinforzo è stato ispirato dal modo in cui i cervelli dei topi e degli esseri umani rispondono alle punizioni e alle ricompense. Questi algoritmi apprendono dai risultati e stabiliscono l'azione successiva.

In genere, l'apprendimento per rinforzo è una buona scelta per i sistemi automatizzati che devono prendere tante piccole decisioni senza la guida umana.

![Algoritmi di apprendimento per rinforzo: Qual è il prossimo passo da fare?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Le domande a cui risponde riguardano sempre l'azione da intraprendere, di solito da una macchina o un robot. Alcuni esempi:

  *	Un sistema di controllo della temperatura per una casa: regolare la temperatura o lasciarla invariata?
  *	Automobile senza pilota: In caso di luce gialla, frenare o accelerare?
  *	Per un robot aspirapolvere: Continuare ad aspirare la polvere o tornare alla stazione di ricarica?

Gli algoritmi di apprendimento per rinforzo raccolgono i dati durante i processi, imparando dai tentativi e dagli errori.

E questo è tutto, le 5 domande a cui l'analisi scientifica dei dati può rispondere.



## Altri video della serie

*Analisi scientifica dei dati per principianti* è una rapida introduzione all'analisi scientifica dei dati in cinque brevi video. Sono disponibili altri quattro video:

  * Video 2: [Sono pronti i dati per l'analisi scientifica?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) **Disponibile ora.**
  * Video 3: [Porre una domanda a cui è possibile rispondere con i dati](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md). **Disponibile ora.**
  * Video 4: [Prevedere una risposta con un modello semplice](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md). **Disponibile ora.**
  * Video 5: [Copiare il lavoro di altre persone per l'analisi scientifica dei dati](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md). Disponibile dal 30 giugno.

## Passaggi successivi

  * [È possibile effettuare il primo esperimento di analisi scientifica dei dati con Azure Machine Learning](machine-learning-create-experiment.md)
  * [È possibile ottenere un'introduzione a Machine Learning in Microsoft Azure](machine-learning-what-is-machine-learning.md)

<!---HONumber=AcomDC_0629_2016-->