<properties
   pageTitle="Usare Data Lake Store Java SDK per lo sviluppo di applicazioni | Azure"
   description="Usare Azure Data Lake Store Java SDK per lo sviluppo di applicazioni"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/12/2016"
   ms.author="nitinme"/>

# Introduzione ad Archivio Azure Data Lake con Java

> [AZURE.SELECTOR]
- [Portale](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [SDK per Java](data-lake-store-get-started-java-sdk.md)
- [API REST](data-lake-store-get-started-rest-api.md)
- [Interfaccia della riga di comando di Azure](data-lake-store-get-started-cli.md)
- [Node.JS](data-lake-store-manage-use-nodejs.md)

Informazioni su come usare Azure Data Lake Store Java SDK per creare un account Azure Data Lake ed eseguire operazioni di base, ad esempio creare cartelle, caricare e scaricare file di dati, eliminare l'account e così via. Per altre informazioni su Data Lake, vedere [Azure Data Lake Store](data-lake-store-overview.md).

## Java SDK di Archivio Azure Data Lake

I collegamenti seguenti forniscono il percorso di download per Java SDK per Archivio Data Lake e le informazioni di riferimento su Java SDK. Per questa esercitazione non è necessario scaricare l'SDK o seguire il documento di riferimento. Questi collegamenti sono solo a scopo informativo.

* Il codice sorgente per Java SDK per Archivio Data Lake è disponibile in [GitHub](https://github.com/Azure/azure-sdk-for-java).
* Le informazioni di riferimento su Java SDK per Archivio Data Lake sono disponibili all'indirizzo [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/).

## Prerequisiti

* Java Development Kit (JDK) 8 con Java versione 1.8.
* IntelliJ o un altro ambiente di sviluppo Java adatto. Questo prerequisito è facoltativo ma consigliato. Le istruzioni riportate di seguito usano IntelliJ.
* **Una sottoscrizione di Azure**. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Abilitare la sottoscrizione di Azure** per l'anteprima pubblica di Data Lake Store. Vedere le [istruzioni](data-lake-store-get-started-portal.md#signup).
* **Creare un'applicazione di Azure Active Directory**. Esistono due modalità di autenticazione con Azure Active Directory: **interattiva** e **non interattiva**. I prerequisiti sono diversi a seconda della modalità di autenticazione.
	* **Per l'autenticazione interattiva**: in Azure Active Directory è necessario creare un'**applicazione client nativa**. Dopo aver creato l'applicazione, recuperare i valori seguenti correlati.
		- Ottenere l'**ID client** e l'**URI di reindirizzamento** per l'applicazione
		- Impostare autorizzazioni delegate

	* **Per l'autenticazione non interattiva** (usata in questo articolo): in Azure Active Directory è necessario creare un'**applicazione Web**. Dopo aver creato l'applicazione, recuperare i valori seguenti correlati.
		- Ottenere l'**ID client**, il **segreto client** e l'**URI di reindirizzamento** per l'applicazione
		- Impostare autorizzazioni delegate
		- Assegnare l'applicazione Azure Active Directory a un ruolo. Il ruolo può essere al livello dell'ambito in cui si desidera concedere l'autorizzazione per l'applicazione di Azure Active Directory. Ad esempio, è possibile assegnare l'applicazione al livello della sottoscrizione o del gruppo di risorse. 

	Per istruzioni su come recuperare questi valori, impostare le autorizzazioni e assegnare i ruoli, vedere [Creare un'applicazione e un'entità servizio di Active Directory tramite il portale](../resource-group-create-service-principal-portal.md).

## Come si esegue l'autenticazione tramite Azure Active Directory?

Il frammento di codice riportato di seguito fornisce il codice per l'autenticazione **non interattiva**, in cui l'applicazione fornisce le proprie credenziali.

Perché questa esercitazione possa funzionare, è necessario concedere all'applicazione le autorizzazioni necessarie per creare risorse in Azure. Ai fini di questa esercitazione, è **consigliabile** concedere all'applicazione autorizzazioni di Collaboratore soltanto per un gruppo di risorse nuovo, vuoto e inutilizzato nella sottoscrizione di Azure.

## Creare un'applicazione Java

1. Aprire IntelliJ e creare un nuovo progetto Java usando il modello **Command Line App**. Completare la procedura guidata per creare il progetto.

2. Fare clic con il pulsante destro del mouse sul progetto sulla sinistra dello schermo e fare clic su **Add Framework Support**. Scegliere **Maven** e fare clic su **OK**.

3. Aprire il file **"pom.xml"** appena creato e aggiungere il frammento di testo seguente tra il tag **</version>** e il tag **</project>**:

    >[AZURE.NOTE] Questo passaggio è temporaneo e non sarà più necessario quando Azure Data Lake Store SDK sarà disponibile in Maven. Quando l'SDK sarà disponibile in Maven l'articolo verrà aggiornato. Tutti gli aggiornamenti futuri di questo SDK saranno disponibili tramite Maven.

        <repositories>
        	<repository>
	            <id>adx-snapshots</id>
	            <name>Azure ADX Snapshots</name>
	            <url>http://adxsnapshots.azurewebsites.net/</url>
	            <layout>default</layout>
	            <snapshots>
                	<enabled>true</enabled>
            	</snapshots>
        	</repository>
        	<repository>
	            <id>oss-snapshots</id>
	            <name>Open Source Snapshots</name>
	            <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
	            <layout>default</layout>
	            <snapshots>
	                <enabled>true</enabled>
	                <updatePolicy>always</updatePolicy>
	            </snapshots>
        	</repository>
    	</repositories>
    	<dependencies>
	        <dependency>
	            <groupId>com.microsoft.azure</groupId>
	            <artifactId>azure-client-authentication</artifactId>
	            <version>1.0.0-20160513.000802-24</version>
	        </dependency>
	        <dependency>
	            <groupId>com.microsoft.azure</groupId>
	            <artifactId>azure-client-runtime</artifactId>
	            <version>1.0.0-20160513.000812-28</version>
	        </dependency>
	        <dependency>
	            <groupId>com.microsoft.rest</groupId>
	            <artifactId>client-runtime</artifactId>
	            <version>1.0.0-20160513.000825-29</version>
	        </dependency>
	        <dependency>
	            <groupId>com.microsoft.azure</groupId>
	            <artifactId>azure-mgmt-datalake-store</artifactId>
	            <version>1.0.0-SNAPSHOT</version>
	        </dependency>
    	</dependencies>


4. Passare a **File**, quindi a **Settings** e infine a **Build, Execution, and Deployment**. Espandere **Build Tools**, **Maven**, quindi espandere **Importing**. Selezionare la casella di controllo **Import Maven projects automatically**. Fare clic su **Apply** e quindi su **OK**.

5. Dal riquadro sinistro passare a **src**, **main**, **java**, **<nome pacchetto>**, quindi aprire il file **Main.java** e sostituire il blocco di codice esistente con il codice seguente. Immettere anche i valori per i parametri indicati nel frammento di codice, ad esempio **localFolderPath**, **\_adlsAccountName**, **\_resourceGroupName** e sostituire i segnaposto per **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** e **SUBSCRIPTION-ID**.

    Il codice crea un account di Archivio Data Lake, crea cartelle nell'archivio, concatena i file, scarica un file e infine elimina l'account.

        package com.company;
        
        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");
        
                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Account(s) displayed.", "Creating files.");
        
                // Create two files in Data Lake Store: file1.csv and file2.csv
                CreateFile("/file1.csv", "123,abc", true);
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("Files created.", "Concatenating files.");
        
                // Concatenate two files in Data Lake Store
                List<String> srcFilePaths = new ArrayList<String>();
                srcFilePaths.add("/file1.csv");
                srcFilePaths.add("/file2.csv");
                ConcatenateFiles(srcFilePaths, "/input.csv");
                WaitForNewline("Files concatenated.", "Downloading file.");
        
                // Download file from Data Lake Store
                DownloadFile("/input.csv", localFolderPath + "input.csv");
                WaitForNewline("File downloaded.", "Deleting file.");
        
                // Delete file from Data Lake Store
                DeleteFile("/input.csv");
                WaitForNewline("File deleted.", "Deleting account.");
        
                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted.", "DONE.");
            }
        
            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
        
                _adlsClient.setSubscriptionId(_subId);
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
                if (!nextAction.isEmpty())
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                    System.out.println(nextAction);
                }
                else
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                }
            }
        
            // Create Data Lake Store account
            public static void CreateAccount() throws InterruptedException, CloudException, IOException {
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }
        
            // Create file
            public static void CreateFile(String path) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path);
            }
        
            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().append(_adlsAccountName, path, bytesContents);
            }
        
            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().concat(_adlsAccountName, destFilePath, srcFilePaths);
            }
        
            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(_adlsAccountName, filePath);
            }
        
            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().getFileStatus(_adlsAccountName, path).getBody().getFileStatus();
            }
        
            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().listFileStatus(_adlsAccountName, directoryPath).getBody().getFileStatuses().getFileStatus();
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(_adlsAccountName, srcPath).getBody();
        
                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());
        
                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();
        
                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }
        
                pWriter.println(fileContents);
                pWriter.close();
            }
        
            // Delete account
            public static void DeleteAccount() throws InterruptedException, CloudException, IOException {
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
            }
        }


6. Eseguire l'applicazione. Seguire le istruzioni per eseguire e completare l'applicazione.

## Passaggi successivi

- [Proteggere i dati in Data Lake Store](data-lake-store-secure-data.md)
- [Usare Azure Data Lake Analytics con Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usare Azure HDInsight con Archivio Data Lake](data-lake-store-hdinsight-hadoop-use-portal.md)

<!---HONumber=AcomDC_0615_2016-->