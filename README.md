# PowerShell Common cmds and scripts

## Enable the Response to `ping` on a Windows Host
`ping` is a useful tool for testing connectivity between hosts. It works by sending a _ICMP echo request_ to the target host and waiting for a _ICMP echo reply_.  
To enable a host to respond to `ping` cmd, you simply set Windows firewall's rule to allow _ICMP echo_ packets get in.  
The cmdlets of `netsh` and PowerShell can do so.
### `netsh`
**#1 Add a new rule into firewall inbound direction**
```
Syntax:
netsh advfirewall firewall add rule name="<customer-name-for-the-new-rule>" protocol=icmpv4:8,any dir={in | out} action={allow | block}
```
Example to allow to respond to `ping`:
```
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=allow
```
Or block the response to `ping`:
```
netsh advfirewall firewall add rule name="ICMP Allow incoming V4 echo request" protocol=icmpv4:8,any dir=in action=block
```
 > Reference https://www.howtogeek.com/howto/windows-vista/allow-pings-icmp-echo-request-through-your-windows-vista-firewall/

**#2 Change the settings of an existing rule**  
By defualt, Windows includes a rule named _"File and Printer Sharing (Echo Request - ICMPv4-In)"_ which can be used to response to `ping`.  
Check the current settings of the rule by typing the following cmd:
```
netsh advfirewall firewall show rule name="File and Printer Sharing (Echo Request - ICMPv4-In)"
```
By modifying the rule's _action_ vlaue to _allow_, the host will response to `ping` from another host.  
To _allow_ icmp echo request on all network profiles (_private, public, and domian_):
```
netsh advfirewall firewall set rule name="File and Printer Sharing (Echo Request - ICMPv4-In)" new action=block
```
Or only for _domain_:
```
netsh advfirewall firewall set rule name="File and Printer Sharing (Echo Request - ICMPv4-In)" profile=domain new action=block
```
 > Notes: The 'new' keyword must be present and must not be the last argument provided. Values after the 'new' keyword are updated in the rule.

### PS of `*-NetFirewall*`
`*-NetFirewall*` means a set of cmdlets avialable in PowerShell.
**#3 Change the settings of existing rules**
Firstly, Get the current settings of _"File and Printer Sharing (Echo Request - ICMPv4-In)"_
```
Show-NetFirewallRule | Where-Object {$_.DisplayName -eq "File and Printer Sharing (Echo Request - ICMPv4-In)"}
```
By running the above command, you can get the unique name of the rule you want. In our case, it is _"FPS-ICMP4-ERQ-In"_.  
Change the settings to allow _ICMPv4 echo request_:
```
Set-NetFirewallRule -Name "FPS-ICMP4-ERQ-In" -Action allow
```
