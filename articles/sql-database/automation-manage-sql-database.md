<properties
	pageTitle="Gestire i database SQL usando Automazione di Azure"
	description="Informazioni su come è possibile usare il servizio Automazione di Azure per gestire database SQL su vasta scala."
	services="sql-database, automation"
	documentationCenter=""
	authors="jodoglevy"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/26/2016"
	ms.author="jolevy"/>



#Gestire i database SQL usando Automazione di Azure

Questa guida fornisce un'introduzione al servizio Automazione di Azure e ne illustra l'utilizzo per semplificare la gestione dei database SQL.


## Informazioni su Automazione di Azure

[Automazione di Azure](https://azure.microsoft.com/services/automation/) è un servizio di Azure che consente di semplificare la gestione del cloud tramite l'automazione dei processi. Usando Automazione di Azure, è possibile automatizzare attività a esecuzione prolungata, manuali, soggette a errori e ripetute frequentemente per migliorare l'affidabilità, l'efficienza e i tempi di esecuzione.

Automazione di Azure offre un motore di esecuzione del flusso di lavoro a elevata disponibilità e affidabilità che garantisce la scalabilità necessaria per rispondere alle esigenze aziendali in base alla crescita dell'organizzazione. In Automazione di Azure i processi possono essere attivati manualmente, da sistemi di terze parti o a intervalli pianificati in modo che le attività abbiano luogo esattamente quando necessario.

Il servizio permette di ridurre i costi operativi e di liberare risorse dello staff IT/DevOp consentendo loro di concentrarsi su attività a valore aggiunto grazie al trasferimento delle attività di gestione del cloud all'esecuzione automatica di Automazione di Azure.


## Come può Automazione di Azure aiutare nella gestione dei database SQL?

Il database SQL di Azure può essere gestito in Automazione di Azure usando i [cmdlet di PowerShell del database SQL di Azure](https://msdn.microsoft.com/library/dn546723.aspx) disponibili negli [strumenti di Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). I cmdlet di PowerShell per database SQL di Azure sono predefiniti in Automazione di Azure, per consentire l'esecuzione di tutte le attività di gestione dei database SQL dall'interno del servizio. È anche possibile abbinare tali cmdlet in Automazione di Azure ai cmdlet per altri servizi di Azure per automatizzare attività complesse in tutti i servizi di Azure e nei sistemi di terze parti.

Automazione di Azure è in grado di comunicare direttamente con i server SQL tramite l'invio di comandi SQL da PowerShell.

La [raccolta di Runbook di Automazione di Azure](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contiene diversi Runbook del team di prodotto e della community per iniziare ad automatizzare la gestione dei database SQL di Azure, altri servizi di Azure e sistemi di terze parti. I Runbook della raccolta includono i seguenti:

 * [Eseguire query SQL su un database SQL Server](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Scalabilità verticale (verso l’alto o il basso) di un database di SQL Azure in una pianificazione](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Troncare una tabella SQL se il database si avvicina alla dimensione massima](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Tabelle dell’indice in un database SQL di Azure se sono molto frammentati](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## Passaggi successivi

A questo punto, dopo aver appreso le nozioni di base di Automazione di Azure e come può essere usato per gestire i database SQL di Azure, seguire i collegamenti seguenti per altre informazioni su Automazione di Azure.

- [Panoramica di Automazione di Azure](../automation/automation-intro.md)
- [Il primo runbook](../automation/automation-first-runbook-graphical.md)
- [Mappa di apprendimento di Automazione di Azure](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Automazione di Azure: SQL Agent nel cloud](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/). 
 

<!---HONumber=AcomDC_0615_2016-->