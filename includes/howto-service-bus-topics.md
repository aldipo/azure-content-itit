## Informazioni su argomenti e sottoscrizioni del bus di servizio

Gli argomenti e le code del bus di servizio supportano un modello di comunicazione con messaggistica di *pubblicazione-sottoscrizione*. Quando si usano gli argomenti e le sottoscrizioni, i componenti di un'applicazione distribuita non comunicano direttamente l'uno con l'altro, ma scambiano messaggi tramite un argomento, che agisce da intermediario.

![Concetti relativi agli argomenti](./media/howto-service-bus-topics/sb-topics-01.png)

Diversamente dalle code del bus di servizio, in cui ogni messaggio viene elaborato da un unico consumer, gli argomenti e le sottoscrizioni offrono una forma di comunicazione "uno a molti" tramite un modello di pubblicazione-sottoscrizione. È possibile registrare più sottoscrizioni a un argomento. Quando un messaggio viene inviato a un argomento, viene reso disponibile affinché ogni sottoscrizione possa gestirlo o elaborarlo in modo indipendente.

La sottoscrizione a un argomento è simile a una coda virtuale che riceve copie dei messaggi che sono stati inviati all'argomento. È possibile registrare regole di filtro per un argomento in base alla sottoscrizione, per poter filtrare o limitare quali messaggi relativi a un argomento vengono ricevuti da quali sottoscrizioni dell'argomento.

Gli argomenti e le sottoscrizioni del bus di servizio consentono il ridimensionamento e l'elaborazione di grandi quantità di messaggi tra un numero elevato di utenti e applicazioni.

## Creare uno spazio dei nomi

Per iniziare a usare gli argomenti e le sottoscrizioni del bus di servizio in Azure, è necessario creare prima di tutto uno *spazio dei nomi del servizio*. Uno spazio dei nomi fornisce un contenitore di ambito per fare riferimento alle risorse del bus di servizio all'interno dell'applicazione.

Per creare uno spazio dei nomi:

1.  Accedere al [portale di Azure classico][].

2.  Nel pannello di navigazione sinistro del portale fare clic su **Bus di servizio**.

3.  Nel riquadro inferiore del portale fare clic su **Crea**. ![][0]

4.  Nella finestra di dialogo **Add a new namespace** immettere un nome per lo spazio dei nomi. Verrà effettuato immediatamente un controllo sulla disponibilità del nome.![][2]

5.  Dopo avere verificato che lo spazio dei nomi è disponibile, scegliere il paese o l'area in cui dovrà essere ospitato. Assicurarsi di usare lo stesso paese/area in cui verranno distribuite le risorse di calcolo.

	> [AZURE.IMPORTANT] selezionare la **stessa area** che si intende scegliere per la distribuzione dell'applicazione. In questo modo sarà possibile ottenere prestazioni ottimali.

6. 	Non modificare i valori predefiniti negli altri campi della finestra di dialogo (**Messaggistica** e **Livello Standard**), quindi fare clic sul segno di spunta OK. A questo punto, lo spazio dei nomi verrà creato e abilitato nel sistema. Potrebbero essere necessari alcuni minuti per consentire al sistema di effettuare il provisioning delle risorse per lo spazio dei nomi creato.

	![][6]

## Recuperare le credenziali di gestione predefinite per lo spazio dei nomi

Per poter eseguire le operazioni di gestione nel nuovo spazio dei nomi, ad esempio creare un argomento o una sottoscrizione, è necessario ottenere le credenziali di gestione per lo spazio dei nomi. È possibile ottenere queste credenziali dal portale.

### Ottenere le credenziali di gestione dal portale

1.  Nel riquadro di navigazione sinistro fare clic sul nodo **Bus di servizio** per visualizzare l'elenco degli spazi dei nomi disponibili: ![][0]

2.  Selezionare lo spazio dei nomi appena creato nell'elenco visualizzato: ![][3]

3.  Fare clic su **Informazioni di connessione**. ![][4]

4.  Nella finestra di dialogo **Accedi a informazioni di connessione** individuare la stringa di connessione che contiene la chiave della firma di accesso condiviso e il nome della chiave. Prendere nota di questi valori, in quanto dovranno essere usati in seguito per eseguire operazioni con lo spazio dei nomi.


  [portale di Azure classico]: http://manage.windowsazure.com
  [0]: ./media/howto-service-bus-topics/sb-queues-13.png
  [2]: ./media/howto-service-bus-topics/sb-queues-04.png
  [3]: ./media/howto-service-bus-topics/sb-queues-09.png
  [4]: ./media/howto-service-bus-topics/sb-queues-06.png
  
  [6]: ./media/howto-service-bus-topics/getting-started-multi-tier-27.png

