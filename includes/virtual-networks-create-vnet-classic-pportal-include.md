## Come creare una rete virtuale classica nel portale di Azure

Per creare una rete virtuale classica in base allo scenario precedente, seguire questa procedura.

1. Da un browser, passare a http://portal.azure.com e, se necessario, accedere con l'account Azure.
2. Fare clic su **NUOVO** > **Rete** > **Rete virtuale**, notare che nell’elenco **Selezionare un modello di distribuzione** è già visualizzata la voce **Classica**, quindi fare clic su **Crea** come illustrato nella figura riportata di seguito.

	![Creare reti virtuali nel portale di Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. Nel riquadro **Rete virtuale** digitare il **Nome** della rete virtuale e quindi fare clic su **Spazio di indirizzi**. Configurare le impostazioni dello spazio di indirizzi per la rete virtuale e la prima subnet, quindi fare clic su **OK**. La figura seguente illustra le impostazioni del blocco CIDR per questo scenario.

	![Riquadro spazio indirizzi](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. Fare clic su **Gruppo di risorse** e selezionare un gruppo di risorse a cui aggiungere la rete virtuale oppure fare clic su **Crea nuovo gruppo di risorse** per aggiungere la rete virtuale a un nuovo gruppo di risorse. La figura seguente illustra le impostazioni del gruppo di risorse per un nuovo gruppo di risorse denominato **TestRG**. Per altre informazioni sui gruppi di risorse, vedere [Panoramica di Gestione risorse di Azure](../articles/virtual-network/resource-group-overview.md/#resource-groups).

	![Crea riquadro gruppo di risorse](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Se necessario, modificare le impostazioni per **Sottoscrizione** e **Percorso** per la rete virtuale.

6. Se non si desidera visualizzare la rete virtuale come un riquadro nella **schermata iniziale**, disabilitare **Aggiungere a Schermata iniziale**.

7. Fare clic su **Crea** e osservare il riquadro **Creazione rete virtuale** come illustrato nella figura riportata di seguito.

	![Creare reti virtuali nel portale](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Attendere che la rete virtuale da creare sia pronta e quando viene visualizzato il riquadro riportato di seguito, fare clic per aggiungere più subnet.

	![Creare reti virtuali nel portale](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Verrà visualizzata la voce **Configurazione** per la rete virtuale, come illustrato di seguito.

	![Creare reti virtuali nel portale](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Fare clic su **Subnet** > **Aggiungi**, quindi digitare un **Nome** e specificare un **Intervallo di indirizzi (blocco CIDR)** per la subnet e quindi fare clic su **OK**. La figura seguente illustra le impostazioni per lo scenario attuale.

	![Creare reti virtuali nel portale di Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

<!---HONumber=AcomDC_0323_2016-->