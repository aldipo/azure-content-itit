<properties
	pageTitle="Introduzione al backup di DPM di Azure | Microsoft Azure"
	description="Introduzione al backup dei server DPM di Azure usando il servizio Backup di Azure"
	services="backup"
	documentationCenter=""
	authors="trinadhk"
	manager="shreeshd"
	editor=""
	keywords="System Center Data Protection Manager, data protection manager, backup di dpm"/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/10/2016"
	ms.author="trinadhk;giridham;jimpark"/>

# Preparazione del backup dei carichi di lavoro in Azure con DPM

> [AZURE.SELECTOR]
- [Server di backup di Azure](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Server di Backup di Azure (classico)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (classico)](backup-azure-dpm-introduction-classic.md)

Questo articolo offre un'introduzione all'uso del servizio Backup di Microsoft Azure per proteggere i server e i carichi di lavoro DPM di System Center. Le informazioni dell'articolo riguardano:

- Il funzionamento del backup del server DPM di Azure
- I prerequisiti per eseguire un backup senza problemi
- Gli errori tipici rilevati e come gestirli
- Scenari supportati

> [AZURE.NOTE] Azure offre due modelli di distribuzione per creare e usare le risorse: [Azure Resource Manager e la distribuzione classica](../resource-manager-deployment-model.md). Questo articolo include informazioni e procedure per il ripristino di VM distribuite con il modello Resource Manager.

DPM di System Center esegue il backup di file e dati delle applicazioni. I dati sottoposti a backup su DPM possono essere archiviati su nastro, disco oppure sottoposti a backup in Azure usando il servizio Backup di Microsoft Azure. DPM interagisce con Backup di Azure nei modi seguenti:

- **DPM distribuito come server fisico o come macchina virtuale locale**: se DPM è distribuito come server fisico o come macchina virtuale Hyper-V locale, è possibile eseguire il backup dei dati in un insieme di credenziali dei servizi di ripristino oltre al backup su disco e su nastro.
- **DPM distribuito come macchina virtuale di Azure**: a partire dalla versione di System Center 2012 R2 con aggiornamento 3, DPM può essere distribuito come macchina virtuale di Azure. Se DPM è distribuito come macchina virtuale di Azure, è possibile eseguire il backup dei dati sui dischi di Azure allegati alla macchina virtuale Azure di DPM oppure è possibile eseguire l'offload dell'archiviazione dei dati eseguendo il backup su un insieme di credenziali dei servizi di ripristino.

## Motivi per eseguire il backup dei server DPM

Di seguito sono elencati i vantaggi aziendali derivanti dall'uso del servizio Backup di Azure per eseguire il backup dei server DPM:

- Per la distribuzione DPM locale è possibile usare Azure come alternativa alla distribuzione a lungo termine su nastro.
- Per le distribuzioni DPM in Azure, Backup di Azure consente di eseguire l'offload dell'archiviazione dal disco di Azure e aumentare le prestazioni archiviando i dati meno recenti in un insieme di credenziali dei servizi di ripristino e i dati nuovi sul disco.

## Funzionamento del backup del server DPM
Per offrire protezione dei dati su disco, il server DPM crea e conserva una replica o una copia dei dati che si trovano sui server protetti. Le repliche sono archiviate nel pool di archiviazione che consiste di un set di dischi nel server DPM o in un volume personalizzato. Se si vuole proteggere i dati del file o dati dell'applicazione, la protezione inizia con la creazione della replica dell'origine dati. La replica è sincronizzata o aggiornata, a intervalli regolari in base alle impostazioni configurate. Quando si usa una protezione a breve termine sui dischi e a lungo termine nel cloud, DPM può eseguire il backup dei dati dal volume di replica all'insieme di credenziali dei servizi di ripristino in modo che non venga causato alcun impatto sul computer protetto.

## Prerequisiti
Di seguito viene descritto come preparare il servizio Backup di Azure all'esecuzione del backup dei dati DPM:

1. **Creare un insieme di credenziali dei servizi di ripristino**: creare un insieme di credenziali nel portale di Azure.
2. **Scaricare le credenziali dell'insieme di credenziali**: scaricare le credenziali usate per registrare il server DPM all'insieme di credenziali dei servizi di ripristino.
3. **Installare l'agente di Backup di Azure e registrare il server**: da Backup di Azure installare l'agente su ogni server DPM e registrare il server DPM all'insieme di credenziali dei servizi di ripristino.

### 1\. Creare un insieme di credenziali dei servizi di ripristino
Per creare un insieme di credenziali dei servizi di ripristino:

1. Accedere al [portale di Azure](https://portal.azure.com/).

2. Scegliere **Sfoglia** dal menu Hub e digitare **Servizi di ripristino** nell'elenco di risorse. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Fare clic su **Insieme di credenziali dei servizi di ripristino**.

    ![Creare un insieme di credenziali dei servizi di ripristino - Passaggio 1](./media/backup-azure-dpm-introduction/open-recovery-services-vault.png)

    Viene visualizzato l'elenco degli insiemi di credenziali dei servizi di ripristino.

3. Scegliere **Aggiungi** dal menu **Insiemi di credenziali dei servizi di ripristino**.

    ![Creare un insieme di credenziali dei servizi di ripristino - Passaggio 2](./media/backup-azure-dpm-introduction/rs-vault-menu.png)

    Si apre il pannello degli insiemi di credenziali dei servizi di ripristino in cui viene chiesto di specificare **Nome**, **Sottoscrizione**, **Gruppo di risorse** e **Località**.

    ![Creare un insieme di credenziali dei servizi di ripristino - Passaggio 5](./media/backup-azure-dpm-introduction/rs-vault-attributes.png)

4. Nel campo **Nome** digitare un nome descrittivo per identificare l'insieme di credenziali. Il nome deve essere univoco per la sottoscrizione di Azure. Digitare un nome che contenga tra i 2 e i 50 caratteri. Deve iniziare con una lettera e può contenere solo lettere, numeri e trattini.

5. Fare clic su **Sottoscrizione** per visualizzare l'elenco di sottoscrizioni disponibili. Se non si è certi di quale sottoscrizione usare, usare la sottoscrizione predefinita (o suggerita). Sono presenti scelte multiple solo se l'account dell'organizzazione è associato a più sottoscrizioni di Azure.

6. Fare clic su **Gruppo di risorse** per visualizzare l'elenco dei gruppi di risorse disponibili oppure fare clic su **Nuovo** per crearne uno nuovo. Per informazioni complete sui gruppi di risorse, vedere [Uso del portale di Azure per distribuire e gestire le risorse di Azure](../azure-portal/resource-group-portal.md).

7. Fare clic su **Località** per selezionare l'area geografica per l'insieme di credenziali.

8. Fare clic su **Create**. La creazione dell'insieme di credenziali dei servizi di ripristino può richiedere alcuni minuti. Monitorare le notifiche di stato nell'area superiore destra del portale. Una volta creato, l'insieme di credenziali viene aperto nel portale.

### Impostare la replica di archiviazione

L'opzione della replica di archiviazione consente di scegliere tra l'archiviazione con ridondanza geografica e l'archiviazione con ridondanza locale. Per impostazione predefinita, l'insieme di credenziali prevede l'archiviazione con ridondanza geografica. Se si tratta del backup primario, lasciare l'opzione impostata sull'archiviazione con ridondanza geografica. Se si vuole un'opzione più economica ma non altrettanto permanente, scegliere l'archiviazione con ridondanza locale. Per altre informazioni sulle opzioni di archiviazione con [ridondanza geografica](../storage/storage-redundancy.md#geo-redundant-storage) e con [ridondanza locale](../storage/storage-redundancy.md#locally-redundant-storage), vedere la [panoramica Replica di archiviazione di Azure](../storage/storage-redundancy.md).

Per modificare le impostazioni di replica di archiviazione:

1. Selezionare l'insieme di credenziali per aprire il dashboard dell'insieme di credenziali e il pannello Impostazioni. Se il pannello **Impostazioni** non si apre, fare clic su **Tutte le impostazioni** nel dashboard dell'insieme di credenziali.

2. Nel pannello **Impostazioni** fare clic su **Infrastruttura di backup** > **Configurazione backup** per aprire il pannello **Configurazione backup**. Nel pannello **Configurazione backup** scegliere l'opzione di replica di archiviazione per l'insieme di credenziali.

    ![Elenco degli insiemi di credenziali per il backup](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Dopo aver scelto l'opzione di archiviazione per l'insieme di credenziali, è possibile associare la macchina virtuale all'insieme di credenziali. Per iniziare l'associazione, è necessario trovare e registrare le macchine virtuali di Azure.


### 2\. Scaricare le credenziali dell’insieme di credenziali

Il file delle credenziali di insieme è un certificato generato dal portale per ogni insieme di credenziali per il backup. Il portale carica quindi la chiave pubblica nel Servizio di controllo di accesso (o ACS). La chiave privata del certificato viene resa disponibile per l'utente come parte del flusso di lavoro indicato come input nel flusso di lavoro di registrazione del computer. In questo modo viene eseguita l'autenticazione del computer per l'invio dei dati di backup a un insieme di credenziali identificato nel servizio Backup di Azure.

Il file delle credenziali di insieme viene usato solo durante il flusso di lavoro di registrazione. È responsabilità dell'utente garantire che il file delle credenziali di insieme non venga danneggiato. Nelle mani di un utente non autorizzato il file delle credenziali di insieme può essere usato per registrare altri computer nello stesso insieme di credenziali. Poiché i dati di backup sono crittografati tramite una passphrase appartenente al cliente, i dati di backup esistenti non possono tuttavia essere compromessi. Per attenuare questo problema, le credenziali dell'insieme di credenziali sono impostate per scadere in 48hrs. Sebbene sia possibile scaricare le credenziali di un insieme di credenziali di un servizio di ripristino un numero illimitato di volte, solo il file delle credenziali più recente è applicabile durante il flusso di lavoro della registrazione.

Il file delle credenziali di insieme viene scaricato dal portale di Azure tramite un canale sicuro. Il servizio Backup di Azure non è a conoscenza della chiave privata del certificato e la chiave privata non è persistente nel portale o nel servizio. Usare la procedura seguente per scaricare le credenziali di insieme in un computer locale.

1. Accedere al [portale di Azure](https://portal.azure.com/).

2. Aprire l'insieme di credenziali dei servizi di ripristino a cui si vuole registrare la macchina DPM.

3. Per impostazione predefinita si apre il pannello Impostazioni. Se non si apre, fare clic su **Impostazioni** nel dashboard dell'insieme di credenziali per aprire il pannello delle impostazioni. Nel pannello Impostazioni fare clic su **Proprietà**.

	![Pannello dell'insieme di credenziali aperto](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. Nella pagina delle proprietà fare clic su **Scarica** in **Credenziali di backup**. Il portale genera il file delle credenziali di insieme, che diventa disponibile per il download.

    ![Scaricare](./media/backup-azure-dpm-introduction/vault-credentials.png)

Il portale genererà una credenziale di insieme usando una combinazione del nome dell'insieme di credenziali e della data attuale. Fare clic su **Salva** per scaricare le credenziali di insieme nella cartella di download dell'account locale o selezionare Salva con nome dal menu Salva per specificare un percorso per le credenziali. La generazione del file potrebbe impiegare fino a un minuto.

### Nota
- Assicurarsi che il file delle credenziali dell'insieme di credenziali venga salvato in un percorso accessibile dal computer. Se vengano archiviate in una condivisione file/SMB, verificare le autorizzazioni di accesso.
- Il file delle credenziali di insieme viene usato solo durante il flusso di lavoro di registrazione.
- Il file delle credenziali di insieme scade dopo 48 ore e può essere scaricato dal portale.



### 3\. Installare un agente di Backup

Dopo aver creato l'insieme di credenziali di Backup di Azure, è necessario installare un agente su tutti i computer Windows (Windows Server, client Windows, server System Center Data Protection Manager o computer del server di Backup di Azure) per eseguire il backup dei dati e delle applicazioni in Azure.

1. Aprire l'insieme di credenziali dei servizi di ripristino a cui si vuole registrare la macchina DPM.

2. Per impostazione predefinita si apre il pannello Impostazioni. Se non si apre, fare clic su **Impostazioni** per aprire il pannello delle impostazioni. Nel pannello Impostazioni fare clic su **Proprietà**.

	![Pannello dell'insieme di credenziali aperto](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

3. Nella pagina Impostazioni fare clic su **Scarica** in **Agente di Backup di Azure**.

    ![Scaricare](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Dopo aver scaricato l'agente, fare doppio clic sul file MARSAgentInstaller.exe per avviare l'installazione dell'agente di Backup di Azure. Scegliere la cartella di installazione e la cartella Scratch necessarie per l'agente. Il percorso della cache specificato deve disporre di uno spazio libero pari almeno al 5% dei dati di backup.

4.	Se si usa un server proxy per connettersi a Internet, nella schermata **Configurazione proxy** immettere i dettagli del server proxy. Se si usa un proxy autenticato, immettere il nome utente e la password in questa schermata.

5.	Per completare l'installazione, l'agente Backup di Azure installerà .NET Framework 4.5 e Windows PowerShell (se non è già disponibile).

6.	Dopo aver installato l'agente, fare clic sul pulsante **Procedi alla registrazione** per continuare con il flusso di lavoro.

    ![Opzione Register](../../includes/media/backup-install-agent/register.png)

7. Nella schermata relativa alle credenziali di insieme individuare e selezionare il file delle credenziali di insieme scaricato in precedenza.

    ![Credenziali di insieme](../../includes/media/backup-install-agent/vc.png)

    Il file delle credenziali di insieme è valido solo per 48 ore dopo che è stato scaricato dal portale. Se si verifica un errore in questa schermata, ad esempio "Il file delle credenziali di insieme fornito è scaduto", accedere al portale di Azure e scaricare nuovamente il file delle credenziali di insieme.

    Verificare che il file delle credenziali di insieme sia disponibile in un percorso accessibile dall'applicazione di installazione. Se si verificano errori relativi all'accesso, copiare il file delle credenziali di insieme in un percorso temporaneo nel computer e ripetere l'operazione.

    Se si verifica un errore di credenziali dell'insieme di credenziali non valide, ad esempio "Le credenziali dell'insieme di credenziali specificate non sono valide", il file è danneggiato o non ha le credenziali più recenti associate al servizio di ripristino. Ripetere l'operazione dopo avere scaricato un nuovo file di archivio delle credenziali dal portale. Questo errore in genere si verifica se l'utente fa clic sull'opzione **Scarica credenziali di insieme** nel portale di Azure in rapida successione. In questo caso è valido solo il secondo file delle credenziali di insieme.

8. Nella schermata **impostazione crittografia**, è possibile generare una passphrase o fornire una passphrase (almeno 16 caratteri). Ricordarsi di salvare la passphrase in un luogo sicuro.

    ![Crittografia](../../includes/media/backup-install-agent/encryption.png)

    > [AZURE.WARNING] Se la passphrase viene persa o dimenticata, Microsoft non potrà fornire assistenza per il recupero dei dati di backup. L'utente finale possiede la passphrase di crittografia, che non è visibile a Microsoft. Salvare il file in un luogo sicuro, in quanto potrebbe essere necessario durante un'operazione di ripristino.

9. Dopo aver selezionato il pulsante **Fine**, il computer sarà registrato nell'insieme di credenziali e sarà possibile avviare il backup in Microsoft Azure.

10. È possibile modificare le impostazioni specificate durante la registrazione del flusso di lavoro facendo clic sull'opzione **Modifica proprietà** nello snap-in di MMC di Backup di Microsoft Azure.

    ![Modifica proprietà](../../includes/media/backup-install-agent/change.png)

    In alternativa, quando si utilizza Data Protection Manager, è possibile modificare le impostazioni specificate durante il flusso di lavoro di registrazione facendo clic sull’opzione **Configura** selezionando **Online** sotto la scheda **Management**.

    ![Configurare il Backup di Azure](../../includes/media/backup-install-agent/configure.png)

## Requisiti e limitazioni

- DPM può essere eseguito come server fisico o come macchina virtuale Hyper-V installata in System Center 2012 SP1 o System Center 2012 R2. Inoltre, è possibile eseguirlo come macchina virtuale di Azure in System Center 2012 R2 (con almeno l'aggiornamento cumulativo 3 di DPM 2012 R2) oppure come macchina virtuale di Windows in VMware con System Center 2012 R2 e almeno il relativo aggiornamento cumulativo 5.
- Se DPM viene eseguito con System Center 2012 SP1, è necessario installare l'aggiornamento cumulativo 2 per System Center Data Protection Manager SP1. Tale procedura è necessaria prima di poter installare Azure Backup Agent.
- È necessario che nel server DPM siano installati Windows PowerShell e .Net Framework 4.5.
- DPM può eseguire il backup della maggior parte dei carichi di lavoro nel servizio Backup di Azure. Per visualizzare un elenco completo degli elementi supportati, consultare gli elementi supportati di Backup di Azure riportati di seguito.
- Usando l'opzione "Copia su nastro", non è possibile ripristinare i dati memorizzati in Backup di Azure.
- È necessario disporre di un account di Azure su cui è abilitata la funzionalità Backup di Azure. Se non si dispone di un account, è possibile creare un account di valutazione gratuita in pochi minuti. Informazioni sui [prezzi di Backup di Azure](https://azure.microsoft.com/pricing/details/backup/).
- Per usare Backup di Azure è necessario che Azure Backup Agent sia installato sui server da sottoporre a backup. Le dimensioni di ogni server devono essere almeno il 10% di quelle dei dati sottoposti a backup. Spazio disponibile come archiviazione locale. Se, ad esempio, si esegue il backup di 100 GB di dati, è necessario un minimo di 10 GB di spazio disponibile nello spazio di lavoro. Anche se la dimensione minima è 10%, è consigliabile usare uno spazio di archiviazione locale del 15% per il percorso della cache.
- I dati verranno memorizzati nell'archiviazione relativa all'insieme di credenziali di Azure. Non esistono limiti relativi alla quantità di dati che è possibile sottoporre a backup in un insieme di credenziali di Backup di Azure, tuttavia la dimensione dell'origine dati (ad esempio, un database o una macchina virtuale) non deve superare i 54400 GB.

Il backup in Azure è supportato per i tipi di file seguenti:

- Crittografati (solo backup completi)
- Compressi (backup incrementali supportati)
- Sparse (backup incrementali supportati)
- Compressi e sparse (considerati come sparse)

Questi tipi di file non sono supportati:

- I server nei file system che distinguono fra minuscole e maiuscole non sono supportati.
- Collegamenti reali (ignorati)
- Reparse point (ignorati)
- Crittografati e compressi (ignorati)
- Crittografati e sparse (ignorati)
- Flusso compresso
- Flusso di tipo sparse

>[AZURE.NOTE] A partire dalla versione di System Center 2012 DPM con SP1, è possibile eseguire il backup su carichi di lavoro (protetti da DPM) in Azure usando il servizio Backup di Microsoft Azure.

<!---HONumber=AcomDC_0518_2016-->