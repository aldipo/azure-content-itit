<properties 
	pageTitle="Panoramica approfondita dei piani di Azure App Service" 
	description="Informazioni sui piani di servizio app per Azure App Service e sui vantaggi offerti all'esperienza di gestione." 
	keywords="servizio app, servizio app di azure, scala, scalabile, piano di servizio app, costo del servizio app"
	services="app-service" 
	documentationCenter="" 
	authors="btardif" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/18/2016" 
	ms.author="byvinyal"/>

#Panoramica approfondita dei piani di Azure App Service#

Un **piano di servizio app** rappresenta un set di funzionalità e capacità che è possibile condividere tra più app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), tra cui app Web, per dispositivi mobili, per la logica o per le API. Questi piani supportano 5 piani tariffari (**Gratuito**, **Condiviso**, **Basic**, **Standard** e **Premium**) ognuno dei quali offre funzionalità e capacità specifiche. Le app di una stessa sottoscrizione e posizione geografica possono condividere un piano. Tutte le app che condividono un piano possono sfruttare tutte le capacità e funzionalità offerte dal relativo livello. Tutte le app associate a un piano specifico vengono eseguite sulle risorse definite dal piano. Se, ad esempio, il piano è configurato per usare due istanze "piccole" nel livello di servizio Standard, tutte le app associate a tale piano verranno eseguite in entrambe le istanze e disporranno delle funzionalità previste dal livello di servizio Standard. Le istanze del piano in cui le app sono in esecuzione sono completamente gestite e a disponibilità elevata.

Questo articolo illustra le principali caratteristiche, come livello e scalabilità, di un piano di servizio app e il modo in cui queste caratteristiche influiscono sulla gestione delle app.

##App e piani di servizio app

Un'app in un servizio app può essere associata a un solo piano per volta.

App e piani sono inclusi in un gruppo di risorse. Un gruppo di risorse funge da confine del ciclo di vita per ogni risorsa contenuta al suo interno. I gruppi di risorse consentono di gestire contemporaneamente tutte le parti di un'applicazione.

La possibilità di avere più piani di servizio app in un unico gruppo di risorse consente di allocare app diverse a risorse fisiche diverse. In questo modo, ad esempio, è possibile separare le risorse tra ambienti di sviluppo, test e produzione. Ciò potrebbe essere utile in uno scenario in cui si desidera allocare un piano con il relativo set di risorse dedicato alle app di produzione e un secondo piano agli ambienti di sviluppo e test. In questo modo, l'esecuzione di un test di carico su una nuova versione delle app non userà le stesse risorse delle app di produzione, che devono rispondere alle richieste dei clienti reali.

La presenza di più piani in un unico gruppo di risorse consente anche di definire un'applicazione che si estende in più aree geografiche. Un'app a disponibilità elevata in esecuzione in due aree includerà, ad esempio, due piani, uno per ogni area, e un'app associata a ogni piano. In una situazione di questo tipo, tutte le copie dell'app saranno associate a un unico gruppo di risorse. La disponibilità di una vista unica di un gruppo di risorse con più piani e più app semplifica la gestione, il controllo e la visualizzazione dell'integrità dell'applicazione.

## Confronto tra creazione di un nuovo piano di servizio app e uso di un piano esistente

Quando si crea una nuova app, è necessario prendere in considerazione la creazione di un nuovo gruppo di risorse, se l'app che si sta per creare rappresenta un nuovo progetto. In questo caso, la creazione di un'app, un piano e un gruppo di risorse nuovi è la scelta più appropriata.

Se l'app che si sta per creare è un componente di un'applicazione più grande, l'app Web deve essere creata nel gruppo di risorse allocato per tale applicazione.

Indipendentemente dal fatto che la nuova app sia completamente nuova o faccia parte di un'applicazione più grande, è possibile scegliere di sfruttare un piano di servizio app esistente o di crearne uno nuovo. Questa scelta dipende principalmente dalla capacità e dal carico previsto. Se la nuova app userà molte risorse e prevede fattori di scala diversi rispetto alle altre app ospitate in un piano esistente, è consigliabile isolarla in un piano dedicato.

La creazione di un nuovo piano consente di allocare un nuovo set di risorse all'app Web e offre maggiore controllo sull'allocazione delle risorse, in quanto ogni piano dispone di un set di istanze specifico.
 
La possibilità di spostare le app tra i piani consente anche di modificare la modalità di allocazione delle risorse nell'applicazione più grande.
 
Infine, se si desidera creare una nuova app in un'area diversa, in cui non è presente un piano, è necessario creare un nuovo piano nell'area per potervi ospitare l'app.

## Creare un piano di servizio app

È possibile creare un **piano di servizio app** vuoto dalla funzionalità di esplorazione dei **piani di servizio app** o durante la creazione di un'app.

A tale scopo, nel [portale di Azure](http://go.microsoft.com/fwlink/?LinkId=529715) fare clic su **NUOVO**, quindi selezionare **Web e dispositivi mobili** e infine **App Web**, **App per dispositivi mobili**, **App per la logica** o **app per le API**. ![][createWebApp]

È quindi possibile selezionare o creare il piano di servizio app per la nuova app.
  
 ![][createASP]

Per creare un nuovo piano del servizio app, fare clic su **+ Crea nuovo**, digitare il nome del **piano del servizio app** e selezionare un **Percorso** appropriato. Fare clic su **Piano tariffario** e selezionare un piano tariffario appropriato per il servizio. Selezionare **Visualizza tutto** per visualizzare più opzioni sui prezzi, ad esempio **Gratuito** e **Condiviso**. Dopo aver scelto il piano tariffario, fare clic sul pulsante **Seleziona**.
 
## Spostare un'app in un piano di servizio app diverso

È possibile spostare un'app in un piano di servizio app diverso nel [portale di Azure](https://portal.azure.com). Le app possono essere spostate tra i piani, purché i piani si trovino nello stesso gruppo di risorse e nella stessa area geografica.

Per spostare un'app in un altro piano, passare all'app che si vuole spostare. Nel menu Impostazioni fare clic su **Cambia il piano di servizio app**.
 
Verrà aperto il pannello di selezione Piano di servizio app. A questo punto, è possibile scegliere un piano esistente o crearne uno nuovo. Vengono visualizzati solo i piani validi, nello stesso gruppo di risorse e nella stessa località geografica.

![][change]

Si noti che è previsto un piano tariffario diverso per ogni piano. Quando si sposta un sito dal piano **Gratuito** al piano **Standard**, l'app può sfruttare tutte le funzionalità e le risorse del piano **Standard**.

## Clonare un'app in un piano di servizio app diverso
Se si vuole spostare l'app in un'area diversa, in alternativa consiste nel clonarla. Con la clonazione si crea una copia dell'app in un piano di servizio app nuovo o esistente o nell'ambiente del servizio app in un'area qualsiasi.

 ![][appclone]
 
È possibile trovare **Clona app** nel menu **Strumenti**.

La clonazione presenta alcune limitazioni. Altre informazioni sono disponibili [qui](../app-service-web/app-service-web-app-cloning-portal.md)

## Scalare un piano di servizio app

Sono disponibili tre modi per ridimensionare un piano:

- Modificare il relativo **piano tariffario**. Un piano con livello **Basic**, ad esempio, può essere convertito in un livello **Standard** o **Premium** e tutte le app associate al piano potranno sfruttare le funzionalità offerte dal nuovo livello di servizio.
- Modificare la **dimensione dell'istanza** del piano: ad esempio, un piano con livello **Basic** e istanze **piccole** può essere modificato per usare istanze **grandi**. Tutte le app associate al piano potranno sfruttare le risorse di memoria e CPU aggiuntive offerte dall'istanza di dimensione maggiore.
- Modificare il numero di **istanze del piano**. Ad esempio, un piano **Standard** con 3 istanze può essere aumentato fino a 10 istanze, mentre un piano **Premium** può essere aumentato a 20 istanze, a seconda della disponibilità. Tutte le app associate a tale piano potranno sfruttare le risorse di memoria e CPU aggiuntive offerte dal numero di istanze maggiore.

È possibile modificare le dimensioni del piano tariffario e delle istanze facendo clic sulla voce **Aumenta prestazioni** in Impostazioni per l'app o per il piano di servizio app. Le modifiche verranno applicate al **Piano di servizio app** e interessano tutte le app ospitate dal piano stesso.
 
 ![][pricingtier]

##Riepilogo

I piani di servizio app rappresentano un set di funzionalità e capacità che è possibile condividere tra le app. I piani di servizio app offrono la flessibilità per allocare app specifiche a un determinato set di risorse e ottimizzare ulteriormente l'utilizzo delle risorse di Azure. In questo modo, se si desidera risparmiare denaro sull'ambiente di test, è possibile condividere un piano tra più app. È anche possibile ottimizzare la velocità effettiva nell'ambiente di produzione scalandolo in più aree e piani.

## Modifiche apportate

* Per una Guida per la modifica di siti Web al servizio App vedere: [servizio App Azure e il relativo impatto sui servizi di Azure esistente](http://go.microsoft.com/fwlink/?LinkId=529714)
   
[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png

<!---HONumber=AcomDC_0518_2016-->