<properties
    pageTitle="Monitorare e gestire un pool di database elastici con C# | Microsoft Azure"
    description="Usare tecniche di sviluppo di database in C# per gestire un pool di database elastici per database SQL di Microsoft Azure."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# Monitorare e gestire un pool di database elastici con C&#x23; 

> [AZURE.SELECTOR]
- [Portale di Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Informazioni su come gestire un [pool di database elastici](sql-database-elastic-pool.md) con C&#x23;.

Per i codici di errore comuni, vedere [Codici di errore SQL per le applicazioni client del database SQL: errore di connessione e altri problemi del database](sql-database-develop-error-messages.md).

I pool di database elastici sono attualmente in anteprima e sono disponibili unicamente con i server di Database SQL V12. Se si usa un server di database SQL V11 è possibile [usare PowerShell per eseguire l'aggiornamento a V12 e creare un pool](sql-database-upgrade-server-portal.md) in un unico passaggio.

Negli esempi viene utilizzata la [libreria di database SQL per .NET](https://msdn.microsoft.com/library/azure/mt349017.aspx) Installare la libreria eseguendo il comando seguente nella [Console di Gestione pacchetti](http://docs.nuget.org/Consume/Package-Manager-Console) in Visual Studio (**Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**):

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre


## Spostare un database in un pool elastico

È possibile spostare un database autonomo all'interno o all'esterno di un pool.

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## Elencare i database in un pool elastico

Per recuperare tutti i database in un pool, chiamare il metodo [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases).

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## Modificare le impostazioni delle prestazioni di un pool

Recuperare le proprietà dei pool esistenti. Modificare i valori ed eseguire il metodo CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## Latenza delle operazioni dei pool elastici

- La modifica del numero minimo di eDTU per database o del numero massimo di eDTU per database in genere viene completata entro 5 minuti.
- La modifica del numero di eDTU per pool dipende dallo spazio totale usato da tutti i database nel pool. Le modifiche richiedono una media di 90 minuti o meno per 100 GB. Ad esempio, se lo spazio totale utilizzato da tutti i database nel pool è pari a 200 GB, la latenza prevista per la modifica del numero di eDTU del pool per ogni pool è di 3 ore o meno.


## Esempio di gestione di un pool C&#x23;

Le librerie seguenti sono necessarie per eseguire questo esempio. È possibile installarle con il comando seguente nella [Console di Gestione pacchetti](http://docs.nuget.org/Consume/Package-Manager-Console) in Visual Studio, scegliendo **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre
    PM> Install-Package Microsoft.Azure.Management.Resources –Pre
    PM> Install-Package Microsoft.Azure.Common.Authentication –Pre

Creare un'app console e sostituire il contenuto del file Program.cs con il codice seguente. Per ottenere l'ID client richiesto e i valori correlati, vedere l'articolo relativo a [registrare l'app e ottenere i valori client per la connessione dell'applicazione al database SQL](sql-database-client-id-keys.md).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbElasticPoolsSample
    {
    class Program
    {
        // pool variables
        static string poolName = "elasticPool1";
        static string poolEdition = "Standard";
        static int poolDtus = 400;
        static int databaseMinDtus = 0;
        static int databaseMaxDtus = 100;

        // authentication variables
        static string subscriptionId = "<your Azure subscription id>";
        static string clientId = "<your client id>";
        static string redirectUri = "<your redirect URI>";
        static string domainName = "<i.e. microsoft.onmicrosoft.com>";
        static AuthenticationResult token;

        // resource group variables
        static string datacenterLocation = "Japan West";
        static string resourceGroupName = "<resource group name>";

        // server variables
        static string serverName = "<server name>";
        static string adminLogin = "<server admin>";
        static string adminPassword = "<server admin password>";
        static string serverVersion = "12.0";

        // database variables
        static string databaseName = "<database name>";


        static void Main(string[] args)
        {
            // Sign in to Azure
            token = GetAccessToken();
            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            // Create a resource group
            ResourceGroup rg = CreateResourceGroup();
            Console.WriteLine(rg.Name + " created.");

            // Create a server
            Console.WriteLine("Creating server... ");
            ServerGetResponse srvr = CreateServer();
            Console.WriteLine("Creation of server " + srvr.Server.Name + ": " + srvr.StatusCode.ToString());

            // Create a pool
            Console.WriteLine("Creating elastic database pool with 400 pool eDTUs... ");
            ElasticPoolCreateOrUpdateResponse epool = CreateElasticDatabasePool();
            Console.WriteLine("Creation of pool " + epool.ElasticPool.Name + ": " + epool.Status.ToString());

            // Open the portal so we can see our operations in action
            string portalPage = @"https://portal.azure.com/#resource/subscriptions/"
                + subscriptionId
                + @"/resourceGroups/"
                + resourceGroupName
                + @"/providers/Microsoft.Sql/servers/"
                + serverName
                + @"/elasticPools/"
                + poolName ;
            System.Diagnostics.Process.Start(portalPage);


            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to update the pool to 1200 pool eDTUs.");
            Console.ReadLine();

            // Update the pool
            Console.WriteLine("Updating elastic database pool to 1200 pool eDTUs...");
            ElasticPoolCreateOrUpdateResponse epool2 = UpdateElasticDatabasePool();
            Console.WriteLine("Update of pool " + epool2.ElasticPool.Name + ": " + epool2.Status.ToString());

            // Create a new database in the pool
            Console.WriteLine("Creating a new database in the pool... ");
            DatabaseCreateOrUpdateResponse db = CreateNewDatabaseInPool();
            Console.WriteLine("Creation of elastic database " + db.Database.Name + ": " + db.Status.ToString());

            // Move an existing database into pool
            Console.WriteLine("Creating a new database, stand-alone, not yet in the pool... ");
            DatabaseCreateOrUpdateResponse db2 = CreateStandAloneDatabase();
            Console.WriteLine("Creation of stand-alone database " + db2.Database.Name + ": " + db2.Status.ToString());

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to move our existing db into the pool.");
            Console.ReadLine();

            Console.WriteLine("Moving stand-alone database into the pool... ");
            DatabaseCreateOrUpdateResponse db3 = MoveExistingDatabaseIntoPool();
            Console.WriteLine("Moving stand-alone database " + db3.Database.Name + ": " + db3.Status.ToString());

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to list all databases in {0}.", poolName);
            Console.ReadLine();

            // List all databases in a pool
            DatabaseListResponse dbs = GetDbsInPool();
            Console.WriteLine("Databases in Elastic Pool {0}", poolName);
            foreach (Database db4 in dbs)
            {
                Console.WriteLine("- " + db4.Name);
            }

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to exit.");
            Console.ReadLine();
        }


        static ElasticPoolCreateOrUpdateResponse CreateElasticDatabasePool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus, 
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }

        static ElasticPoolCreateOrUpdateResponse UpdateElasticDatabasePool()
        {
            int newPoolDtus = 1200;
            int newDatabaseMinDtus = 10;
            int newDatabaseMaxDtus = 50;

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve existing pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Configure create or update parameters with existing property values, override those to be changed.
            ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    DatabaseDtuMax = newDatabaseMaxDtus,
                    DatabaseDtuMin = newDatabaseMinDtus,
                    Dtu = newPoolDtus
                }
            };
            var updatePoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, updatePoolParameters);
            return updatePoolResponse;
        }

        static DatabaseCreateOrUpdateResponse CreateNewDatabaseInPool()
        {
            databaseName = "newDbInThePool";

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve current pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Create a database
            DatabaseCreateOrUpdateParameters databaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = "Default",
                    Edition = currentPool.Properties.Edition,
                    ElasticPoolName = poolName
                }
            };

            var dbResult = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, databaseParameters);
            return dbResult;
        }

        static DatabaseCreateOrUpdateResponse MoveExistingDatabaseIntoPool()
        {
            databaseName = "standaloneDB";

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve current database
            var currentDatabase = sqlClient.Databases.Get(resourceGroupName, serverName, databaseName).Database;

            // Retrieve current pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Configure create or update parameters with existing property values, override those to be changed.
            DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentDatabase.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    RequestedServiceObjectiveName = "ElasticPool",
                    ElasticPoolName = currentPool.Name,
                    MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
                    Collation = currentDatabase.Properties.Collation,
                }
            };
            // Update the database
            var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, updatePooledDbParameters);
            return dbUpdateResponse;
        }

        static DatabaseListResponse GetDbsInPool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            //List databases in the elastic pool
            DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases(resourceGroupName, serverName, poolName);
            return dbListInPool;
        }
     

        static ServerGetResponse CreateServer()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = adminLogin,
                    AdministratorLoginPassword = adminPassword,
                    Version = serverVersion
                }
            };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }

        static DatabaseCreateOrUpdateResponse CreateStandAloneDatabase()
        {
            databaseName = "standaloneDB";
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a database
            DatabaseCreateOrUpdateParameters databaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = "Standard",
                    RequestedServiceObjectiveName = "S0",
                    CreateMode = "Default"
                }
            };

            var dbResult = sqlClient.Databases.CreateOrUpdate(resourceGroupName,serverName, databaseName, databaseParameters);
            return dbResult;
        }

        static ResourceGroup CreateResourceGroup()
        {
            Microsoft.Rest.TokenCredentials creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);
            // Create a resource management client 
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = datacenterLocation,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }
    }
    }

## Risorse aggiuntive

- [Database SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [API di Gestione risorse di Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Creare un nuovo pool di database elastici con C#](sql-database-elastic-pool-create-csharp.md)
- [Quando usare un pool di database elastici](sql-database-elastic-pool-guidance.md)
- Vedere l'articolo sull'[aumento del numero di istanze con il database SQL di Azure](sql-database-elastic-scale-introduction.md): usare gli strumenti di database elastici per aumentare il numero di istanze, spostare dati, eseguire query o creare transazioni.

<!---HONumber=AcomDC_0615_2016-->