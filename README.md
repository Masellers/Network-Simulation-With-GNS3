# Network Simulation With GNS3 and Analysis with Wireshark

In this Project, we are using a program called GNS3 to simulate and practice networking and routing. We will be configuring and pinging a host on another network and using wireshark to capture packets that we are sending.


# Utilities Used

### - GNS3
GNS3 can be downloaded [here](https://www.gns3.com/software/download)

### - Wireshark


# **Install GNS3**

### Install
You will need to make an account first in order to download but after downloading GNS3 you can follow the install guide provided by GNS3 [here](https://docs.gns3.com/docs/getting-started/installation/windows/).

The installer will ask if you want to sign-up for and install SolarWinds but this is optional. User may need to download additional routers or devices and add them to GNS3 but these can be downloaded and installed for free.

 
# **Step One: Start New Project**

### Start New Project
A new Project can be easily started at the top left of the screen 

<img src="https://i.imgur.com/tGSXoJj.png" height="80%" width="80%" alt="New Project"/>

### Add Devices and Connect them
We will now add devices to our project. We can use the option shown in the image below to add ethernet connection between devices and finally we will label them using the option available in GNS3
* **We are labeling the devices beforehand to show what our finished project will look like**
* Any IP addresses can be used and devices can be clicked and dragged into whatever layout is prefered by the user
  

<img src="https://i.imgur.com/eoJBS7s.png" height="80%" width="80%" alt="Devices"/>
<img src="https://i.imgur.com/Wkdg0EG.png" height="80%" width="80%" alt="Ethernet"/>
<img src="https://i.imgur.com/TkRpNSN.png" height="80%" width="80%" alt="Labels"/>

### Run All Devices

**Devices can be run by pressing the green arrow on the tool bar at the top of the screen**

# **Step 2: Assign IP addresses**

### Assign IP addresses to Router Interfaces
In order to assign IP address we will need to open the console, which can be done by right clicking each devices that we will be interacting with and selecting **Console**. We have already planned out what IP addresses we will be assigning to each device in the previous step. In order to assign an IP address to interface f0/0 and f0/1 on router 1 (R1) we can use the following commands within the console:
* `configure terminal`
* `int f0/0`
* `ip address 192.168.0.1 255.255.255.0`
* `no shutdown`
* `exit`
* `int f0/1`
* `ip address 10.0.1.1 255.255.255.0`
* `no shutdown`
* `end`

These are the inputs for R1 but we will use use the exact same method for router 2 (R2), assigning the correct IP addresses according to our predefined layout. Additionally, we can use the command `sh ip interface brief` to check our work.

<img src="https://i.imgur.com/pMGSEQF.png" height="80%" width="80%" alt="Router 1"/>


### Assign IP addresses to Host devices
The Process of assigning IP addresses to PC1 and PC2 is easier. We can simply type the following commands into the console for each device
* For PC1, `ip 192.168.0.2/24 192.168.0.1`
* For PC2, `ip 172.168.0.2/24 172.168.0.1`

At this point we can use a ping command to and from each device and its corresponding gateway but we won't be able to ping from PC1 to PC2 until we finish routing in the next step. When we ping, It will show 4/5 or 80% success rate. This is caused by ARP and we will look at this again later in Wireshark so we will type `clear arp-cache` after we ping.

<img src="https://i.imgur.com/VlcERGm.png" height="80%" width="80%" alt="PC2 IP address"/>

<img src="https://i.imgur.com/BPPlnYX.png" height="80%" width="80%" alt="Ping1"/>

# **Step 3: Routing**

### Routing
In order to route these networks together use the following commands:
* In the R1 console, `configure terminal`, and then `ip route 172.168.0.0 255.255.255.0 10.0.1.2`
* In the R2 console, `configure terminal`, and then `ip route 192.168.0.0 255.255.255.0 10.0.1.1`

Now we can ping from PC1 to PC2 or from PC2 to PC1, but before we ping we will open wireshark to capture the packets that we are sending.

### Start Packet Capture and Ping
In order to start our packet capture, right click on one of the ethernet connections between the source and destination and select **Start capture** from the menu. Then finally, ping from one device to the other.

<img src="https://i.imgur.com/ejOyNYG.png" height="80%" width="80%" alt="Start Capture"/>
<img src="https://i.imgur.com/CFikBCS.png" height="80%" width="80%" alt="Ping2"/>

# **Step 4: WireShark**
In our Wireshark packet capture we can see our ARP packets followed by ICMP request and reply packets

<img src="https://i.imgur.com/xGbDc1r.png" height="80%" width="80%" alt="WireShark"/>

