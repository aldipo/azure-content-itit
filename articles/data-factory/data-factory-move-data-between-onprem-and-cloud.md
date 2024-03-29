<properties 
	pageTitle="Spostare dati con Gateway di gestione dati | Microsoft Azure"
	description="Configurare un gateway dati per spostare dati tra origini locali e il cloud. Usare Gateway di gestione dati in Azure Data Factory per spostare dati." 
    keywords="gateway dati, integrazione di dati, spostare dati, credenziali del gateway"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/28/2016" 
	ms.author="spelluru"/>

# Spostare dati tra origini locali e il cloud con Gateway di gestione dati
Questo articolo offre una panoramica sull'integrazione tra archivi dati locali e archivi dati cloud nonché sull'elaborazione nel cloud con Data Factory. Si basa sull'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md) e su altri articoli che illustrano i concetti di base relativi a Data Factory. Si presuppone una certa familiarità con concetti relativi alle data factory, quali le pipeline, le attività, i set di dati e l'attività di copia.

## Funzionamento di Azure Data Factory con Gateway di gestione dati 
Esiste la possibilità di spostare facilmente i dati da origini locali al cloud e viceversa. Azure Data Factory è un agente che si installa in locale e offre le funzionalità di gateway di gestione dati necessarie, inclusa un'applicazione per impostare le credenziali del gateway.

## Funzionamento del gateway dati
Il Gateway di gestione dati offre le funzionalità seguenti:

1.	Consente di modellare le origini dati locali e le origini dati nel cloud all'interno di un'unica istanza di Data Factory e di spostare i dati al suo interno.
2.	Consente di monitorare e gestire lo stato del gateway in un'unica schermata attraverso un dashboard di Data factory basato sul cloud.
3.	Consente di gestire in modo sicuro l'accesso alle origini dati locali.
	1. Non è richiesta alcuna modifica del firewall aziendale. Il gateway stabilisce soltanto connessioni basate su HTTP in uscita per accedere a Internet.
	2. È possibile crittografare le informazioni sulle credenziali per gli archivi dati locali usando il proprio certificato.
4.	Consente di spostare i dati in modo efficace: i dati vengono trasferiti in parallelo e sono resilienti ai problemi di rete intermittente grazie alla logica di ripetizione dei tentativi automatica.

## Considerazioni per l'uso del Gateway di gestione dati
1.	Una singola istanza del Gateway di gestione dati può essere usata per più origini dati locali. Tenere presente, però, che **una sola istanza del gateway può essere associata a una singola istanza di Data factory di Azure** e non può essere condivisa con altre istanze di Data factory.
2.	In un computer può essere installata **una sola istanza del Gateway di gestione dati**. Si supponga di avere due istanze di Data Factory che richiedono l'accesso alle origini dati locali: è necessario installare i gateway nei due computer locali in cui ogni gateway sia associato a un'istanza separata di Data Factory.
3.	Il **gateway non deve trovarsi nello stesso computer dell'origine dati**; tuttavia, la prossimità all'origine dati riduce il tempo di connessione tra il gateway e l'origine dati. Si consiglia di installare il gateway in un computer diverso da quello che ospita l'origine dati locale in modo che il gateway non si contenda le risorse con l'origine dati.
4.	È possibile disporre di **più gateway su diversi computer che si connettono alla stessa origine dati locale**. Ad esempio, potrebbero essere disponibili due gateway che servono due data factory, ma la stessa origine dati locale viene registrata con entrambe le data factory.
5.	Se un gateway è già installato nel computer per uno scenario **Power BI**, installare un **gateway separato per Data factory di Azure** in un altro computer.
6.	È necessario **usare il gateway anche quando si usa ExpressRoute**.
7.	È necessario considerare l'origine dati come un'origine dati locale, ovvero come se fosse protetta da firewall, anche quando si usa **ExpressRoute** e **il gateway** per stabilire la connettività tra il servizio e l'origine dati.

## Installare Gateway di gestione dati

### Installazione del gateway - Prerequisiti
1.	Sono supportati i **sistemi operativi** Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 e Windows Server 2012 R2. L’installazione di Gateway di gestione dati sul controller di dominio al momento non è supportata.
2.	La **configurazione** consigliata per il computer gateway è di almeno 2 GHz, 4 core, 8 GB di RAM e un disco da 80 GB.
3.	Se il computer host entra in stato di ibernazione, il gateway non può rispondere alla richiesta di dati. Pertanto, configurare una **combinazione per il risparmio di energia** appropriata nel computer prima di installare il gateway. L'installazione del gateway invia un messaggio se il computer è configurato per l'ibernazione.

Dato che le esecuzioni delle attività di copia seguono una frequenza specifica, l'utilizzo delle risorse nel computer (CPU e memoria) segue lo stesso ciclo costituito da periodi di massimo utilizzo alternati a periodi di inattività. L'utilizzo delle risorse dipende molto anche dalla quantità di dati da spostare. Quando sono in corso più processi di copia, l'utilizzo delle risorse aumenta durante i periodi di massimo utilizzo. Dato che la configurazione illustrata sopra corrisponde alla configurazione minima, a seconda del carico previsto per lo spostamento dei dati è sempre consigliabile disporre di maggiori risorse.

### Installare
È possibile installare Gateway di gestione dati scaricando un pacchetto di installazione MSI dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=39717). Il pacchetto con estensione msi può inoltre essere usato per aggiornare il Gateway di gestione dati esistente alla versione più recente mantenendo tutte le impostazioni. Il collegamento al pacchetto MSI è disponibile sul Portale di Azure seguendo la procedura dettagliata riportata di seguito.


### Procedure consigliate per l'installazione:
1.	Configurare la combinazione per il risparmio di energia nel computer host del gateway in modo che il computer non entri in stato di ibernazione. Se il computer host entra in stato di ibernazione, il gateway non può rispondere alla richiesta di dati.
2.	È necessario eseguire il backup del certificato associato al gateway.

## Aggiornare Gateway di gestione dati
Per impostazione predefinita, Gateway di gestione dati viene aggiornato automaticamente quando è disponibile una versione più recente del gateway. Il gateway non viene aggiornato finché non vengono eseguite tutte le operazioni pianificate. Nessun'altra attività viene elaborata dal gateway fino al completamento dell'operazione di aggiornamento. Se l'aggiornamento non riesce, viene eseguito il rollback del gateway alla versione precedente.

Il tempo di aggiornamento pianificato nel portale verrà visualizzato nel pannello delle proprietà del gateway, nella home page di Gestione configurazione di Gateway di gestione dati e nel messaggio di notifica nell'area di notifica. È disponibile un'opzione per installare l'aggiornamento immediatamente o attendere che il gateway venga aggiornato automaticamente all'ora pianificata. Ad esempio, la schermata seguente mostra il messaggio di notifica in Gestione configurazione di Gateway di gestione dati con il pulsante Aggiorna su cui si fa clic per avviare immediatamente l'installazione.

![Aggiorna in Gestione configurazione di Gateway di gestione dati](./media/data-factory-move-data-between-onprem-and-cloud/gateway-auto-update-config-manager.png)

Il messaggio di notifica nell'area di notifica sarà simile al seguente:

![Messaggio nell'area di notifica](./media/data-factory-move-data-between-onprem-and-cloud/gateway-auto-update-tray-message.png)

Lo stato dell'operazione di aggiornamento, manuale o automatica, sarà visualizzato nell'area di notifica. Alla successiva apertura di Gestione configurazione di Gateway di gestione dati verrà visualizzato un messaggio sulla barra di notifica per indicare che il gateway è stato aggiornato, insieme a un collegamento [all'argomento Novità](data-factory-gateway-release-notes.md).

La scheda Aggiorna di Gestione configurazione di Gateway di gestione dati visualizza la pianificazione dell'aggiornamento, nonché la data e l'ora dell'ultima installazione o dell'ultimo aggiornamento del gateway. Se l'aggiornamento automatico è disabilitato, visualizza un messaggio a questo proposito, ma non si potrà abilitare la funzionalità nella scheda. Per abilitarla, si dovrà usare il cmdlet.
  

## Notifiche/icone nell'area di notifica
L'immagine seguente illustra alcune delle icone visualizzate nell'area di notifica.

![icone dell'area di notifica](./media/data-factory-move-data-between-onprem-and-cloud/gateway-tray-icons.png)

Se si sposta il cursore sul messaggio di notifica o sull'icona nell'area di notifica, verranno visualizzati i dettagli relativi allo stato dell'operazione del gateway o di aggiornamento in una finestra popup.

## Per abilitare o disabilitare la funzionalità di aggiornamento automatico
È possibile abilitare/disabilitare la funzionalità di aggiornamento seguendo questa procedura:

1. Avviare Windows PowerShell nel computer gateway.
2. Passare alla cartella C:\\Programmi\\Microsoft Data Management Gateway\\1.0\\PowerShellScript.
3. Eseguire il comando seguente per disattivare (disabilitare) la funzionalità di aggiornamento automatico.

		.\GatewayAutoUpdateToggle.ps1  -off

4. Per riattivarla:
	
		.\GatewayAutoUpdateToggle.ps1  -on  

## Considerazioni su porte e sicurezza
È necessario considerare due firewall, ovvero il **firewall aziendale** in esecuzione nel router centrale dell'organizzazione e **Windows Firewall**, configurato come servizio daemon nel computer locale in cui è installato il gateway.

![firewall](./media/data-factory-move-data-between-onprem-and-cloud/firewalls.png)

## Connettere il gateway ai servizi cloud
Per mantenere la connettività del gateway con Azure Data Factory e altri servizi cloud è necessario verificare che la regola in uscita per le porte **TCP** **80** e **443** sia configurata. È possibile abilitare facoltativamente le porte da **9350** a **9354** che vengono usate dal bus di servizio di Microsoft Azure per stabilire la connessione tra Azure Data Factory e il Gateway di gestione dati e possono migliorare le prestazioni della comunicazione tra di essi.

A livello di firewall aziendale è necessario configurare le porte in uscita e i domini seguenti:

| Nomi di dominio | Porte | Descrizione |
| ------ | --------- | ------------ |
| *.servicebus.windows.net | 443, 80 | Listener in Inoltro del Bus di servizio su TCP (richiede 443 per l'acquisizione del token di Controllo di accesso) | 
| *.servicebus.windows.net | 9350-9354 | Inoltro del bus di servizio su TCP facoltativo | 
| *.core.windows.net | 443 | HTTPS | 
| *.clouddatahub.net | 443 | HTTPS | 
| graph.windows.net | 443 | HTTPS |
| login.windows.net | 443 | HTTPS | 

A livello di Windows Firewall queste porte in uscita sono generalmente abilitate. In caso contrario, è possibile configurare le porte e i domini nel modo appropriato nel computer gateway.

## Impostare le credenziali del gateway
La porta in ingresso **8050** verrà usata dall'applicazione di **impostazione delle credenziali** per inoltrare le credenziali al gateway quando si configura un servizio collegato locale nel portale di Azure, come illustrato più avanti nell'articolo. Durante la configurazione del gateway, l'installazione di Gateway di gestione dati apre la porta nel computer gateway per impostazione predefinita.
 
Se si usa un firewall di terze parti, è possibile aprire manualmente la porta 8050. In caso di problemi del firewall durante la configurazione del gateway, è possibile provare a usare il comando seguente per installare il gateway senza configurare il firewall.

	msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Se si sceglie di non aprire la porta 8050 nel computer gateway, per configurare un servizio collegato locale sarà necessario usare meccanismi diversi dall'uso dell'applicazione di **impostazione delle credenziali** per configurare le credenziali dell'archivio dati. È ad esempio possibile usare il cmdlet di PowerShell [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx). Per informazioni su come impostare le credenziali dell'archivio dati, vedere la sezione [Impostare le credenziali e la sicurezza](#set-credentials-and-securityy).

**Per copiare dati da un archivio dati di origine a un archivio dati sink:**

È necessario assicurarsi che le regole del firewall siano abilitate correttamente sul firewall aziendale, su Windows Firewall nel computer del gateway e sull'archivio dati stesso, in modo da consentire al gateway di connettersi all'origine e al sink. È necessario abilitare le regole per ogni archivio dati interessato dall'operazione di copia.

Per copiare ad esempio da **un archivio dati locale a un sink del database SQL di Azure o un sink di Azure SQL Data Warehouse** è necessario consentire la comunicazione **TCP** in uscita sulla porta **1433** per Windows Firewall e il firewall aziendale. È anche necessario configurare le impostazioni per il firewall del server di Azure SQL per aggiungere l'indirizzo IP del computer gateway all'elenco di indirizzi IP consentiti.

### Considerazioni sui server proxy
Per impostazione predefinita, Gateway di gestione dati userà le impostazioni del proxy da Internet Explorer e userà le credenziali predefinite per accedervi. Se questo non soddisfa le proprie esigenze, è possibile configurare ulteriormente le **impostazioni del server proxy**, come illustrato più avanti, per assicurarsi che il gateway possa connettersi ad Azure Data Factory:

1.	Dopo l'installazione del servizio Gateway di gestione dati, in Esplora file creare una copia sicura di "C:\\Program Files\\Microsoft Data Management Gateway\\1.0\\Shared\\diahost.exe.config" per eseguire il backup del file originale.
2.	Avviare Notepad.exe come amministratore e aprire il file di testo "C:\\Program Files\\Microsoft Data Management Gateway\\1.0\\Shared\\diahost.exe.config". Il tag predefinito per system.net è il seguente:

			<system.net>
				<defaultProxy useDefaultCredentials="true" />
			</system.net>	

	È quindi possibile aggiungere i dettagli relativi al server proxy, ad esempio l'indirizzo del proxy, all'interno di tale tag padre, ad esempio:

			<system.net>
			      <defaultProxy enabled="true">
			            <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
			      </defaultProxy>
			</system.net>

	È possibile aggiungere altre proprietà all'interno del tag del proxy per specificare le impostazioni obbligatorie, ad esempio scriptLocation. Per informazioni sulla sintassi, vedere [Elemento <proxy> (Impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx).

			<proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>

3. Salvare il file di configurazione nel percorso originale, quindi riavviare il servizio Gateway di gestione dati per rilevare le modifiche. È possibile eseguire questa operazione da **Start** > **Services.msc** oppure in **Gestione configurazione di Gateway di gestione dati** > fare clic sul pulsante **Arresta servizio** e quindi su **Avvia servizio**. Se il servizio non viene avviato, è probabile che una sintassi non corretta del tag XML sia stata aggiunta al file di configurazione dell'applicazione modificato.

Oltre ai punti precedenti, è necessario assicurarsi anche Microsoft Azure sia stato aggiunto all'elenco elementi consentiti aziendale. È possibile scaricare l'elenco di indirizzi IP validi per Microsoft Azure dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=41653).

### Possibili sintomi di problemi correlati al firewall e al server proxy:
Se si verificano errori analoghi ai seguenti, è possibile che siano dovuti a una configurazione non corretta del firewall o del server proxy, che impedisce al servizio Gateway di gestione dati di connettersi ad Azure Data Factory per l'autenticazione. Per assicurarsi che la configurazione del firewall e del server proxy sia corretta, vedere la sezione precedente.

1.	Quando si tenta di registrare il gateway, viene visualizzato l'errore seguente: "Impossibile registrare la chiave del gateway. Prima di tentare di registrare nuovamente la chiave del gateway, verificare che il Gateway di gestione dati sia in stato di connessione e il relativo servizio host sia stato avviato".
2.	Quando si apre Gestione configurazione, lo stato del gateway visualizzato può essere "Disconnesso" o "Connessione". Quando si visualizzano i registri eventi di Windows, in "Visualizzatore eventi" > "Registri applicazioni e servizi" > "Gateway di gestione dati" vengono visualizzati messaggi di errore, ad esempio "Impossibile connettersi al server remoto" oppure "Un componente del Gateway di gestione dati non risponde e verrà riavviato automaticamente. Nome del componente: Gateway".

## Risoluzione dei problemi di gateway


- Per informazioni dettagliate, vedere i log del gateway nei registri eventi di Windows. a cui è possibile accedere tramite il **Visualizzatore eventi** di Windows in **Registri applicazioni e servizi** > **Gateway di gestione dati**. Durante la risoluzione dei problemi correlati al gateway è possibile cercare eventi a livello di errore nel Visualizzatore eventi.
- Se il gateway smette di funzionare dopo la **modifica del certificato**, riavviare (arrestare e avviare) il **servizio Gateway di gestione dati** con lo strumento Gestione configurazione di Gateway di gestione dati di Microsoft o l'applet del pannello di controllo Servizi. Se viene ancora visualizzato un errore, è necessario assegnare le autorizzazioni esplicite per l'utente del servizio Gateway di gestione dati ad accedere al certificato nella console di gestione dei certificati (Certmgr.msc). L'account utente predefinito per il servizio è: **NT Service\\DIAHostService**.
- Se l'applicazione **Gestione credenziali** non riesce a **crittografare** le credenziali quando si fa clic sul pulsante Crittografa nell'editor di Data Factory, verificare che l'applicazione sia in esecuzione nel computer in cui è installato il gateway. In caso contrario, eseguire l'applicazione nel computer gateway e provare a crittografare le credenziali.
- Se vengono visualizzati errori di connessione all'archivio dati o relativi al driver, avviare **Gestione configurazione di Gateway di gestione dati** nel computer gateway, passare alla scheda **Diagnostica**, selezionare o immettere i valori appropriati per i campi nel gruppo **Esegui il test della connessione a un'origine dati locale con questo gateway** e fare clic su **Test connessione** per verificare se è possibile connettersi all'origine dati locale dal computer gateway usando le informazioni e le credenziali di connessione. Se il test della connessione non riesce dopo aver installato un driver, riavviare il gateway in modo che rilevi la modifica più recente.

	![Test di connessione](./media/data-factory-move-data-between-onprem-and-cloud/TestConnection.png)
		
## Procedura dettagliata: Uso del Gateway di gestione dati 
In questa procedura dettagliata viene creata un'istanza di Data Factory con una pipeline che sposta i dati da un database di SQL Server locale a un BLOB di Azure.

### Passaggio 1: creare un'istanza di Azure Data Factory
In questo passaggio si usa il portale di Azure per creare un'istanza di Azure Data Factory denominata **ADFTutorialOnPremDF**. È anche possibile creare un'istanza di Data Factory con i cmdlet di Azure Data Factory.

1.	Dopo l'accesso al [portale di Azure](https://portal.azure.com), fare clic su **NUOVO** nell'angolo inferiore sinistro, selezionare **Analisi dei dati** nel pannello **Crea** e fare clic su **Data factory** nel pannello **Analisi dei dati**.

	![Nuovo->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)
  
6. Nel pannello **Nuova data factory**:
	1. Immettere **ADFTutorialOnPremDF** nel campo **Nome**.
	2. Fare clic su **NOME GRUPPO DI RISORSE** e selezionare **ADFTutorialResourceGroup**. È possibile selezionare un gruppo di risorse esistente o crearne uno nuovo. Per creare un nuovo gruppo di risorse:
		1. Fare clic su **Crea un nuovo gruppo di risorse**.
		2. Nel pannello **Crea gruppo di risorse**, immettere un **nome** per il gruppo di risorse, quindi fare clic su **OK**.

7. **Aggiungi alla Schermata iniziale** è selezionato nel pannello **Nuova data factory**.

	![Aggiungi a schermata iniziale](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

8. Nel pannello **Nuova data factory** fare clic su **Crea**.

	È necessario specificare un nome univoco globale per l'istanza di Azure Data Factory. Se viene visualizzato un errore simile a **Nome "ADFTutorialOnPremDF" per la data factory non disponibile**, cambiare il nome della data factory (ad esempio, nomeutenteADFTutorialOnPremDF) e provare di nuovo a crearla. Durante l'esecuzione dei passaggi rimanenti in questa esercitazione usare questo nome anziché ADFTutorialOnPremDF.

9. Cercare le notifiche generate dal processo di creazione facendo clic sul pulsante **Notifiche** nella barra del titolo, come illustrato nella seguente figura. Fare di nuovo clic sul pulsante per chiudere la finestra delle notifiche.

	![Hub NOTIFICHE](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNotificationsHub.png)

11. Al termine della procedura di creazione, viene visualizzato il pannello di **Data factory** come mostrato di seguito:

	![Home page di Data Factory](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

### Passaggio 2: Creare un Gateway di gestione dati
5. Nel pannello **DATA FACTORY** fare clic sul riquadro **Creare e distribuire** per avviare l'**Editor** per la data factory.

	![Riquadro Creare e distribuire](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
6.	Nell'editor di Data Factory fare clic su **... (puntini di sospensione)** nella barra degli strumenti e quindi su **Nuovo gateway dati**.

	![Nuovo gateway di dati nella barra degli strumenti](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. Nel pannello **Crea**, immettere **adftutorialgateway** per il **nome** e fare clic su **OK**.

	![Pannello Crea gateway](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

3. Nel pannello **Configura** fare clic su **Installa direttamente nel computer**. Viene scaricato il pacchetto di installazione per il gateway, che viene installato, configurato e registrato nel computer.

	> [AZURE.NOTE] 
	Usare Internet Explorer o un Web browser compatibile con Microsoft ClickOnce.
	> 
	> Se si usa Chrome, accedere al [Chrome Web Store](https://chrome.google.com/webstore/), eseguire una ricerca con la parola chiave "ClickOnce", scegliere una delle estensioni ClickOnce e installarla.
	>  
	> È necessario eseguire la stessa operazione per Firefox (installazione del componente aggiuntivo). Fare clic sul pulsante **Apri menu** sulla barra degli strumenti, ovvero le **tre linee orizzontali** nell'angolo superiore destro. Fare clic su **Componenti aggiuntivi**, eseguire una ricerca con la parola chiave "ClickOnce", scegliere una delle estensioni ClickOnce e installarla.

	![Gateway - Pannello Configura](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

	Si tratta del metodo più semplice (con un clic) di scaricare, installare, configurare e registrare il gateway in un unico passaggio. È possibile vedere l'applicazione **Gateway di gestione dati di Microsoft Configuration Manager** installata nel computer. È anche possibile trovare l'eseguibile **ConfigManager.exe** nella cartella: **C:\\Program Files\\Microsoft Data Management Gateway\\1.0\\Shared**.

	È anche possibile scaricare e installare manualmente il gateway usando i collegamenti nel pannello e registrarlo usando il tasto visualizzato nella casella di testo **REGISTRA CON TASTO**.
	
	Vedere le sezioni all'inizio di questo articolo per dettagli sul gateway, incluse le procedure consigliate e altre considerazioni importanti.

	>[AZURE.NOTE] È necessario essere un amministratore nel computer locale per installare e configurare correttamente il gateway di gestione dati. È possibile aggiungere altri utenti al gruppo di Windows locale degli utenti del gateway di gestione dati. I membri di questo gruppo potranno usare lo strumento Gestione configurazione del gateway di gestione dati per configurare il gateway.

5. Attendere un paio di minuti e quindi avviare l'applicazione **Gestione configurazione di Gateway di gestione dati** nel computer. Nella finestra **Cerca** digitare **Gateway di gestione dati** per accedere a questa utilità. È anche possibile trovare l'eseguibile **ConfigManager.exe** nella cartella: **C:\\Program Files\\Microsoft Data Management Gateway\\1.0\\Shared**.

	![Gestione configurazione di gateway](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)

6. Attendere finché i valori non sono impostati come segue:
	1. **Stato** è impostato su **Avviato**.
	2. **Nome gateway** è impostato su **adftutorialgateway**.
	3. **Nome istanza** è impostato su **adftutorialgateway**.
	4. **Registrazione** è impostato su **Registrato**.
	5. La barra di stato visualizza **Connesso al servizio cloud del Gateway di gestione dati** insieme a un **segno di spunta verde**.

8. Passare alla scheda **Certificati**. Il certificato specificato in questa scheda viene usato per crittografare e decrittografare le credenziali per l'archivio dati locale specificato nel portale. Fare clic su **Modifica** per usare il proprio certificato. Per impostazione predefinita, il gateway usa il certificato generato automaticamente dal servizio Data Factory.

	![Configurazione certificati del gateway](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)
9. (Facoltativo) Passare alla scheda **Diagnostica**, selezionare l'opzione **Abilita registrazione dettagliata per la risoluzione dei problemi** se si vuole abilitare la registrazione dettagliata che è possibile usare per risolvere i problemi del gateway. Le informazioni sulla registrazione si trovano nel **Visualizzatore eventi**, nel nodo **Registri applicazioni e servizi** -> **Gateway di gestione dati**.

	![Scheda Diagnostica](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

	È possibile usare questa pagina anche per **testare la connessione** a un'origine dati locale usando il gateway.
10. Nel portale di Azure fare clic su **OK** nel pannello **Configura** e quindi nel pannello **Nuovo gateway dati**.
6. Verrà visualizzato **adftutorialgateway** in **Gateway dati** nella visualizzazione albero a sinistra. Se si fa clic su di esso, viene visualizzato l'oggetto JSON associato.
	

### Passaggio 3: Creare servizi collegati 
In questo passaggio verranno creati due servizi collegati: **AzureStorageLinkedService** e **SqlServerLinkedService**. Il servizio **SqlServerLinkedService** collega un database SQL Server locale, mentre il servizio collegato **AzureStorageLinkedService** collega un archivio BLOB di Azure alla data factory. Più avanti nella procedura dettagliata verrà creata una pipeline che copia i dati dal database SQL Server locale all'archivio BLOB di Azure.

#### Aggiungere un servizio collegato a un database di SQL Server locale
1.	Nell'**editor di Data Factory** fare clic su **Nuovo archivio dati** sulla barra degli strumenti e selezionare **SQL Server**.

	![Nuovo servizio collegato di SQL Server](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
3.	Nell'**editor JSON** seguire questa procedura:
	1. Per **gatewayName** specificare **adftutorialgateway**.
	2. Se si usa l'autenticazione di Windows:
		1. In **connectionString**:
			1. Impostare **Sicurezza integrata** su **true**.
			2. Per il database specificare il **nome del server** e il **nome del database**.
			2. Rimuovere i valori nei campi **ID utente** e **Password**.
		3. Specificare il nome utente e la password per le proprietà **userName** e **password**.
		
				"typeProperties": {
            		"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;",
            		"gatewayName": "adftutorialgateway",
            		"userName": "<Specify user name if you are using Windows Authentication. Example: <domain>\<user>",
            		"password": "<Specify password for the user account>"
        		}

	4. Se si usa l'autenticazione SQL:
		1. Per il database specificare il **nome del server**, il **nome del database**, l'**ID utente** e la **password** in **connectionString**.
		2. Rimuovere le ultime due proprietà JSON, **userName** e **password**, da JSON.
		3. Rimuovere il carattere finale **, (virgola)** alla fine della riga che specifica il valore della proprietà **gatewayName**.

				"typeProperties": {
            		"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
	           		"gatewayName": "<Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database>"
    		    }
	
		Le credenziali saranno **crittografate** con un certificato di proprietà del servizio Data Factory. Se invece si intende usare il certificato associato a Gateway di gestione dati, vedere [Impostare le credenziali e la sicurezza](#set-credentials-and-security).
    
2.	Fare clic su **Distribuisci** nella barra dei comandi per distribuire il servizio collegato di SQL Server.

#### Aggiungere un servizio collegato per un account di archiviazione di Azure
 
1. Nell'**editor di Data factory** fare clic su **Nuovo archivio dati** nella barra dei comandi e quindi su **Archiviazione di Azure**.
2. Nel campo **Nome account** immettere il nome dell'account di archiviazione di Azure.
3. Nel campo **Chiave account** immettere la chiave per l'account di archiviazione di Azure.
4. Fare clic su **Distribuisci** per distribuire **AzureStorageLinkedService**.
   
 
### Passaggio 4: Creare set di dati di input e di output
In questo passaggio vengono creati i set di dati di input e di output che rappresentano i dati di input e di output per l'operazione di copia (database SQL Server locale => archiviazione BLOB di Azure). Prima di creare i set di dati o le tabelle (set di dati rettangolari), è necessario eseguire quanto segue (i passaggi dettagliati seguono l'elenco):

- Creare una tabella denominata **emp** nel database SQL Server aggiunto come servizio collegato all'istanza di Data factory e inserire una coppia di voci di esempio nella tabella.
- Creare un contenitore BLOB denominato **adftutorial** nell'account di archiviazione BLOB di Azure aggiunto come servizio collegato alla data factory.

### Preparare SQL Server locale per l'esercitazione

1. Nel database specificato per il servizio collegato di SQL Server locale (**SqlServerLinkedService**) usare lo script SQL seguente per creare la tabella **emp** nel database.


        CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL, 
			FirstName varchar(50),
			LastName varchar(50),
    		CONSTRAINT PK_emp PRIMARY KEY (ID)
		)
		GO
 

2. Inserire un esempio nella tabella:


        INSERT INTO emp VALUES ('John', 'Doe')
		INSERT INTO emp VALUES ('Jane', 'Doe')



### Creare la tabella di input

1. Nell'**editor di Data Factory** fare clic su **Nuovo set di dati** nella barra dei comandi e quindi su **Tabella SQL Server**.
2.	Sostituire lo script JSON nel riquadro a destra con il testo seguente:

		{
		  "name": "EmpOnPremSQLTable",
		  "properties": {
		    "type": "SqlServerTable",
		    "linkedServiceName": "SqlServerLinkedService",
		    "typeProperties": {
		      "tableName": "emp"
		    },
		    "external": true,
		    "availability": {
		      "frequency": "Hour",
		      "interval": 1
		    },
		    "policy": {
		      "externalData": {
		        "retryInterval": "00:01:00",
		        "retryTimeout": "00:10:00",
		        "maximumRetry": 3
		      }
		    }
		  }
		}

	Tenere presente quanto segue:
	
	- L'oggetto **type** è impostato su **SqlServerTable**.
	- **tablename** è impostato su **emp**.
	- **linkedServiceName** è impostato su **SqlServerLinkedService**; questo servizio collegato è stato creato nel passaggio 2.
	- Per una tabella di input non generata da un'altra pipeline in Data factory di Azure, è necessario impostare **external** su **true**. Indica che i dati di input sono generati al di fuori del servizio Azure Data Factory. È possibile specificare i criteri di dati esterni necessari usando l'elemento **externalData** nella sezione **Policy**.

	Vedere il [riferimento agli script JSON][json-script-reference] per dettagli sulle proprietà JSON.

2. Fare clic su **Distribuisci** sulla barra dei comandi per distribuire la set di dati (la tabella è un set di dati rettangolare). Controllare che sulla barra del titolo sia visualizzato un messaggio analogo a **DISTRIBUZIONE TABELLA COMPLETATA**.


### Creazione della tabella di output

1.	Nell'**Editor di Data factory** fare clic su **Nuovo set di dati** sulla barra dei comandi e selezionare **Archiviazione BLOB di Azure**.
2.	Sostituire lo script JSON nel riquadro a destra con il testo seguente:

		{
		  "name": "OutputBlobTable",
		  "properties": {
		    "type": "AzureBlob",
		    "linkedServiceName": "AzureStorageLinkedService",
		    "typeProperties": {
		      "folderPath": "adftutorial/outfromonpremdf",
		      "format": {
		        "type": "TextFormat",
		        "columnDelimiter": ","
		      }
		    },
		    "availability": {
		      "frequency": "Hour",
		      "interval": 1
		    }
		  }
		}
  
	Tenere presente quanto segue:
	
	- L’oggetto **type** è impostato su **AzureBlob**.
	- **linkedServiceName** è impostato su **AzureStorageLinkedService**. Questo servizio collegato è stato creato nel passaggio 2.
	- **folderPath** è impostato su **adftutorial/outfromonpremdf** dove outfromonpremdf è la cartella nel contenitore adftutorial. È necessario creare solo il contenitore **adftutorial**.
	- L'oggetto **availability** è impostato su **hourly** (l'oggetto **frequency** è impostato su **hour** e l'oggetto **interval** è impostato su **1**). Il servizio Data factory genererà una porzione di dati di output ogni ora nella tabella **emp** nel database SQL di Azure.

	Se non si specifica un oggetto **fileName** per una **tabella di input**, tutti i file e i BLOB della cartella di input (**folderPath**) vengono considerati input. Se si specifica un oggetto fileName nel JSON, solo il file/BLOB specificato viene considerato un input. Vedere i file di esempio nell'[esercitazione][adf-tutorial] per gli esempi.
 
	Se non è stato specificato **fileName** per una **tabella di output**, i file generati in **folderPath** vengono denominati con il formato seguente: Data.<Guid>.txt. Ad esempio: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.

	Per impostare **folderPath** e **fileName** dinamicamente in base all'ora **SliceStart**, usare la proprietà partitionedBy. Nell'esempio seguente folderPath usa Year, Month e Day dall'oggetto SliceStart (ora di inizio della sezione elaborata), mentre fileName usa Hour dall'oggetto SliceStart. Ad esempio, se una sezione viene generata per 2014-10-20T08:00:00, folderName è impostato su wikidatagateway/wikisampledataout/2014/10/20 e fileName è impostato su 08.csv.

	  	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
        	{ "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 

	Vedere il [riferimento agli script JSON][json-script-reference] per dettagli sulle proprietà JSON.

2.	Fare clic su **Distribuisci** sulla barra dei comandi per distribuire la set di dati (la tabella è un set di dati rettangolare). Controllare che sulla barra del titolo sia visualizzato un messaggio analogo a **DISTRIBUZIONE TABELLA COMPLETATA**.
  

### Passaggio 5: Creare ed eseguire una pipeline
In questo passaggio viene creata una **pipeline** con un'**attività di copia** che usa **EmpOnPremSQLTable** come input e **OutputBlobTable** come output.

1.	Nel pannello **DATA FACTORY** fare clic sul riquadro **Creare e distribuire** per avviare l'**Editor** per la data factory.

	![Riquadro Creare e distribuire](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2.	Fare clic su **Nuova pipeline** sulla barra dei comandi. Se il pulsante non è presente, fare clic su **...(puntini di sospensione)** per visualizzarlo.
2.	Sostituire lo script JSON nel riquadro a destra con il testo seguente:


		{
		  "name": "ADFTutorialPipelineOnPrem",
		  "properties": {
		    "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
		    "activities": [
		      {
		        "name": "CopyFromSQLtoBlob",
		        "description": "Copy data from on-prem SQL server to blob",
		        "type": "Copy",
		        "inputs": [
		          {
		            "name": "EmpOnPremSQLTable"
		          }
		        ],
		        "outputs": [
		          {
		            "name": "OutputBlobTable"
		          }
		        ],
		        "typeProperties": {
		          "source": {
		            "type": "SqlSource",
		            "sqlReaderQuery": "select * from emp"
		          },
		          "sink": {
		            "type": "BlobSink"
		          }
		        },
		        "Policy": {
		          "concurrency": 1,
		          "executionPriorityOrder": "NewestFirst",
		          "style": "StartOfInterval",
		          "retry": 0,
		          "timeout": "01:00:00"
		        }
		      }
		    ],
		    "start": "2015-02-13T00:00:00Z",
		    "end": "2015-02-14T00:00:00Z",
		    "isPaused": false
		  }
		}

	Tenere presente quanto segue:
 
	- Nella sezione delle attività esiste una sola attività con **type** impostato su **Copy**.
	- **Input** per l'attività è impostato su **EmpOnPremSQLTable** e **output** per l'attività è impostato su **OutputBlobTable**.
	- Nella sezione **transformation** **SqlSource** è specificato come **tipo di origine** e **BlobSink** come **tipo di sink**.
	- La query SQL *select * from emp* viene specificata per la proprietà **sqlReaderQuery** di **SqlSource****.

	Sostituire il valore della proprietà **start** con il giorno corrente e il valore di **end** con il giorno successivo. Per la data e ora di inizio è necessario usare il [formato ISO](http://en.wikipedia.org/wiki/ISO_8601), ad esempio 2014-10-14T16:32:41Z. Il valore di **end** è facoltativo, ma in questa esercitazione verrà usato.
	
	Se non si specifica alcun valore per la proprietà **end**, il valore verrà calcolato come "**start + 48 hours**". Per eseguire la pipeline illimitatamente, specificare **9/9/9999** come valore per la proprietà **end**.
	
	Si definisce quanto tempo durerà l'elaborazione delle sezioni di dati in base alle proprietà della **disponibilità** definite per ogni tabella di Data factory di Azure.
	
	Nell'esempio precedente sono visualizzate 24 sezioni di dati perché viene generata una sezione di dati ogni ora.
	
2. Fare clic su **Distribuisci** sulla barra dei comandi per distribuire la set di dati (la tabella è un set di dati rettangolare). Controllare che sulla barra del titolo sia visualizzato un messaggio analogo a **DISTRIBUZIONE PIPELINE COMPLETATA**.
5. A questo punto, chiudere il pannello dell'**Editor** facendo clic su **X**. Fare di nuovo clic su **X** per chiudere il pannello ADFTutorialDataFactory con la barra degli strumenti e la visualizzazione ad albero. Se viene visualizzato un messaggio analogo a **Eventuali modifiche non salvate verranno rimosse**, fare clic su **OK**.
6. Dovrebbe essere visualizzato di nuovo il pannello **DATA FACTORY** per **ADFTutorialOnPremDF**.

**Congratulazioni.** Azure Data Factory, i servizi collegati, le tabelle e una pipeline sono stati creati correttamente e la pipeline è stata pianificata.

#### Visualizzare la data factory in una vista Diagramma 
1. Nel **portale di Azure**, fare clic sul riquadro **Diagramma** nella home page per l'istanza di Data factory **ADFTutorialOnPremDF**:

	![Collegamento al diagramma](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Viene visualizzato un diagramma simile al seguente:

	![Vista Diagramma](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

	È possibile eseguire lo zoom avanti, lo zoom indietro e lo zoom al 100%, adattare alla finestra, posizionare automaticamente pipeline e tabelle e visualizzare le informazioni sulla derivazione, evidenziando gli elementi upstream e downstream degli elementi selezionati. È possibile fare doppio clic su un oggetto (tabella di input/output o pipeline) per visualizzare le relative proprietà.

### Passaggio 6: Monitorare i set di dati e la pipeline
In questo passaggio viene usato il portale di Azure per monitorare le attività in un'istanza di Azure Data Factory. È anche possibile usare i cmdlet di PowerShell per monitorare i set di dati e le pipeline. Per altre informazioni sul monitoraggio, vedere [Monitorare e gestire le pipeline](data-factory-monitor-manage-pipelines.md).

1. Passare al **portale di Azure** (se è stato chiuso)
2. Se il pannello per **ADFTutorialOnPremDF** è chiuso, aprirlo facendo clic su **ADFTutorialOnPremDF** nella **Schermata iniziale**.
3. Vengono visualizzati il **numero** e i **nomi** delle tabelle e delle pipeline create nel pannello.

	![Home page di Data Factory](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)
4. A questo punto, fare clic sul riquadro **Set di dati**.
5. Nel pannello **Set di dati**, fare clic su **EmpOnPremSQLTable**.

	![Sezioni EmpOnPremSQLTable](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Le sezioni di dati fino all'ora corrente sono state già generate e sono in stato **Pronto**. Ciò è dovuto al fatto che i dati sono stati inseriti nel database SQL Server database che è sempre presente. Verificare che non sia visualizzata alcuna sezione in **Sezioni con errori** nella parte inferiore della pagina.


	Gli elenchi **Sezioni aggiornate di recente** e **Sezioni non riuscite di recente** sono ordinati in base a **ORA ULTIMO AGGIORNAMENTO**. L'ora di aggiornamento di una sezione viene modificata nelle situazioni seguenti.
    

	-  Lo stato della sezione viene aggiornato manualmente, ad esempio usando **Set-AzureRmDataFactorySliceStatus** oppure facendo clic su **ESEGUI** nel pannello **SEZIONE** della sezione.
	-  Lo stato della sezione cambia a causa di un'esecuzione, ad esempio un'esecuzione avviata, un'esecuzione terminata con errore, un'esecuzione terminata correttamente e così via.
 
	Fare clic sul titolo degli elenchi oppure sui **... (puntini di sospensione)** per visualizzare un elenco più ampio delle sezioni. Fare clic su **Filtro** sulla barra degli strumenti per filtrare le sezioni.
	
	Per visualizzare le sezioni di dati ordinate invece in base agli orari di inizio/fine, fare clic sul riquadro **Sezioni dati (in base all'ora della sezione)**.

7. A questo punto, nel pannello **Set di dati**, fare clic su **OutputBlobTable**.

	![sezioni OputputBlobTable][image-data-factory-output-blobtable-slices]
8. Controllare che le sezioni fino all'ora corrente siano state generate e siano in stato **Pronto**. Attendere finché lo stato delle sezioni fino all'ora corrente non viene impostato **Pronto**.
9. Fare clic su una qualsiasi sezione di dati dell'elenco per visualizzare il pannello **SEZIONE DI DATI**.

	![Pannello Sezione dati](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

	Se lo stato della sezione non è **Pronto**, sarà possibile visualizzare le sezioni upstream che non sono pronte e bloccano l'esecuzione della sezione corrente nell'elenco **Sezioni upstream non pronte**.

10. Fare clic sull'**esecuzione attività** dall'elenco nella parte inferiore della pagina per visualizzare i **dettagli dell'esecuzione attività**.

	![pannello Dettagli esecuzione attività][image-data-factory-activity-run-details]

11. Fare clic su **X** per chiudere tutti i pannelli fino
12. a tornare al pannello iniziale di **ADFTutorialOnPremDF**.
14. (Facoltativo) Fare clic su **Pipeline** e su **ADFTutorialOnPremDF**, quindi eseguire il drill-through delle tabelle di input (**utilizzate**) o delle tabelle di output (**generate**).
15. Usare strumenti come **Esplora archivi Azure** per verificare l'output.

	![Esplora archivi Azure](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## Spostare il gateway da un computer a un altro
Questa sezione illustra la procedura per spostare il client del gateway da un computer a un altro.

2. Nel portale passare alla **Home page di Data factory** e fare clic sul riquadro **Servizi collegati**.

	![Collegamento Gateway dati](./media/data-factory-move-data-between-onprem-and-cloud/DataGatewaysLink.png)
3. Selezionare il gateway nella sezione **GATEWAY DATI** nel pannello **Servizi collegati**.
	
	![Pannello Servizi collegati con gateway selezionato](./media/data-factory-move-data-between-onprem-and-cloud/LinkedServiceBladeWithGateway.png)
4. Nel pannello **Gateway dati** fare clic su **Scaricare e installare il gateway dati**.
	
	![Collegamento di download del gateway](./media/data-factory-move-data-between-onprem-and-cloud/DownloadGatewayLink.png)
5. Nel pannello **Configura** fare clic su **Scaricare e installare il gateway dati** e seguire le istruzioni per installare il gateway dati nel computer.

	![Pannello Configura](./media/data-factory-move-data-between-onprem-and-cloud/ConfigureBlade.png)
6. Tenere aperto **Gestione configurazione di Gateway di gestione dati di Microsoft**.
 
	![Gestione configurazione](./media/data-factory-move-data-between-onprem-and-cloud/ConfigurationManager.png)
7. Nel pannello **Configura** del portale fare clic su **Ricrea chiave** nella barra dei comandi e su **Sì** per il messaggio di avviso. Fare clic sul **pulsante Copia** accanto al testo della chiave per copiare la chiave negli Appunti. Il gateway nel computer precedente smetterà di funzionare non appena si ricrea la chiave.
	
	![Ricrea chiave](./media/data-factory-move-data-between-onprem-and-cloud/RecreateKey.png)
	 
8. Incollare la **Chiave** nella casella di testo della pagina **Registra gateway** in **Gestione configurazione di Gateway di gestione dati** nel computer. (Facoltativo) Selezionare la casella di controllo **Mostra chiave del gateway** per visualizzare il testo della chiave.
 
	![Copia della chiave e registrazione](./media/data-factory-move-data-between-onprem-and-cloud/CopyKeyAndRegister.png)
9. Fare clic su **Registra** per registrare il gateway con il servizio cloud.
10. Nella pagina **Specificare un certificato** fare clic su **Sfoglia** per selezionare lo stesso certificato usato con il gateway precedente, immettere la **password**, fare clic su **Fine**.
 
	![Specificare un certificato](./media/data-factory-move-data-between-onprem-and-cloud/SpecifyCertificate.png)

	È possibile esportare un certificato dal gateway precedente seguendo questa procedura: avviare Gestione configurazione di Gateway di gestione dati nel computer precedente, passare alla scheda **Certificato**, fare clic su **Esporta** e seguire le istruzioni.
10. Dopo la registrazione del gateway, nella home page di Gestione configurazione di Gateway di gestione dati verranno visualizzati **Registrazione** impostato su **Registrato** e **Stato** impostato su **Avviato**.

## Impostare le credenziali e la sicurezza
Per crittografare le credenziali in Data Factory Editor, seguire questa procedura:

1. Avviare il browser web nel **computer gateway**, passare al [portale di Azure](http://portal.azure.com), cercare la data factory se necessario, aprire la data factory nel pannello **DATA FACTORY** e quindi fare clic su **Creare e distribuire** per avviare l'editor di Data Factory.
1. Fare clic su un **servizio collegato** esistente nella visualizzazione albero per vedere la relativa definizione JSON o creare un nuovo servizio collegato che richieda un gateway di gestione dati, ad esempio SQL Server o Oracle.
2. Nell'editor JSON specificare il nome del gateway per la proprietà **gatewayName**.
3. Immettere il nome del server per la proprietà **Origine dati** in **connectionString**.
4. Immettere il nome del server per la proprietà **Catalogo iniziale** in **connectionString**.
5. Fare clic sul pulsante **Crittografa** sulla barra dei comandi per avviare l'applicazione ClickOnce **Gestione credenziali**. Verrà visualizzata la finestra di dialogo **Impostazione credenziali**. ![Finestra di dialogo Impostazione credenziali](./media/data-factory-move-data-between-onprem-and-cloud/setting-credentials-dialog.png)
6. Nella finestra di dialogo **Impostazione credenziali** seguire questa procedura:
	1.	Selezionare l'**autenticazione** che sarà usata dal servizio Data Factory per connettersi al database.
	2.	Per l'impostazione **NOME UTENTE** immettere il nome dell'utente che ha accesso al database.
	3.	Per l'impostazione **PASSWORD** immettere la password dell'utente.
	4.	Fare clic su **OK** per crittografare le credenziali e chiudere la finestra di dialogo.
5.	Verrà ora visualizzata una proprietà **encryptedCredential** in **connectionString**.
		
			{
	    		"name": "SqlServerLinkedService",
		    	"properties": {
		        	"type": "OnPremisesSqlServer",
			        "description": "",
		    	    "typeProperties": {
		    	        "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
		            	"gatewayName": "adftutorialgateway"
		        	}
		    	}
			}

Se si accede al portale da un computer diverso dal computer del gateway, è necessario assicurarsi che l'applicazione di gestione credenziali possa connettersi al computer del gateway. Se l'applicazione non riesce a raggiungere il computer del gateway, non sarà possibile impostare le credenziali per l'origine dati e per testare la connessione all'origine dati.

Quando si usa l'applicazione **Impostazione credenziali** avviata dal portale di Azure per impostare le credenziali per un'origine dati locale, il portale crittografa le credenziali usando il certificato specificato nella scheda **Certificato** di **Gestione configurazione di Gateway di gestione dati** del computer gateway.

Se si vuole un approccio basato su API per crittografare le credenziali, è possibile usare il cmdlet di PowerShell [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx). Questo cmdlet consente di crittografare le credenziali mediante il certificato usato dal gateway. È possibile aggiungere le credenziali crittografate restituite da questo cmdlet all'elemento **EncryptedCredential** di **connectionString** nel file JSON da usare con il cmdlet [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) oppure nel frammento di codice JSON dell'editor di Data Factory nel portale.

	"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

Esiste un altro approccio per impostare le credenziali usando l'editor delle data factory. Se si crea un servizio collegato di SQL Server usando l'editor e si immettono le credenziali in testo normale, le credenziali vengono crittografate tramite un certificato che appartiene al servizio Data Factory, NON tramite il certificato usato dal gateway. Anche se questo approccio potrebbe apparire leggermente più veloce, in alcuni casi risulta meno sicuro. È pertanto consigliabile seguire questo approccio solo per scopi di sviluppo o di test.


## Creare e registrare il Gateway di gestione dati con Azure PowerShell 
Questa sezione descrive come creare e registrare un gateway con i cmdlet di Azure PowerShell.

1. Avviare **Azure PowerShell** in modalità di amministrazione.
2. Accedere all'account Azure eseguendo il comando seguente e immettendo le credenziali di Azure.

	Login-AzureRmAccount
2. Usare il cmdlet **New-AzureRmDataFactoryGateway** per creare un gateway logico come illustrato di seguito:

		$MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

	**Comando di esempio e output**:


		PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

		Name              : MyGateway
		Description       : gateway for walkthrough
		Version           :
		Status            : NeedRegistration
		VersionStatus     : None
		CreateTime        : 9/28/2014 10:58:22
		RegisterTime      :
		LastConnectTime   :
		ExpiryTime        :
		ProvisioningState : Succeeded
		Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI

	
4. In Azure PowerShell passare alla cartella **C:\\Programmi\\Microsoft Data Management Gateway\\1.0\\PowerShellScript** ed eseguire lo script **RegisterGateway.ps1** associato alla variabile locale **$Key**, come mostrato nel comando seguente per registrare l'agente client installato nel computer con il gateway logico creato in precedenza.

		PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
		
		Agent registration is successful!

	È possibile registrare il gateway in un computer remoto usando il parametro IsRegisterOnRemoteMachine. Esempio:
		
		.\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true

5. È possibile usare il cmdlet **Get-AzureRmDataFactoryGateway** per ottenere l'elenco di gateway nell'istanza di Data factory. Quando lo **stato** è **online**, il gateway è pronto per essere usato.

		Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

È possibile rimuovere un gateway con il cmdlet **Remove-AzureRmDataFactoryGateway** e aggiornare la descrizione per un gateway usando i cmdlet **Set-AzureRmDataFactoryGateway**. Per la sintassi e altri dettagli relativi a questi cmdlet, vedere Riferimento ai cmdlet di Data Factory.

## Elencare i gateway usando PowerShell

	Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup

## Rimuovere il gateway usando PowerShell
	
	Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force 


## Flusso di dati per la copia mediante il Gateway di gestione dati
Quando si usa un'attività di copia in una pipeline di dati per inserire dati locali nel cloud ai fini di una successiva elaborazione o per esportare nuovamente i dati memorizzati nel cloud in un archivio dati locale, l'attività di copia usa un gateway per trasferire i dati dall'origine dati locale nel cloud e viceversa.

Di seguito sono riportati un flusso di dati generale e un riepilogo dei passaggi per la copia con il gateway di dati: ![Flusso di dati mediante gateway](./media/data-factory-move-data-between-onprem-and-cloud/data-flow-using-gateway.png)

1.	Viene creato un nuovo gateway per l'istanza di Data factory di Azure usando il [portale di Azure](https://portal.azure.com) oppure un [cmdlet di PowerShell](https://msdn.microsoft.com/library/dn820234.aspx).
2.	Tramite il pannello "Servizi collegati" viene definito un nuovo servizio collegato per un archivio dati locale all'interno del gateway. Una parte della configurazione del servizio collegato consiste nell'uso dell'applicazione Impostazione credenziali per specificare i tipi di autenticazione e le credenziali come illustrato nella procedura dettagliata. La finestra di dialogo dell'applicazione Impostazione credenziali comunicherà con l'archivio dati per eseguire il test della connessione e con il gateway per salvare le credenziali.
3.	Il gateway crittograferà le credenziali tramite il certificato associato al gateway (fornito dallo sviluppatore) prima di salvare le credenziali nel cloud.
4.	Il servizio di spostamento di Data Factory comunica con il gateway per la pianificazione e la gestione dei processi tramite un canale di controllo che usa una coda condivisa del bus di servizio di Azure. Quando occorre avviare il processo di attività di copia, Data Factory inserisce in coda la richiesta insieme alle informazioni sulle credenziali. Il gateway avvia il processo dopo avere eseguito il polling della coda.
5.	Il gateway decrittografa le credenziali tramite lo stesso certificato e quindi si connette all'archivio dati locale con il tipo di autenticazione appropriato.
6.	Il gateway copia i dati dall'archivio dati locale in una risorsa di archiviazione cloud o viceversa in base alla configurazione dell'attività di copia nella pipeline di dati. Nota: per questo passaggio il gateway comunica direttamente con un servizio di archiviazione basato sul cloud, ad esempio BLOB di Azure, Azure SQL e così via, su un canale protetto (HTTPS).

<!---HONumber=AcomDC_0629_2016-->