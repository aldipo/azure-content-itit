<properties 
	pageTitle="Connettersi a un account DocumentDB con supporto del protocollo per MongoDB | Microsoft Azure" 
	description="Informazioni su come connettersi a un account DocumentDB con supporto del protocollo per MongoDB, ora disponibile in anteprima. Connettersi usando la stringa di connessione MongoDB." 
	keywords="stringa di connessione mongodb"
	services="documentdb" 
	authors="stephbaron" 
	manager="jhubbard" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/31/2016" 
	ms.author="stbaro"/>

# Procedura di connessione a un account DocumentDB con supporto del protocollo per MongoDB

Informazioni su come connettersi a un account DocumentDB con supporto del protocollo per MongoDB usando il formato URI della stringa di connessione standard di MongoDB.

## Ottenere informazioni sulla stringa di connessione dell'account

1. In una nuova finestra accedere al [portale di Azure](https://portal.azure.com).
2. Nel pannello **Tutte le impostazioni**, fare clic su **Stringa di connessione**. Per passare a **Tutte le impostazioni**, nell'indice fare clic su **Esplora**, fare clic su **Account DocumentDB** e quindi selezionare l'account DocumentDB con il supporto del protocollo per MongoDB.

	![Screenshot del pannello Tutte le impostazioni](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. Verrà visualizzato il pannello **Informazioni sulla stringa di connessione** con tutte le informazioni necessarie per connettersi all'account usando un driver per MongoDB, inclusa una stringa di connessione predefinita.

	![Screenshot del pannello Stringa di connessione](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## Requisiti della stringa di connessione

È importante notare che DocumentDB supporta il formato URI della stringa di connessione standard di MongoDB, con un paio di requisiti specifici: gli account DocumentDB richiedono l'autenticazione e la comunicazione sicura con SSL. Il formato della stringa di connessione è quindi:

	mongodb://username:password@host:port/[database]?ssl=true

Dove i valori di questa stringa sono disponibili nel pannello Stringa di connessione illustrato in precedenza.

- Nome utente (obbligatorio)
	- Nome dell'account DocumentDB
- Password (obbligatorio)
	- Password dell'account DocumentDB
- Host (obbligatorio)
	- FQDN dell'account DocumentDB
- Porta (obbligatorio)
	- 10250
- Database (facoltativo)
	- Database predefinito usato dalla connessione
- ssl=true (obbligatorio)

Si consideri ad esempio l'account indicato sopra in Informazioni sulla stringa di connessione. Una stringa di connessione valida è la seguente:
	
	mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## Connessione con il driver C# per MongoDB
Come già accennato, tutti gli account di DocumentDB richiedono l'autenticazione e la comunicazione sicura con SSL. Anche se il formato URI della stringa di connessione di MongoDB supporta un parametro della stringa di query ssl=true, l'uso del driver C# per MongoDB richiede l'oggetto MongoClientSettings durante la creazione di un elemento MongoClient. Date le informazioni sull'account indicate in precedenza, il frammento di codice seguente illustra come connettersi all'account e usare il database "Attività".

	        MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
	

## Passaggi successivi


- Informazioni su come [usare MongoChef](documentdb-mongodb-mongochef.md) con un account DocumentDB con supporto del protocollo per MongoDB.
- Esplorare DocumentDB con supporto del protocollo per trovare [esempi](documentdb-mongodb-samples.md) di MongoDB.

 

<!---HONumber=AcomDC_0601_2016-->