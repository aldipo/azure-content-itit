<properties
	pageTitle="Creazione e caricamento di un file VHD Linux in Azure"
	description="Informazioni su come creare e caricare un disco rigido virtuale Azure (VHD) contenente un sistema operativo Linux."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="szarkos"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/09/2016"
	ms.author="szark"/>

# Informazioni per le distribuzioni non approvate #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Importante**: il contratto di servizio della piattaforma di Azure si applica alle macchine virtuali che utilizzano il sistema operativo Linux solo quando viene utilizzata una delle [distribuzioni supportate](virtual-machines-linux-endorsed-distros.md) di PowerShell. Tutte le distribuzioni di Linux disponibili nella raccolta immagini di Azure sono distribuzioni approvate con la configurazione richiesta.

- [Linux in Azure - Distribuzioni supportate](virtual-machines-linux-endorsed-distros.md)
- [Supporto delle immagini Linux in Microsoft Azure](http://support2.microsoft.com/kb/2941892)

Tutte le distribuzioni eseguite in Azure dovranno soddisfare numerosi prerequisiti per poter essere eseguite correttamente sulla piattaforma. Questo articolo non è in alcun modo esaustivo in quanto ogni distribuzione è diversa. È persino possibile che, anche nel caso in cui tutti i criteri seguenti vengano soddisfatti, sia comunque necessario modificare in modo significativo il sistema Linux per garantirne il funzionamento corretto sulla piattaforma.

Per questo motivo, è consigliabile iniziare, laddove possibile, con una delle [distribuzioni approvate di Linux su Azure](virtual-machines-linux-endorsed-distros.md). Gli articoli seguenti forniscono le istruzioni per preparare le varie distribuzioni Linux approvate che sono supportate in Azure:

- **[Distribuzioni basate su CentOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES e openSUSE](../virtual-machines-linux-create-upload-vhd-suse)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

La restante parte di questo articolo sarà incentrata sulle indicazioni generali per l'esecuzione della distribuzione Linux in Azure.


## Note generali sull'installazione di Linux ##

- Il formato VHDX non è supportato in Azure, solo nei **VHD fissi**. È possibile convertire il disco in formato VHD tramite la console di gestione di Hyper-V o il cmdlet convert-vhd. Se si usa VirtualBox, ciò significa che è stato selezionato **Dimensioni fisse** anziché il valore predefinito allocato in modo dinamico durante la creazione del disco.

- Durante l'installazione del sistema operativo Linux è consigliabile usare partizioni standard anziché LVM, che spesso è la scelta predefinita per numerose installazioni. In questo modo sarà possibile evitare conflitti di nome LVM con le VM clonate, in particolare se fosse necessario collegare un disco del sistema operativo a un'altra VM per la risoluzione dei problemi. Se si preferisce, su dischi di dati si può usare [LVM](virtual-machines-linux-configure-lvm.md) o [RAID](virtual-machines-linux-configure-raid.md).

- NUMA non è supportato per VM di dimensioni maggiori a causa di un bug presente nelle versioni del kernel di Linux inferiori a 2.6.37. Questo problema incide principalmente sulle distribuzioni che usano il kernel upstream Red Hat 2.6.32. L'installazione manuale dell'agente Linux di Azure (waagent) disabiliterà automaticamente NUMA nella configurazione GRUB per il kernel Linux.

- Non configurare una partizione swap nel disco del sistema operativo. L'agente Linux può essere configurato in modo da creare un file swap sul disco temporaneo delle risorse. Altre informazioni su questo argomento sono disponibili nei passaggi seguenti.

- Tutti i dischi rigidi virtuali devono avere dimensioni multiple di 1 MB.


### Installazione di Linux senza Hyper-V ###

In alcuni casi, i programmi di installazione di Linux potrebbero non includere i driver per Hyper-V nel disco RAM iniziale (initrd o initramfs), a meno che non rilevi di essere in esecuzione in un ambiente Hyper-V. Quando si usa un sistema di virtualizzazione diverso (ad esempio Virtualbox, KVM e così via) per preparare l'immagine Linux, potrebbe essere necessario ricreare initrd per assicurarsi che almeno i moduli kernel `hv_vmbus` e `hv_storvsc` siano disponibili sul disco RAM iniziale. Questo è un problema noto, almeno nei sistemi basati sulla distribuzione upstream di Red Hat.

Il meccanismo per ricreare l'immagine initrd o initramfs può variare a seconda della distribuzione. Per la procedura appropriata, consultare la documentazione della distribuzione o il supporto. Di seguito è riportato un esempio di come ricreare l'immagine initrd tramite l'utilità `mkinitrd`:

Innanzitutto, eseguire il backup dell'immagine initrd esistente:

	# cd /boot
	# sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Quindi, ricompilare initrd con i moduli kernel `hv_vmbus` e `hv_storvsc`:

	# sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### Ridimensionamento dei dischi rigidi virtuali ###

Le dimensioni virtuali delle immagini VHD su Azure devono essere allineate a 1 MB. Solitamente, i VHD creati usando Hyper-V sono già allineati correttamente. Se il disco rigido virtuale non è allineato correttamente, è possibile che venga visualizzato un messaggio di errore simile al seguente quando si tenta di creare un'*immagine* dal disco rigido virtuale:

	"The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Per risolvere questo problema, è possibile ridimensionare la macchina virtuale usando la console di gestione di Hyper-V o il cmdlet Powershell [Resize-VHD](http://technet.microsoft.com/library/hh848535.aspx). Se l'ambiente è diverso da Windows, si consiglia di usare qemu-img per convertire (se necessario) e ridimensionare il disco rigido virtuale:

> [AZURE.NOTE] Esiste un bug noto nelle versioni qemu-img > = 2.2.1 risultante in un disco rigido virtuale non formattato correttamente. Il problema verrà risolto in una versione futura di qemu-img. Al momento è consigliabile utilizzare la versione 2.2.0 o inferiore di qemu-img. Riferimento: https://bugs.launchpad.net/qemu/+bug/1490611


 1. Il ridimensionamento diretto del disco rigido virtuale con strumenti quali `qemu-img` o `vbox-manage` può determinare un disco rigido virtuale che non può essere avviato. Pertanto, è consigliabile convertire prima il VHD in un'immagine disco RAW. Se l'immagine della VM è già stata creata come immagine disco RAW (questa è l'impostazione predefinita per alcuni hypervisor, ad esempio KVM), è possibile saltare questo passaggio:

		# qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Calcolare le dimensioni richieste dell'immagine disco per assicurare che le dimensioni virtuali siano allineate a 1 MB. A questo scopo, può essere utile il seguente script bash shell. Lo script usa "`qemu-img info`" per determinare le dimensioni virtuali dell'immagine disco, quindi calcola le dimensioni al successivo 1 MB:

		rawdisk="MyLinuxVM.raw"
		vhddisk="MyLinuxVM.vhd"

		MB=$((1024*1024))
		size=$(qemu-img info -f raw --output json "$rawdisk" | \
		       gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

		rounded_size=$((($size/$MB + 1)*$MB))
		echo "Rounded Size = $rounded_size"

 3. Ridimensionare il disco RAW usando $rounded\_size, come impostato nello script precedente:

		# qemu-img resize MyLinuxVM.raw $rounded_size

 4. A questo punto, convertire nuovamente il disco RAW in un disco rigido virtuale a dimensione fissa:

		# qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## Requisiti del kernel Linux ##

I driver di Linux Integration Services (LIS) per Hyper-V e Azure vengono forniti direttamente nel kernel Linux upstream. Per molte distribuzioni che includono una versione recente del kernel Linux (cioè 3.x) questi driver sono già disponibili; in alternativa, fornire versioni backport dei driver con i relativi kernel. Questi driver vengono aggiornati costantemente nel kernel upstream con nuove correzioni e funzionalità, pertanto, quando è possibile, è consigliabile eseguire una [distribuzione approvata](virtual-machines-linux-endorsed-distros.md), che include tali correzioni e aggiornamenti.

Se si esegue una variante di Red Hat Enterprise Linux versioni **6.0-6.3**, è necessario installare i driver LIS più recenti per Hyper-V. I driver sono disponibili [a questo indirizzo](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Per quanto riguarda RHEL **6.4 e versioni successive** (e distribuzioni derivate) i driver LIS sono già inclusi nel kernel e pertanto non sono necessari pacchetti di installazione aggiuntivi per eseguire tali sistemi su Azure.

Se è necessario un kernel personalizzato, è consigliabile usare una versione più recente del kernel (ad esempio **3.8 e versioni successive**). Per le distribuzioni o i fornitori che gestiscono il proprio kernel, è consigliabile eseguire il backport ai driver LIS dal kernel upstream al kernel personalizzato. Anche se si esegue già una versione relativamente recente del kernel, è generalmente consigliabile tenere traccia di eventuali correzioni upstream nei driver LIS ed eseguirne il backport in base alle esigenze. La posizione dei file di origine dei driver LIS è disponibile nel file [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) nell'albero di origine del kernel Linux:

	F:	arch/x86/include/asm/mshyperv.h
	F:	arch/x86/include/uapi/asm/hyperv.h
	F:	arch/x86/kernel/cpu/mshyperv.c
	F:	drivers/hid/hid-hyperv.c
	F:	drivers/hv/
	F:	drivers/input/serio/hyperv-keyboard.c
	F:	drivers/net/hyperv/
	F:	drivers/scsi/storvsc_drv.c
	F:	drivers/video/fbdev/hyperv_fb.c
	F:	include/linux/hyperv.h
	F:	tools/hv/

Come minimo, l'assenza delle seguenti patch causa problemi in Azure e quindi tali patch devono essere incluse nel kernel. L'elenco non è esaustivo o completo per tutte le distribuzioni:

- [ata\_piix: defer disks to the Hyper-V drivers by default](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: Account for in-transit packets in the RESET path](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: avoid usage of WRITE\_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: Disable WRITE SAME for RAID and virtual host adapter drivers](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: NULL pointer dereference fix](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: ring buffer failures may result in I/O freeze](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)


## Agente Linux di Azure ##

L'[agente Linux di Azure](virtual-machines-linux-agent-user-guide.md) (waagent) è necessario per eseguire correttamente il provisioning di una macchina virtuale Linux in Azure. È possibile scaricare la versione più recente, inviare problemi o inviare richieste pull all'[archivio GitHub dell'agente Linux](https://github.com/Azure/WALinuxAgent).

- L'agente Linux viene rilasciato con la licenza Apache 2.0. Molte distribuzioni forniscono già pacchetti RPM o Debian per l'agente, che pertanto può, in alcuni casi, essere installato e aggiornato con un livello minimo di impegno.

- L'agente Linux di Azure richiede Python 2.6 o versioni successive.

- Inoltre, l'agente richiede il modulo python-pyasn1, che nella maggior parte delle distribuzioni viene fornito come pacchetto separato installabile.

- In alcuni casi l'agente Linux di Azure potrebbe non essere compatibile con NetworkManager. Molti dei pacchetti RPM/Debian forniti dalle distribuzioni configurano NetworkManager come in conflitto con il pacchetto waagent, pertanto disinstalleranno NetworkManager quando si installa il pacchetto dell'agente Linux.


## Requisiti generali di sistema relativi a Linux ##

- Modificare la riga di avvio del kernel in GRUB o GRUB2 per includere i parametri seguenti. In questo modo si garantisce inoltre che tutti i messaggi della console vengano inviati alla prima porta seriale, agevolando così il supporto di Azure nella risoluzione dei problemi di debug:

		console=ttyS0 earlyprintk=ttyS0 rootdelay=300

	In questo modo si garantisce inoltre che tutti i messaggi della console vengano inviati alla prima porta seriale, agevolando così il supporto di Azure nella risoluzione dei problemi di debug.

	Inoltre, è consigliabile *rimuovere* i parametri seguenti, se esistenti:

		rhgb quiet crashkernel=auto

	L'avvio grafico e l'avvio silenzioso non sono utili in un ambiente cloud se tutti i log devono essere inviati alla porta seriale.

	L'opzione `crashkernel` può essere configurata, ma si tenga presente che questo parametro ridurrà la quantità di memoria disponibile nella macchina virtuale di almeno 128 MB, il che potrebbe causare problemi con le macchine virtuali di dimensioni inferiori.

- Installazione dell'agente Linux di Azure

	L'agente Linux di Azure è necessario per eseguire il provisioning di un'immagine Linux su Azure. Molte distribuzioni forniscono l'agente come pacchetto RPM o Debian (il pacchetto è in genere denominato "WALinuxAgent" or "walinuxagent"). È inoltre possibile installare l'agente manualmente seguendo la procedura indicata nella [Guida all'agente Linux](virtual-machines-linux-agent-user-guide.md).

- Verificare che il server SSH sia installato e configurato per l'esecuzione all'avvio. Questo è in genere il valore predefinito.

- Non creare l'area di swap sul disco del sistema operativo.

	L'agente Linux di Azure può configurare automaticamente l'area di swap utilizzando il disco risorse locale collegato alla VM dopo il provisioning in Azure. Si noti che il disco risorse locale è un disco *temporaneo* e potrebbe essere svuotato in seguito al deprovisioning della macchina virtuale. Dopo aver installato l'agente Linux di Azure come illustrato nel passaggio precedente, modificare i parametri seguenti in /etc/waagent.conf in modo appropriato:

		ResourceDisk.Format=y
		ResourceDisk.Filesystem=ext4
		ResourceDisk.MountPoint=/mnt/resource
		ResourceDisk.EnableSwap=y
		ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- In "/etc/sudoers" è necessario rimuovere o impostare come commento le righe seguenti, se esistenti:

		Defaults targetpw
		ALL    ALL=(ALL) ALL

- Infine, eseguire i comandi seguenti per effettuare il deprovisioning della macchina virtuale:

		# sudo waagent -force -deprovision
		# export HISTSIZE=0
		# logout

	>[AZURE.NOTE] Dopo l'esecuzione del comando "waagent -force -deprovision", su Virtualbox potrebbe essere visualizzato l'errore seguente: `[Errno 5] Input/output error`. Questo messaggio di errore non è critico e può essere ignorato.

- Arrestare la macchina virtuale e caricare il VHD in Azure.

<!---HONumber=AcomDC_0518_2016-->