<properties
    pageTitle="Sviluppo di applicazioni Web Python Flask con DocumentDB | Microsoft Azure"
    description="Esaminare un'esercitazione del database sull'utilizzo di DocumentDB per archiviare e accedere ai dati da un'applicazione web Python Flask ospitata in Azure. Trovare soluzioni di sviluppo dell'applicazione." 
	keywords="Sviluppo di applicazioni, esercitazione sul database, Python Flask, applicazione Web Python, sviluppo Web Python, DocumentDB, Azure, Microsoft Azure"
    services="documentdb"
    documentationCenter="python"
    authors="aliuy"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="04/18/2016"
    ms.author="andrl"/>

# Sviluppo di applicazioni Web Python Flask con DocumentDB

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.JS](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

Questa esercitazione mostra come usare Azure DocumentDB per archiviare e accedere ai dati forniti da un'applicazione Web Python ospitata in Azure e presuppone che si siano già usati Python e Siti Web di Azure.

In questa esercitazione del database vengono trattati i seguenti argomenti:

1. Creare ed eseguire il provisioning di un account DocumentDB.
2. Creare un'applicazione MVC Python.
3. Connettersi e usare Azure DocumentDB dall'applicazione Web.
4. Distribuire l'applicazione Web in Siti Web di Azure.

Seguendo questa esercitazione, si creerà una semplice applicazione di voto che consente di votare per un sondaggio.

![Schermata dell'applicazione Web per un elenco di azioni creata in questa esercitazione del database](./media/documentdb-python-application/image1.png)


## Prerequisiti per l'esercitazione del database

Prima di seguire le istruzioni di questo articolo, verificare che siano disponibili i seguenti elementi:

- Un account Azure attivo. Se non si dispone di un account, è possibile creare un account di valutazione gratuita in pochi minuti. Per informazioni dettagliate, vedere la pagina relativa alla [versione di valutazione gratuita di Azure](https://azure.microsoft.com/pricing/free-trial/).
- [Visual Studio 2013](http://www.visualstudio.com/) o versione successiva oppure [Visual Studio Express](), che è la versione gratuita. Le istruzioni riportate in questa esercitazione sono scritte in modo specifico per Visual Studio 2015. 
- Python Tools per Visual Studio, disponibile su [GitHub](http://microsoft.github.io/PTVS/). Questa esercitazione usa Python Tools per Visual Studio 2015. 
- Azure Python SDK per Visual Studio 2.4 o versione successiva, disponibile su [azure.com](https://azure.microsoft.com/downloads/). Questa esercitazione usa Microsoft Azure SDK per Python 2.7.
- Python 2.7 disponibile su [python.org][2]. Questa esercitazione usa Python 2.7.11. 

> [AZURE.IMPORTANT] Se si installa Python 2.7 per la prima volta, nella schermata relativa alla personalizzazione di Python 2.7.11 assicurarsi di selezionare **Add python.exe to Path**.
> 
>    ![Schermata Customize Python 2.7.11, in cui è necessario selezionare Add python.exe to Path](./media/documentdb-python-application/image2.png)

- Compilatore Microsoft Visual C++ per Python 2.7 disponibile nell'[Area download Microsoft][3].

## Passaggio 1: Creare un account di database di DocumentDB

Il primo passaggio consiste nella creazione di un account DocumentDB. Se si ha già un account, è possibile andare al [Passaggio 2: Creare una nuova applicazione Web Python Flask](#step-2:-create-a-new-python-flask-web-application).

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

<br/> Verrà ora illustrata in dettaglio la procedura per creare un'applicazione Web Python Flask completamente nuova.

## Passaggio 2: Creare una nuova applicazione Web Python Flask

1. Dal menu **File** di Visual Studio scegliere **Nuovo** e quindi fare clic su **Progetto**.

    Verrà visualizzata la finestra di dialogo **Nuovo progetto**.

2. Nel riquadro sinistro, espandere **Templates** e quindi **Python**, quindi fare clic su **Web**.

3. Selezionare **Flask Web Project** nel riquadro centrale, quindi digitare **tutorial** nella casella **Name** e fare clic su **OK**. Tenere presente che i nomi di pacchetto Python devono essere tutti in lettere minuscole, come descritto nella [guida di stile per il codice Python](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).

	Se è la prima volta che si usa Python Flask, si tratta di un framework di sviluppo di applicazioni Web che consente di creare applicazioni Web in Python più velocemente.

	![Schermata della finestra Nuovo progetto in Visual Studio con Python selezionato a sinistra, progetto web Flask selezionato nella parte centrale ed esercitazioni sul nome nella casella nome](./media/documentdb-python-application/image9.png)

4. Nella finestra **Python Tools for Visual Studio** selezionare **Install into a virtual environment**.

	![Schermata dell'esercitazione del database - Python Tools per la finestra di Visual Studio](./media/documentdb-python-application/image10.png)

5. Nella finestra **Add Virtual Environment** è possibile accettare le impostazioni predefinite e usare Python 2.7 come ambiente di base, perché PyDocumentDB attualmente non supporta Python 3.x, quindi fare clic su **Create**. Questo permette di configurare l'ambiente virtuale Python per il progetto.

	![Schermata dell'esercitazione del database - Python Tools per la finestra di Visual Studio](./media/documentdb-python-application/image10_A.png)

    Se l'installazione dell'ambiente riesce, nella finestra di output verrà visualizzato il messaggio `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`.

## Passaggio 3: Modificare l'applicazione Web Python Flask

### Aggiungere i pacchetti Python Flask al progetto

Dopo la configurazione del progetto sarà necessario aggiungere i pacchetti Flask necessari per il progetto, compreso pydocumentdb, il pacchetto python per DocumentDB.

1. In Esplora soluzioni aprire il file denominato **requirements.txt** e sostituire il contenuto esistente con quello riportato di seguito:

    	flask==0.9
    	flask-mail==0.7.6
    	sqlalchemy==0.7.9
    	flask-sqlalchemy==0.16
    	sqlalchemy-migrate==0.7.2
    	flask-whooshalchemy==0.55a
    	flask-wtf==0.8.4
    	pytz==2013b
    	flask-babel==0.8
    	flup
    	pydocumentdb>=1.0.0

2. Salvare il file **requirements.txt**.
3. In Esplora soluzioni fare clic con il pulsante destro del mouse su **env** e scegliere **Install from requirements.txt**.

	![Schermata che mostra env (Python 2.7) selezionata con l'installazione da requirements.txt evidenziato nell'elenco](./media/documentdb-python-application/image11.png)

    Al termine dell'installazione, la finestra di output visualizza quanto segue:

        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2

    > [AZURE.NOTE] In rari casi è possibile che venga visualizzato un errore nella finestra di output. In un'eventualità di questo tipo, verificare se l'errore è correlato alla pulizia. Talvolta la pulizia può avere esito negativo, ma l'installazione viene comunque completata correttamente (scorrere verso l'alto nella finestra di output per verificarlo). È possibile controllare l'installazione [verificando l'ambiente virtuale](#verify-the-virtual-environment). Se l'installazione non è riuscita, ma la verifica ha esito positivo, è possibile continuare.

### Verifica dell'ambiente virtuale

È importante verificare che tutto sia installato correttamente.

1. Premere **CTRL**+**MAIUSC**+**B** per compilare la soluzione.
2. Al termine della compilazione, avviare il sito Web premendo **F5**. Verranno avviati il server di sviluppo Flask e il Web browser. Dovrebbe essere visualizzata la pagina seguente:

	![Il progetto di sviluppo web Python Flask vuoto visualizzato in un browser](./media/documentdb-python-application/image12.png)

3. Per arrestare il debug del sito Web, premere **MAIUSC**+**F5**.

### Creare definizioni di database, raccolta e documento

Ora viene creata l'applicazione di voto aggiungendo nuovi file e aggiornandone altri.

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **tutorial**, quindi scegliere **Aggiungi** e infine **Nuovo elemento**. Selezionare **Empty Python File** e denominare il file **forms.py**.  
2. Aggiungere il codice seguente al file forms.py e quindi salvare il file.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
	deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### Aggiungere le importazioni necessarie a views.py

1. In Esplora soluzioni espandere la cartella **tutorial** e aprire il file **views.py**. 
2. Aggiungere le istruzioni import seguenti all'inizio del file **views.py** e quindi salvare il file. Queste importano i pacchetti PythonSDK e Flask di DocumentDB.

	```python
	from forms import VoteForm
	import config
	import pydocumentdb.document_client as document_client
	```


### Create database, raccolta e documento

- Ancora in **views.py** aggiungere il codice seguente alla fine del file. Verrà creato il database usato dal form. Non eliminare nessuna porzione del codice esistente dal file **views.py**. Aggiungere semplicemente la parte nuova alla fine del file.

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION }, { 'offerType': 'S1' })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```

> [AZURE.TIP] Il metodo **CreateCollection** accetta un **RequestOptions** facoltativo come 3ª parametro. Consente di specificare il tipo di offerta per la raccolta. Se non viene specificato alcun valore offerType, la raccolta verrà creata usando il tipo di offerta predefinito. Per altre informazioni sui tipi di offerta di DocumentDB, vedere [Livelli di prestazioni in DocumentDB](documentdb-performance-levels.md).


### Leggere database, raccolta, documento e inviare il form

- Ancora in **views.py** aggiungere il codice seguente alla fine del file. Verrà configurato il form e verranno letti il database, la raccolta e il documento. Non eliminare alcuna parte del codice esistente dal file **views.py**. Aggiungere semplicemente la parte nuova alla fine del file.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### Creare i file HTML

1. Nella cartella **tutorial** in Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella **templates**, scegliere **Aggiungi** e infine **Nuovo elemento**. 
2. Selezionare **Pagina HTML** e nella casella del nome digitare **create.html**. 
3. Ripetere i passaggi 1 e 2 per creare altri due file HTML: results.html e vote.html.
4. Aggiungere il codice seguente al file **create.html** nell'elemento `<body>`. Consente di visualizzare un messaggio che comunica l'avvenuta creazione di un nuovo database, di una raccolta e di un documento.

	```html
	{% extends "layout.html" %}
	{% block content %}
	<h2>{{ title }}.</h2>
	<h3>{{ message }}</h3>
	<p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
	{% endblock %}
	```

5. Aggiungere il codice seguente al file **results.html** nell'elemento `<body`. Consente di visualizzare i risultati del sondaggio.

	```html
	{% extends "layout.html" %}
	{% block content %}
	<h2>Results of the vote</h2>
		<br />
		
	{% for choice in vote_object.choices %}
	<div class="row">
		<div class="col-sm-5">{{choice}}</div>
	        <div class="col-sm-5">
	        	<div class="progress">
	        		<div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
	                    		{{vote_object.choices[choice]}}
				</div>
			</div>
	        </div>
	</div>
	{% endfor %}
	
	<br />
	<a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
	{% endblock %}
	```

6. Aggiungere il codice seguente al file **vote.html** nell'elemento `<body`. Consente di visualizzare il sondaggio e di accettare i voti. Alla registrazione dei voti il controllo viene passato a views.py che riconoscerà il voto espresso e ne effettuerà l'aggiunta al documento.

	```html
	{% extends "layout.html" %}
	{% block content %}
	<h2>What is your favorite way to host an application on Azure?</h2>
	<form action="" method="post" name="vote">
		{{form.hidden_tag()}}
	        {{form.deploy_preference}}
	        <button class="btn btn-primary" type="submit">Vote</button>
	</form>
	{% endblock %}
	```

7. Nella cartella **templates** sostituire il contenuto del file **index.html** con il codice seguente. Questo fungerà da pagina di destinazione per l'applicazione.
	
	```html
	{% extends "layout.html" %}
	{% block content %}
	<h2>Python + DocumentDB Voting Application.</h2>
	<h3>This is a sample DocumentDB voting application using PyDocumentDB</h3>
	<p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
	<p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
	{% endblock %}
	```

### Aggiungere un file di configurazione e modificare \_\_init\_\_.py

1. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **tutorial**, scegliere **Aggiungi**, **Nuovo elemento** e infine **Empty Python File**, quindi denominare il file **config.py**. Questo file config è richiesto per i moduli in Flask. È possibile usarlo anche per fornire una chiave privata. Tale chiave non sarà tuttavia necessaria per questa esercitazione.

2. Aggiungere il codice seguente al file config.py. Nel passaggio successivo sarà necessario modificare i valori di **DOCUMENTDB\_HOST** e **DOCUMENTDB\_KEY**.

	```python
	CSRF_ENABLED = True
	SECRET_KEY = 'you-will-never-guess'
	
	DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
	DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
	
	DOCUMENTDB_DATABASE = 'voting database'
	DOCUMENTDB_COLLECTION = 'voting collection'
	DOCUMENTDB_DOCUMENT = 'voting document'
	```

3. Nel [portale di Azure](https://portal.azure.com/) passare al pannello **Chiavi** facendo clic su **Sfoglia** e quindi su **Account DocumentDB**, fare doppio clic sul nome dell'account da usare e quindi fare clic sul pulsante **Chiavi** nell'area **Informazioni di base**. Nel pannello **Chiavi** copiare il valore **URI** e incollarlo nel file **config.py** come valore della proprietà **DOCUMENTDB\_HOST**.
4. Tornare al portale di Azure. Nel pannello **Chiavi** copiare il valore della **chiave primaria** o della **chiave secondaria** e incollarlo nel file **config.py** come valore della proprietà **DOCUMENTDB\_KEY**.
5. Aggiungere la riga seguente al file **\_\_init\_\_.py**. 

        app.config.from_object('config')

    Il contenuto del file deve essere analogo al seguente:

	```python
	from flask import Flask
	app = Flask(__name__)
	app.config.from_object('config')
	import tutorial.views
	```

6. Dopo aver aggiunto tutti i file, l'aspetto di Esplora soluzioni dovrebbe risultare simile al seguente:

	![Schermata della finestra Esplora soluzioni di Visual Studio](./media/documentdb-python-application/image15.png)


## Passaggio 4: Esecuzione dell'applicazione Web in locale

1. Premere **CTRL**+**MAIUSC**+**B** per compilare la soluzione.
2. Al termine della compilazione, avviare il sito Web premendo **F5**. Dovrebbe venire visualizzata la schermata seguente.

	![Schermata di Python + DocumentDB applicazione voto visualizzati in un browser web](./media/documentdb-python-application/image16.png)

3. Fare clic su **Create/Clear the Voting Database** per generare il database.

	![Schermata della pagina di creazione dell'applicazione web - informazioni dettagliate sullo sviluppo](./media/documentdb-python-application/image17.png)

4. Fare quindi clic su **Voto** e selezionare un'opzione.

	![Schermata dell'applicazione Web con una domanda relativa al voto](./media/documentdb-python-application/image18.png)

5. Ogni voto espresso va a incrementare il contatore appropriato.

	![Schermata dei risultati della pagina voto visualizzata](./media/documentdb-python-application/image19.png)

6. Per arrestare il debug del sito Web, premere MAIUSC+F5.

## Passaggio 5: Distribuire l'applicazione Web in Siti Web di Azure

Ora che l'applicazione completa funziona correttamente con DocumentDB, sarà possibile distribuirla in Siti Web di Azure.

1. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni, assicurarsi che il progetto non sia ancora in esecuzione localmente e quindi selezionare **Pubblica**.  

 	![Schermata dell'esercitazione selezionata in Esplora soluzioni, con l'opzione Pubblica evidenziata](./media/documentdb-python-application/image20.png)

2. Nella finestra di dialogo **Pubblica sul Web** selezionare **App Web di Microsoft Azure** e quindi fare clic su **Avanti**.

	![Screenshot della finestra Pubblica sul Web con App Web di Microsoft Azure evidenziato](./media/documentdb-python-application/image21.png)

3. Nella finestra **App Web di Microsoft Azure** fare clic su **Nuovo**.

	![Screenshot della finestra App Web di Microsoft Azure](./media/documentdb-python-application/select-existing-website.png)

4. Nella finestra **Crea sito in Microsoft Azure** immettere i valori per **Nome app Web**, **Piano di Servizio app**, **Gruppo di risorse** e **Area**, quindi fare clic su **Crea**.

	![Schermata della finestra di dialogo Crea sito in Microsoft Azure](./media/documentdb-python-application/create-site-on-microsoft-azure.png)

5. Nella finestra di dialogo **Pubblica sul Web** fare clic su **Pubblica**.

	![Schermata della finestra di dialogo Crea sito in Microsoft Azure](./media/documentdb-python-application/publish-web.png)

3. Dopo alcuni secondi, Visual Studio completerà la pubblicazione dell'applicazione Web e avvierà un browser in cui sarà possibile ammirare il proprio lavoro in esecuzione in Azure.

## Risoluzione dei problemi

Se si tratta della prima app Python eseguita nel computer, assicurarsi che le cartelle seguenti o i percorsi di installazione equivalenti siano inclusi nella variabile PATH:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Se si riceve un errore nella pagina di voto e il progetto ha un nome diverso da **tutorial**, assicurarsi che **\_\_init\_\_.py** faccia riferimento al nome di progetto corretto nella riga `import tutorial.view`.

## Passaggi successivi

Congratulazioni. Si è creata la prima applicazione web Python usando Azure DocumentDB e la si è pubblicata in Siti Web di Azure.

Aggiorniamo e miglioriamo spesso questo argomento in base al feedback degli utenti. Dopo aver completato l'esercitazione, utilizzare i pulsanti per la votazione nella parte superiore e inferiore della pagina e assicurarsi di includere il feedback sui miglioramenti che si desidera vedere. Se si desidera contattarci, è possibile includere l'indirizzo di posta elettronica nel commento per il follow-up.

Per aggiungere altre funzionalità all'applicazione Web, esaminare le API disponibili in [DocumentDB Python SDK](documentdb-sdk-python.md).

Per altre informazioni su Azure, Visual Studio e Python, vedere il [centro per sviluppatori di Python](https://azure.microsoft.com/develop/python/).

Per altre esercitazioni su Python Flask, vedere [The Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).

  [Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
  [2]: https://www.python.org/downloads/windows/
  [3]: https://www.microsoft.com/download/details.aspx?id=44266
  [Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
  [Azure portal]: http://portal.azure.com

<!---HONumber=AcomDC_0420_2016-->