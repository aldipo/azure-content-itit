### Aggiungere o rimuovere prefissi se non è ancora stata creata una connessione del gateway VPN

- **Per aggiungere** prefissi di indirizzo aggiuntivi a un gateway di rete locale creato ma ancora privo di una connessione del gateway VPN, usare l'esempio seguente.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')


- **Per rimuovere** un prefisso di indirizzo da un gateway di rete locale che non ha una connessione VPN, usare l'esempio seguente. Escludere i prefissi non più necessari. In questo esempio, non è più necessario il prefisso 20.0.0.0/24 usato nell'esempio precedente. In tal modo, sarà possibile aggiornare il gateway di rete locale ed escludere tale prefisso.

		$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
		Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### Aggiungere o rimuovere prefissi se è già stata creata una connessione del gateway VPN

Se è stata creata la connessione VPN e si vuole aggiungere o rimuovere i prefissi di indirizzo IP contenuti nel gateway di rete locale, è necessario seguire questa procedura nell'ordine indicato. Questo comporterà periodi di inattività per la connessione VPN.

>[AZURE.IMPORTANT] Non eliminare il gateway VPN. In caso contrario sarà necessario ripercorrere i passaggi necessari per ricrearlo, nonché riconfigurare il router locale con le nuove impostazioni.
 
1. Rimuovere la connessione IPsec. 
2. Modificare i prefissi per il gateway di rete locale. 
3. Creare una nuova connessione IPsec. 

È possibile usare l'esempio seguente come linea guida.

	$gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

	Remove-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg

	$local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
	Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
	
	New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

<!---HONumber=AcomDC_0427_2016-->