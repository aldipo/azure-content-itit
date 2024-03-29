<properties
   pageTitle="Uso di SAP in macchine virtuali (VM) Linux | Microsoft Azure"
   description="Informazioni sull'uso di SAP in macchine virtuali (VM) Linux in Microsoft Azure"
   services="virtual-machines-linux,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="juergent"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="na"
   ms.date="05/17/2016"
   ms.author="sedusch"/>

# Uso di SAP in macchine virtuali (VM) Linux

[767598]: https://service.sap.com/sap/support/notes/767598
[773830]: https://service.sap.com/sap/support/notes/773830
[826037]: https://service.sap.com/sap/support/notes/826037
[965908]: https://service.sap.com/sap/support/notes/965908
[1139904]: https://service.sap.com/sap/support/notes/1139904
[1173395]: https://service.sap.com/sap/support/notes/1173395
[1245200]: https://service.sap.com/sap/support/notes/1245200
[1409604]: https://service.sap.com/sap/support/notes/1409604
[1558958]: https://service.sap.com/sap/support/notes/1558958
[1585981]: https://service.sap.com/sap/support/notes/1585981
[1588316]: https://service.sap.com/sap/support/notes/1588316
[1590719]: https://service.sap.com/sap/support/notes/1590719
[1605680]: https://service.sap.com/sap/support/notes/1605680
[1619720]: https://service.sap.com/sap/support/notes/1619720
[1619726]: https://service.sap.com/sap/support/notes/1619726
[1619967]: https://service.sap.com/sap/support/notes/1619967
[1750510]: https://service.sap.com/sap/support/notes/1750510
[1752266]: https://service.sap.com/sap/support/notes/1752266
[1757924]: https://service.sap.com/sap/support/notes/1757924
[1757928]: https://service.sap.com/sap/support/notes/1757928
[1758182]: https://service.sap.com/sap/support/notes/1758182
[1758496]: https://service.sap.com/sap/support/notes/1758496
[1772688]: https://service.sap.com/sap/support/notes/1772688
[1814258]: https://service.sap.com/sap/support/notes/1814258
[1882376]: https://service.sap.com/sap/support/notes/1882376
[1909114]: https://service.sap.com/sap/support/notes/1909114
[1922555]: https://service.sap.com/sap/support/notes/1922555
[1928533]: https://service.sap.com/sap/support/notes/1928533
[1941500]: https://service.sap.com/sap/support/notes/1941500
[1956005]: https://service.sap.com/sap/support/notes/1956005
[1973241]: https://service.sap.com/sap/support/notes/1973241
[1999351]: https://service.sap.com/sap/support/notes/1999351
[2015553]: https://service.sap.com/sap/support/notes/2015553
[2039619]: https://service.sap.com/sap/support/notes/2039619
[2121797]: https://service.sap.com/sap/support/notes/2121797
[2134316]: https://service.sap.com/sap/support/notes/2134316
[2178632]: https://service.sap.com/sap/support/notes/2178632
[2191498]: https://service.sap.com/sap/support/notes/2191498
[2233094]: https://service.sap.com/sap/support/notes/2233094

[azure-portal]: https://portal.azure.com
[azure-script-ps]: https://go.microsoft.com/fwlink/p/?LinkID=395017

[planning-guide]: virtual-machines-linux-sap-planning-guide.md "SAP NetWeaver in Macchine virtuali Linux - Guida alla pianificazione e all'implementazione"
[planning-guide-classic]: virtual-machines-windows-classic-sap-planning-guide.md
[deployment-guide]: virtual-machines-linux-sap-deployment-guide.md "SAP NetWeaver in Macchine virtuali Linux - Guida alla distribuzione"
[deployment-guide-classic]: virtual-machines-windows-classic-sap-deployment-guide.md
[dbms-guide]: virtual-machines-linux-sap-dbms-guide.md "SAP NetWeaver in Macchine virtuali Linux - Guida alla distribuzione DBMS"
[dbms-guide-classic]: virtual-machines-windows-classic-sap-dbms-guide.md
[dr-guide-classic]: virtual-machines-windows-classic-sap-dr-guide.md
[ha-guide-classic]: virtual-machines-windows-classic-sap-ha-guide.md

[getting-started]: virtual-machines-linux-sap-getting-started-arm.md
[getting-started-windows-classic]: virtual-machines-windows-classic-sap-getting-started.md

[getting-started-windows-classic-dbms]: virtual-machines-windows-classic-sap-getting-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-planning]: virtual-machines-windows-classic-sap-getting-started.md#f2a5e9d8-49e4-419e-9900-af783173481c
[getting-started-windows-classic-deployment]: virtual-machines-windows-classic-sap-getting-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]: virtual-machines-windows-classic-sap-getting-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]: virtual-machines-windows-classic-sap-getting-started.md#4bb7512c-0fa0-4227-9853-4004281b1037

[getting-started-planning]: virtual-machines-linux-sap-getting-started-arm.md#3da0389e-708b-4e82-b2a2-e92f132df89c
[getting-started-deployment]: virtual-machines-linux-sap-getting-started-arm.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-dbms]: virtual-machines-linux-sap-getting-started-arm.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82

[deployment-guide-2.2]: virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 "Risorse SAP"
[deployment-guide-3]: virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e "Scenari di distribuzione di VM per SAP in Microsoft Azure"
[deployment-guide-3.1.2]: virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab "Distribuzione di una VM con un'immagine personalizzata"
[deployment-guide-3.2]: virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 "Scenario 1: Distribuzione di una VM da Azure Marketplace per SAP"
[deployment-guide-3.3]: virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 "Scenario 2: Distribuzione di una VM con un'immagine personalizzata per SAP"
[deployment-guide-3.4]: virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 "Scenario 3: Spostamento di una VM locale usando un disco rigido virtuale di Azure non generalizzato con SAP"
[deployment-guide-4.1]: virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 "Distribuzione dei cmdlet di Azure PowerShell"
[deployment-guide-4.2]: virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e "Scaricare e importare i cmdlet di PowerShell pertinenti per SAP"
[deployment-guide-4.3]: virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc "Aggiungere la VM a un dominio locale (solo per Windows)"
[deployment-guide-4.4]: virtual-machines-linux-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d "Scaricare, installare e abilitare l'agente di macchine virtuali di Azure"
[deployment-guide-4.4.2]: virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 "Linux"
[deployment-guide-4.5]: virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca "Configurare l'estensione di monitoraggio avanzato di Azure per SAP"
[deployment-guide-4.5.1]: virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 "Azure PowerShell"
[deployment-guide-4.5.2]: virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f "Interfaccia della riga di comando di Azure"
[deployment-guide-5.1]: virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 "Controllo dello stato di preparazione del monitoraggio avanzato di Azure per SAP"
[deployment-guide-5.2]: virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 "Controllo dell'integrità della configurazione dell'infrastruttura di monitoraggio di Azure"
[deployment-guide-5.3]: virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 "Risoluzione di altri problemi dell'infrastruttura di monitoraggio di Azure per SAP"
[deployment-guide-install-vm-agent-windows]: virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-configure-proxy]: virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d "Configurare il proxy"
[deployment-guide-configure-monitoring-scenario-1]: virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b "Configurare il monitoraggio"
[deployment-guide-troubleshooting-chapter]: virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b "Controlli e risoluzione dei problemi per la configurazione del monitoraggio end-to-end per SAP in Azure"
[deployment-guide-figure-100]: ./media/virtual-machines-linux-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-300]: ./media/virtual-machines-linux-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]: ./media/virtual-machines-linux-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-500]: ./media/virtual-machines-linux-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-600]: ./media/virtual-machines-linux-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-700]: ./media/virtual-machines-linux-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]: ./media/virtual-machines-linux-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-50]: ./media/virtual-machines-linux-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-900]: ./media/virtual-machines-linux-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-1000]: ./media/virtual-machines-linux-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-1100]: ./media/virtual-machines-linux-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]: ./media/virtual-machines-linux-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]: ./media/virtual-machines-linux-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-1400]: ./media/virtual-machines-linux-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-5]: virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-6]: virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-7]: virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-11]: virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-14]: virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-azure-cli-installed]: virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]: virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda

[planning-guide-1.2]: virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff "Risorse"
[planning-guide-2.1]: virtual-machines-linux-sap-planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 "Solo cloud: distribuzioni di macchine virtuali in Azure senza dipendenze dalla rete locale dei clienti"
[planning-guide-2.2]: virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 "Più sedi locali: distribuzione di una singola VM o di più VM SAP in Azure che devono essere completamente integrate con la rete locale"
[planning-guide-3.1]: virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a "Aree di Azure"
[planning-guide-3.2]: virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 "Concetto di macchina virtuale di Microsoft Azure"
[planning-guide-3.2.1]: virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 "Domini di errore"
[planning-guide-3.2.2]: virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 "Domini di aggiornamento"
[planning-guide-3.2.3]: virtual-machines-linux-sap-planning-guide.md#18810088-f9be-4c97-958a-27996255c665 "Set di disponibilità di Azure"
[planning-guide-3.3.2]: virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Archiviazione Premium di Azure"
[planning-guide-5.1.1]: virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 "Spostamento di una VM da locale ad Azure con un disco non generalizzato"
[planning-guide-5.1.2]: virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c "Distribuzione di una VM con un'immagine specifica del cliente"
[planning-guide-5.2]: virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 "Preparazione di VM con SAP per Azure"
[planning-guide-5.2.1]: virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef "Preparazione per lo spostamento di una VM da locale ad Azure con un disco non generalizzato"
[planning-guide-5.2.2]: virtual-machines-linux-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 "Preparazione per la distribuzione di una VM con un'immagine specifica del cliente per SAP"
[planning-guide-5.3.1]: virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 "Differenza tra un disco di Azure e un'immagine di Azure"
[planning-guide-5.3.2]: virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a "Caricamento di un disco rigido virtuale da locale ad Azure"
[planning-guide-5.4.2]: virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 "Copia di dischi tra account di archiviazione di Azure"
[planning-guide-5.5.1]: virtual-machines-linux-sap-planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 "Struttura di VM/dischi rigidi virtuali per distribuzioni SAP"
[planning-guide-5.5.3]: virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d "Impostazione del montaggio automatico per dischi collegati"
[planning-guide-7]: virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 "Concetti della distribuzione solo cloud di istanze SAP"
[planning-guide-7.1]: virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 "Scenario di formazione/dimostrativo di una singola VM con SAP NetWeaver"
[planning-guide-9.1]: virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 "Soluzione di monitoraggio di Azure per SAP"
[planning-guide-11.4.1]: virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 "Disponibilità elevata per SAP Application Server"
[planning-guide-11.5]: virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f "Uso dell'avvio automatico per le istanze di SAP"
[planning-guide-microsoft-azure-networking]: virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd "Rete di Microsoft Azure"
[planning-guide-storage-microsoft-azure-storage-and-data-disks]: virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f "Archiviazione: Archiviazione di Microsoft Azure e dischi dati"
[planning-guide-azure-premium-storage]: virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 "Archiviazione Premium di Azure"
[planning-guide-figure-100]: ./media/virtual-machines-linux-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-200]: ./media/virtual-machines-linux-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-300]: ./media/virtual-machines-linux-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-400]: ./media/virtual-machines-linux-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]: ./media/virtual-machines-linux-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]: ./media/virtual-machines-linux-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]: ./media/virtual-machines-linux-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-figure-1300]: ./media/virtual-machines-linux-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]: ./media/virtual-machines-linux-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]: ./media/virtual-machines-linux-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]: ./media/virtual-machines-linux-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]: ./media/virtual-machines-linux-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-2100]: ./media/virtual-machines-linux-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]: ./media/virtual-machines-linux-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]: ./media/virtual-machines-linux-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]: ./media/virtual-machines-linux-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]: ./media/virtual-machines-linux-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]: ./media/virtual-machines-linux-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]: ./media/virtual-machines-linux-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]: ./media/virtual-machines-linux-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]: ./media/virtual-machines-linux-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-3000]: ./media/virtual-machines-linux-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]: ./media/virtual-machines-linux-sap-planning-guide/3200-sap-ha-with-sql.png

[dbms-guide-2]: virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 "Struttura di una distribuzione RDBMS"
[dbms-guide-2.1]: virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f "Memorizzazione nella cache per VM e dischi rigidi virtuali"
[dbms-guide-2.2]: virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 "RAID software"
[dbms-guide-2.3]: virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 "Archiviazione di Microsoft Azure"
[dbms-guide-3]: virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 "Disponibilità elevata e ripristino di emergenza con VM di Azure"
[dbms-guide-5]: virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 "Specifiche per RDBMS SQL Server"
[dbms-guide-5.5.1]: virtual-machines-linux-sap-dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 "SQL Server 2012 SP1 CU4 e versioni successive"
[dbms-guide-5.5.2]: virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b "SQL Server 2012 SP1 CU3 e versioni precedenti"
[dbms-guide-5.6]: virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 "Uso di immagini di SQL Server al di fuori di Microsoft Azure Marketplace"
[dbms-guide-5.8]: virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 "Riepilogo generale su SQL Server per SAP in Azure"
[dbms-guide-8.4.1]: virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 "Configurazione dell'archiviazione"
[dbms-guide-8.4.2]: virtual-machines-linux-sap-dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d "Backup e ripristino"
[dbms-guide-8.4.3]: virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c "Considerazioni sulle prestazioni per il backup e il ripristino"
[dbms-guide-8.4.4]: virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 "Altri"
[dbms-guide-figure-100]: ./media/virtual-machines-linux-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]: ./media/virtual-machines-linux-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]: ./media/virtual-machines-linux-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]: ./media/virtual-machines-linux-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]: ./media/virtual-machines-linux-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]: ./media/virtual-machines-linux-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]: ./media/virtual-machines-linux-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]: ./media/virtual-machines-linux-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]: ./media/virtual-machines-linux-sap-dbms-guide/900-sap-cache-server-on-premises.png

[dbms-guide-900-sap-cache-server-on-premises]: virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[Logo_Windows]: ./media/virtual-machines-linux-sap-shared/Windows.png
[Logo_Linux]: ./media/virtual-machines-linux-sap-shared/Linux.png

[vm-size-specs]: virtual-machines-size-specs.md
[azure-subscription-service-limits-subscription]: azure-subscription-service-limits.md#subscription
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]: vpn-gateway-cross-premises-options.md
[vpn-gateway-site-to-site-create]: vpn-gateway-site-to-site-create.md
[virtual-machines-deploy-rmtemplates-azure-cli]: virtual-machines-deploy-rmtemplates-azure-cli.md "Distribuire e gestire le macchine virtuali usando modelli di Gestione risorse di Azure e l'interfaccia della riga di comando di Azure"
[virtual-machines-deploy-rmtemplates-powershell]: virtual-machines-deploy-rmtemplates-powershell.md "Gestire le macchine virtuali usando Gestione risorse di Azure e PowerShell"
[virtual-machines-linux-capture-image-resource-manager]: virtual-machines-linux-capture-image-resource-manager.md
[virtual-machines-manage-availability]: virtual-machines-manage-availability.md
[virtual-machines-linux-how-to-attach-disk]: virtual-machines-linux-how-to-attach-disk.md
[virtual-networks-reserved-private-ip]: virtual-networks-static-private-ip-arm-ps.md
[virtual-machines-sql-server-infrastructure-services]: virtual-machines-sql-server-infrastructure-services.md
[storage-redundancy]: storage-redundancy.md
[storage-scalability-targets]: storage-scalability-targets.md
[virtual-networks-manage-dns-in-vnet]: virtual-networks-manage-dns-in-vnet.md
[resource-groups-networking]: resource-groups-networking.md
[virtual-networks-static-private-ip-arm-pportal]: virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-multiple-nics]: virtual-networks-multiple-nics.md
[virtual-network-deploy-multinic-arm-template]: virtual-network-deploy-multinic-arm-template.md
[virtual-network-deploy-multinic-arm-ps]: virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-cli]: virtual-network-deploy-multinic-arm-cli.md
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-about-vpn-devices]: vpn-gateway-about-vpn-devices.md
[vpn-gateway-vpn-faq]: vpn-gateway-vpn-faq.md
[vpn-gateway-create-site-to-site-rm-powershell]: vpn-gateway-create-site-to-site-rm-powershell.md
[powershell-install-configure]: powershell-install-configure.md
[xplat-cli]: xplat-cli.md
[virtual-machines-deploy-rmtemplates-azure-cli]: virtual-machines-deploy-rmtemplates-azure-cli.md
[xplat-cli-azure-resource-manager]: xplat-cli-azure-resource-manager.md
[virtual-machines-linux-create-upload-vhd-suse]: virtual-machines-linux-create-upload-vhd-suse.md
[virtual-machines-linux-capture-image-resource-manager]: virtual-machines-linux-capture-image-resource-manager.md
[storage-use-azcopy]: storage-use-azcopy.md
[virtual-machines-linux-capture-image-resource-manager-capture]: virtual-machines-linux-capture-image-resource-manager.md#capture-the-vm
[storage-azure-cli]: storage-azure-cli.md
[storage-powershell-guide-full-copy-vhd]: storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-azure-cli-copy-blobs]: storage-azure-cli.md#copy-blobs
[virtual-machines-linux-agent-user-guide]: virtual-machines-linux-agent-user-guide.md
[virtual-machines-size-specs]: virtual-machines-size-specs.md
[virtual-machines-sql-server-performance-best-practices]: virtual-machines-sql-server-performance-best-practices.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]: virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md
[virtual-machines-sql-server-alwayson-availability-groups-powershell]: virtual-machines-sql-server-alwayson-availability-groups-powershell.md
[virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener]: virtual-machines-sql-server-configure-ilb-alwayson-availability-group-listener.md
[virtual-networks-configure-vnet-to-vnet-connection]: virtual-networks-configure-vnet-to-vnet-connection.md
[azure-subscription-service-limits]: azure-subscription-service-limits.md
[virtual-machines-configuring-oracle-data-guard]: virtual-machines-configuring-oracle-data-guard.md
[virtual-machines-linux-configure-raid]: virtual-machines-linux-configure-raid.md
[virtual-machines-attach-disk-preview]: virtual-machines-attach-disk-preview.md
[virtual-machines-workload-template-sql-alwayson]: virtual-machines-workload-template-sql-alwayson.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]: virtual-machines-linux-how-to-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux
[resource-group-authoring-templates]: resource-group-authoring-templates.md
[virtual-machines-linux-update-agent]: virtual-machines-linux-update-agent.md
[virtual-machines-linux-create-upload-vhd-step-1]: virtual-machines-linux-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[deploy-template-powershell]: resource-group-template-deploy.md#deploy-with-powershell
[deploy-template-cli]: resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]: resource-group-template-deploy.md#deploy-with-the-preview-portal
[virtual-networks-udr-overview]: virtual-networks-udr-overview.md
[resource-group-overview]: resource-group-overview.md
[virtual-machines-linux-agent-user-guide-command-line-options]: virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]: virtual-machines-linux-capture-image-resource-manager.md
[virtual-networks-udr-overview]: virtual-networks-udr-overview.md
[virtual-networks-nsg]: virtual-networks-nsg.md
[storage-premium-storage-preview-portal]: storage-premium-storage-preview-portal.md
[storage-introduction]: storage-introduction.md
[virtual-machines-upload-image-windows-resource-manager]: virtual-machines-upload-image-windows-resource-manager.md
[virtual-machines-azure-resource-manager-architecture]: virtual-machines-azure-resource-manager-architecture.md
[virtual-machines-windows-tutorial]: virtual-machines-windows-tutorial.md
[virtual-networks-create-vnet-arm-pportal]: virtual-networks-create-vnet-arm-pportal.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]: virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md
[virtual-machines-linux-tutorial]: virtual-machines-linux-tutorial.md

[msdn-set-azurermvmaemextension]: https://msdn.microsoft.com/library/azure/mt670598.aspx

[virtual-machines-azurerm-versus-azuresm]: virtual-machines-azurerm-versus-azuresm.md

[install-extension-cli]: https://github.com/Azure/azure-linux-extensions/blob/master/AzureEnhancedMonitor/README.md

[azure-quickstart-templates-github]: https://github.com/Azure/azure-quickstart-templates
[sap-templates-2-tier-marketplace-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-user-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[templates-101-simple-windows-vm]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[template-201-vm-from-specialized-vhd]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd

[sap-pam]: https://support.sap.com/pam "Product Availability Matrix SAP"

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Modello di distribuzione classica.

Cloud computing è un termine ampiamente diffuso che sta assumendo un'importanza sempre più rilevante nel settore IT, dalle piccole imprese fino alle grandi aziende e alle multinazionali. Microsoft Azure è la piattaforma di servizi cloud di Microsoft che offre un'ampia gamma di nuove possibilità. I clienti possono infatti effettuare rapidamente il provisioning e il deprovisioning di applicazioni come servizi cloud, senza essere soggetti a eventuali limiti tecnici o di budget. Anziché investire tempo e denaro nell'infrastruttura hardware, le aziende possono concentrarsi sull'applicazione, sui processi aziendali e sui vantaggi per clienti e utenti.

Grazie ai servizi inclusi in Macchine virtuali di Microsoft Azure, Microsoft offre una piattaforma di infrastruttura distribuita come servizio (IaaS) completa. Le applicazioni basate su SAP NetWeaver sono supportate in Macchine virtuali di Azure (IaaS). I white paper seguenti descrivono come pianificare e implementare applicazioni basate su SAP NetWeaver nella piattaforma Microsoft Azure.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

##  <a name="3da0389e-708b-4e82-b2a2-e92f132df89c"></a>Pianificazione e implementazione

Titolo: SAP NetWeaver in Macchine virtuali Linux - Guida alla pianificazione e all'implementazione

Riepilogo: Si tratta del primo documento da esaminare se si sta valutando la possibilità di eseguire SAP NetWeaver in Macchine virtuali di Azure. Questa Guida alla pianificazione e implementazione permette di valutare se è possibile distribuire un sistema basato su SAP NetWeaver esistente o pianificato in un ambiente di Macchine virtuali di Azure. Illustra vari scenari di distribuzione di SAP NetWeaver e include configurazioni SAP specifiche per Azure. Il documento elenca e descrive tutte le informazioni di configurazione necessarie relative a SAP/Azure per eseguire un landscape SAP ibrido. Vengono anche descritte le misure che è possibile adottare per garantire la disponibilità elevata dei sistemi basati su SAP NetWeaver nella soluzione IaaS.

Ultimo aggiornamento: marzo 2016

[Questa guida è disponibile qui][planning-guide]
## <a name="6aadadd2-76b5-46d8-8713-e8d63630e955"></a>Distribuzione

Titolo: SAP NetWeaver in Macchine virtuali Linux - Guida alla distribuzione

Riepilogo: In questo documento vengono fornite indicazioni generali sulle procedure per la distribuzione del software SAP NetWeaver nelle macchine virtuali in Azure. Questo documento descrive tre scenari di distribuzione specifici, con particolare attenzione all'abilitazione delle estensioni di monitoraggio di Azure per SAP, inclusi alcuni suggerimenti per la risoluzione dei problemi delle estensioni di monitoraggio di Azure per SAP. Prima di passare a questo documento, è necessario aver letto la Guida alla pianificazione e all'implementazione.

Ultimo aggiornamento: marzo 2016

[Questa guida è disponibile qui][deployment-guide]

## <a name="1343ffe1-8021-4ce6-a08d-3a1553a4db82"></a>Guida alla distribuzione DBMS

Titolo: SAP NetWeaver in Macchine virtuali Linux - Guida alla distribuzione DBMS

Riepilogo: Questo documento presenta le considerazioni sulla pianificazione e l'implementazione per i sistemi DBMS che dovranno essere eseguiti con SAP. Nella prima parte vengono elencate e presentate alcune considerazioni di carattere generale. Le parti successive descrivono invece le distribuzioni supportate da SAP di diversi sistemi DBMS in Azure. I sistemi DBMS presentati sono SQL Server, SAP ASE e Oracle. In queste sezioni più specifiche vengono illustrate le considerazioni di cui è necessario tenere conto quando vengono eseguiti sistemi SAP con tali sistemi DBMS in Azure. Vengono presentati argomenti quali i metodi di backup e disponibilità elevata supportati dai diversi sistemi DBMS in Azure per l'utilizzo con applicazioni SAP.

Ultimo aggiornamento: marzo 2016

[Questa guida è disponibile qui][dbms-guide]

<!---HONumber=AcomDC_0518_2016-->