<properties 
   pageTitle="Configurare MPIO per il dispositivo StorSimple | Microsoft Azure"
   description="Descrive come configurare Multipath I/O per il dispositivo StorSimple connesso a un host che esegue Windows Server 2012 R2"
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/27/2016"
   ms.author="alkohli" />

# Configurare Multipath I/O per il dispositivo StorSimple

Microsoft ha creato il supporto per funzionalità Multipath I/O (MPIO) in Windows Server per consentire la creazione di configurazioni SAN a elevata disponibilità e con tolleranza d’errore. MPIO utilizza componenti di percorso fisico ridondanti, adattatori, cavi e interruttori, per creare percorsi logici tra il server e il dispositivo di archiviazione. Se si verifica un errore del componente, che causa l’errore di un percorso logico, la logica dei percorsi multipli utilizza un percorso alternativo per l’I/O, in modo che le applicazioni possano continuare ad accedere ai dati. Inoltre, in base alla configurazione, MPIO può anche migliorare le prestazioni bilanciando nuovamente il carico tra tutti questi percorsi. Per ulteriori informazioni, vedere [Panoramica di Multipath I/O](https://technet.microsoft.com/library/cc725907.aspx "Panoramica e funzionalità di MPIO").

Per la disponibilità elevata della soluzione StorSimple, è necessario configurare MPIO nel dispositivo StorSimple. Quando MPIO è installato nei server host che eseguono Windows Server 2012 R2, i server possono dunque tollerare un errore di collegamento, di rete o di interfaccia.

MPIO è una funzionalità facoltativa in Windows Server, e non è installata per impostazione predefinita. Deve essere installata come funzionalità tramite Server Manager. In questo argomento vengono descritti i passaggi da seguire per installare e utilizzare la funzionalità MPIO in un host che esegue Windows Server 2012 R2 e connesso a un dispositivo fisico StorSimple.

>[AZURE.NOTE] **Questa procedura è valida solo per StorSimple serie 8000. La funzionalità MPIO non è attualmente supportata in un dispositivo virtuale StorSimple.**

È necessario attenersi alla seguente procedura per configurare MPIO nel dispositivo StorSimple:

- Passaggio 1: Installare MPIO nell'host Windows Server

- Passaggio 2: Configurare MPIO per volumi StorSimple

- Passaggio 3: Montare i volumi StorSimple nell'host

- Passaggio 4: Configurare MPIO per la disponibilità elevata e il bilanciamento del carico

Ognuno dei passaggi precedenti viene illustrato nelle sezioni seguenti.

## Passaggio 1: Installare MPIO nell'host Windows Server

Per installare questa funzionalità nell'host Windows Server, completare la procedura seguente.

#### Per installare MPIO nell'host

1. Aprire Server Manager nell'host Windows Server. Per impostazione predefinita, quando un membro del gruppo Administrators accede a un computer che esegue Windows Server 2012 R2 o Windows Server 2012 viene avviato Server Manager. Se Server Manager non è già aperto, fare clic su **Start > Server Manager**. ![Server Manager](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Fare clic su **Server Manager > Dashboard > Aggiungi ruoli e funzionalità**. Verrà avviata la procedura guidata **Aggiungi ruoli e funzionalità**. ![Aggiunta guidata ruoli e funzionalità 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. Nella procedura guidata **Aggiungi ruoli e funzionalità**, effettuare le seguenti operazioni:

	- Nella pagina **Prima di iniziare**, fare clic su **Avanti**.
	- Nella pagina **Seleziona tipo di installazione**, accettare l’impostazione predefinita di **Installazione basata su ruoli o basata su funzionalità**. Fare clic su **Avanti**.![Aggiunta guidata ruoli e funzionalità 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
	- Nella pagina **Selezione server di destinazione**, scegliere **Selezionare un server dal pool di server**. Il server host deve essere rilevato automaticamente. Fare clic su **Avanti**.
	- Nella pagina **Selezione ruoli server**, fare clic su **Avanti**.
	- Nella pagina **Selezione funzionalità**, selezionare **Multipath I/O** e fare clic su **Avanti**.![Aggiunta guidata ruoli e funzionalità 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
	- Nella pagina **Conferma selezioni per l’installazione**, confermare la selezione, quindi selezionare **Riavvia automaticamente il server di destinazione se necessario**, come mostrato di seguito. Fare clic su **Installa**.![Aggiunta guidata ruoli e funzionalità 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
	- Al termine dell'installazione si riceverà una notifica. Fare clic su **Chiudi** per chiudere la procedura guidata.![Aggiunta guidata ruoli e funzionalità 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## Passaggio 2: Configurare MPIO per volumi StorSimple

Per identificare i volumi StorSimple, è necessario configurare MPIO. Per configurare MPIO in modo che riconosca i volumi StorSimple, effettuare le seguenti operazioni.

#### Per configurare MPIO per i volumi StorSimple

1. Aprire la **configurazione MPIO**. Fare clic su **Server Manager > Dashboard > Strumenti > MPIO**.

2. Nella finestra di dialogo **Proprietà MPIO**, selezionare la scheda **Individua percorsi multipli**.

3. Selezionare **Aggiungi supporto per dispositivi iSCSI**, quindi fare clic su **Aggiungi**. ![Proprietà MPIO Individua percorsi multipli](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Riavviare il server quando richiesto.
5. Nella finestra di dialogo **Proprietà MPIO**, fare clic sulla scheda **Dispositivi MPIO**. Fare clic su **Aggiungi**. </br>![Proprietà MPIO Dispositivi MPIO](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Nella finestra di dialogo **Aggiungi supporto MPIO**, in **ID hardware dispositivo**, immettere il numero di serie del dispositivo. Per ottenere il numero di serie del dispositivo, accedere al servizio StorSimple Manager e navigare fino a **Dispositivi > Dashboard**. Il numero di serie del dispositivo è visualizzato nel riquadro destro **Riepilogo rapido** del dashboard dispositivo. </br>![Aggiungi supporto MPIO](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Riavviare il server quando richiesto.

## Passaggio 3: Montare i volumi StorSimple nell'host

Dopo la configurazione di MPIO in Windows Server, i volumi creati nel dispositivo StorSimple possono essere montati ed è quindi possibile sfruttare MPIO per garantire la ridondanza. Per montare un volume, attenersi alla seguente procedura.

#### Per montare i volumi nell'host

1. Aprire la finestra **Proprietà iniziatore iSCSI** sull'host Windows Server. Fare clic su **Server Manager > Dashboard > Strumenti > Iniziatore iSCSI**.
2. Nella finestra di dialogo **Proprietà iniziatore iSCSI** fare clic sulla scheda Individuazione, quindi fare clic su **Individua portale destinazione**.
3. Nella finestra di dialogo **Individua portale destinazione**, effettuare le operazioni seguenti:
	
	- Immettere l'indirizzo IP della porta DATA del dispositivo StorSimple (ad esempio, immettere DATA 0).
	- Fare clic su **OK** per tornare alla finestra di dialogo **Proprietà iniziatore iSCSI**.

	>[AZURE.IMPORTANT] **Se si utilizza una rete privata per le connessioni iSCSI, immettere l'indirizzo IP della porta DATA connessa alla rete privata.**

4. Ripetere i passaggi 2-3 per una seconda interfaccia di rete (ad esempio, DATA 1) sul dispositivo. Tenere presente che queste interfacce devono essere abilitate per iSCSI. Per ulteriori informazioni, andare a [Modificare le interfacce di rete](storsimple-modify-device-config.md#modify-network-interfaces).
5. Selezionare la scheda **Destinazioni** nella finestra di dialogo **Proprietà iniziatore iSCSI**. Dovrebbe essere visualizzato l'IQN destinazione del dispositivo StorSimple in **Destinazioni individuate**. ![Scheda destinazioni proprietà iniziatore iSCSI](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Fare clic su **Connetti** per stabilire una sessione iSCSI con il dispositivo StorSimple. Verrà visualizzata una finestra di dialogo **Connessione alla destinazione**.

7. Nella finestra di dialogo **Connessione alla destinazione**, selezionare la casella di controllo **Consenti percorsi multipli**. Fare clic su **Advanced**.

8. Nella finestra di dialogo **Impostazioni avanzate**, effettuare le seguenti operazioni:
	- 	 Nell’elenco a discesa **Scheda locale**, selezionare **Iniziatore iSCSI Microsoft**.
	- 	 Nell'elenco a discesa **IP iniziatore**, selezionare l’indirizzo IP dell'host.
	- 	 Nell’elenco a discesa **IP portale di destinazione**, selezionare l’IP dell’interfaccia dispositivo.
	- 	 Fare clic su **OK** per tornare alla finestra di dialogo **Proprietà iniziatore iSCSI**.

9. Fare clic su **Proprietà**. Nella finestra di dialogo **Proprietà**, fare clic su **Aggiungi sessione**.
10. Nella finestra di dialogo **Connessione alla destinazione**, selezionare la casella di controllo **Consenti percorsi multipli**. Fare clic su **Advanced**.
11. Nella finestra di dialogo **Impostazioni avanzate**:										
	-  Nell’elenco a discesa **Scheda locale**, selezionare Iniziatore iSCSI Microsoft.
	-  Nell’elenco a discesa **IP iniziatore**, selezionare l’indirizzo IP corrispondente all'host. In questo caso, si sta eseguendo la connessione di due interfacce di rete sul dispositivo a una singola interfaccia di rete sull'host. Pertanto, questa interfaccia è identica a quella fornita per la prima sessione.
	-  Nell’elenco a discesa **IP portale di destinazione**, selezionare l’indirizzo IP per la seconda interfaccia di dati abilitata sul dispositivo.
	-  Fare clic su **OK** per tornare alla finestra di dialogo Proprietà iniziatore iSCSI. È stata aggiunta una seconda sessione alla destinazione.

12. Aprire **Gestione computer** passando a **Server Manager > Dashboard > Gestione computer**. Nel riquadro sinistro, fare clic su **Archiviazione > Gestione disco**. I volumi creati nel dispositivo StorSimple che sono visibili a questo host verranno visualizzati in **Gestione disco** come nuovi dischi.

13. Inizializzare il disco e creare un nuovo volume. Durante il processo di formattazione, selezionare una dimensione blocco di 64 KB. ![Gestione disco](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. In **Gestione disco**, fare clic con il pulsante destro del mouse su **Disco** e selezionare **Proprietà**.
15. Nella finestra di dialogo **Multi-Path Disk Device Properties** di StorSimple Model ####, fare clic sulla scheda **MPIO**. ![StorSimple 8100 Multi-Path Disk DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. Nella sezione **Nome DSM**, fare clic su **Dettagli** e verificare che i parametri impostati siano quelli predefiniti. I parametri predefiniti sono:

	- Periodo di verifica percorso = 30
	- Numero di tentativi = 3
	- Periodo di rimozione PDO = 20
	- Intervallo tentativi = 1
	- Verifica percorso attivata = deselezionata.


>[AZURE.NOTE] **Non modificare i parametri predefiniti.**

## Passaggio 4: Configurare MPIO per la disponibilità elevata e il bilanciamento del carico

Per la disponibilità elevata e il bilanciamento del carico basato su percorsi multipli, è necessario aggiungere manualmente più sessioni per dichiarare i diversi percorsi disponibili. Ad esempio, se l'host dispone di due interfacce connesse alla rete SAN e il dispositivo dispone di due interfacce connesse alla rete SAN, è necessario configurare quattro sessioni con permutazioni percorso corrette (se ciascuna interfaccia DATA e ciascuna interfaccia host si trova in una subnet IP diversa e non è instradabile saranno necessarie solo due sessioni).

>[AZURE.IMPORTANT] **È consigliabile non combinare interfacce di rete da 1 GbE e da 10 GbE. Se si utilizzano due interfacce di rete, devono essere di tipo identico.**

Nella procedura seguente viene descritto come aggiungere le sessioni quando un dispositivo StorSimple con due interfacce di rete viene connesso a un host con due interfacce di rete.

### Per configurare MPIO per la disponibilità elevata e il bilanciamento del carico

1. Eseguire un rilevamento della destinazione: nella finestra di dialogo **Proprietà iniziatore iSCSI**, nella scheda **Individuazione**, fare clic su **Individua portale**.
2. Nella finestra di dialogo **Connessione alla destinazione**, immettere l’indirizzo IP di una delle interfacce di rete del dispositivo.
3. Fare clic su **OK** per tornare alla finestra di dialogo **Proprietà iniziatore iSCSI**.

4. Nella finestra di dialogo **Proprietà iniziatore iSCSI**, selezionare la scheda **Destinazioni**, evidenziare la destinazione scoperta, quindi fare clic su **Connetti**. Viene visualizzata la finestra di dialogo **Connessione alla destinazione**.

5. Nella finestra di dialogo **Connessione alla destinazione**:
	
	- Lasciare l’impostazione di destinazione selezionata predefinita **Aggiungi connessione all’elenco Destinazioni preferite**. In questo modo il dispositivo tenta automaticamente di riavviare la connessione ogni volta che il computer viene riavviato.
	- Selezionare la casella di controllo **Consenti percorsi multipli**.
	- Fare clic su **Advanced**.

6. Nella finestra di dialogo **Impostazioni avanzate**:
	- Nell’elenco a discesa **Scheda locale**, selezionare **Iniziatore iSCSI Microsoft**.
	- Nell'elenco a discesa **IP iniziatore**, selezionare l’indirizzo IP dell'host.
	- Nell’elenco a discesa **IP portale di destinazione**, selezionare l’indirizzo IP dell’interfaccia di dati abilitata sul dispositivo.
	- Fare clic su **OK** per tornare alla finestra di dialogo Proprietà iniziatore iSCSI.

7. Fare clic su **Proprietà** e nella finestra di dialogo **Proprietà**, fare clic su **Aggiungi sessione**.

8. Nella finestra di dialogo **Connessione alla destinazione**, selezionare la casella di controllo **Consenti percorsi multipli**, quindi fare clic su **Avanzate**.

9. Nella finestra di dialogo **Impostazioni avanzate**:
	1. Nell’elenco a discesa **Scheda locale**, selezionare **Iniziatore iSCSI Microsoft**.
	2. Nell’elenco a discesa **IP iniziatore**, selezionare l’indirizzo IP corrispondente alla seconda interfaccia dell'host.
	3. Nell’elenco a discesa **IP portale di destinazione**, selezionare l’indirizzo IP per la seconda interfaccia di dati abilitata sul dispositivo.
	4. Fare clic su **OK** per tornare alla finestra di dialogo **Proprietà iniziatore iSCSI**. È stata aggiunta ora una seconda sessione alla destinazione.

10. Ripetere i passaggi da 8 a 10 per aggiungere altre sessioni (percorsi) alla destinazione. Con due interfacce nell'host e due sul dispositivo, è possibile aggiungere un totale di quattro sessioni.

11. Dopo aver aggiunto le sessioni (percorsi) desiderate, nella finestra di dialogo **Proprietà iniziatore iSCSI**, selezionare la destinazione e fare clic su **Proprietà**. Nella scheda Sessioni della finestra di dialogo **Proprietà**, notare i quatto identificatori di sessione che corrispondono alle possibili permutazioni di percorso. Per annullare una sessione, selezionare la casella di controllo accanto a un identificatore di sessione, quindi fare clic su **Disconnetti**.

12. Per visualizzare i dispositivi presentati all'interno delle sessioni, selezionare la scheda **Dispositivi**. Per configurare il criterio MPIO per un dispositivo selezionato, fare clic su **MPIO**. Verrà visualizzata la finestra di dialogo **Dettagli dispositivo**. Nella scheda **MPIO**, è possibile selezionare le impostazioni **Criterio di bilanciamento del carico** appropriate. È inoltre possibile visualizzare il tipo di percorso **Attivo** o **Standby**.

## Passaggi successivi

Ulteriori informazioni in [Utilizzare il servizio StorSimple Manager per modificare la configurazione del dispositivo StorSimple](storsimple-modify-device-config.md).
 

<!---HONumber=AcomDC_0504_2016-->