<properties
	pageTitle="Considerazioni di progettazione dell'identità ibrida di Azure Active Directory - Panoramica | Microsoft Azure"
	description="Panoramica e mappa dei contenuti della guida alle considerazioni sulla progettazione di identità ibrida"
	documentationCenter=""
	services="active-directory"
	authors="yuridio"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
	ms.date="04/25/2016"
	ms.author="yuridio"/>

# Considerazioni di progettazione dell'identità ibrida di Azure Active Directory

I dispositivi basati sull’utente stanno proliferando nel mondo aziendale, e le applicazioni software come servizio (SaaS) basate sul cloud sono facili da utilizzare. Come risultato, mantenere il controllo sull’accesso alle applicazioni degli utenti tra centri dati interni e piattaforme cloud è arduo. Le soluzioni di gestione delle identità di Microsoft abbracciano funzionalità locali e basate sul cloud, creando una singola identità utente per l’autenticazione e l’autorizzazione per tutte le risorse, indipendentemente dalla loro ubicazione. Tutto questo viene chiamato Identità ibrida. Esistono diverse opzioni di progettazione e configurazione per le identità ibride che utilizzano soluzioni Microsoft e in alcuni casi può essere difficile determinare quale combinazione soddisfa al meglio le necessità dell’organizzazione. Questa Guida alle considerazioni di progettazione dell’entità ibrida aiuta a capire come progettare la soluzione con identità ibrida che soddisfa al meglio le necessità commerciali e tecnologiche dell’organizzazione. Questa guida presenta una serie di passaggi e attività che possono essere seguiti per progettare una soluzione con identità ibrida che soddisfa i requisiti specifici della propria organizzazione. Nel corso dei passaggi e delle attività, la guida presenta tecnologie rilevanti e opzioni delle funzionalità disponibili per le organizzazioni per soddisfare i requisiti di qualità funzionale e del servizio (come disponibilità, scalabilità, prestazioni, maneggevolezza e sicurezza). In particolare, la finalità della guida alle considerazioni sulla progettazione dell’identità ibrida è quella di rispondere alle seguenti domande:

- Quali domande e risposte sono necessarie per guidare una specifica progettazione di una identità ibrida che permetta di avere la migliore tecnologia o risoluzione dei problemi in grado di soddisfare i propri requisiti?
- Quale sequenza di attività deve essere portata a termine per progettare una soluzione con identità ibrida che permetta di avere la migliore tecnologia o risoluzione dei problemi? 
- Quale tecnologia con identità ibrida o opzioni di configurazione sono disponibili per soddisfare i propri requisiti? Quali sono i compromessi tra queste opzioni in modo che si possa selezionare la migliore opzione per il proprio business?


## A chi si rivolge questa guida?
 CIO, CITO, Chief Identity Architects, Enterprise Architects e IT Architects responsabili per la progettazione di una soluzione con identità ibrida per medie o grandi organizzazioni.

## Come può essere d’aiuto questa guida? 
E’ possibile utilizzare la guida per capire come progettare una soluzione con identità ibrida che possa integrare un sistema di gestione delle identità basato sul cloud alla propria soluzione con identità locale. Il seguente grafico mostra un esempio di soluzione con identità ibrida che permette agli amministratori IT di integrare la propria soluzione Windows Server Active Directory in locale con Microsoft Azure Active Directory per permettere agli utenti di utilizzare Single Sign-On (SSO) nelle applicazioni situate nel cloud e in locale.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


L’illustrazione precedente è un esempio di soluzione con identità ibrida che sta spingendo i servizi cloud a integrare funzionalità locali per fornire un’esperienza unica di autenticazione all’utente finale e facilitare l’informatica nella gestione di queste risorse. Sebbene questo possa essere uno scenario molto comune, il progetto di identità ibrida di ogni organizzazione è diverso dall’esempio illustrato nella Figura 1 poiché ogni organizzazione presenta diversi requisiti. Questa guida fornisce una serie di passaggi e attività che possono essere seguiti per progettare una soluzione con identità ibrida che soddisfa i requisiti specifici della propria organizzazione. Nel corso dei passaggi e delle attività, la guida presenta tecnologie rilevanti e opzioni delle funzionalità disponibili per soddisfare i requisiti di qualità delle funzionalità e del servizio della propria organizzazione.

**Presupposti**: Avere esperienza con Windows Server, Servizi di Dominio di Active Directory e Azure Active Directory. In questo documento, si presuppone che il lettore cerchi il modo in cui queste soluzioni possano soddisfare le proprie necessità commerciali nella propria soluzione o in una soluzione integrata.

## Panoramica delle considerazioni di progettazione
Questa guida presenta una serie di passaggi e attività che possono essere seguiti per progettare una soluzione con identità ibrida che soddisfi i propri requisiti. Questi passaggi sono presentati in sequenza ordinata: Tuttavia, le considerazioni sulla progettazione presenti nei passaggi successivi possono richiedere il cambiamento di decisioni prese nei passaggi precedenti a causa di scelte di progettazione conflittuali. Nel corso della documentazione è stato fatto il possibile per segnalare all’utente potenziali conflitti di progettazione.

Si otterrà il progetto che soddisfa al meglio i propri requisiti solo dopo aver eseguito l’iterazione nei passaggi tutte le volte necessarie ad incorporare tutte le considerazioni del documento.

| Fase identità ibrida | Elenco argomenti |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Determinazione dei requisiti d’identità | [Determinazione delle necessità commerciali](active-directory-hybrid-identity-design-considerations-business-needs.md)<br>[Determinazione dei requisiti di sincronizzazione delle directory](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br>[Determinazione dei requisiti di Multi-Factor Authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br>[Definizione di una strategia di adozione dell’identità ibrida](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md) |
| Pianificare il potenziamento della sicurezza dei dati attraverso soluzioni d’identità avanzate | [Determinazione dei requisiti di protezione dati](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)<br>[Determinazione dei requisiti di gestione dei contenuti](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br>[Determinazione dei requisiti di controllo di accesso](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br>[Determinazione dei requisiti di incident response](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)<br>[Definizione della strategia di protezione dati](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) |
| Pianificare il ciclo di vita dell’identità ibrida | [Determinazione delle attività di gestione delle identità ibride](active-directory-hybrid-identity-design-considerations-hybridId-management-tasks.md)<br>[Gestione della sincronizzazione](active-directory-hybrid-identity-design-considerations-hybridId-management-tasks.md)<br>[Determinazione della strategia di adozione per la gestione delle identità ibride](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##Scaricare questa guida
È possibile scaricare una versione pdf della Guida alle considerazioni sulla progettazione di identità ibride di [raccolta Technet](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288).

                                                             

<!---HONumber=AcomDC_0427_2016-->