<properties 
   pageTitle="Come creare gruppi di sicurezza di rete in modalità ARM mediante il portale di Azure | Microsoft Azure"
   description="Informazioni su come creare e distribuire gruppi di sicurezza di rete in ARM mediante il portale di Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/04/2016"
   ms.author="telmos" />

# Come gestire gruppi di sicurezza di rete tramite il portale di Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Questo articolo illustra il modello di distribuzione Gestione risorse. È anche possibile creare gruppi di sicurezza di rete con il [modello di distribuzione classica](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

I comandi di esempio PowerShell riportati di seguito prevedono un ambiente semplice già creato in base allo scenario precedente. Se si desidera eseguire i comandi così come sono visualizzati in questo documento, creare innanzitutto l'ambiente di test distribuendo [questo modello](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), fare clic su **Distribuisci in Azure**, sostituire i valori di parametro predefiniti, se necessario e seguire le istruzioni nel portale. La procedura seguente usa **RG-NSG** come nome del gruppo di risorse in cui è stato distribuito il modello.

## Creare il gruppo di sicurezza di rete NSG-FrontEnd

Per creare il gruppo di sicurezza di rete (NSG, Network Security Group) **NSG-FrontEnd** come illustrato nello scenario precedente, seguire questa procedura.

1. Da un browser, passare a http://portal.azure.com e, se necessario, accedere con l'account Azure.
2. Fare clic su **Sfoglia >** > **Gruppi di sicurezza di rete**.

    ![Portale di Azure - Gruppi di sicurezza di rete](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)

3. Nel pannello **Gruppi di sicurezza di rete** fare clic su **Aggiungi**.
  
    ![Portale di Azure - Gruppi di sicurezza di rete](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)

4. Nel pannello **Crea gruppo di sicurezza di rete** creare un NSG denominato *NSG-FrontEnd* nel gruppo di risorse *RG-NSG*, quindi fare clic su **Crea**.

	![Portale di Azure - Gruppi di sicurezza di rete](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## Creare regole in un gruppo di sicurezza di rete esistente

Per creare regole in un gruppo di sicurezza di rete esistente dal portale di Azure, seguire questa procedura.

2. Fare clic su **Sfoglia >** > **Gruppi di sicurezza di rete**.

3. Nell'elenco dei gruppi di sicurezza di rete, fare clic su **NSG-FrontEnd** > **Regole di sicurezza in ingresso**

	![Portale di Azure - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. Nell'elenco delle **regole di sicurezza in ingresso**, fare clic su **Aggiungi**.

	![Portale di Azure - Aggiungi regola](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5. Nel pannello **Aggiungi regola di sicurezza in ingresso**, creare una regola denominata *web-rule* con priorità *200* che consente l'accesso tramite *TCP* alla porta *80* a qualsiasi VM da qualsiasi origine, quindi fare clic su **OK**. Notare che la maggior parte di queste impostazioni ha già i valori predefiniti.

	![Portale di Azure - Impostazioni delle regole](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. La nuova regola del gruppo di sicurezza di rete verrà visualizzata dopo pochi secondi.

	![Portale di Azure - Nuova regola](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)

7. Ripetere i passaggi fino al 6 per creare una regola in entrata denominata *rdp-rule* con priorità *250*, che consente l'accesso tramite *TCP* alla porta *3389* per qualsiasi VM da qualsiasi origine.

## Associare il gruppo di sicurezza di rete alla subnet FrontEnd

1. Fare clic su **Sfoglia >** > **Gruppi di risorse** > **RG-NSG**.
2. Nel pannello **RG-NSG** fare clic su **...** > **TestVNet**.

	![Portale di Azure - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)

3. Nel pannello **Impostazioni** fare clic su **Subnet** > **FrontEnd** > **Gruppo di sicurezza di rete** > **NSG-FrontEnd**.

	![Portale di Azure - Impostazioni della subnet](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)

4. Nel pannello **FrontEnd** fare clic su **Salva**.

	![Portale di Azure - Impostazioni della subnet](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## Creare il gruppo di sicurezza di rete NSG-BackEnd

Per creare il gruppo di sicurezza di rete **NSG-BackEnd** e associarlo alla subnet **BackEnd**, seguire questa procedura.

1. Ripetere la procedura illustrata in [Creare il gruppo di sicurezza di rete NSG-FrontEnd](#Create-the-NSG-FrontEnd-NSG) per creare un NSG denominato *NSG-BackEnd*
2. Ripetere la procedura illustrata [Creare regole in un gruppo di sicurezza di rete esistente](#Create-rules-in-an-existing-NSG) per creare le regole **in entrata** nella tabella seguente.

	|Regola in entrata|Regola in uscita|
	|---|---|
	|![Portale di Azure - Regola in entrata](./media/virtual-networks-create-nsg-arm-pportal/figure17.png)|![Portale di Azure - Regola in uscita](./media/virtual-networks-create-nsg-arm-pportal/figure18.png)|

3. Ripetere la procedura illustrata in [Associare il gruppo di sicurezza di rete alla subnet FrontEnd](#Associate-the-NSG-to-the-FrontEnd-subnet) per associare il gruppo di sicurezza di rete **NSG-Backend** alla subnet **BackEnd**.

## Passaggi successivi

- Informazioni su come [gestire i gruppi di sicurezza esistenti](virtual-network-manage-nsg-arm-portal.md)
- [Abilitare la registrazione](virtual-network-nsg-manage-log.md) per i gruppi di sicurezza di rete.

<!---HONumber=AcomDC_0330_2016-->