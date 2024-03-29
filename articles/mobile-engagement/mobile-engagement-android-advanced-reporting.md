<properties
	pageTitle="Opzioni di segnalazione avanzata per Android SDK per Azure Mobile Engagement"
	description="Descrive come eseguire la segnalazione avanzata per l'acquisizione di analisi per Android SDK per Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="05/12/2016"
	ms.author="piyushjo;ricksal" />

# Opzioni di segnalazione con Engagement in Android

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-advanced-reporting.md)

Questo argomento descrive scenari di segnalazione aggiuntivi nell'applicazione Android. È possibile scegliere alcune opzioni da applicare all'app creata nell'esercitazione [introduttiva](mobile-engagement-android-get-started.md).

## Prerequisiti

[AZURE.INCLUDE [Prerequisiti](../../includes/mobile-engagement-android-prereqs.md)]

Anche se l'esercitazione completata era volutamente diretta e semplice, è possibile scegliere tra diverse opzioni.

## Modifica delle classi `Activity`

Nell'[esercitazione introduttiva](mobile-engagement-android-get-started.md) è stato sufficiente fare in modo che le sottoclassi `*Activity` ereditassero dalle corrispondenti classi `Engagement*Activity`. Se, ad esempio, l'attività legacy estende `ListActivity`, si fa in modo di estendere `EngagementListActivity`.

> [AZURE.IMPORTANT] Quando si usa `EngagementListActivity` o `EngagementExpandableListActivity`, assicurarsi che tutte le chiamate a `requestWindowFeature(...);` vengano eseguite prima della chiamata a `super.onCreate(...);`. In caso contrario, si verificherà un arresto anomalo del sistema.

Queste classi sono incluse nella cartella `src` e possono essere copiate nel progetto. Sono reperibili anche in **JavaDoc**.

## Metodo alternativo: chiamare manualmente `startActivity()` e `endActivity()`

Se non si può o non si vuole eseguire l'overload delle classi `Activity`, è possibile avviare e terminare le attività chiamando direttamente i metodi di `EngagementAgent`.

> [AZURE.IMPORTANT] Android SDK non chiama mai il metodo `endActivity()`, neanche alla chiusura dell'applicazione (in Android le applicazioni in realtà non vengono mai chiuse). Per questo motivo, è *ALTAMENTE* consigliabile chiamare il metodo `startActivity()` nel callback `onResume` di *TUTTE* le attività e il metodo `endActivity()` nel callback `onPause()` di *TUTTE* le attività. È l'unico modo per evitare la perdita di sessioni. In caso di perdita di una sessione, il servizio Engagement non si disconnetterà mai dal back-end di Engagement, dato che il servizio rimane connesso fintanto che una sessione è in sospeso.

Di seguito è fornito un esempio:

	public class MyActivity extends Some3rdPartyActivity
	{
	  @Override
	  protected void onResume()
	  {
	    super.onResume();
	    String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
	    EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
	  }

	  @Override
	  protected void onPause()
	  {
	    super.onPause();
	    EngagementAgent.getInstance(this).endActivity();
	  }
	}

Questo esempio è molto simile alla classe `EngagementActivity` e alle relative varianti, il cui codice di origine è disponibile nella cartella `src`.

## Uso di Application.onCreate()

Il codice inserito in `Application.onCreate()` e in altri callback dell'applicazione verrà eseguito per tutti i processi dell'applicazione, incluso il servizio Engagement. È possibile che si verifichino effetti collaterali indesiderati, ad esempio allocazioni di memoria e thread superflui nel processo di Engagement oppure ricevitori o servizi di trasmissione duplicati.

Se si esegue l'override di `Application.onCreate()`, è consigliabile aggiungere il frammento di codice seguente all'inizio della funzione `Application.onCreate()`:

	 public void onCreate()
	 {
	   if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
	     return;

	   ... Your code...
	 }

È possibile eseguire la stessa operazione per `Application.onTerminate()`, `Application.onLowMemory()` e `Application.onConfigurationChanged(...)`.

È anche possibile estendere `EngagementApplication` anziché `Application`: il callback esegue il controllo del processo `Application.onCreate()` e chiama `Application.onApplicationProcessCreate()` solo se il processo corrente non è quello che ospita il servizio Engagement. Per gli altri callback vengono applicate le stesse regole.

## Tag nel file AndroidManifest.xml

Nel tag service nel file AndroidManifest.xml l'attributo `android:label` consente di scegliere il nome del servizio Engagement così come verrà presentato agli utenti finali nella schermata dei servizi in esecuzione sul telefono. È consigliabile impostare questo attributo su `"<Your application name>Service"`, ad esempio `"AcmeFunGameService"`.

Se si specifica l'attributo `android:process`, il servizio Engagement verrà eseguito nel relativo processo (l'esecuzione di Engagement nello stesso processo dell'applicazione può ridurre la reattività del thread principale o dell'interfaccia utente).

## Compilazione con ProGuard

Se si compila il pacchetto dell'applicazione con ProGuard, è necessario mantenere alcune classi. È possibile usare il frammento di codice di configurazione seguente:

	-keep public class * extends android.os.IInterface
	-keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
	<methods>;
 	}

<!---HONumber=AcomDC_0518_2016-->