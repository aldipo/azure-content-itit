<properties 
	pageTitle="Distribuire l'app per la logica da Visual Studio | Microsoft Azure" 
	description="Creare un progetto in Visual Studio per gestire l'app per la logica." 
	authors="stepsic-microsoft-com" 
	manager="erikre" 
	editor="" 
	services="app-service\logic" 
	documentationCenter=""/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/03/2016"
	ms.author="stepsic"/>
	
# Eseguire una distribuzione da Visual Studio

Anche se il [portale di Azure](https://portal.azure.com/) rappresenta un ottimo modo per progettare e gestire le app per la logica, è possibile distribuire l'app per la logica anche da Visual Studio. Questa scelta offre due funzionalità principali:

- Archiviare l'app per la logica insieme ad altre risorse nella soluzione in modo che possa contenere tutti gli aspetti dell'applicazione
- Controllare la definizione dell'app per la logica archiviata nel codice sorgente in modo da poter usare TFS o Git per tenere traccia delle relative revisioni 

Per eseguire la procedura seguente è necessario avere installato Azure SDK 2.7 o versione successiva. L'[SDK più recente per Visual Studio](https://azure.microsoft.com/downloads/) è disponibile qui.

## Creare un progetto

1. Andare al menu **File** e selezionare **Nuovo** > **Progetto**. In alternativa, è possibile selezionare **Aggiungi** e quindi **Nuovo progetto** per aggiungerlo a una soluzione esistente: ![File menu](./media/app-service-logic-deploy-from-vs/filemenu.png)

2. Nella finestra di dialogo **Cloud** selezionare **Gruppo di risorse di Azure**. Digitare un **Nome** e fare clic su **OK**. ![Add new project](./media/app-service-logic-deploy-from-vs/addnewproject.png)

3. È ora necessario selezionare **App per la logica** o **App per la logica e App per le API**. Se si seleziona **App per la logica** è necessario puntare delle API esistenti. Se si seleziona **App per la logica e App per le API** è anche possibile creare contemporaneamente un'app per le API nuova e vuota. In questo documento è stata selezionata App per la logica. ![Select Azure template](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

4. Dopo aver selezionato il **Modello** fare clic su **OK**.

Il progetto di app per la logica viene aggiunto alla soluzione. Il file di distribuzione verrà visualizzato in Esplora soluzioni: ![Distribuzione](./media/app-service-logic-deploy-from-vs/deployment.png)

## Configurazione dell'app per la logica

Dopo aver creato un progetto è possibile modificare la definizione dell'app per la logica all'interno di Visual Studio. Fare clic sul file JSON in Esplora soluzioni. Viene visualizzata una definizione di segnaposto che è possibile compilare con la logica dell'applicazione.

È consigliabile usare **parametri** in tutta la definizione. L'uso dei parametri è utile se si vuole effettuare la distribuzione in un ambiente di sviluppo e di produzione. In tal caso è necessario inserire tutta la configurazione specifica dell'ambiente nel file `*.parameters.json` e usare i parametri anziché le stringhe effettive.

Attualmente Visual Studio non include una finestra di progettazione JSON predefinita, quindi se si vuole usare un'interfaccia grafica, invece di usare JSON, è necessario usare il portale di Azure.

Se è stata creata in precedenza un'app per la logica all'interno del portale di Azure e si vuole ora archiviarla nel controllo del codice sorgente, esistono tre modi per eseguire questa operazione:

- Passare alla **visualizzazione Codice** nel portale e copiare la definizione.
- Usare l'[API REST](https://msdn.microsoft.com/library/azure/mt643787.aspx) delle app per la logica per ottenere la definizione.
- Usare [PowerShell di Azure Resource Manager](../powershell-azure-resource-manager.md), in particolare il [comando `Get-AzureResource`](https://msdn.microsoft.com/library/dn654579.aspx) per scaricare la definizione.

## Distribuzione dell'app per la logica

Infine, dopo aver configurato l'app è possibile distribuirla direttamente da Visual Studio in pochi passaggi.

1. Fare clic con il pulsante destro sul progetto in Esplora soluzioni e passare a **Distribuisci** > **Nuova distribuzione...** ![New deployment](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Verrà richiesto di accedere alla sottoscrizione di Azure.

3. A questo punto è necessario scegliere i dettagli del gruppo di risorse in cui si vuole distribuire l'app per la logica. ![Deploy to resource group](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Assicurarsi di selezionare il modello e i file dei parametri appropriati per il gruppo di risorse (ad esempio, se si effettua la distribuzione in un ambiente di produzione è opportuno scegliere il file dei parametri di produzione). 
4.  Selezionare il pulsante Distribuisci
5. Verrà chiesto di correggere gli eventuali errori rilevati. Ad esempio: ![Deploy to resource group](./media/app-service-logic-deploy-from-vs/deploytoresourcegrouperror.png)
 
    
6. Lo stato della distribuzione viene visualizzato nella finestra **Output**. Potrebbe essere necessario scegliere il **provisioning Azure**. ![Output](./media/app-service-logic-deploy-from-vs/output.png)

In futuro sarà possibile modificare l'app per la logica nel controllo del codice sorgente e usare Visual Studio per distribuire nuove versioni.

> [AZURE.NOTE] Se si modifica la definizione direttamente nel portale di Azure, alla successiva distribuzione da Visual Studio tali modifiche verranno sovrascritte.

> [AZURE.TIP] Se non si vuole usare Visual Studio, ma si desidera comunque utilizzare gli strumenti necessari per distribuire l'app per la logica dal controllo di origine, è possibile usare l'[API](https://msdn.microsoft.com/library/azure/dn948510.aspx) o [PowerShell](../powershell-azure-resource-manager.md) direttamente per automatizzare le distribuzioni.

<!---HONumber=AcomDC_0511_2016-->