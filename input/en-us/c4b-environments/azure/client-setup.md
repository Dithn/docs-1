---
Order: 27
xref: c4b-azure-client-setup
Title: Client Setup
Description: How to setup a client machine to use Chocolatey for Business Azure Environment
RedirectFrom: en-us/quick-deployment/azure/client-setup
---

## Summary

Once you have your Chocolatey for Business Azure Environment deployed, you'll need to get clients talking to it.
To do that, you'll need to do the following on the clients:

1. Setup DNS to allow access to the environment.
1. Install the SSL/TLS certificate, if self-signed, so clients can access HTTPS components.
1. Install Chocolatey components and configure the client for Chocolatey for Business (C4B) deployments.

## DNS

Ensure that you have [configured DNS](xref:c4b-azure#dns-configuration) to direct clients to your deployed environment.

Once you've added the required CNAME record, clients should be able to access it.

## SSL Certificate

> :memo: **NOTE**
>
> If you used an SSL certificate from an external Certificate Authority (CA), or internally trusted PKI CA, your clients will automatically trust it and you can skip this section.

If you used a self-signed certificate to deploy your Chocolatey for Business Azure Environment, you will need to import this certificate to the `Trusted Root Certification Authorities` store on the clients.

1. Open the Microsoft Management Console (`MMC.msc`)
1. Select **File** -> **Add/Remove Snap-in...**
1. Select **Certificates** and click **Add >**
1. Choose **Computer account** and click **Next**, **Finish**, then **OK**
1. Expand **Certificates (Local Computer)**
1. Right-click **Trusted Root Certification Authorities**, and select **All Tasks** -> **Import**

    ![Importing SSL Certificate in MMC](/assets/images/c4b-azure/MMC-Import-Certificate.png)

1. Click **Next**
1. Browse to the self-signed certificate file
    1. You may need to adjust the filetype so that you can see `.pfx` files

    ![Changing file type when browsing for certificate file in MMC](/assets/images/c4b-azure/MMC-Browse-FileType.png)

1. Click **Next**
1. Enter the password supplied when creating the certificate
1. Click **Next**, **Next**, then **Finish**
1. Close the Microsoft Management Console

## Client Setup Script

To on-board clients, you run the `ClientSetup.ps1` script provided with your Chocolatey for Business Azure Environment.

You will need the following values ready when running this script:

* `FQDN`: The fully qualified domain name used to access your environment.
* `ccmClientCommunicationSalt`: This is the client-side salt additive. More information about this can be found in the [C4B Config Settings](xref:ccm-client#config-settings) docs.
* `ccmServiceCommunicationSalt`: This is the server-side salt additive. More information about this can be found in the [C4B Config Settings](xref:ccm-client#config-settings) docs.
* `ChocoUserPassword`: The password for the `chocouser` account which is used by the client to access your environment's Nexus component.

Except for the `FQDN`, all of these values are available in your deployed environment's Azure Key Vault.
See [Accessing Services](xref:c4b-azure#accessing-services) for more information about retrieving values from the Vault.

When you're ready, run the following on the client from an elevated (Run as Administrator) PowerShell console:

```powershell
# Please fill in the following values
$fqdn = 'Replace with FQDN for your Chocolatey for Business QDE Azure Environment' 
$clientCommunicationSalt = 'Your ccmClientCommunicationSalt'  #This value is stored within your Azure Key Vault
$serverCommunicationSalt = 'Your ccmServiceCommunicationSalt' #This value is stored within your Azure Key Vault
$password = 'Your ChocoUserPassword' #This value is stored within your Azure Key Vault

# Touch NOTHING below this line
$user = 'chocouser'

$securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
$credential = [pscredential]::new($user, $securePassword)
$downloader = [System.Net.WebClient]::new()
$downloader.Credentials = $credential

$script =  $downloader.DownloadString("https://$($fqdn)/nexus/repository/choco-install/ClientSetup.ps1")

$params = @{
    Credential      = $credential
    ClientSalt      = $clientCommunicationSalt
    ServerSalt      = $serverCommunicationSalt
}

& ([scriptblock]::Create($script)) @params
```

This script will accomplish the following on your client:

1. Install Chocolatey from the installation script hosted in your internal raw Nexus repository
1. Add the `ChocolateyInternal` source, and enable it for self-service
1. Disable the default `Chocolatey` Community Repository
1. Install your Chocolatey license using the `chocolatey-license` package
1. Install the Chocolatey Licensed Extension (without context menus for Package Builder)
1. Install the `ChocolateyGUI` package on the endpoint, for self-service support
1. Install the `chocolatey-agent` package, which supports self-service and CCM communication
1. Enable and disable features related to configuring self-service access on the endpoint
1. Setup the communication channel between the endpoint and CCM, using the correct URL and salts
1. Opt the endpoint into CCM Deployments
