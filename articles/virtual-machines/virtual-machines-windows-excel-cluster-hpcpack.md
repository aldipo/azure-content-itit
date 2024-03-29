<properties
 pageTitle="Cluster HPC Pack per Excel e SOA | Microsoft Azure"
 description="Introduzione all'esecuzione di carichi di lavoro Excel e SOA su larga scala in un cluster HPC Pack in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="05/25/2016"
 ms.author="danlep"/>

# Introduzione all'uso di un cluster HPC Pack in Azure per l'esecuzione di carichi di lavoro di Excel e SOA

In questo articolo viene illustrato come distribuire un cluster HPC Pack nei servizi di infrastruttura di Azure (IaaS) mediante un modello di Guida introduttiva di Azure o uno script di distribuzione di Azure PowerShell. Usare le immagini di macchina virtuale di Azure Marketplace progettate per l'esecuzione di carichi di lavoro di architettura orientata ai servizi (SOA) o Microsoft Excel con HPC Pack. È possibile usare il cluster per eseguire semplici servizi Excel HPC e SOA da un computer client locale. I servizi Excel HPC includono l'offload di cartelle di lavoro di Excel e funzioni definite dall'utente di Excel o UDF.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Nel seguente diagramma viene illustrato il cluster HPC Pack che verrà creato, a livello elevato.

![Cluster HPC con nodi che eseguono carichi di lavoro di Excel][scenario]

## Prerequisiti

*   **Computer client**: è necessario un computer client basato su Windows per eseguire lo script di distribuzione del cluster di Azure PowerShell (se si sceglie tale metodo di distribuzione) e l'invio di processi di Excel e SOA di esempio al cluster.

*   **Sottoscrizione di Azure**: se non è disponibile una sottoscrizione di Azure, è possibile creare un [account gratuito](https://azure.microsoft.com/pricing/free-trial/) in pochi minuti.

*   **Quota di core**: potrebbe essere necessario aumentare la quota di core, soprattutto se si distribuiscono più nodi del cluster con dimensioni delle macchine virtuali multicore. Se si usa un modello di Guida introduttiva di Azure, tenere presente che la quota di core in Gestione risorse è indicata per area di Azure, quindi potrebbe essere necessario aumentare la quota in un'area specifica. Vedere [Sottoscrizione di Azure e limiti dei servizi, quote e vincoli](../azure-subscription-service-limits.md). Per aumentare una quota, è possibile [aprire una richiesta di assistenza clienti online](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) senza alcun addebito.

*   **Licenza di Microsoft Office**: se si distribuiscono nodi di calcolo usando un'immagine di macchina virtuale di HPC Pack di Marketplace con Microsoft Excel, nei nodi di calcolo viene installata una versione di valutazione di Microsoft Excel Professional Plus 2013 di 30 giorni. Al termine del periodo di valutazione, per continuare a eseguire i carichi di lavoro è necessario attivare Excel fornendo una licenza valida di Microsoft Office. Vedere [Attivazione di Excel](#excel-activation) più avanti in questo articolo.


## Passaggio 1. Configurazione di un cluster HPC Pack in Azure

Verranno mostrati due metodi per configurare il cluster: nel primo, vengono usati un modello di Guida introduttiva di Azure e il portale di Azure e nel secondo, viene usato uno script di distribuzione di Azure PowerShell.


### Uso di un modello di Guida introduttiva
Usare un modello di Guida introduttiva di Azure per distribuire con rapidità e facilità un cluster HPC Pack nel portale di Azure. Aprendo il modello nel portale di anteprima, si ottiene un'interfaccia utente semplice in cui è possibile immettere le impostazioni per il cluster. Di seguito sono riportati i passaggi necessari.

>[AZURE.TIP]È possibile usare un [modello di Azure Markeplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) che crea un cluster simile, specifico per i carichi di lavoro di Excel. La procedura è leggermente diversa da quella che segue.

1.  Visitare la [pagina del modello per la creazione del cluster HPC su GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Se lo si desidera, esaminare le informazioni sul modello e il codice sorgente.

2.  Fare clic su **Distribuisci in Azure** per avviare una distribuzione con il modello nel portale di Azure.

    ![Modello di distribuzione in Azure][github]

3.  Nel portale, attenersi alla procedura seguente per immettere i parametri del modello di cluster HPC.

    a. Nella pagina **Parametri**, immettere i valori per i parametri del modello. (Per visualizzare informazioni della Guida, fare clic sull'icona accanto a ogni impostazione). Nella seguente schermata vengono visualizzati valori di esempio. In questo esempio viene creato un nuovo cluster HPC Pack denominato *hpc01* nel dominio *hpc.local* composto da un nodo head e 2 nodi di calcolo. I nodi di calcolo verranno creati da un'immagine di VM di HPC Pack che include Microsoft Excel.

    ![Immettere i parametri][parameters]

    >[AZURE.NOTE]La VM del nodo head verrà creata automaticamente [dall'immagine più recente del Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) di HPC Pack 2012 R2 su Windows Server 2012 R2. Attualmente l'immagine è basata su HPC Pack 2012 R2 Update 3.
    >
    >Le VM del nodo di calcolo verranno create dall'immagine più recente della famiglia di nodi di calcolo selezionata. Selezionare l'opzione **ComputeNodeWithExcel** per l'immagine del nodo di calcolo di HPC Pack più recente che include una versione di valutazione di Microsoft Excel Professional Plus 2013. Se si desidera distribuire un cluster per sessioni SOA generiche o per l'offload di UDF di Excel, scegliere l'opzione **ComputeNode** (senza Excel installato).

    b. Selezionare la sottoscrizione.

    c. Creare un nuovo gruppo di risorse per il cluster, ad esempio*hpc01RG*.

    d. Scegliere un percorso per il gruppo di risorse, ad esempio gli Stati Uniti centrali.

    e. Nella pagina **Note legali** esaminare le condizioni. Se si accettano le condizioni, fare clic su **Crea**. Quindi, dopo aver impostato i valori per il modello, fare clic su **Crea**.

4.  Al termine della distribuzione (in genere richiede circa 30 minuti), esportare il file del certificato del cluster dal nodo head del cluster. In un passaggio successivo questo certificato pubblico verrà importato nel computer client per fornire l'autenticazione sul lato server per l'associazione HTTP protetta.

    a. Connettersi al nodo head da Desktop remoto nel portale di Azure.

     ![Connettersi al nodo head][connect]

    b. Seguire le procedure standard in usare Gestione certificati per esportare il certificato del nodo head (situato in Cert: \\LocalMachine\\My) senza la chiave privata. In questo esempio esportare *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Esportare il certificato][cert]

### Uso dello script di distribuzione di HPC Pack IaaS

Lo script di distribuzione di HPC Pack IaaS offre un altro modo versatile per distribuire un cluster HPC Pack. Esso crea un cluster nel modello di distribuzione classica, mentre il modello usa il modello di distribuzione di Azure Resource Manager. Inoltre, lo script è compatibile con una sottoscrizione nel servizio Azure globale o Azure Cina.

**Ulteriori prerequisiti**

* **Azure PowerShell**: [installare e configurare Azure PowerShell](../powershell-install-configure.md) (versione 0.8.10 o versione successiva) nel computer client.

* **Script di distribuzione di HPC Pack IaaS**: scaricare e decomprimere la versione più recente dello script dal [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Controllare la versione dello script eseguendolo `New-HPCIaaSCluster.ps1 –Version`. Questo articolo si basa sulla versione dello script 4.5.0 o versione successiva.

**Creazione del file di configurazione**

 Lo script di distribuzione di HPC Pack IaaS usa come input un file di configurazione XML che descrive l'infrastruttura del cluster HPC. Per distribuire un cluster costituito da un nodo head e 18 nodi di calcolo creati dall'immagine del nodo di calcolo che include Microsoft Excel, sostituire i valori per il proprio ambiente nel file di configurazione di esempio riportato di seguito. Per altre informazioni sul file di configurazione, vedere il file Manual.rtf nella cartella dello script e l'articolo relativo alla [creazione di un cluster HPC con lo script di distribuzione di HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Note sul file di configurazione**

* Il valore di **VMName** del nodo head **DEVE** essere identico al valore di **ServiceName**. In caso contrario non è possibile eseguire i processi SOA.

* Assicurarsi di specificare **EnableWebPortal** per consentire la generazione e l'esportazione del certificato del nodo head.

* Il file specifica uno script PostConfig.ps1 di post-configurazione di PowerShell per configurare determinate impostazioni nel nodo head, ad esempio la stringa di connessione di archiviazione di Azure, la rimozione del ruolo del nodo di calcolo dal nodo head e l'impostazione di tutti i nodi in linea quando vengono distribuiti. Di seguito è riportato uno script di esempio.

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Esecuzione dello script**

1.  Aprire la console di PowerShell nel computer client come amministratore.

2.  Passare alla cartella dello script (E:\\IaaSClusterScript in questo esempio).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Eseguire il comando seguente per distribuire il cluster HPC Pack. In questo esempio si presuppone che il file di configurazione si trovi in E:\\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Per qualche istante, viene eseguito lo script di distribuzione di HPC Pack. Con l'esecuzione dello script, il certificato del cluster viene esportato, scaricato e salvato nella cartella Documenti dell'utente corrente sul computer client. Lo script genera un messaggio simile al seguente. In un passaggio successivo, il certificato verrà importato nell'archivio certificati appropriato.
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## Passaggio 2. Offload di cartelle di lavoro di Excel ed esecuzione di funzioni definite dall'utente da un client locale

### Attivazione di Excel

Quando si usa l'immagine di VM ComputeNodeWithExcel per carichi di lavoro di produzione, per attivare Excel per i nodi di calcolo è necessario disporre di un codice di licenza di Microsoft Office valido. In caso contrario, la versione di valutazione di Excel scade entro 30 giorni e l'esecuzione delle cartelle di lavoro di Excel restituisce costantemente l'errore COMException (0x800AC472). In questo caso, accedere al nodo head e usare clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` per tutti i nodi di calcolo di Excel tramite Gestione cluster HPC.
    
Questa operazione ripristina il periodo di valutazione di Excel per altri 30 giorni. Tale operazione può essere eseguita al massimo due volte. Successivamente, sarà necessario fornire un codice di licenza di Office valido.

Office Professional Plus 2013 installato in questa immagine di VM è un'edizione multilicenza con un codice GVLK (Generic Volume License Key) attivabile tramite Servizio di gestione delle chiavi (KMS, Key Management Service)/AD-BA (Active Directory-Based Activation, attivazione basata su Active Directory) oppure tramite MAK (Multiple Activation Key, codice ad attivazione multipla). Per KMS/AD-BA usare un server KMS esistente oppure configurarne uno nuovo, che può trovarsi nel nodo head, mediante Microsoft Office 2013 Volume License Pack. Quindi, attivare il codice Product Key host KMS tramite Internet o telefono. Quindi usare clusrun `ospp.vbs` per impostare il server e la porta KMS e attivare Office per tutti nodi di calcolo di Excel. Per usare MAK, usare prima clusrun `ospp.vbs` per immettere il codice e quindi attivare tutti i nodi di calcolo di Excel tramite Internet o telefono.

>[AZURE.NOTE]Con questa immagine di VM non è possibile usare codici Product Key per attivazione singola di Office Professional Plus 2013. Se si dispone di codici e supporti di installazione validi di edizioni di Office o Excel diverse da questa edizione multilicenza di Office Professional Plus 2013, è anche possibile disinstallare questa edizione e installare l'edizione disponibile. Il nodo di calcolo di Excel reinstallato può essere acquisito come immagine di VM personalizzata da usare in una distribuzione su vasta scala.

### Offload di cartelle di lavoro di Excel

Per eseguire l'offload di una cartella di lavoro di Excel da eseguire nel cluster HPC Pack in Azure, attenersi alla procedura riportata di seguito. A tale scopo, è necessario disporre di Excel 2010 o 2013 già installato nel computer client.

1. Usare uno dei metodi illustrati nel passaggio 1 per distribuire un cluster HPC Pack con l'immagine del nodo di calcolo di Excel. Ottenere il file del certificato del cluster (con estensione cer) e nome utente e password del cluster.

2. Nel computer client, importare il certificato del cluster in Cert: \\CurrentUser\\Root.

3. Verificare che Excel sia installato. Creare un file Excel.exe.config con i seguenti contenuti nella stessa cartella di Excel.exe del computer client. In questo modo il componente aggiuntivo di HPC Pack 2012 R2 Excel COM verrà caricato correttamente.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.	Scaricare la versione completa di [installazione di HPC Pack 2012 R2 Update 3](http://www.microsoft.com/download/details.aspx?id=49922) e installare il client di HPC Pack oppure scaricare e installare le [utilità client di HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923) e il file ridistribuibile Visual C++ 2010 appropriato per il computer ([x64](http://www.microsoft.com/download/details.aspx?id=14632)[x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.	In questo esempio viene usata una cartella di lavoro di esempio di Excel denominata ConvertiblePricing\_Complete.xlsb, disponibile per il download [qui](https://www.microsoft.com/it-IT/download/details.aspx?id=2939).

6.	Copiare la cartella di lavoro di Excel in una cartella di lavoro, ad esempio D:\\Excel\\Run.

7.	Aprire la cartella di lavoro di Excel. Nella barra multifunzione **Sviluppo** fare clic su **COM Add-Ins** e verificare che il componente aggiuntivo HPC Pack Excel COM venga caricato correttamente, come illustrato nella schermata seguente.

    ![Componente aggiuntivo Excel per HPC Pack][addin]

8.	Modificare la macro VBA HPCControlMacros in Excel cambiando le righe commentate, come illustrato nello script seguente. Sostituire con i valori appropriati per il proprio ambiente.

    ![Macro Excel per HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.	Copiare la cartella di lavoro di Excel in una directory di caricamento, ad esempio D:\\Excel\\Upload, come specificato nella costante HPC\_DependsFiles nella macro VBA.

10.	Scegliere il pulsante **Cluster** nel foglio di lavoro per eseguire la cartella di lavoro nel cluster IaaS di Azure.

### Esecuzione delle funzioni definite dall'utente di Excel

Per eseguire funzioni definite dall'utente di Excel, seguire i passaggi da 1 a 3 della precedente configurazione del computer client. Per le funzioni definite dall'utente di Excel, non è necessario che l'applicazione Excel sia installata sui nodi di calcolo, pertanto nel passaggio 1 è possibile scegliere un'immagine normale del nodo di calcolo anziché l'immagine del nodo di calcolo con Excel.

>[AZURE.NOTE] Esiste un limite di 34 caratteri nella finestra di dialogo del connettore del cluster di Excel 2010 e 2013. Se il nome completo del cluster è più lungo, ad esempio hpcexcelhn01.southeastasia.cloudapp.azure.com, lo spazio disponibile nella finestra di dialogo non sarà sufficiente. La soluzione alternativa consiste nell’impostare una variabile a livello di computer, ad esempio *CCP\_IAASHN* con il valore del nome di cluster lungo e l'input *%CCP\_IAASHN%* nella finestra di dialogo come nome del nodo head del cluster.

Dopo aver correttamente distribuito il cluster, continuare con la procedura seguente per eseguire una funzione definita dall'utente di Excel integrata di esempio. Per funzioni definite dall'utente di Excel personalizzate, consultare queste [risorse](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) per compilare librerie XLL e distribuirle sul cluster IaaS.

1.	Aprire una nuova cartella di lavoro di Excel. Nella barra multifunzione **Sviluppo** fare clic su **Componenti aggiuntivi**. Quindi, nella finestra di dialogo fare clic su **Sfoglia**, accedere alla cartella %CCP\_HOME%Bin\\XLL32 e selezionare il ClusterUDF32.xll di esempio. Se il ClusterUDF32 non esiste nel computer client, è possibile copiarlo dalla cartella %CCP\_HOME%Bin\\XLL32 nel nodo head.

    ![Selezionare la UDF][udf]

2.	Fare clic su **File** > **Opzioni** > **Avanzate**. In **Formule** selezionare **Consenti l'esecuzione di funzioni XLL definite dall'utente in un cluster di calcolo**. Fare clic su **Opzioni** e immettere il nome completo del cluster in **Nome del nodo head del cluster**. (Come indicato in precedenza la casella di immissione è limitata a 34 caratteri, pertanto un nome di cluster lungo potrebbe non essere adatto. Qui è possibile usare variabili a livello di computer per i nomi lunghi del cluster).

    ![Configurare la UDF][options]

3.	Fare clic sulla cella con valore =XllGetComputerNameC() e premere Invio per eseguire il calcolo della funzione definita dall'utente nel cluster IaaS. La funzione recupererà semplicemente il nome del nodo di calcolo in cui viene eseguita la funzione definita dall'utente. Per la prima esecuzione, una finestra di dialogo per l'immissione delle credenziali richiede nome utente e password per connettersi al cluster IaaS.

    ![Eseguire una UDF][run]

    Quando esistono varie celle da elaborare, premere Alt-Shift-Ctrl + F9 per eseguire il calcolo su tutte le celle.

## Passaggio 3. Esecuzione di un carico di lavoro SOA da un client locale

Per eseguire applicazioni SOA generiche nel cluster HPC Pack IaaS, usare in primo luogo uno dei metodi illustrati al passaggio 1 per distribuire il cluster IaaS, usando l'immagine di un nodo di calcolo generico (perché non è necessario Excel nei nodi di calcolo). Attenersi quindi alla procedura seguente:

1. Dopo avere recuperato il certificato del cluster, importarlo nel computer client, in Cert:\\CurrentUser\\Root.

2. Installare le [utilità client HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) e [HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923) in modo che sia possibile sviluppare ed eseguire applicazioni client SOA.

3. Scaricare il [codice di esempio](https://www.microsoft.com/download/details.aspx?id=41633) HelloWorldR2. Aprire HelloWorldR2.sln in Visual Studio 2010 o 2012.

4. Compilare innanzitutto il progetto EchoService e distribuire il servizio nel cluster IaaS nello stesso modo usato per la distribuzione in un cluster locale. Per informazioni dettagliate, vedere il file Readme.doc in HelloWordR2. Modificare e compilare HelloWorldR2 e altri progetti come descritto di seguito per generare le applicazioni client SOA in esecuzione in un cluster IaaS di Azure da un computer client locale.

### Uso dell'associazione Http con coda di archiviazione di Azure

Per usare l'associazione Http con una coda di archiviazione di Azure, apportare alcune modifiche al codice di esempio.

* Aggiornare il nome del cluster.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Facoltativamente, usare il nome predefinito TransportScheme in SessionStartInfo o impostarlo in modo esplicito su Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Usare l'associazione predefinita per il BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Oppure impostare in modo esplicito usando l'associazione basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Facoltativamente, impostare il flag UseAzureQueue su true in SessionStartInfo. Se non lo si imposta, verrà impostato in modo predefinito su true quando il nome del cluster dispone di suffissi di dominio di Azure e il TransportScheme è Http.

    ```
    info.UseAzureQueue = true;
```

###Uso dell'associazione Http senza coda di archiviazione di Azure

A tale scopo, impostare in modo esplicito il flag UseAzureQueue su false in SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### Uso dell'associazione NetTcp

Per usare l'associazione NetTcp, la configurazione è simile alla connessione a un cluster locale. È necessario aprire alcuni endpoint nella VM del nodo head. Se si usa lo script di distribuzione HPC Pack IaaS per creare il cluster, ad esempio, impostare gli endpoint nel portale di Azure classico effettuando le operazioni seguenti.


1. Arrestare la VM.

2. Aggiungere le porte TCP 9090, 9087, 9091, 9094 rispettivamente per Sessione, Broker, Broker worker e Servizi dati

    ![Configurare gli endpoint][endpoint]

3. Avviare la VM.

L'applicazione client SOA non richiede alcuna modifica, ad eccezione della modifica al nome head per il nome completo del cluster IaaS.

## Passaggi successivi

* Per altre informazioni sull'esecuzione di carichi di lavoro di Excel con HPC Pack, vedere [queste risorse](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx).

* Vedere l'articolo relativo alla [gestione dei servizi SOA in Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) per altre informazioni sulla distribuzione e sulla gestione dei servizi SOA con HPC Pack.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png

<!---HONumber=AcomDC_0615_2016-->