# PowerShell Builds Active Directory Domain Service
* Setup the first domain controller
* Create organizations' users, computers, and departements
* Deploy Group Policy
* Schedule tasks
* Secure Active Directory

## Setup a Windows Server 2019 as the first domain controller

Install a fresh new Windows Server 2019 (Win2k19) and update to date by using 'sconfig' utility
Configure static IP address for the Windows Server and set the DNS to its own IP
```
Get the index number of the NIC of the Win2k19
Get-NetIPAddress -IPAddress <precent-dhcp-ip-address-of-NIC>
```
Change to static IP adddress
```
New-NetIPAddress -InterfaceIndex <NIC-index-number> -IPAdress <static-IP-address> -DefaultGateway <gateway-IP> -PrefixLength <1-32> 
```
Set DNS to its own (127.0.0.1)
```
Set-ClientDNSServerAddress -InterfaceIndex <NIC-index-number> -ServerAdress 127.0.0.1
```
Rename the Win2k19 name to Win2k19-dc1
```
Rename-Computer "Win2k19-dc1"
```

Install AD feature including Windows Server Management Tools
```
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```
Install AD forest with domain name (DNS server is installed by default)
```
Install-ADDSForest -DomainName "corp.local"
```
Change the DNS to its own static IP
```
Set-ClientDNSServerAddress -InterfaceIndex <NIC-index-number> -ServerAdress <static-IP-address>
```
