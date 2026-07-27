# IT-troubleshooting-guide
Some common troubleshooting issues


Scenario 1: DNS Resolution Failure
User can ping IP addresses but can't reach websites

attempt to resolve #1
"Can you open Command Prompt and type ping 8.8.8.8 — does that work?"
if it works, it's a DNS issue
if it fails, it's a connectivity problem, not DNS (see Scenario 2)

Test DNS directly, nslookup google.com, if this fails while the ping above worked, it's a DNS-specific fault

Check DNS assignment by typing ipconfig /all look for the DNS Servers line. Blank or 127.0.0.1 means DNS was never assigned properly

Set DNS manually if needed: Network Connections, adapter Properties, IPv4, set DNS to 8.8.8.8 (Google) or 1.1.1.1 (Cloudflare)
Flush the cache by typing ipconfig /flushdns in CMD then retest

Prevention tips
For the user, restart the router before calling as this fixes the majority of DNS issues



Scenario 2: No Internet Access (Connected but Not Working)

User shows as connected to WiFi/Ethernet but nothing loads

questions to ask the caller first
"Is it every website/app, or just one specific thing?"
"Is this a new setup?"
"are there any other devices in the house having the same problem right now?"

Step by step to fix
Check the IP address: ipconfig — if it shows an IP starting with 169.254.x.x, the machine never got an address from the router and is thus a DHCP fail
Ping the gateway: ping 192.168.1.1 (or whatever the router's IP is) confirms whether the machine can reach the router
Ping outside the network: ping 8.8.8.8 — confirms whether the router itself has internet
Release and renew the IP, in CMD, type ipconfig /release then ipconfig /renew to force a fresh IP from DHCP
Check Device Manager for a yellow warning icon on the network adapter, this points to a problem with the driver as opposed to the nerwork
Restart the network adapter — Network Connections → right-click adapter → Disable, wait a few seconds, then enable


The fix
Most of the time this comes down to either a stale DHCP lease (fixed by release/renew) or the adapter itself needing a restart. If ping to the gateway fails but everything else looks fine, it's usually a cabling or WiFi issue rather than a software one.

Prevention tips
For the user, If reconnecting doesn't help, restart the machine before calling in as this can clear a number of issues



Scenario 3: Active Directory Password Reset

user forgot their password or their account is locked out

questions to ask first
"How many times have you tried? If it's more than 3-4, the account may be locked, not just wrong."
"Did you recently change your password somewhere else?"
confirm the identity of the user 

Step-by-step (from the Domain Controller)
Log into the Domain Controller as an administrator
Reset the password:
INSERT IMAGE 
Confirm the account is in good standing:
INSERT IMAGE 
If the account was locked rather than just forgotten, unlock it separately
INSERT IMAGE
Let the user know their temporary password and have them change it on next login

Prevention tips
For the user, A password manager avoids this ticket entirely.



Scenario 4: Slow Boot or High CPU at Startup
Computer takes several minutes to become usable after turning on

Questions to ask first
"How long are we talking — under a minute, or several minutes?"
"Did this start after a Windows update or new software install?"
"Is it slow only at startup, or all the time?" (rules out a general performance issue vs a startup-specific one)


Step-by-step
Open Task Manager, Startup tab
Look at the Startup impact column, anything marked High could be disabled
Right-click non-essential entries (toolbars, updater utilities, chat apps that don't need to launch on boot) and disable
Restart and time how long it takes to become responsive
If it's still slow, check the Services tab on task manager for anything not needed to run at boot
Check Disk usage in Task Manager during startup, if it's maxed at 100%, this likely points towards a failing or fragmented drive rather than software bottleneck


The fix
In the most cases, this is solved by disabling 2-3 unnecessary startup programmes. If disk usage stays pinned at 100% even after that, it's worth checking hardware (failing HDD/SSD).


Prevention tips
For the user: Be selective about what you let install a launch on startup as most toolbars and utilities don't need it.
