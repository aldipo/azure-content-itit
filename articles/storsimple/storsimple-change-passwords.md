<properties 
   pageTitle="Modificare le password di StorSimple | Microsoft Azure" 
   description="Descrive come utilizzare il servizio StorSimple Manager per modificare le password di StorSimple Snapshot Manager e di amministratore del dispositivo." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="03/24/2016"
   ms.author="alkohli"/>

# Utilizzare il servizio StorSimple Manager per modificare la password di StorSimple

## Panoramica 

La pagina **Configura**del portale di Azure classico contiene tutti i parametri del dispositivo che è possibile riconfigurare in un dispositivo StorSimple gestito da un servizio StorSimple Manager. In questa esercitazione viene illustrato come utilizzare la pagina**Configura** per modificare la password dell’amministratore del dispositivo o la password di StorSimple Snapshot Manager.

## Configurare la password dell’amministratore del dispositivo

Quando si utilizza l'interfaccia di Windows PowerShell per accedere al dispositivo StorSimple, è necessario immettere una password amministratore del dispositivo. Quando il primo dispositivo StorSimple è registrato con un servizio, la password predefinita per questa interfaccia è*Password1*. Per la protezione dei dati, è necessario modificare la password al termine del processo di registrazione. Non è possibile uscire dal processo di registrazione senza modificare la password. Per altre informazioni, vedere [Passaggio 3: Configurare e registrare il dispositivo tramite Windows PowerShell per StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

La password impostata la prima volta durante la registrazione attraverso l'interfaccia di Windows PowerShell può quindi essere modificata tramite il portale di Azure classico. Eseguire i passaggi seguenti per modificare la password di amministratore del dispositivo.

#### Per configurare la password dell’amministratore del dispositivo

1. Nel portale classico, fare clic su**Dispositivi**>**Configura** per il dispositivo.

2. Scorrere verso il basso fino alla sezione **Password amministratore del dispositivo** Specificare una password amministratore contenente dagli 8 ai 15 caratteri. La password deve contenere una combinazione di 3 o più lettere maiuscole, minuscole, numeri e caratteri speciali.

3. Confermare la password.

4. Fare clic su **Save** nella parte inferiore della pagina.

A questo punto, la password amministratore del dispositivo dovrebbe essere aggiornata. È possibile utilizzare la password modificata per accedere all'interfaccia di Windows PowerShell.

## Configurare la password di StorSimple Snapshot Manager

Il software di Gestione snapshot StorSimple si trova nell'host di Windows e consente agli amministratori di gestire i backup del dispositivo StorSimple sotto forma di snapshot locali e cloud.

Quando si configura un dispositivo in StorSimple Snapshot Manager, verrà richiesto di specificare l'indirizzo IP del dispositivo e la password per autenticare il dispositivo di archiviazione. Questa password viene configurata dapprima utilizzando l'interfaccia di Windows PowerShell. Per altre informazioni, vedere [Passaggio 3: Configurare e registrare il dispositivo tramite Windows PowerShell per StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

La password impostata la prima volta durante la registrazione attraverso l'interfaccia di Windows PowerShell può quindi essere modificata tramite il portale classico. Eseguire i passaggi seguenti per modificare la password di StorSimple Snapshot Manager.

#### Per configurare la password di StorSimple Snapshot Manager

1. Nel portale classico, fare clic su**Dispositivi**>**Configura** per il dispositivo.

2. Scorrere verso il basso fino alla sezione **StorSimple Snapshot Manager**. Immettere una password composta da 14 o 15 caratteri. Assicurarsi che la password contenga una combinazione di 3 o più lettere maiuscole, minuscole, numeri e caratteri speciali.

3. Confermare la password.

4. Fare clic su **Save** nella parte inferiore della pagina.

La password di StorSimple Snapshot Manager ora deve essere aggiornata.
 

## Passaggi successivi

- Ulteriori informazioni sulla [sicurezza di StorSimple](storsimple-security.md).

- [Ulteriori informazioni su come modificare la configurazione del dispositivo](storsimple-modify-device-config.md).

- Ulteriori informazioni sull’[utilizzo del servizio StorSimple Manager per amministrare il dispositivo StorSimple](storsimple-manager-service-administration.md).

<!---HONumber=AcomDC_0330_2016-->