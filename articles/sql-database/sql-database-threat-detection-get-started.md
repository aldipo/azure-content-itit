<properties
   pageTitle="Introduzione al rilevamento delle minacce nel database SQL"
   description="Introduzione al rilevamento delle minacce nel database SQL nel portale di Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/11/2016"
   ms.author="ronmat; ronitr"/>


# Introduzione al rilevamento delle minacce nel database SQL

## Panoramica

La funzionalità di rilevamento delle minacce individua le attività di database che indicano la presenza di potenziali minacce alla sicurezza nel database. Questa funzionalità è in anteprima ed è supportata per la versione V12 del database SQL di Azure.

Il rilevamento delle minacce offre un nuovo livello di protezione, che consente ai clienti di rilevare e rispondere alle minacce potenziali non appena si verificano, fornendo avvisi di sicurezza sulle attività anomale. Gli utenti possono esaminare gli eventi sospetti tramite il [servizio di controllo del database SQL di Azure](sql-database-auditing-get-started.md) per determinare se sono il risultato di un tentativo di accesso, una violazione o un exploit dei dati nel database. Il rilevamento delle minacce rende più semplice affrontare le minacce potenziali al database, senza dover essere esperti della sicurezza o gestire sistemi di controllo di sicurezza avanzati.

Ad esempio, la funzionalità di rilevamento delle minacce individua determinate attività anomale nel database che indicano potenziali tentativi di attacco SQL injection. L'attacco SQL injection è uno dei problemi di sicurezza comuni delle applicazioni Web su Internet, che viene usato per attaccare le applicazioni guidate dai dati. Gli autori di attacchi sfruttano le vulnerabilità delle applicazioni per introdurre istruzioni SQL dannose nei campi di immissione dell'applicazione, per violare o modificare i dati nel database.


## Configurare il rilevamento delle minacce per il database

1. Avviare il portale di Azure all'indirizzo [https://portal.azure.com](https://portal.azure.com).

2. Passare al pannello di configurazione del database SQL che si vuole monitorare. Nel pannello Impostazioni selezionare **Controllo e rilevamento delle minacce**.

	![Riquadro di spostamento][1]

3. Nel pannello di configurazione **Controllo e rilevamento delle minacce** attivare il controllo selezionando **SÌ** per visualizzare le impostazioni di rilevamento delle minacce.

	![Riquadro di spostamento][2]

4. Impostare il rilevamento delle minacce su **SÌ**.

5. Configurare l'elenco di indirizzi di posta elettronica che riceveranno avvisi di sicurezza in caso di rilevamento di attività di database anomale.

6. Fare clic su **Salva** nel pannello di configurazione di **Controllo e rilevamento delle minacce** per salvare i criteri di controllo e rilevamento delle minacce nuovi o aggiornati.

	![Riquadro di spostamento][3]


## Esaminare le attività di database anomale quando viene rilevato un evento sospetto

1. Si riceverà una notifica tramite posta elettronica al rilevamento di attività di database anomale. <br/> Il messaggio di posta elettronica fornirà informazioni sull'evento di sicurezza sospetto, inclusi la natura delle attività anomale, il nome del database, il nome del server e l'ora dell'evento. Verranno anche fornite informazioni sulle possibili cause e le azioni consigliate per analizzare e ridurre il rischio di una potenziale minaccia al database.<br/>

	![Riquadro di spostamento][4]

2. Nel messaggio di posta elettronica fare clic sul collegamento relativo al **log di controllo SQL di Azure** che avvierà il portale di Azure visualizzando i record di controllo pertinenti intorno all'ora dell'evento sospetto.

	![Riquadro di spostamento][5]

3. Fare clic sui record di controllo per visualizzare altri dettagli sulle attività di database sospette, come l'istruzione SQL, il motivo dell'errore e l'indirizzo IP del client.

	![Riquadro di spostamento][6]

4. Nel pannello dei record di controllo fare clic su **Apri in Excel** per aprire un modello di Excel preconfigurato per importare ed eseguire analisi più approfondite del log di controllo intorno all'ora dell'evento sospetto.<br/> **Nota:** in Excel 2010 o versione successiva sono richiesti Power Query e l'impostazione **Combinazione rapida**

	![Riquadro di spostamento][7]

5. Per configurare l'impostazione **Combinazione rapida** - Nella scheda della barra multifunzione**POWER QUERY** selezionare **Opzioni** per visualizzare la finestra di dialogo Opzioni. Selezionare la sezione Privacy e scegliere la seconda opzione - "Ignora i livelli di privacy per un potenziale miglioramento delle prestazioni":

	![Riquadro di spostamento][8]

6. Per caricare i log di controllo SQL, assicurarsi che i parametri nella scheda Impostazioni siano configurati correttamente e quindi selezionare "Dati" sulla barra multifunzione e fare clic sul pulsante "Aggiorna tutto".

	![Riquadro di spostamento][9]

7. I risultati vengono visualizzati nel foglio dei **log di controllo SQL** che consente di eseguire un'analisi più approfondita delle attività anomale rilevate e di ridurre l'impatto dell'evento di sicurezza nell'applicazione.


<!--Image references-->
[1]: ./media/sql-database-threat-detection-get-started/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection-get-started/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection-get-started/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection-get-started/4_td_email.png
[5]: ./media/sql-database-threat-detection-get-started/5_td_audit_records.png
[6]: ./media/sql-database-threat-detection-get-started/6_td_audit_record_details.png
[7]: ./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png
[8]: ./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png
[9]: ./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png

<!---HONumber=AcomDC_0615_2016-->