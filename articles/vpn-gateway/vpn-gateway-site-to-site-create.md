<properties
   pageTitle="Creare una rete virtuale con una connessione di gateway VPN da sito a sito con il portale di Azure classico | Microsoft Azure"
   description="Creare una rete virtuale con una connessione di gateway VPN da sito a sito per configurazioni cross-premise e ibride usando il modello di distribuzione classico."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/13/2016"
   ms.author="cherylmc"/>

# Creare una rete virtuale con una connessione VPN da sito a sito usando il portale di Azure classico

> [AZURE.SELECTOR]
- [Portale di Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Portale di Azure classico](vpn-gateway-site-to-site-create.md)
- [PowerShell - Gestione risorse](vpn-gateway-create-site-to-site-rm-powershell.md)


In questo argomento verrà illustrata la creazione di una rete virtuale e una connessione VPN da sito a sito alla rete locale. Le connessioni da sito a sito possono essere usate per le configurazioni cross-premise e ibride. Questo articolo si applica al modello di distribuzione classica e fa uso del portale di Azure classico.


**Informazioni sui modelli di distribuzione di Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Diagramma di connessione
 
![Diagramma da sito a sito](./media/vpn-gateway-site-to-site-create/site2site.png "Da sito a sito")

**Strumenti e modelli di distribuzione per le connessioni da sito a sito**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Per connettere più reti virtuali senza creare una connessione a un percorso locale, vedere [Configurare una connessione tra reti virtuali per il modello di distribuzione classica](virtual-networks-configure-vnet-to-vnet-connection.md). Se si sta cercando una configurazione della connessione di tipo diverso, vedere [Topologie di connessione del gateway VPN di Azure](vpn-gateway-topology.md).

 
## Prima di iniziare

Prima di iniziare la configurazione, verificare che ci siano le condizioni seguenti:

- Un dispositivo VPN compatibile e un utente che può configurarlo. Vedere [Informazioni sui dispositivi VPN](vpn-gateway-about-vpn-devices.md). Se non si ha familiarità con la configurazione del dispositivo VPN o con gli intervalli di indirizzi IP disponibili nella configurazione di rete locale, sarà necessario coordinarsi con qualcuno che possa fornire tali dettagli.

-  Un indirizzo IP pubblico esterno per il dispositivo VPN. L’indirizzo IP non può trovarsi dietro un NAT.

- Una sottoscrizione di Azure. Se non è disponibile una sottoscrizione di Azure, è possibile attivare i [vantaggi dell'abbonamento a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/pricing/free-trial/).


## Creare la rete virtuale

1. Accedere al [portale di Azure classico](https://manage.windowsazure.com/).

2. Nell'angolo inferiore sinistro della schermata fare clic su **Nuovo**. Nel riquadro di spostamento, fare clic su **Servizi di rete**, quindi fare clic su **Rete virtuale**. Fare clic su **Custom Create** per avviare la configurazione guidata.

3. Inserire le informazioni nelle pagine seguenti per creare la rete virtuale.

## Pagina Dettagli della rete virtuale

Immettere le seguenti informazioni.

- **Nome**: assegnare un nome alla rete virtuale. Ad esempio, *EastUSVNet*. Questo nome della rete virtuale viene usato quando si distribuiscono le macchine virtuali e le istanze PaaS e pertanto è consigliabile non specificare un nome troppo complicato.
- **Indirizzo**: la località è direttamente correlata alla posizione fisica (regione) in cui si desidera che le risorse (macchine virtuali) devono risiedere. Se ad esempio si desidera che le macchine virtuali distribuite nella rete virtuale vengano posizionate fisicamente nell'area *Stati Uniti occidentali*, selezionare tale posizione. È possibile modificare il percorso associato alla rete virtuale dopo averla creata.

## Pagina Server DNS e connettività VPN

Immettere le informazioni riportate di seguito e quindi fare clic sulla freccia Avanti in basso a destra.

- **Server DNS**: immettere il nome del server DNS e l'indirizzo IP o selezionare un server DNS registrato in precedenza dal menu di scelta rapida. Questa impostazione non crea un server DNS, ma consente di specificare i server DNS da usare per la risoluzione dei nomi per la rete virtuale.
- **Configura una VPN Site-to-Site**: selezionare la casella di controllo per **configurare una VPN Site-to-Site**.
- **Rete locale**: una rete locale rappresenta la posizione fisica locale. È possibile selezionare una rete locale creata in precedenza oppure crearne una nuova. Tuttavia, se si sceglie di usare una rete locale creata in precedenza, è opportuno visitare la pagina di configurazione **Reti locali** e verificare che l'indirizzo IP (indirizzo IPv4 pubblico) per il dispositivo VPN che si usa per la connessione sia corretto.

## Pagina Connettività Site-to-Site

Se si crea una nuova rete locale, verrà visualizzata la pagina **Connettività Site-to-Site**. Se si desidera usare una rete locale creata in precedenza, questa pagina non verrà visualizzata nella procedura guidata e sarà possibile passare alla sezione successiva.

Immettere le informazioni riportate di seguito e quindi fare clic sulla freccia Avanti.

- 	**Nome**: nome da assegnare al sito di rete locale.
- 	**Indirizzo IP dispositivo VPN**: indirizzo IPv4 pubblico del dispositivo VPN locale che verrà usato per la connessione ad Azure. Il dispositivo VPN non può trovarsi dietro un NAT.
- 	**Spazio di indirizzi**: includono l'indirizzo IP iniziale e CIDR (conteggio indirizzi). Si tratta dell'opzione con cui è possibile specificare gli intervalli di indirizzi da inviare tramite il gateway di rete virtuale al percorso locale. Se un indirizzo IP di destinazione rientra negli intervalli specificati qui, verrà instradato tramite il gateway della rete virtuale.
- 	**Aggiungi spazio di indirizzi**: se si vogliono inviare più intervalli di indirizzi tramite il gateway di rete virtuale, questa opzione consente di specificare i singoli intervalli di indirizzi aggiuntivi. È possibile aggiungere o rimuovere gli intervalli in un secondo momento nella pagina **Rete locale**.

## Spazi di indirizzi della rete virtuale

Specificare l'intervallo di indirizzi che si desidera usare per la rete virtuale. Questi sono gli indirizzi IP dinamici (DIP) che verranno assegnati alle macchine virtuali e alle istanze di altri ruoli distribuiti nella rete virtuale.

È particolarmente importante selezionare un intervallo che non si sovrapponga con gli intervalli utilizzati per la rete locale. Sarà necessario coordinarsi con l'amministratore di rete. L'amministratore di rete dovrà selezionare un intervallo di indirizzi IP dallo spazio di indirizzi della rete locale da usare per la rete virtuale.

Immettere le seguenti informazioni, quindi fare clic sul segno di spunta in basso a destra per configurare la rete.

- **Spazio di indirizzi**: includono indirizzo IP iniziale e conteggio indirizzi. Verificare che gli spazi di indirizzi specificati non si sovrappongano agli spazi di indirizzi presenti nella rete locale.
- **Aggiungi subnet**: includono indirizzo IP iniziale e conteggio indirizzi. Non sono necessarie altre subnet, ma è possibile crearne una separata per le macchine virtuali che avranno DIP statici. Oppure è possibile decidere di collocare le macchine virtuali in una subnet separata dalle istanze di altri ruoli.
- **Aggiungi subnet gateway**: fare clic per aggiungere la subnet del gateway. La subnet gateway viene usata solo per il gateway di rete virtuale ed è obbligatoria per la configurazione.

Fare clic sul segno di spunta nella parte inferiore della pagina per iniziare a creare la rete virtuale. Al termine della creazione della rete virtuale, in **Stato** verrà visualizzato **Creato** nella pagina **Reti** del portale di Azure classico. Dopo aver creato la rete virtuale, è quindi possibile configurare il gateway di rete virtuale.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## Configurare il gateway di rete virtuale

Configurare quindi il gateway di rete virtuale per creare una connessione da sito a sito sicura. Vedere [Configurare un gateway di rete virtuale nel portale di Azure classico](vpn-gateway-configure-vpn-gateway-mp.md).

## Passaggi successivi

Dopo aver completato la connessione, è possibile aggiungere macchine virtuali alle reti virtuali. Per altre informazioni, vedere la documentazione relativa alle [macchine virtuali](https://azure.microsoft.com/documentation/services/virtual-machines/).

<!---HONumber=AcomDC_0518_2016-->