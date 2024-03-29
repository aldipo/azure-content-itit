<properties
	pageTitle="Creare una macchina virtuale che esegue MySQL | Microsoft Azure"
	description="Creare una macchina virtuale di Azure che esegue Windows Server 2012 R2 e il database MySQL usando il modello di distribuzione classica."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/15/2016"
	ms.author="cynthn"/>


# Installare MySQL in una macchina virtuale creata con il modello di distribuzione classica che esegue Windows Server 2012 R2.

[MySQL](http://www.mysql.com) è un database SQL open source molto diffuso. Questa esercitazione illustra come installare ed eseguire la versione community di MySQL 5.6.23 come MySQL Server in una macchina virtuale che esegue Windows Server 2012 R2. Per istruzioni sull'installazione di MySQL su Linux, fare riferimento a: [Come installare MySQL in Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## Creare una macchina virtuale che esegue Windows Server 2012 R2

Se non si ha ancora una VM che esegue Windows Server 2012 R2, è possibile crearla usando questa [esercitazione](virtual-machines-windows-classic-tutorial.md).

## Collegamento di un disco dati

Dopo aver creato la macchina virtuale, facoltativamente è possibile collegare un disco dati aggiuntivo. L'operazione è consigliata per i carichi di lavoro di produzione e per evitare l'esaurimento dello spazio nell'unità del sistema operativo (C:), che include il sistema operativo.

Vedere [Come collegare un disco dati a una macchina virtuale Windows](virtual-machines-windows-classic-attach-disk.md) e seguire le istruzioni per collegare un disco vuoto. Impostare le preferenze della cache dell'host su **Nessuno** o **Sola lettura**.

## Accesso alla macchina virtuale

Quindi, [collegarsi alla macchina virtuale](virtual-machines-windows-classic-connect-logon.md) in modo da poter installare MySQL.

##Installare ed eseguire MySQL Community Server nella macchina virtuale

Per installare, configurare ed eseguire la versione Community di MySQL Server, seguire questa procedura:

> [AZURE.NOTE] La procedura si riferisce alla versione Community 5.6.23.0 di MySQL e a Windows Server 2012 R2. Con altre versioni di MySQL o Windows Server, l'esperienza potrebbe essere diversa.

1.	Dopo avere eseguito la connessione alla macchina virtuale tramite Desktop remoto, fare clic su **Internet Explorer** nella schermata Start.
2.	Selezionare il pulsante **Strumenti** nell'angolo superiore destro (l'icona della ruota dentata) e quindi fare clic su **Opzioni Internet**. Fare clic sulla scheda **Sicurezza**, quindi sull'icona **Siti attendibili** e infine sul pulsante **Siti**. Aggiungere http://*.mysql.com all'elenco dei siti attendibili. Fare clic su **Chiudi**, e quindi su **OK**.
3.	Nella barra degli indirizzi di Internet Explorer, digitare http://dev.mysql.com/downloads/mysql/.
4.	Usare il sito di MySQL per individuare e scaricare l'ultima versione del programma di installazione di MySQL per Windows. Quando si sceglie il programma di installazione di MySQL, scaricare la versione che contiene il set di file completo (ad esempio il file mysql-installer-community-5.6.23.0.msi da 282,4 MB) e salvare il programma di installazione.
5.	Al termine del download del programma di installazione, fare clic su **Esegui** per avviare l'installazione.
6.	Nella pagina **License Agreement** accettare il contratto di licenza e fare clic su **Next**.
7.	Nella pagina **Choosing a Setup Type** fare clic sul tipo di installazione desiderata, quindi fare clic su **Next**. Nei passaggi seguenti si presuppone che sia stato selezionato il tipo di installazione **Server only**.
8.	Nella pagina **Installation** fare clic su **Execute**. Al termine dell'installazione, fare clic su **Next**.
9.	Nella pagina **Product Configuration** fare clic su **Next**.
10.	Nella pagina **Type and Networking** specificare il tipo di configurazione e le opzioni di connettività desiderate, inclusa se necessario la porta TCP. Fare clic su **Show Advanced Options** e quindi su **Next**.

	![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11.	Nella pagina **Accounts and Roles** specificare una password radice complessa per MySQL. Aggiungere altri account utente di MySQL in base alle esigenze, quindi fare clic su **Next**.

	![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12.	Nella pagina **Windows Service** specificare le modifiche alle impostazioni predefinite per l'esecuzione di MySQL Server come servizio di Windows in base alle esigenze e quindi fare clic su **Next**.

	![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13.	Nella pagina **Advanced Options** specificare le modifiche alle opzioni di accesso in base alle esigenze e quindi fare clic su **Next**.

	![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14.	Nella pagina **Apply Server Configuration** fare clic su **Execute**. Dopo aver completato la procedura di configurazione, fare clic su **Finish**.
15.	Nella pagina **Product Configuration** fare clic su **Next**.
16.	Nella pagina **Installation Complete** fare clic su **Copy Log to Clipboard** se si desidera esaminare il log in un secondo momento, quindi fare clic su **Finish**.
17.	Dalla schermata Start digitare **mysql** e quindi fare clic su **MySQL 5.6 Command Line Client**.
18.	Immettere la password radice specificata nel passaggio 11. Verrà visualizzato un prompt in cui è possibile inviare comandi per la configurazione di MySQL. Per informazioni dettagliate sui comandi e la sintassi, vedere i [manuali di riferimento di MySQL](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

	![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19.	È anche possibile configurare impostazioni di configurazione predefinite per il server, ad esempio le unità e le directory dati e di base, specificando le voci nel file C:\\Programmi (x86)\\MySQL\\MySQL Server 5.6\\my-default.ini. Per altre informazioni, vedere [l'articolo relativo alle impostazioni di configurazione predefinite di MySQL 5.1.2 Server](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## Configurare gli endpoint

Se si desidera rendere disponibile il servizio MySQL Server per i computer client di MySQL su Internet è necessario configurare un endpoint per la porta TCP su cui è in ascolto il servizio MySQL Server e creare una nuova regola di Windows Firewall. Normalmente è la porta TCP 3306, a meno che non sia stata specificata una porta diversa nella pagina **Type and Networking** (passaggio 10 della procedura precedente).


> [AZURE.NOTE] È opportuno considerare con attenzione le implicazioni di sicurezza di questa operazione, poiché in questo modo il servizio MySQL Server diventa disponibile per tutti i computer su Internet. È possibile definire il set di indirizzi IP di origine che sono autorizzati a usare l'endpoint con un elenco di controllo di accesso (ACL). Per altre informazioni, vedere la pagina [Come configurare gli endpoint a una macchina virtuale](virtual-machines-windows-classic-setup-endpoints.md).


Per configurare un endpoint per il servizio MySQL Server:

1.	Nel portale di Azure classico fare clic su **Macchine virtuali**, quindi sul nome della macchina virtuale MySQL e infine su **Endpoint**.
2.	Nella barra dei comandi fare clic su **Aggiungi**.
3.	Nella pagina **Aggiungi un endpoint a una macchina virtuale** fare clic sulla freccia destra.
4.	Se si sta usando la porta TCP predefinita di MySQL numero 3306, fare clic su **MySQL** in **Nome** e quindi fare clic sul segno di spunta.
5.	Se si usa una porta TCP diversa, digitare un nome univoco in **Nome**. Selezionare **TCP** nel protocollo, digitare il numero della porta sia in **Porta pubblica** che in **Porta privata**, quindi fare clic sul segno di spunta.

## Aggiungere una regola di Windows Firewall per consentire il traffico di MySQL

Per aggiungere una regola di Windows Firewall che consenta a MySQL di ricevere dati da Internet, è necessario eseguire questo comando in un prompt dei comandi di Windows PowerShell con privilegi elevati nella macchina virtuale del server MySQL.

	New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


	
## Testare la connessione remota


Per testare la connessione remota al servizio MySQL Server in esecuzione nella macchina virtuale di Azure, è necessario innanzitutto determinare il nome DNS corrispondente al servizio cloud che contiene la macchina virtuale che esegue MySQL Server.

1.	Nel portale di Azure classico fare clic su **Macchine virtuali** quindi sul nome della macchina virtuale MySQL e infine su **Dashboard**.
2.	Dal dashboard della macchina virtuale, prendere nota del valore **Nome DNS** sotto la sezione **Riepilogo rapido**. Di seguito è fornito un esempio:

	![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.	Da un computer locale che esegue MySQL o il client MySQL, eseguire questo comando per accedere come utente MySQL:

		mysql -u <yourMysqlUsername> -p -h <yourDNSname>

	Ad esempio, per il nome utente MySQL dbadmin3 e il nome DNS testmysql.cloudapp.net per la macchina virtuale, il comando è il seguente:

		mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## Passaggi successivi

Per altre informazioni sull'esecuzione di MySQL, vedere la [documentazione di MySQL](http://dev.mysql.com/doc/).

<!---HONumber=AcomDC_0629_2016-->