# NBN Settings in pfSense

#### **NOTE:** *WHEN WRITING THIS GUIDE, IT WAS BASED ON [PFSENSE v2.7.2-CE](https://www.pfsense.org/download/)*


# Table of Contents

***First download the required image file from the pfSense file and follow [those steps](https://docs.netgate.com/pfsense/en/latest/install/write-memstick.html) to install in on your device.***


1. <a href="#dynamic-settings">Dynamic Settings</a>
    - <a href="#wan-interface-dynamic">WAN Interface (Dynamic)</a>
    - <a href="#interface-assignement-dynamic">Interface Assignement (Dynamic)</a>
2. <a href="#pppoe-settings">PPPoE Settings</a>
    - <a href="#creating-vlan-pppoe">Creating VLAN (PPPoE)</a>
    - <a href="#assigning-ppp-interfaces-pppoe">Assigning PPP Interfaces (PPPoE)</a>
    - <a href="#interface-assignement-pppoe">Interface Assignement (PPPoE)</a>
    - <a href="#wan-interface-pppoe">WAN Interface (PPPoE)</a>
3. <a href="#lan-interface-general">LAN Interface (General)</a>



# Dynamic Settings

Those steps are followed when the ISP router is configured with no Usename/Password, mainly becuase those are being configured at the ISP end rather that at your end, _i.e. Superloop NBN in Australia_


## WAN Interface (Dynamic)

Open PFSense main UI page by navigating to 192.168.1.1, then from there navigate to *Interfaces-->WAN*. In the screen, fill in as following:

_*General Configuration*_
- *IPv4 Configuration Type* : choose *DHCP*
- *IPv6 Configuration Type* : choose *DHCP6*
- *MTU* : 1500
- *MSS* : 1492

![dynamic-wan-general](/screenshots/dynamic-wan-general.jpeg)

_*DHCP Client Configuration*_

Leave everything as it is no changes

![dynamic-dhcp-client-configuration](/screenshots/dynamic-dhcp-client-configuration.jpeg)


_*DHCP6 Client Configuration*_

Leave everything as it is no changes. Only make sure that the _DHCP6 Prefix Delegation size_ is set to 64

![dynamic-dhcp6-client-configuration](/screenshots/dynamic-dhcp6-client-configuration.jpeg)


_*Reserved Networks*_

Enable both the *Block private networks and loopback addresses* and *Block bogon networks*

When done click on *Save*

![wan-reserved-networks](/screenshots/wan-reserved-networks.png)



## Interface Assignement (Dynamic)

On the same page, head to *Interface Assignments* and select the following:
- *WAN* : select the desired WAN network adapter port (inbound)
- *LAN* : sleect the second unused network adapter port (outbound)

When done click on *Save*

![dynamic-interface-assignment](/screenshots/dynamic-interface-assignment.jpeg)



# PPPoE Settings

Those steps are followed when the ISP router is configured to have PPPoE settigns with Usename/Password, _i.e. iiNET NBN in Australia_


## Creating VLAN (PPPoE)

Open PFSense main UI page by navigating to 192.168.1.1, then from there navigate to *Interfaces-->Assignments*. On that page, click on *VLANs* and add a new one by clicking the green **+ Add** button

In the screen, fill in as following:
- *Parent Interface* : choose the ethernet port adapter where the NBN will be connected to (inbound)
- *VLAN Tag*: set it to 2 
- *VLAN Priority*: set to 0
- *Description*: anything that tells what this VLAN will be used for. I prefer setting it to the ISP name

When done click on *Save*

![ppoe-vlans](/screenshots/ppoe-vlans.png)



## Assigning PPP Interfaces (PPPoE)

On the same page, head to *PPPs* and add a new one by clicking the green **+ Add** button

In the screen, fill in as following:
- *Link Type* : choose *PPPoE*
- *Link Interface(s)* : choose the VLAN that was created previously
- *Username*: enter your ISP username here
- *Password*: enter your ISP password here and confirm it in the next textbox
- Enable the *Configure NULL service name*

When done click on *Save*

![ppoe-ppp](/screenshots/ppoe-ppp.png)



## Interface Assignement (PPPoE)

On the same page, head to *Interface Assignments* and select the following:
- *WAN* : select the previously created PPPOE interface that has the ISP user next to it
- *LAN* : sleect the second unused network adapter port (outbound)
- *Available network ports* : select the other network adapter port that was used while creating the VLAN in the first step (inboud)

When done click on *Save*

![ppoe-interface-assignment](/screenshots/ppoe-interface-assignment.png)



## WAN Interface (PPPoE)

Navigate to *Interfaces-->WAN* and fill in as following:
_*General Configuration*_

- Make sure the interface is enabled
- *Description* : WAN
- *IPv4 Configuration Type* : PPPoE
- *IPv6 Configuration Type* : None
- *MTU* : 1500
- *MSS* : 1492

![pppoe-wan-general](/screenshots/pppoe-wan-general.png)

_*PPPoE Configuration*_

- *Username*: your ISP username here
- *Password*: your ISP password here and confirm it in the next textbox
- *Periodic reset* : Disabled

![ppoe-wan-pppoe-config](/screenshots/ppoe-wan-pppoe-config.png)

_*Reserved Networks*_

Enable both the *Block private networks and loopback addresses* and *Block bogon networks*

When done click on *Save*

![wan-reserved-networks](/screenshots/wan-reserved-networks.png)



## LAN Interface (General)

Navigate to *Interfaces-->LAN* and fill in as following:
_*General Configuration*_

- Make sure the interface is enabled
- *Description* : LAN
- *IPv4 Configuration Type* : Static IPv4
- *IPv6 Configuration Type* : Track Interface
- Leave *MAC Address*, *MTU* and *MSS* blank
- *Speed and Duplex*: Default (no preference, typically autoselect)

![ppoe-lan-general](/screenshots/ppoe-lan-general.png)

_*Static IPv4 Configuration*_
- *IPv4 Address* : 192.168.1.1 and */* to 24
- *IPv4 Upstream gateway* : None

![ppoe-lan-ipv4](/screenshots/ppoe-lan-ipv4.png)

_*Track IPv6 Interface*_
- Keep *IPv6 Interface* as blank
- *IPv6 Prefix ID* : 0

![ppoe-lan-ipv6](/screenshots/ppoe-lan-ipv6.png)

_*Reserved Networks*_
Disable both the *Block private networks and loopback addresses* and *Block bogon networks*

![ppoe-lan-reserved-networks](/screenshots/ppoe-lan-reserved-networks.png)



## License
This document guide is licensed under the CC0 1.0 Universal license. The terms of the license are detailed in [LICENSE](/LICENSE)
