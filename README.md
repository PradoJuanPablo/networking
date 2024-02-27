<p align="center">
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/88d8ebc1-2d0d-41fa-abad-94374e32f6d2">

</p>

<h1>Cisco Networking</h1>
In this tutorial, we're going to create a Small Office Home Office (SOHO) network in Cisco Packet Tracer. <br />


<h2>Environments and Technologies Used</h2>

- Cisco Packet Tracer

<h2>Operating Systems Used </h2>

- Windows 11

<h2>High-Level Steps</h2>

- Creating our Network Topology
- Device Configuration
- Testing Connectivity

<h2>Creating our Network Topology</h2>

<p>
<img width="1186" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/a86ae3ce-ed80-4d17-a1c4-d74c816623e0">

</p>
<p>
In this network we're going to have:
  
- One Router and Swicth
- 3 departments (Admin/It, Finance/HR, Customer Service/Reception)
- Host devices and Wireless Access Points

We connect all devices to the switch using a Copper Straight-Through cable
  
</p>
<br />
<h3>Subnetting</h3>
<p>
<img width="537" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/9af36709-3023-4284-864e-560c8c5ec2d1">

  
<img width="450" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/611e44a8-2c31-486c-9aab-5c65958f1c35">

<img width="295" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/b82c691b-4fe8-4399-b470-a0a777e89279">

</p>
<p>
We are given an IP address of 192.168.1.0 that will be our base network. From that we need to create 3 subnets for each of the departments. 

The formual to find the number of subnets is: 2^n (n is the number of borrowed network bits)

So our equation is 2^n=3 and we need to find the value of n and we need to find a number that is equal to or greater than 3.
2^2=4 which is greater than 3 so the number of borrowed network bits will be 2. 

Given the base network address, we can see that it is a Class C address. The subnet mask for a Class C IP address is 255.255.255.0 which in binary is 11111111.11111111.11111111.00000000

After borrowing 2 network bits to satisfy the need for 3 subnets, our new binary address is 11111111.11111111.11111111.11000000 which in decimal is 255.255.255.192 or a /26 in slash notation. 

Now we find the block size. We can see that when an subnet mask ends with 192, the block size is 64. Making our block size 64. 

Now lets allocate IPs to each department:

- The 1st Subnet will have a Network ID of: 192.168.1.0
- The 2nd Subnet will have a Network ID of: 192.168.1 64
- The 3rd Subnet will have a Network ID of: 192.168.1.128





Next we need to find the Broadcast ID. The Broadcast IP will be the last IP before moving onto the next subnet.

- 1st Subnet Broadcast ID: 192.168.1.63
- 2nd Subnet Broadcast ID: 192.168.1.127
- 3rd Subnet Broadcast ID: 192.168.1.191


Lastly, lets from the range of avaliable Host IDs. Which are the IPs that will be given to the host devices. These IPs are between the network ID and broadcast ID.

- 1st Subnet Host Range: 192.168.1.1 - 192.168.1.62
- 2nd Subnet Host Range: 192.168.1.65 - 192.168.1.126
- 3rd Subnet Host Range: 192.168.1.129 - 192.168.1.190


Now that we finished subnetting, we can begin configuring the devices. 
</p>
<br />

<h2>Device Configuration</h2>
<h3>Configuring VLANs</h3>
<p>
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/db31d47a-8f5c-4e1e-9e6d-9408b1297cf0">

<p>
Now we need to configure each VLAN and group the ports to correspond to wach VLAN. To do this we enter the switch and enter global configuration mode (conf t) and enter:
  
- <b>interface range fa0/2-4</b>:
    This command means we are entering interface configuration mode and we want to configure the settings for ports fa0/2, fa0/3, fa0/4
- <b>switchport mode access</b>:
    This command tells the switch that the interface will be connected to a device like a computer or printer. The switch will add information to the data going through that port to make sure it goes to the right place on the network
- <b>switchport access vlan 10</b>:
    This command sets up the interface to connect devices to VLAN 10. So any traffic that comes through this interface will be associated with VLAN 10.
- <b>do wr</b>: short for "write memory" is used to save the configuration settings of the device even if it is restarted.

  The same commands are applied to the other VLANs with their corresponding interfaces
</p>
<br />

<h3>Configuring Wireless Access Points</h3>

<p>
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/34eb20d8-9c7d-4c86-8491-cc23add9c08c">


<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/5dc56dae-efbd-4938-aa74-740fd19808d7">

<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/f86ae909-6c51-48a8-bd4e-e8914ee2ba02">



</p>
<p>
To configure the access point we click on the access point > "Config" tab >  Port 1 > and set the SSID to the appropriate department > Select WPA2-PSK and set the password
</p>
<br />

<h3>Configuring Trunk Port</h3>

<p>
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/d05b114f-fe61-442c-9108-2e67048fdedb">

</p>
<p>
Since the router has one connection to the switch and the switch has multiple VLANs within it, that interface must be able to allow passage of multiple VLANs. To do that we need to make that interface a trunk port, which allows traffic from multiple VLANs. To configure that we need to find out which interface leads to the router. By hovering over the cable we can see that the interface we are looking for is fa0/1 on the switch. So we enter the swicth and enter: 
  
- <b>int fa0/1</b>: This is the port we are entering configuration in 
- <b>switchport mode trunk</b>: This command instructs the interface to allow passage of multiple VLAN traffic
- <b>do wr</b>: This command saves the configuration settings
  
</p>
<br />

<h3>Opening Router Interface</h3>

<p>
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/0694524f-926c-46ab-a2b8-0750c51203bd">

</p>
<p>
By default, port gig0/0 on the router is closed and we must open it to allow traffic to pass. To do that we enter the CLI and enter:

- <b>int gig0/0</b>: This is the port we are entering configuration in 
- <b>no shut</b>: This command opens the port
- <b>do wr</b>: This command saves the configuration settings
  
</p>
<br />

<h3>Inter VLAN Routing</h3>

<p>
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/163960b0-217b-4000-a238-96de57faa71f">

</p>
<p>
We will now create multiple sub interfaces from a single phyical interface and assign them an IP address that will act as the default gateway to its respective VLAN. To do that we get into tge router and type:

VLAN 10:
- <b>int gig0/0.10</b>: This command specifies a subinterface on gig0/0 
- <b>encapsulation dot1Q 10</b>: This command tells the device to add a tag to the data it sends and recieves on this interface to know which VLAN the data belongs to. 
- <b>ip address 192.168.1.1 255.255.255.192</b>: This command assigns the an IP to the subinterface. The IP we use is the first usable IP in the host range. This IP will act as the default gateway for the subnet. All the subnets will have the same Subnet Mask. 

VLAN 20
- <b>int gig0/0.20</b>: 
- <b>encapsulation dot1Q 20</b>: 
- <b>ip address 192.168.1.65 255.255.255.192</b>: 


VLAN 30
- <b>int gig0/0.30</b>: 
- <b>encapsulation dot1Q 30</b>: 
- <b>ip address 192.168.1.129 255.255.255.192</b>: 
  
<br />


<h3>Configuring DHCP</h3>

<p>
<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/3c5158eb-44f3-491f-a8b6-a9a0419cbb12">

<img width="500" alt="image" src="https://github.com/PradoJuanPablo/networking/assets/160810181/d9141d84-bba3-461f-b2e4-ee42220edb90">


</p>
<p>
Now the devices connected to the network must be able to obtain IP addresses automatically, to do that we must configure Dynamic Host Configuration Protocol(DHCP) which is repsonsible for automatic IP assignment. 

To configure the DHCP server, we must enable DHCP service on the device, which for Cisco equipment comes enabled by default.

- <b>service dhcp</b>: This command will enable DHCP services on the device
- <b>ip dhcp pool Admin-Pool</b>: This command creates a pool that represents the Admin/It subnet. 
- <b>network 192.168.1.0 255.255.255.192</b>: This command assigns a network address to the pool we created
- <b>default-router 192.168.1.1</b>: This command assigns the default gateway. The IP used for the default gateway will be the IP address of the subinterface that we created and assigned to each VLAN (gig0/0.10)
- <b>dns-server 192.168.1.1</b>: This command tells the Cisco device to use the DNS server located at 192.168.1.1 to resolve domain names to IP addresses.
- <b>domain-name Admin.com</b>: This command will specify the domain name for the devices in the subnet.
- <b>exit</b>:

Now the devices in the subnet will be assigned IPs automatically

Repeat these configs to the other two remaining subnets and include their respective subnet information. 
  
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


