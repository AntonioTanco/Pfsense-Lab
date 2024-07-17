<h1>🌐 PFSense Lab </h1>

[![YouTube](http://i.ytimg.com/vi/mIEBfNq9spo/hqdefault.jpg)](https://www.youtube.com/watch?v=mIEBfNq9spo)
<h3>Project Overview</h3>

Created a PFSense Firewall within VirtualBox with a Linux client to showcase my knowledge on networking. In this project I've created an "Office" VLAN within the Pfsense admin portal that allows us to segement our "Office" Network from our "LAN" network. I created subnets for both the "Office" and the "LAN" network and used DHCP to create a pool of usable addresses for assignment of any devices attached to the respective port interface.

</br>Firewall rules we also implemented to control the flow of traffic originating from either interfaces: LAN0, LAN1 as an added layer of security.

___

<h3>Network Design</h3>


![PFSense Lab - Network Toplogy v1](https://github.com/user-attachments/assets/60c931d5-32e7-4ffc-ab02-f19f76200bf1)

NOTE: Both the PFSense Box and Linux Mint Client were virtualized using VirtualBox. In order to which from the "LAN" Network to the "Office" network it requires that we also change the adapter that is attached to our Linux Client: "Mint" from either: **LAN0 or ***LAN1.

![PFSense - Interface Assignments](https://github.com/user-attachments/assets/b3d9e417-ec79-462f-ad0f-c9981ed0fc17)

<h3>Understanding the Assigned NICs</h3>
<p>The network design is explained in the .txt file named: Network Design Breakdown.txt</p>


<i>Network Design Breakdown.txt</i>

```
# 3 Adapters passed into PFSense VM via VirtualBox

Adapter 1*: WAN | Assigned IP-Address via DHCP from our ISP

Adapter 2**: LAN | Assigned a Static IP-Address  of: 192.168.5.1 in Pfsense

Adapter 3***: OPT | Assigned a Static IP Address of: 192.168.10.1 in Pfsense

# PFSense Assignments/DHCP Configuration


Adapter 1* : WAN (em0) | Ipv4 Configuration Type = DHCP | Enabled
	
	We want WAN (em0) to receive an address from our ISP via DHCP which was: 10.0.2.15/24
	Subnet mask: 255.255.255.0

Adapter 2** : LAN (em0) | Ipv4 Configuration Type = Static | Enabled
	
	We want LAN (em1) to be a static ip address of: 192.168.1.1/24
	Subnet mask: 255.255.255.0

	DHCP Config:
	In networking, it is typical to have other networking hardware such as access points, unmanaged/managed Switches which will need addressing that 
	we don't ever want to change so for these network appliances we can use any address from: 192.168.1.2 - 192.168.1.9 | 7 Devices that have
	static ip addressing for ease of management as these addresses will be purposely left out of our DHCP Address pool.

	Subnet: 192.168.1.0
	Subnet range: 192.168.1.0 - 192.168.1.254
	Address pool range: 192.168.1.10 - 192.168.1.254
	Gateway: 192.168.1.1

Adapter 3** : OFFICE (em1) | Ipv4 Configuration Type = Static | Enabled

	NOTE: OPT was renamed to OFFICE in PFSense and a VLAN named OFFICE was assigned to this interface (em1) 
	
	We want LAN (em1) to be a static ip address of: 192.168.5.1/24
	Subnet mask: 255.255.255.0

	DHCP Config:
	In networking it is typical to have other networking hardware such as access points, unmanaged/managed Switches which will need addressing that 
	we don't ever want to change so for these network appliances we can use any address from: 192.168.1.2 - 192.168.1.9 | 7 Devices that have
	static ip assignments for ease of management as these addresses will be purposely left out of our DHCP Address pool.

	Subnet: 192.168.5.0
	Subnet range: 192.168.5.0 - 192.168.5.254
	Address pool range: 192.168.5.10 - 192.168.5.254
	Gateway: 192.168.5.1

	
	
```

<h3>Network Segementation</h3>
<p>In a real IT Environment, networks designed at scale aren't flat networks. This is done by evaulating and understanding the network requirements of an organization at scale and breaking down this large network into many different smaller subnets for the sake of performance, scalability and security.</p>

</br>
<p>For the sake of my Lab, I have choosen to segement both the LAN Network and the Office Network from each other to mimic that of a real world environment. This means that the "Office" Network should be contained within itself and same for the "LAN" Network; this was achived by subnetting what would have been a large flat network otherwise. A VLAN assignment was also made with PFSense for the "Office" Network.</p>

<h5>VLAN Assignment</h5>

![PFSense - VLANs](https://github.com/user-attachments/assets/811cdb27-ce7f-451b-b981-b41a225510bc)

<h3>Network Security</h3>
<p>Network are secured in the real world using zero-trust principles that are implemented into networks through segmentation, firewall rules, ACLs/VPNs, active threat detection/prevention, monitoring, etc... 

In this Lab, I've created Firewall rules which restrict the flow of traffic from one Network to another; in our case it is the "Office" and "LAN" networks and vice versa. For Example, I don't want anyone within the "Office" network to be able to reach the PFSense Firewall console, a rule within PFSense was created to blocks any IPv4 traffic that originates from the "Office" subnet on port 443, 80 to the destination address: 192.168.1.1 (This Firewall) to be be blocked.</p>

<h4>Office Firewall Rules</h4>

![PFSense - OFFICE Firewall Rules](https://github.com/user-attachments/assets/5292d362-34e4-4d06-95f8-c58e76945439)

<h4>LAN Firewall Rules</h4>

![PFSense - LAN Firewall Rules](https://github.com/user-attachments/assets/506d9500-f174-4309-b510-23c5aafbc8e3)




