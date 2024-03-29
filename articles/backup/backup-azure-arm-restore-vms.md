
<properties
	pageTitle="Ripristinare macchine virtuali dal backup con il portale di Azure | Microsoft Azure"
	description="Ripristinare una macchina virtuale di Azure da un punto di ripristino con il portale di Azure"
	services="backup"
	documentationCenter=""
	authors="markgalioto"
	manager="jwhit"
	editor=""
	keywords="ripristinare il backup; come ripristinare; punto di ripristino."/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/10/2016"
	ms.author="trinadhk; jimpark;"/>


# Usare il portale di Azure per ripristinare macchine virtuali

> [AZURE.SELECTOR]
- [Ripristinare VM nel portale classico](backup-azure-restore-vms.md)
- [Ripristinare VM nel Portale di Azure](backup-azure-arm-restore-vms.md)


È possibile proteggere i dati con il servizio di backup mediante la creazione di snapshot dei dati a intervalli definiti. Questi snapshot sono noti come punti di ripristino e vengono archiviati negli insiemi di credenziali dei servizi di ripristino. Se o quando è necessario ripristinare o ricreare una VM, è possibile ripristinare la macchina virtuale da qualsiasi punto di ripristino salvato. Quando si ripristina un punto di ripristino, si ripristina lo stato della VM corrispondente al momento in cui è stato creato il punto di ripristino. Questo articolo illustra come ripristinare una VM.

> [AZURE.NOTE] Azure offre due modelli di distribuzione per creare e usare le risorse: [Azure Resource Manager e il modello classico](../resource-manager-deployment-model.md). Questo articolo include informazioni e procedure per il ripristino di VM distribuite con il modello Resource Manager.



## Ripristinare un punto di ripristino

1. Accedere al [portale di Azure](http://portal.azure.com/)

2. Scegliere **Sfoglia** dal menu Azure e nell'elenco dei servizi digitare **Servizi di ripristino**. L'elenco di servizi viene modificato in base a quanto digitato. Quando viene visualizzato **Insiemi di credenziali dei servizi di ripristino**, selezionare questa opzione.

    ![Aprire l'insieme di credenziali dei Servizi di ripristino](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Viene visualizzato l'elenco di insiemi di credenziali della sottoscrizione.

    ![Elenco di insiemi di credenziali dei Servizi di ripristino](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)

3. Dall'elenco selezionare l'insieme di credenziali associato alla VM da ripristinare. Quando si fa clic sull'insieme di credenziali, viene aperto il dashboard corrispondente.

    ![Elenco di insiemi di credenziali dei Servizi di ripristino](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)

4. Nel dashboard dell'insieme di credenziali fare clic su **Macchine virtuali di Azure** nel riquadro **Elementi di backup** per visualizzare le VM associate all'insieme di credenziali.

    ![Dashboard dell'insieme di credenziali](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    Viene aperto il pannello **Elementi di backup**, che include l'elenco di macchine virtuali di Azure.

    ![Elenco di VM nell'insieme di credenziali](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)

5. Dall'elenco selezionare una VM per aprire il dashboard. Il dashboard della VM apre l'area di monitoraggio che include il riquadro Punti di ripristino.

    ![Elenco di VM nell'insieme di credenziali](./media/backup-azure-arm-restore-vms/vm-blade.png)

6. Scegliere **Ripristina** dal menu del dashboard della VM

    ![Elenco di VM nell'insieme di credenziali](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    Viene aperto il pannello Ripristina.

    ![Pannello Ripristina](./media/backup-azure-arm-restore-vms/restore-blade.png)

7. Nel pannello **Ripristina** fare clic su **Punto di ripristino** per aprire il pannello in cui **selezionare il punto di ripristino**.

    ![Pannello Ripristina](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Per impostazione predefinita, la finestra di dialogo visualizza tutti i punti di ripristino degli ultimi 30 giorni. Usare il **filtro** nella parte superiore del pannello per modificare l'intervallo di tempo dei punti di ripristino. Per impostazione predefinita, vengono visualizzati i punti di ripristino di ogni coerenza. Modificare il filtro **Tutti i punti di ripristino** per selezionare una coerenza specifica dei punti di ripristino. Per altre informazioni sui tipi di punto di ripristino, vedere la spiegazione relativa alla [Coerenza dei dati](./backup-azure-vms-introduction.md#data-consistency).
    - Nell'elenco **Coerenza dei punti di ripristino** scegliere:
        - Punti di ripristino coerenti con l'arresto anomalo del sistema
        - Punti di ripristino coerenti con l'applicazione
        - Punti di ripristino coerenti con il file system
        - Tutti i punti di ripristino  

8. Scegliere un punto di ripristino e fare clic su **OK**.

    ![Scegliere il punto di ripristino](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    Il pannello **Ripristina** indica che il punto di ripristino è stato impostato.

    ![Punto di ripristino impostato](./media/backup-azure-arm-restore-vms/recovery-point-set.png)

9. Nel pannello **Ripristino** si apre automaticamente la schermata di **configurazione del ripristino** dopo aver impostato il punto di ripristino.

    ![Configurazione guidata del ripristino configurata](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

## Scelta di una configurazione di ripristino per la macchina virtuale

Dopo aver selezionato il punto di ripristino, scegliere una configurazione per la macchina virtuale di ripristino. È possibile configurare la macchina virtuale ripristinata tramite il portale di Azure o PowerShell.

> [AZURE.NOTE] Il portale offre un'opzione di creazione rapida per la macchina virtuale ripristinata. Se si desidera personalizzare la configurazione della macchina virtuale da ripristinare, usare PowerShell per ripristinare i dischi di cui è stato eseguito il backup e allegarli all'opzione di configurazione della macchina virtuale. Vedere [Ripristino di una VM con configurazioni di rete speciali](#restoring-vms-with-special-network-configurations).

1. Se non è già aperto, passare al pannello **Ripristina**. Assicurarsi che sia stato selezionato un **punto di ripristino**, quindi fare clic su **Configurazione di ripristino** per aprire il pannello **Configurazione di ripristino**.

    ![Configurazione guidata del ripristino configurata](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

2. Nel pannello della **configurazione di ripristino** specificare o selezionare i valori per ciascuno dei campi seguenti:
    - **Nome macchina virtuale**: specificare un nome per la macchina virtuale. Il nome deve essere univoco per il gruppo di risorse (per una VM Azure Resource Manager) oppure per il servizio cloud (per una VM classica). Non è possibile sostituire la macchina virtuale quando si tratta di un macchina virtuale presente nella sottoscrizione.
    - **Gruppo di risorse**: usare un gruppo di risorse esistente oppure crearne uno nuovo. Se si ripristina una VM classica, usare questo campo per specificare il nome del nuovo servizio cloud. Quando si crea un nuovo gruppo di risorse/servizio cloud, è necessario che il nome sia univoco a livello globale. Il nome del servizio cloud è in genere associato a un URL pubblico, ad esempio: [serviziocloud].cloudapp.net. Se si prova a usare un nome già usato per il gruppo di risorse cloud/servizio cloud, Azure assegna al gruppo di risorse/servizio cloud lo stesso nome della VM. Azure visualizza i gruppi di risorse/servizi cloud e le VM non associate ai gruppi di affinità. Per altre informazioni, vedere [Come eseguire la migrazione da gruppi di affinità a una rete virtuale (VNet) regionale](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
    - **Rete virtuale**: selezionare la rete virtuale (VNET) quando si crea la macchina virtuale. Questo campo indica tutte le VNET associate alla sottoscrizione. Il gruppo di risorse della macchina virtuale è visualizzato tra parentesi. 
    - **Subnet**: se la VNET ha subnet, la prima subnet viene selezionata per impostazione predefinita. Se sono presenti altre subnet, selezionare quella desiderata.
    - **Account di archiviazione**: questa opzione apre l'elenco di account di archiviazione nella stessa posizione dell'insieme di credenziali dei servizi di ripristino. Quando si sceglie un account di archiviazione, è necessario scegliere uno degli account che condividono la stessa posizione dell'insieme di credenziali dei Servizi di ripristino. Gli account di archiviazione con ridondanza della zona non sono supportati. Se non sono disponibili account di archiviazione con la stessa posizione dell'insieme di credenziali dei Servizi di ripristino, è necessario crearne uno prima di iniziare l'operazione di ripristino. Il tipo di replica dell'account di archiviazione viene visualizzato tra parentesi. 
    
    > [AZURE.NOTE] È necessario selezionare una VNET quando si ripristina una VM basata su Azure Resource Manager. Una VNET è facoltativa per una macchina virtuale classica.

3. Nel pannello **Configurazione di ripristino** fare clic su **OK** per finalizzare la configurazione di ripristino.

4. Nel pannello **Ripristina** fare clic su **Ripristina** per attivare l'operazione di ripristino.

    ![Configurazione di ripristino completata](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## Tenere traccia dell'operazione di ripristino

Dopo l'attivazione dell'operazione di ripristino, il servizio di backup crea un processo per tenere traccia dell'operazione di ripristino. Il servizio di backup crea anche la notifica e la visualizza temporaneamente nell'area di notifica del portale. Se la notifica non viene visualizzata, è sempre possibile fare clic sull'icona Notifiche per visualizzare tutte le notifiche.

![Ripristino attivato](./media/backup-azure-arm-restore-vms/restore-notification.png)

Per visualizzare l'operazione durante l'elaborazione oppure per visualizzarla dopo il completamento, aprire l'elenco Processi di backup.

1. Scegliere **Sfoglia** dal menu Azure e nell'elenco dei servizi digitare **Servizi di ripristino**. L'elenco di servizi viene modificato in base a quanto digitato. Selezionare **Insiemi di credenziali dei servizi di ripristino** quando viene visualizzato.

    ![Aprire l'insieme di credenziali dei Servizi di ripristino](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Viene visualizzato l'elenco di insiemi di credenziali della sottoscrizione.

    ![Elenco di insiemi di credenziali dei Servizi di ripristino](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)

2. Dall'elenco selezionare l'insieme di credenziali associato alla VM ripristinata. Quando si fa clic sull'insieme di credenziali, viene aperto il dashboard corrispondente.

3. Nel riquadro **Processi di backup** del dashboard dell'insieme di credenziali fare clic su **Macchine virtuali di Azure** per visualizzare i processi associati all'insieme di credenziali.

    ![Dashboard dell'insieme di credenziali](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    Viene aperto il pannello **Processi di backup** e viene visualizzato l'elenco dei processi.

    ![Elenco di VM nell'insieme di credenziali](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)

## Ripristino delle macchine virtuali con configurazioni di rete speciali
È possibile eseguire il backup e ripristinare le macchine virtuali con le configurazioni di rete speciali seguenti. Queste configurazioni richiedono tuttavia alcune considerazioni speciali nel corso del processo di ripristino.

- Macchine virtuali nel servizio di bilanciamento del carico (interno ed esterno)
- Macchine virtuali con più indirizzi IP riservati
- Macchine virtuali con più NIC

>[AZURE.IMPORTANT] Quando si crea la configurazione di rete speciale per macchine virtuali, è necessario usare PowerShell per il ripristino da disco.

Per potere ricreare completamente le macchine virtuali dopo il ripristino su disco, seguire questa procedura:

1. Ripristinare i dischi da un insieme di credenziali dei servizi di ripristino tramite [PowerShell](../backup-azure-vms-automation.md#restore-an-azure-vm)

2. Creare la configurazione della macchina virtuale necessaria per il servizio di bilanciamento del carico/per più NIC/per più indirizzi IP riservati tramite i cmdlet di PowerShell e usarla per creare la macchina virtuale con la configurazione desiderata.
	- Creare una macchina virtuale nel servizio cloud con [bilanciamento del carico interno](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
	- Creare una macchina virtuale connessa al [servizio di bilanciamento del carico con connessione Internet](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/)
	- Creare una macchina virtuale con [più NIC](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
	- Creare una macchina virtuale con [più indirizzi IP riservati](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## Passaggi successivi
Dopo avere appreso come ripristinare le VM, è possibile vedere l'articolo sulla risoluzione dei problemi per ottenere informazioni sugli errori comuni con le macchine virtuali. Vedere anche l'articolo sulla gestione di attività con le macchine virtuali.

- [Risoluzione dei problemi](backup-azure-vms-troubleshoot.md#restore)
- [Gestire le macchine virtuali](backup-azure-manage-vms.md)

<!---HONumber=AcomDC_0615_2016-->