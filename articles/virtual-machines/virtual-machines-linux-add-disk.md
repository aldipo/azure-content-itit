<properties
	pageTitle="Aggiungere un disco a una VM Linux | Microsoft Azure"
	description="Informazioni su come aggiungere un disco persistente alla VM Linux"
	keywords="macchina virtuale Linux, aggiungere un disco di risorse"
	services="virtual-machines-linux"
	documentationCenter=""
	authors="rickstercdn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager" />

<tags
	ms.service="virtual-machines-linux"
	ms.topic="article"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.date="04/29/2016"
	ms.author="rclaus"/>

# Aggiungere un disco a una VM Linux

Questo articolo illustra come collegare un disco persistente alla VM per poter mantenere i dati, anche se si effettua di nuovo il provisioning della VM per manutenzione o ridimensionamento. Per aggiungere un disco, è necessaria l'[interfaccia della riga di comando di Azure](../xplat-cli-install.md) configurata in modalità Azure Resource Manager (`azure config mode arm`).

## Comandi rapidi

Negli esempi di comandi seguenti sostituire i valori compresi tra &lt; e &gt; con i valori dell'ambiente locale.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## Collegare un disco

Il collegamento di un nuovo disco è un’operazione rapida. È sufficiente digitare `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` per creare e collegare un nuovo disco GB per la macchina virtuale. Se non si indica in modo esplicito un account di archiviazione, tutti i dischi creati vengono assegnati allo stesso account di archiviazione in cui si trova il disco del sistema operativo. Dovrebbe essere visualizzata una schermata analoga alla seguente:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Output

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## Connettersi alla VM Linux per montare il nuovo disco

> [AZURE.NOTE] In questo argomento si esegue la connessione a una macchina virtuale usando nomi utente e password; per usare coppie di chiavi pubbliche e private per comunicare con la macchina virtuale, vedere [Come usare SSH con Linux in Azure](virtual-machines-linux-ssh-from-linux.md). È possibile modificare la connettività **SSH** delle macchine virtuali create con il comando `azure vm quick-create` usando il comando `azure vm reset-access` per reimpostare completamente l'accesso **SSH**, aggiungere o rimuovere utenti o aggiungere file di chiave pubblica per proteggere l'accesso.

Sarà necessario SSH nella VM di Azure per partizionare, formattare e montare il nuovo disco in modo che la VM Linux possa usarlo. Se non si ha familiarità con la connessione a **ssh**, il comando assume il formato `ssh <username>@<FQDNofAzureVM> -p <the ssh port>` e ha l'aspetto seguente:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Output

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Ora che si è connessi alla macchina virtuale, si è pronti per collegare un disco. Trovare prima il disco usando `dmesg | grep SCSI`. Il metodo usato per trovare il nuovo disco può variare. In questo caso, avrà un aspetto simile al seguente:

```bash
dmesg | grep SCSI
```

Output

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

e, nel caso di questo argomento, il disco `sdc` è il disco desiderato. Eseguire ora la partizione del disco con `sudo fdisk /dev/sdc`. Presupponendo che in questo caso il disco sia `sdc`, renderlo un disco principale nella partizione 1 e accettare le altre impostazioni predefinite.

```bash
sudo fdisk /dev/sdc
```

Output

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Creare la partizione digitando `p` al prompt:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

E scrivere un file system nella partizione usando il comando **mkfs**, specificando il tipo di file system e il nome del dispositivo. In questo argomento, vengono usati `ext4` e `/dev/sdc1` riportati sopra:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Output

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Creare ora una directory per montare il nuovo file system usando `mkdir`:

```bash
sudo mkdir /datadrive
```

E montare la directory usando `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Il disco dati è ora pronto per l'uso come `/datadrive`.

```bash
ls
```

Output

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

> [AZURE.NOTE] È inoltre possibile connettersi alla macchina virtuale Linux usando una chiave SSH per l'identificazione. Per informazioni dettagliate, vedere [Come usare SSH con Linux in Azure](virtual-machines-linux-ssh-from-linux.md).

## Passaggi successivi

- Tenere presente che il nuovo disco non è in genere disponibile sulla macchina virtuale se la macchina virtuale viene riavviata, a meno che non si scrivano queste informazioni nel file [fstab](http://en.wikipedia.org/wiki/Fstab).
- Esaminare le raccomandazioni per [ottimizzare le prestazioni della macchina virtuale Linux](virtual-machines-linux-optimization.md) per garantire che la VM Linux sia configurata correttamente.
- Espandere la capacità di archiviazione aggiungendo altri dischi e [configurare RAID](virtual-machines-linux-configure-raid.md) per migliorare le prestazioni.

<!---HONumber=AcomDC_0504_2016-->