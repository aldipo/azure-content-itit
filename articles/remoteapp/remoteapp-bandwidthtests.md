<properties 
    pageTitle="Azure RemoteApp - Scenari comuni di test relativi all'utilizzo della larghezza di banda di rete | Microsoft Azure"
	description="Informazioni sugli scenari di utilizzo comune che consentono di determinare le esigenze di larghezza di banda di rete di Azure RemoteApp."
	services="remoteapp"
	documentationCenter="" 
	authors="lizap" 
	manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/27/2016" 
    ms.author="elizapo" />
    
# Azure RemoteApp - Scenari comuni di test relativi all'utilizzo della larghezza di banda di rete

Come accennato nell'articolo [Uso previsto della larghezza di banda di rete di Azure RemoteApp](remoteapp-bandwidth.md), il modo migliore per capire in che modo Azure RemoteApp influisce sulla rete consiste nell'eseguire alcuni test di utilizzo. Eseguire questi test per un periodo di tempo specifico e misurare la larghezza di banda necessaria per ogni scenario. È anche possibile misurare la perdita di pacchetti e l'instabilità di rete per comprendere i modelli di rete che verranno creati nell'ambiente specifico.

    
Quando si valuta l'uso della larghezza di banda, tenere presente che l'utilizzo varia tra i diversi utenti aziendali. Ad esempio, gli utenti che leggono o scrivono contenuti testuali in genere usano meno larghezza di banda rispetto agli utenti che lavorano con i video. Per risultati ottimali, è opportuno analizzare le esigenze specifiche dell'utente e creare una combinazione degli scenari seguenti che rappresenti al meglio gli utenti della propria azienda. Ricordarsi di [riesaminare i fattori che influiscono sull'uso della larghezza di banda e sull'esperienza utente](remoteapp-bandwidthexperience.md) per individuare facilmente i test ideali.

Leggere prima di tutto le informazioni sui test, scegliere la combinazione e quindi eseguirli. È possibile tenere traccia delle prestazioni tramite la tabella seguente.

>[AZURE.NOTE] Se non si ha la possibilità o il tempo per eseguire i test di rete, consultare le [stime/raccomandazioni di base sulla larghezza di banda di rete](remoteapp-bandwidthguidelines.md). Dal momento che il chilometraggio può variare, nei limiti del possibile è *consigliabile* eseguire test propri.


## Test di utilizzo
Ogni test ha una durata diversa e verifica funzioni/funzionalità differenti che utilizzano la larghezza di banda di rete. Ricordarsi di scegliere la combinazione di test che meglio rappresenta i singoli utenti aziendali.
 
### PowerPoint complesso/esecutivo: esecuzione per 900-1000 secondi

Un utente presenta 45-50 diapositive ad alta fedeltà usando Microsoft Office PowerPoint in modalità schermo intero. Le diapositive devono contenere immagini, transizioni (con animazioni) e sfondi con sfumature di colore tipiche dell'azienda. L'utente deve dedicare almeno 20 secondi a ogni diapositiva.
    
Questo scenario genera picchi di traffico quando si passa da una diapositiva all'altra durante la presentazione.
    
### PowerPoint semplice: esecuzione per ~620 secondi

Un utente presenta un file di PowerPoint semplice con circa 30 diapositive usando Microsoft Office PowerPoint in modalità schermo intero. Le diapositive sono più ricche di testo rispetto allo scenario del PowerPoint complesso/esecutivo e presentano sfondi e immagini più semplici (diagrammi neri).
    
### Internet Explorer: esecuzione per ~250 secondi

Un utente naviga sul Web con Internet Explorer. L'utente naviga e scorre attraverso una combinazione di testo, immagini naturali e alcuni diagrammi schematici. Le pagine Web vengono archiviate nell'unità disco locale del server Host sessione Desktop remoto (Host sessione RD) come un file con estensione MHT. L'utente scorre con PGSU, PGGIÙ, freccia SU e freccia GIÙ, con intervalli variabili per ogni tasto/tipo di scorrimento:
    
    - Down - 250 keystrokes very 500 ms
    - Page Up - 36 keystrokes every 1000 ms
    - Down - 75 keystrokes every 100 ms
    - Page Down - 20 keystrokes every 500 ms
    - Up - 120 keystrokes every 300 ms
    
### Documento PDF semplice: esecuzione per ~610 secondi
Un utente legge ed esegue ricerche in un documento PDF in vari modi con Adobe Acrobat Reader. Il documento contiene tabelle, grafici semplici e più tipi di caratteri testo. Il documento è composto da 35-40 pagine. L'utente scorre a due velocità diverse, indietro e in avanti, con quattro dimensioni dello zoom diverse (Adatta alla pagina, Adatta in larghezza, 100% e un'altra scelta). Lo zoom assicura il rendering del testo (tipo di carattere) in varie dimensioni. L'utente scorre con PGSU, PGGIÙ, freccia SU e freccia GIÙ, con intervalli variabili per ogni scorrimento.

### Documento PDF misto: esecuzione per ~320 secondi
Un utente legge ed esegue ricerche in un documento PDF in vari modi con Adobe Acrobat Reader. Il documento è costituito da immagini ad alta qualità (incluse fotografie), tabelle, grafici semplici e più tipi di caratteri di testo. L'utente scorre a due velocità diverse, indietro e in avanti, con quattro dimensioni dello zoom diverse (Adatta alla pagina, Adatta in larghezza, 100% e un'altra scelta). Lo zoom assicura il rendering del testo (tipo di carattere) in varie dimensioni. L'utente scorre con PGSU, PGGIÙ, freccia SU e freccia GIÙ, con intervalli variabili per ogni scorrimento.

### Riproduzione di video Flash - esecuzione per ~180 secondi
Un utente visualizza un video codificato in Adobe Flash incorporato in una pagina Web. La pagina Web viene memorizzata nel disco rigido locale del server Host sessione Desktop remoto. Il video viene riprodotto in Internet Explorer da un plug-in del lettore incorporato.

Questo scenario emula la visualizzazione di pagine Web inclusive di contenuti formattati multimediali da parte degli utenti. La maggior parte dei dati devono essere elaborati in VORB.

### Digitazione remota in Word: esecuzione per ~1800 secondi
Un utente digita un documento in una sessione RDP. Le pressioni dei tasti vengono inviate dal lato client tramite la sessione RDP a un documento in Microsoft Word in esecuzione nella sessione remota. La velocità di digitazione è pari a un carattere ogni 250 ms (7050 caratteri totali).

Questo è uno degli scenari più comuni per un knowledge worker. Permette di verificare la velocità di risposta di un utente durante la digitazione in un moderno elaboratore. Questo scenario è sensibile anche a piccole variazioni nell'uso della larghezza di banda.

## Monitoraggio dei risultati di test

La tabella seguente consente di valutare gli scenari nel proprio ambiente. I dati riportati di seguito sono forniti esclusivamente a scopo illustrativo e potrebbero variare largamente da ciò che si osserva.

Per semplicità, si ipotizza che tutti gli scenari vengano testati utilizzando la stessa risoluzione dello schermo di 1920 x 1080 pixel e trasporti TCP su una rete con latenza (ritardo) inferiore a 200 ms e jitter di rete di circa l'1% in intervalli superiori ai 120 ms.

Informazioni sulla tabella:
- La colonna **Utilizzo medio** indica la larghezza di banda di rete che non influenza significativamente la produttività dell'utente ma non esclude la possibilità di glitch video o audio occasionali. Il sistema è in grado di eseguire il ripristino rapidamente sfruttando la logica dinamica. Le stime relative alla larghezza di banda di rete tentano di garantire la qualità dell'esperienza utente.
 - La colonna **Problemi evidenti (punto di interruzione)** contiene la larghezza di banda di rete in cui gli utenti potrebbero notare importanti problemi nella loro esperienza con conseguente impatto sulla produttività per periodi di tempo misurabili. A questo punto gli algoritmi RDP possono essere problematici e non riuscire a garantire la qualità dell'esperienza utente per via di una larghezza di banda di rete insufficiente.
 - La colonna **Consigliata** contiene la larghezza di banda di rete consigliata per un'esperienza buona o eccellente. È in genere superiore al valore della colonna corrispondente **Utilizzo medio**.
 - La colonna **Note** include osservazioni e commenti.
 
| Test | Utilizzo medio | Problemi evidenti (punto di interruzione) | Larghezza di banda di rete raccomandata | Note |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| PPT complesso/esecutivo | 10 MB/s | 1 MB/s | > 10 MB/s, 100 MB/s preferito | A 1 MB/s vengono perse molte animazioni |
| PPT semplice | 5 MB/s | 256 KB/s | 10 MB/s | A 256 KB/s le diapositive si caricano con ritardo notevole |
| Internet Explorer | 10 MB/s | 1 MB/s | > 10 MB/s, 100 MB/s preferito | A 1 MB/s i video Web sono sfocati e irregolari, lo scorrimento veloce presenta problemi |
| PDF semplice | 1 MB/s | 256 KB/s | 5 MB/s | A 256 KB/s la pagina necessita di alcuni istanti per caricarsi |
| PDF misto | 1 MB/s | 256 KB/s | 5 MB/s | A 256 KB/s la pagina si carica lentamente |
| Riproduzione di video Flash | 10 MB/s | 1 MB/s | > 10 MB/s, 100 MB/s preferito | A 1 MB/s il video è granuloso e alcuni fotogrammi vengono rimossi |
| Digitazione remota in Word | 256 KB/s | 128 KB/s | 1 MB/s | A 256 KB/s l'utente potrebbe notare un intervallo tra la pressione dei tasti |

Per valutare la larghezza di banda di rete per utente, creare una combinazione degli scenari precedenti e la proporzione corrispondente di larghezza di banda di rete necessaria. Selezionare il valore più alto necessario per i propri scenari. Dal momento che gli utenti non usano quasi mai il sistema da soli, prendere in considerazione alcune riserve per gli utenti che lavorano contemporaneamente sulla stessa rete.
     
## Altre informazioni
- [Uso previsto della larghezza di banda di rete di Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - In che modo la larghezza di banda di rete influisce sulla qualità dell'esperienza?](remoteapp-bandwidthexperience.md)

- [Larghezza di banda di rete di Azure RemoteApp - Linee guida generali (se non è possibile verificare direttamente)](remoteapp-bandwidthguidelines.md)

<!---HONumber=AcomDC_0629_2016-->