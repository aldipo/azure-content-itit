<properties
	pageTitle="Segnalazione della posizione per Android SDK per Azure Mobile Engagement"
	description="Descrive come configurare la segnalazione della posizione per Android SDK per Azure Mobile Engagement"
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

# Segnalazione della posizione per Android SDK per Azure Mobile Engagement

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

Questo argomento descrive come eseguire la segnalazione della posizione per l'applicazione Android.

## Prerequisiti

[AZURE.INCLUDE [Prerequisiti](../../includes/mobile-engagement-android-prereqs.md)]

## Segnalazione della posizione

Per fare in modo che le posizioni vengano segnalate, è necessario aggiungere alcune righe di configurazione tra i tag `<application>` e `</application>`.

### Segnalazione differita della posizione

La segnalazione differita della posizione consente di segnalare il paese, l'area geografica e la località associati ai dispositivi. Questo tipo di segnalazione della posizione usa solo le posizioni di rete, sulla base dell'ID di cella o della connessione Wi-Fi. L'area del dispositivo viene segnalata al massimo una volta per sessione. Il GPS non viene mai usato, per cui l'impatto di questo tipo di segnalazione della posizione sulla batteria è minimo, se non addirittura nullo.

Le aree segnalate vengono usate per calcolare statistiche geografiche relative a utenti, sessioni, eventi ed errori. Possono essere usate anche come criteri nelle campagne Reach.

Per abilitare la segnalazione differita della posizione, è possibile utilizzare la configurazione descritta in precedenza in questa procedura:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

È necessario aggiungere anche le autorizzazioni seguenti, se mancanti:

	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Oppure è possibile continuare a utilizzare ``ACCESS_FINE_LOCATION`` se è già utilizzato nell'applicazione.

### Segnalazione della posizione in tempo reale

La segnalazione della posizione in tempo reale consente di segnalare la latitudine e la longitudine associate ai dispositivi. Per impostazione predefinita, la segnalazione differita della posizione usa solo posizioni di rete (in base all'ID di cella o alla connessione Wi-Fi) ed è attiva solo quando l'applicazione viene eseguita in primo piano, ad esempio durante una sessione.

Le posizioni in tempo reale *NON* sono usate per calcolare dati statistici. Il loro unico scopo è consentire l'uso del criterio di definizione del recinto virtuale in tempo reale <Reach-Audience-geofencing> nelle campagne Reach.

Per abilitare la segnalazione della posizione in tempo reale, aggiungere una riga di codice dove si è impostata la stringa di connessione di Engagement nell'attività dell'utilità di avvio. Il risultato avrà l'aspetto seguente:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

È necessario aggiungere anche le autorizzazioni seguenti, se mancanti:

	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Oppure è possibile continuare a utilizzare ``ACCESS_FINE_LOCATION`` se è già utilizzato nell'applicazione.

#### Segnalazione basata su GPS

Per impostazione predefinita, la segnalazione della posizione in tempo reale usa solo posizioni di rete. Per abilitare l'uso di posizioni basate su GPS (che sono molto più precise), utilizzare l’oggetto di configurazione:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

È necessario aggiungere anche le autorizzazioni seguenti, se mancanti:

	<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### Segnalazione in background

Per impostazione predefinita, la segnalazione della posizione in tempo reale è attiva solo quando l'applicazione viene eseguita in primo piano, ad esempio durante una sessione. Per abilitare la segnalazione anche in background, usare questo oggetto di configurazione:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Quando l'applicazione viene eseguita in background, vengono segnalate solo le posizioni basate sulla rete, anche se è abilitato il GPS.

La segnalazione della posizione in background verrà arrestata se l'utente riavvia il dispositivo. Per fare in modo che venga riavviata automaticamente al riavvio, aggiungere quanto segue:

	<receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
		   android:exported="false">
		<intent-filter>
		    <action android:name="android.intent.action.BOOT_COMPLETED" />
		</intent-filter>
	</receiver>

È necessario aggiungere anche le autorizzazioni seguenti, se mancanti:

	<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## Autorizzazioni Android M

A partire da Android M, alcune autorizzazioni vengono gestite in fase di esecuzione e richiedono l'approvazione dell'utente.

Le autorizzazioni di runtime verranno disattivate per impostazione predefinita per le installazioni di nuove app se la destinazione è il livello 23 dell’API Android. In caso contrario verranno attivate per impostazione predefinita.

L'utente può abilitare o disabilitare le autorizzazioni dal menu delle impostazioni del dispositivo. La disattivazione delle autorizzazioni dal menu di sistema elimina i processi in background dell'applicazione. Si tratta di un comportamento del sistema e non influisce sulla possibilità di ricevere push in background.

Nel contesto della segnalazione della posizione in Mobile Engagement, le autorizzazioni che richiedono l'approvazione in fase di esecuzione sono:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

È necessario richiedere le autorizzazioni all'utente con una finestra di dialogo di sistema standard. Se l'utente approva, è necessario specificare ``EngagementAgent`` per tenere in considerazione la modifica in tempo reale (in caso contrario la modifica verrà elaborata la volta successiva che l'utente avvia l'applicazione).

Ecco un esempio di codice da utilizzare in un'attività dell'applicazione per richiedere autorizzazioni e inoltrare il risultato, se positivo, a ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

<!---HONumber=AcomDC_0518_2016-->