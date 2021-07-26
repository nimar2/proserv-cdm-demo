# *ProServ Data Steward Demo Handbook*


## *Prerequisites* : 
 - Azure Subscription with a Resource Group to deploy
 - User must have owner access to the resource group
 - [Download and install](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli) the current release of the Azure CLI to run the deployment script. After the installation is complete, you will need to close and reopen any active Windows Command Prompt or PowerShell windows to use the Azure CLI.
 - Data Engineer demo must be already executed on this resource group and user should have necessary permission on source datasets


## Collect data needed to run the scripts

Before you get started, copy and paste below variables in a text editor for later use.:


| Variable Name		       | Description	             					    | How to Get			      |
|----------------------------- | -------------------------------------------------------------------|------------------------------------------
|TenantID | Unique identifier (Tenant ID) of the Azure Active Directory instance | Azure portal -> Azure Active Directory -> Copy TenantId |
|SubscriptionId | Subscription Id where the deployment has to be done | Azure Portal -> Subscriptions ->  Select the subscription Name -> Copy Subscription ID |
|ResourceGroupName | Resource Group name where the deployment to be made | Azure Portal -> Copy Resource Group Name |
|SynapseWorkspaceName |	Synapse workspace name | Azure Portal -> Copy Synapse workspace name |
|SyanpseDefaultADLSName | Default ADLS Storage account name to be linked with Synapse. | Azure Portal -> Copy Default ADLS Storage account name |
|AmericasADLSName | ADLS Storage account name in Americas region to be used for Third Party data. | Azure Portal -> Copy ADLS Storage account name in Americas region |
|ApacADLSName | ADLS Storage account name in APAC region to be used for Contoso data. | Azure Portal -> Copy ADLS Storage account name in APAC region |
|PurviewAccountName | Azure Purview Account Name |

## Prepare starter kit and run the client-side setup scripts

1. [Download the starter kit](https://github.com/charlskv-neu/proserv-cdm-demo/tree/development) , and extract its contents to the location of your choice.

2. On your computer, enter **PowerShell** in the search box on the Windows taskbar. In the search list, right-click **Windows PowerShell**, and then select **Run as administrator**.


3. Use the following command to navigate to the directory where the setup script is residing in the Powershell IDE. Replace path-to-starter-kit with the folder path of the extracted Setup-ProServ-Demo-Environment.ps1 file.

	```powershell
	cd <path-to-starter-kit>\data-steward-persona
	dir -Path <path-to-starter-kit> -Recurse | Unblock-File
	```

4. Use the following command to run the setup kit. 

	- Replace the TenantID, SubscriptionID, ResourceGroupName, SynapseWorkspaceName, SyanpseDefaultADLSName, AmericasADLSName, ApacADLSName and PurviewAccountName placeholders using the data collected earlier.
	
	```powershell
	.\Setup-ProServ-Governance-Demo-Environment.ps1 -TenantId <TenantID> -SubscriptionId <SubscriptionId> -ResourceGroupName <ResourceGroupName> -SynapseWorkspaceName <SynapseWorkspaceName> -SyanpseDefaultADLSName <SyanpseDefaultADLSName> -AmericasADLSName <AmericasADLSName> -ApacADLSName <ApacADLSName> -PurviewAccountName <PurviewAccountName>
	```
	
	- Complete the Azure login when prompted
	
5. Once the deployment is complete, verify the purview account is created. Also, confirm the access controls are set correctly.

6. Below are the three steps performed by this script post which the source registration and scan has to be done manually.

	- Provision the purview account
	- Purview account is given Storage Blob Data Reader role at all 3 ADLS accounts
	- Grant Purview API access to Synapse Managed Identity and logged in user

***