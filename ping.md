# Enable the Response to `ping` on a Windows Host
`ping` is a useful tool for testing connectivity between hosts. It works by sending a ICMP echo request to the target host and waiting for a ICMP echo reply.  
To enable a host to respond to ping cmd, you simply set Windows firewall's rule to allow ICMP echo packets get in.  
You can use cmd of `netsh` or PowerShell commands to do so.

## #1 Add a new rule into firewall inbound direction
Using `netsh` to add a new rule into the firewall.
```
Syntax:
netsh advfirewall firewall add rule name="<customer-name-for-the-new-rule>" protocol=icmpv4:8,any dir={in | out} action={allow | block}
```
An example to allow the response to `ping`:
```
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow
```
Or block the response by seting the value of _action_ to _block_.  
(Reference https://www.howtogeek.com/howto/windows-vista/allow-pings-icmp-echo-request-through-your-windows-vista-firewall/)

## #2 Set the Values of existing rules
By defualt, Windows includes a rule named "File and Printer Sharing (Echo Request - ICMPv4-In)" which can be used to response to ping.  
### `netsh`
First of all, check the current settings of the rule by typing the following cmd:  
```
netsh advfirewall firewall show rule name="File and Printer Sharing (Echo Request - ICMPv4-In)"
```
By modifying the rule's action vlaue to allow, the host will response to ping from another host.  
To allow icmp echo request on all network profiles (private, public, and domian):  
```
netsh advfirewall firewall set rule name="File and Printer Sharing (Echo Request - ICMPv4-In)" new action=allow
```
Or only for domain:
```
netsh advfirewall firewall set rule name="File and Printer Sharing (Echo Request - ICMPv4-In)" profile=domain new action=allow
```
> Notes: The 'new' keyword must be present and must not be the last argument provided. Values after the 'new' keyword are updated in the rule.

### PS \*-NetFirewall\*
`*-NetFirewall*` means a set of cmdlets avialable in PowerShell.  
Firstly, get the current settings of "File and Printer Sharing (Echo Request - ICMPv4-In)":
```
Show-NetFirewallRule | Where-Object {$_.DisplayName -eq "File and Printer Sharing (Echo Request - ICMPv4-In)"}
```
By running the above command, you can get the unique name of the rule you want. In our case, it is "FPS-ICMP4-ERQ-In".
Change the settings to allow ICMPv4 echo request:
```
Set-NetFirewallRule -Name "FPS-ICMP4-ERQ-In" -Action allow
```
