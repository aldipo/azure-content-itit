<properties
    pageTitle="Configurare un Account RunAs di Azure | Microsoft Azure"
    description="Esercitazione che illustra come creare, testare e usare l'autenticazione dell'entità di sicurezza in Automazione di Azure."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
	keywords="nome entità servizio, setspn, autenticazione di Azure"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/09/2016"
    ms.author="magoedte"/>

# Autenticare runbook con account RunAs di Azure
Questo argomento illustra come configurare un account di Automazione dal portale di Azure usando la nuova funzionalità di account RunAs, anche detta entità servizio, per accedere alle risorse di Azure Resource Manager nella sottoscrizione con i runbook di automazione. Quando si crea un nuovo account di automazione nel portale di Azure, per impostazione predefinita viene creata automaticamente una nuova entità servizio assegnata al ruolo Collaboratore di controllo degli accessi in base al ruolo (RBAC) nella sottoscrizione. Questo semplifica il processo e consente di iniziare rapidamente la compilazione e distribuzione di runbook per supportare le esigenze di automazione.

Con un'entità servizio è possibile:

* Standardizzare l'autenticazione con Azure quando si gestiscono risorse di Azure Resource Manager con i runbook
* Automatizzare l'uso di runbook globali configurati negli avvisi di Azure.


>[AZURE.NOTE] La [funzionalità di integrazione degli avvisi](../azure-portal/insights-receive-alert-notifications.md) di Azure con i runbook globali di Automazione richiede un account di Automazione configurato con un'entità servizio. È possibile selezionare un account di automazione in cui è già definito un utente dell'entità servizio o scegliere di crearne uno nuovo.



L'articolo illustra come creare l'account di automazione dal portale di Azure e aggiornarne uno con un account RunAs usando Azure PowerShell e come eseguire l'autenticazione con l'entità servizio nei runbook.

## Creare un account di automazione dal portale di Azure
La procedura descritta in questa sezione permette di creare un nuovo account di automazione di Azure e un'entità servizio dal portale di Azure.

>[AZURE.NOTE] L'utente che esegue queste operazioni *deve* essere un membro del ruolo Amministratori della sottoscrizione.

1. Accedere al portale di Azure come amministratore del servizio per la sottoscrizione di Azure da gestire.
2. Selezionare **Account di automazione**.
3. Nel pannello Account di automazione fare clic su **Aggiungi**.<br>![Aggiungi account di Automazione](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. Nella casella **Nome** del pannello **Aggiungi account di Automazione** digitare un nome per il nuovo account di Automazione.
5. Se si hanno più sottoscrizioni, specificare quella per il nuovo account, un **Gruppo di risorse** nuovo o esistente e la **Località** per il data center di Azure.
6. Assicurarsi che sia selezionato il valore **Sì** per l'opzione **Crea un account RunAs di Azure** e fare clic sul pulsante **Crea**.  

    ![Aggiungi account di Automazione, avviso](media/automation-sec-configure-azure-runas-account/add-account-decline-create-runas-msg.png)

    >[AZURE.NOTE] Se si sceglie di non creare l'account RunAs selezionando **No**, verrà visualizzato un messaggio di avviso nel pannello **Aggiungi account di Automazione**. Durante la creazione e l'assegnazione al ruolo **Collaboratore** nella sottoscrizione, l'account non ha un'identità di autenticazione corrispondente nel servizio directory delle sottoscrizioni e, di conseguenza, non ha accesso alle risorse nella sottoscrizione. Questo impedisce ai runbook che fanno riferimento a questo account di autenticarsi ed eseguire attività sulle risorse di Azure Resource Manager.

    ![Aggiungi account di Automazione, avviso](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

    >[AZURE.NOTE] Se dopo aver selezionato il pulsante **Crea** si riceve un messaggio di errore di autorizzazione negata, l'account non è un membro del ruolo Amministratori della sottoscrizione.

7. Mentre Azure crea l'account di Automazione, è possibile tenere traccia dello stato di avanzamento in **Notifiche** dal menu.

### Risorse incluse
Quando la creazione dell'account di automazione viene completata, vengono automaticamente create diverse risorse, riepilogate nella tabella seguente.

Risorsa|Descrizione 
----|----
Runbook AzureAutomationTutorial|Runbook di esempio che illustra l'autenticazione con l'account RunAs e la visualizzazione delle prime 10 VM di Azure nella sottoscrizione.
AzureRunAsCertificate|Asset del certificato creato se si è scelto di creare l'account RunAs durante la creazione dell'account di Automazione o usando lo script di PowerShell riportato più avanti per un account esistente. La durata di questo certificato è di un anno. 
AzureRunAsConnection|Asset della connessione creato se si è scelto di creare l'account RunAs durante la creazione dell'account di Automazione o usando lo script di PowerShell riportato più avanti per un account esistente.
Moduli|15 moduli con cmdlet per Azure, PowerShell e Automazione da iniziare a usare immediatamente nei runbook.  

## Aggiornare un account di automazione tramite PowerShell
La procedura seguente aggiorna un account di automazione esistente e crea l'entità servizio tramite PowerShell. Questa procedura è necessaria se è stato creato un account ma si è scelto di non creare l'account RunAs.

Prima di procedere, verificare quanto segue:

1. È stato scaricato e installato [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855), se si esegue Windows 7. Se si esegue Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 o Windows 7 SP1, è disponibile per l'installazione [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395).
2. Azure PowerShell 1.0. Per informazioni su questa versione e su come installarla, vedere [Come installare e configurare Azure PowerShell](../powershell-install-configure.md). 
3. È stato creato un account di automazione. A questo account viene fatto riferimento come valore per i parametri -AutomationAccountName e -ApplicationDisplayName nello script riportato più avanti in questo articolo.


Lo script di PowerShell configura quanto segue:

* Un'applicazione Azure AD che verrà autenticata con il certificato autofirmato. Verrà creato un account dell'entità servizio per questa applicazione in Azure AD e a questo account verrà assegnato il ruolo Collaboratore nella sottoscrizione corrente, che può essere cambiato in Proprietario o in qualsiasi altro ruolo. Per altre informazioni, vedere l'articolo [Controllo degli accessi in base al ruolo in Automazione di Azure](../automation/automation-role-based-access-control.md).  
* Un asset del certificato di Automazione nell'account di automazione specificato denominato **AzureRunAsCertificate**, che contiene il certificato usato nell'entità servizio.
* Un asset della connessione di Automazione nell'account di automazione specificato denominato **AzureRunAsConnection**, che contiene l'ID applicazione, l'ID tenant, l'ID sottoscrizione e l'identificazione personale del certificato.  


### Eseguire lo script di PowerShell

1. Salvare lo script seguente nel computer. Per questo esempio, salvare il file con il nome **New-AzureServicePrincipal.ps1**.  

    ```
    #Requires -RunAsAdministrator
    Param (
    [Parameter(Mandatory=$true)]
    [String] $ResourceGroup,

    [Parameter(Mandatory=$true)]
    [String] $AutomationAccountName,

    [Parameter(Mandatory=$true)]
    [String] $ApplicationDisplayName,

    [Parameter(Mandatory=$true)]
    [String] $SubscriptionId,

    [Parameter(Mandatory=$true)]
    [String] $CertPlainPassword,

    [Parameter(Mandatory=$false)]
    [int] $NoOfMonthsUntilExpired = 12
    )

    Login-AzureRmAccount
    Import-Module AzureRM.Resources
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    $CurrentDate = Get-Date
    $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
    $KeyId = (New-Guid).Guid
    $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")

    $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"

    $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
    Export-PfxCertificate -Cert ("Cert:\localmachine\my" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose

    $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
    $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

    $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
    $KeyCredential.StartDate = $CurrentDate
    $KeyCredential.EndDate= $EndDate
    $KeyCredential.KeyId = $KeyId
    $KeyCredential.Type = "AsymmetricX509Cert"
    $KeyCredential.Usage = "Verify"
    $KeyCredential.Value = $KeyValue

    # Use Key credentials
    $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential

    New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
    Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose

    $NewRole = $null
    $Retries = 0;
    While ($NewRole -eq $null -and $Retries -le 2)
    {
      # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
      Sleep 5
      New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
      Sleep 5
      $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
      $Retries++;
    }

    # Get the tenant id for this subscription
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1

    # Create the automation resources
    New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose

    # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    $ConnectionAssetName = "AzureRunAsConnection"
    Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
    ```
<br>
2. Avviare **Windows PowerShell** con diritti utente elevati nel computer dalla schermata **Start**.
3. Dalla shell della riga di comando di PowerShell con privilegi elevati passare alla cartella contenente lo script creato nel passaggio 1 ed eseguire lo script modificando i valori dei parametri *-ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* e *-CertPlainPassword*.<br>

    ```
    .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName> 
     -AutomationAccountName <NameofAutomationAccount> `
     -ApplicationDisplayName <DisplayNameofAutomationAccount> `
     -SubscriptionId <SubscriptionId> `
     -CertPlainPassword "<StrongPassword>"
    ```   
<br>

    >[AZURE.NOTE] Verrà richiesto di autenticarsi con Azure dopo aver eseguito lo script. È *necessario* accedere con un account di amministratore del servizio nella sottoscrizione. <br>
4. Dopo aver completato lo script, passare alla sezione successiva per testare e verificare la nuova configurazione di credenziali.

### Verificare l'autenticazione
A questo punto dell'esercitazione viene eseguito un breve test per confermare che sia possibile eseguire l'autenticazione usando la nuova entità servizio. Se non si riesce a eseguire l'autenticazione, tornare al passaggio 1 e verificare nuovamente i singoli passaggi.

1. Nel portale di Azure aprire l'account di automazione creato in precedenza.  
2. Fare clic sul riquadro **Runbook** per aprire l'elenco dei runbook.
3. Creare un nuovo runbook facendo clic sul pulsante **Aggiungi runbook** e quindi selezionare **Crea un nuovo runbook** nel pannello **Aggiungi runbook**.
4. Denominare il runbook *Test-SecPrin-Runbook* e selezionare PowerShell in **Tipo di runbook**. Fare clic su **Crea** per creare il runbook.
5. Nel pannello **Modifica runbook di PowerShell** incollare il codice seguente nell'area di disegno:<br>

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```  
<br>
6. Salvare il runbook facendo clic su **Salva**.
7. Fare clic su **Riquadro di test** per aprire il pannello **Test**.
8. Fare clic su **Avvia** per avviare il test.
9. Viene creato un [processo del runbook](automation-runbook-execution.md) e il relativo stato viene visualizzato nel riquadro.  
10. Lo stato del processo è inizialmente *In coda* per indicare che è in attesa della disponibilità di un ruolo di lavoro per runbook nel cloud. Lo stato passerà quindi a *Avvio in corso* quando un thread di lavoro richiede il processo e quindi a *In esecuzione* quando l'esecuzione del runbook viene effettivamente avviata.  
11. Al termine del processo del runbook, viene visualizzato l'output. In questo caso lo stato visualizzato dovrebbe essere **Completato**.<br> ![Verifica del runbook dell'entità di sicurezza](media/automation-sec-configure-azure-runas-account/runbook-test-results.png)<br>
12. Chiudere il pannello **Test** per tornare all'area di disegno.
13. Chiudere il pannello **Modifica runbook di PowerShell**.
14. Chiudere il pannello **Test-SecPrin-Runbook**.

## Codice di esempio per l'autenticazione con le risorse di Resource Manager

È possibile usare il codice di esempio aggiornato seguente, tratto dal runbook di esempio AzureAutomationTutorial, per eseguire l'autenticazione usando l'account RunAs per gestire le risorse di Resource Manager con i runbook.

   ```
   $connectionName = "AzureRunAsConnection"
   $SubId = Get-AutomationVariable -Name 'SubscriptionId'
   try
   {
      # Get the connection "AzureRunAsConnection "
      $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

      "Logging in to Azure..."
      Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
	  "Setting context to a specific subscription"	 
	  Set-AzureRmContext -SubscriptionId $SubId	 		 
   }
   catch {
       if (!$servicePrincipalConnection)
       {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
       } else{
           Write-Error -Message $_.Exception
           throw $_.Exception
       }
   } 
   ```

Lo script include due righe di codice aggiuntive per supportare il riferimento al contesto di una sottoscrizione per poter lavorare facilmente in più sottoscrizioni. Un asset della variabile denominato SubscriptionId contiene l'ID della sottoscrizione e dopo l'istruzione del cmdlet Add-AzureRmAccount viene indicato il [cmdlet Set-AzureRmContext](https://msdn.microsoft.com/library/mt619263.aspx) con il set di parametri *-SubscriptionId*. Se il nome della variabile è troppo generico, è possibile modificarlo includendo un prefisso o un'altra convenzione di denominazione per identificarla più facilmente in base alle proprie esigenze. In alternativa, è possibile usare il set di parametri -SubscriptionName invece di -SubscriptionId con un asset della variabile corrispondente.

## Passaggi successivi
- Per altre informazioni sulle entità servizio, vedere [Oggetti applicazione e oggetti entità servizio](../active-directory/active-directory-application-objects.md).
- Per altre informazioni sul controllo degli accessi in base al ruolo in Automazione di Azure, vedere [Controllo degli accessi in base al ruolo in Automazione di Azure](../automation/automation-role-based-access-control.md).

<!---HONumber=AcomDC_0615_2016-->