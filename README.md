<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 22.04

<h2>High-Level Steps</h2>

- Create our Virtual Machines using Azure
- Observe ICMP Traffic
- Configuring a Firewall (Network Security Group)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>
<b>1. Create our Virtual Machines</b>

- Create a Resource Group. I've called mine "NA-Lab"
- Create a Windows 10 Virtual Machine. When creating, select the previously created Resource Group and create a new Virtual Network (Vnet) and Subnet in setup process.
- Create a Linux (Ubuntu) VM. While creating, select the previously created Resource Group and the Vnet you created when creating the Windows 10 VM. <b>THE VIRTUAL NETWORK MUST BE THE SAME FOR BOTH VMS!</b>
- Ensure both VMs are in the same Virtual Network/Subnet
- We are done. Make sure to keep both VMs running for the next step.
</p>
<p>
<img src="https://i.imgur.com/Rd06cbU.png" height="80%" width="80%" alt="Create Our VMs Steps"/>
<img src="https://i.imgur.com/3tUuwHk.png" height="80%" width="80%" alt="Create Our VMs Steps"/>
<img src="https://i.imgur.com/8Twb92j.png" height="80%" width="80%" alt="Create Our VMs Steps"/>
<img src="https://i.imgur.com/8s41hYQ.png" height="80%" width="80%" alt="Create Our VMs-Steps"/>
<img src="https://i.imgur.com/u4fidSP.png" height="80%" width="80%" alt="Create Our VMs Steps"/>
<img src="https://i.imgur.com/BRynZao.png" height="80%" width="80%" alt="Create Our VMs Steps"/>
<img src="https://i.imgur.com/NVmhY4I.png" height="80%" width="80%" alt="Create Our VMs Steps"/>
</p>
<br />

<p>
<b>2. Observe ICMP Traffic</b>

- If using a Mac, make sure to install Microsoft Remote Desktop first. For this lab I will be using a Windows machine.
- Use Remote Desktop to connect to your Windows 10 VM
- Within your Windows 10 VM, install <a href="https://www.wireshark.org/#downloadLink">Wireshark</a>. Make sure to tick "Install Ncap version" during the installation process.
- Open Wireshark and start a packet capture. Make sure to select the correct NIC (Usually Ethernet) and click the blue play button in the top left.
- Within Wirehsark, filter for ICMP traffic only by typing ICMP in the text bar below the toolbar.
- Retrieve the private IP address of the Linux VM and attempt to ping it from the Windows 10 VM. Observe the ping requests and replies within Wireshark. <b>We can see a total of 8 packets were captured showing request and reply ICMP communication between the two VMs.</b>
- From the Windows 10 VM, open command line or Powershell and attempt to ping a public website and observe the traffic in Wireshark. <b>Can also see 8 packets were captured when pinging google.com showing request and reply ICMP communication between Windows 10 VM and the google.com web server. Furthermore, we know that we have successfully reached the destination over ICMP.</b>
</p>
<p>
<img src="https://i.imgur.com/3y5uayG.png" height="80%" width="80%" alt="Observe ICMP Traffic Steps"/>
<img src="https://i.imgur.com/bidnBQD.png" height="80%" width="80%" alt="Observe ICMP Traffic Steps"/>
<img src="https://i.imgur.com/g6LEuwC.png" height="80%" width="80%" alt="Observe ICMP Traffic Steps"/>
<img src="https://i.imgur.com/rmLEAw2.png" height="80%" width="80%" alt="Observe ICMP Traffic Steps"/>
<img src="https://i.imgur.com/DX1n8dj.png" height="80%" width="80%" alt="Observe ICMP Traffic Steps"/>
<img src="https://i.imgur.com/QHCcRve.png" height="80%" width="80%" alt="Observe ICMP Traffic Steps"/>
</p>
<br />

<p>
<b>3. Configuring a Firewall (Network Security Group)</b>

- Open the Network Security Group (NSG) of the Linux VM in Azure and disable inbound and outbound ICMP traffic. To do this open the VM in Azure -> Network Settings -> Observe the Network Security Group Panel, click "Create port rule" -> Configure the settings to Block ICMP Traffic to and from the Linux VM (Refer to screenshot for this step if not sure what to configure), click Add.
- Back in the Windows VM ping the Linux VM. You will see in Wirehsark "No response found!" messages to the ping requests. Also observe the "Request timed out" messages in command line or Powershell. This demonstrates that we successfully created a virtual firewall rule to block incoming ICMP traffic to the Linux VM.
- Open NSG of the Linux VM in Azure and delete the block ICMP rule that we created. Click the trash can icon that is next to it to delete it.
- Go back to the Windows 10 VM and ping the Linux VM. Observe Wirehsark and you should see request and reply communication between the two VMs again. You should also see successful replies from the Linux VM in Powershell from the ping request. We can successfully communicate with the Linux VM again over ICMP.
- You can stop the packet capture and close Wireshark.
</p>
<p>
<img src="https://i.imgur.com/oA2Dyma.png" height="80%" width="80%" alt="Configuring a Firewall Steps"/>
<img src="https://i.imgur.com/pl3H0mk.png" height="80%" width="80%" alt="Configuring a Firewall Steps"/>
<img src="https://i.imgur.com/w7RunW8.png" height="80%" width="80%" alt="Configuring a Firewall Steps"/>
<img src="https://i.imgur.com/WywnzPT.png" height="80%" width="80%" alt="Configuring a Firewall Steps"/>
<img src="https://i.imgur.com/4tIRCIK.png" height="80%" width="80%" alt="Configuring a Firewall Steps"/>
<img src="https://i.imgur.com/OH6iwx0.png" height="80%" width="80%" alt="Configuring a Firewall Steps"/>
</p>
<br />


<p>
<b>4. Observe SSH Traffic</b>

- Log back into the Windows VM.
- Open Wireshark and start a packet capture.
- Filter for SSH traffic only! Can do this by using filters "ssh" or "tcp.port == 22"
- From the Windows 10 VM SSH into your Ubuntu VM via its private IP address. Command will use the format ssh username@privateipaddress, for us is ssh labuser@10.0.0.5.
- During the connection process you will notice SSH traffic generating in Wirehsark telling us the two hosts are conducting an SSH handshake and encrypted session setup. When the key exchange between the hosts is complete, observe the packet that says "New Keys". This tells us the SSH connection is established and all subsiquent packets going forward will be encrypted. 
- Once connected, you will see the username change to labuser@linux-vm, this signals you have successfully logged into the Linux VM over SSH.
- Type in commands and observe the SSH traffic populating in Wireshark. You will notice packets being creating for individual keystrokes and commands being entered. The payloads for the packets will be encrypted (unreadable) as a secure encrypted tunnel has been created between the two hosts using SSH.
- Stop Packet Capture in Wireshark
<p>
<img src="https://i.imgur.com/5RpgCn7.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/2xkrgzf.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/s68OYMV.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/ePiSE6S.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/2V889Nq.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/oSnHsmv.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/ZZ0rSYh.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/NDGv8Ts.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
<img src="https://i.imgur.com/BhAqiUW.png" height="80%" width="80%" alt="SSH Traffic Steps"/>
</p>
<br />

<p>
<b>4. Observe DHCP Traffic</b>

- Open Wireshark, start a packet capture and filter for DHCP traffic.
- Open notepad and enter the following:<br />
- ipconfig /release
- ipconfig /renew
- Save the file as a .bat file in any directory. I have saved it to C:\programdata\dhcp.bat
- This file will execute commands to release the current IP address from the VM and request a new IP address from the DHCP server. This will create DHCP traffic in Wireshark which we will observe.
- Open Powershell as an administrator, change the directory to where you saved the bat file, and use ls command to confirm the file is there. Next type .\dhcp.bat and press enter.
- You should disconnect from the VM as your IP was released, then within 1-30 seconds be reconnected automatically once the VM obtains a new IP address. If you don't automatically re-connect to the VM, manually re-connect with the RDP client.
- Open Wirehsark and observe the traffic. You should see packets showing the Discover, Offer, Request and Acknowledge process between the VM (10.0.0.4) and the DHCP Server (168.63.129.16). The release packet was sent by the VM to the DHCP server by using the ipconfig /release command. Then ipconfig /renew was run immediately afterwards which initiated the Discover, Offer, Request and Acknowledge process between the VM and the DHCP server for my vnet.
- Stop the packet capture in Wireshark.
<p>
<img src="https://i.imgur.com/SqPJYGq.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/Ln0lznZ.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/rF1nsgn.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/aOC0dop.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/WQUgaeD.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/4hXufla.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/qir3hbx.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/478xkKc.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
<img src="https://i.imgur.com/YD2bgLG.png" height="80%" width="80%" alt="DHCP Traffic Steps"/>
</p>
<br />

<p>
<b>5. Observe DNS Traffic</b>

- Open Wireshark, start a packet capture and filter for DNS traffic.
- Open Powershell and type nslookup disney.com. This command queries the DNS server to find the IP associated with a domain name. It can also provide us with DNS records used by domain names.
- Observe the DNS traffic in Wirehsark. You can see we captured the communcation between the VM (10.0.0.4) and the DNS Server (168.63.129.16). Our VM asked the DNS server for the IP address of disney.com, after same failed responses from the DNS server (indicated by the "No such same") it eventually found the correct A record for disney.com and returned the IP 130.211.198.204 to us.
- Stop the packet capture.
<p>
<img src="https://i.imgur.com/STAivtx.png" height="80%" width="80%" alt="DNS Traffic Steps"/>
<img src="https://i.imgur.com/u6QybIY.png" height="80%" width="80%" alt="DNS Traffic Steps"/>
<img src="https://i.imgur.com/q2keFbj.png" height="80%" width="80%" alt="DNS Traffic Steps"/>
</p>
<br />

<p>
<b>6. Observe RDP (Remote Desktop Protocol)</b>

- RDP allows is a protocol that allows for a remote connection between hosts using a GUI.
- Open Wireshark, start a packet capture and filter for RDP traffic. If you cannot filter using "rdp" use the filter "tcp.port == 3389 || udp.port == 3389" (as RDP can use both TCP and UDP protocols). 
- You should observe a constant flow of packets. This is because RDP is constantly streaming a GUI of the VM (10.0.0.4) to my computer (122.150.109.81). Traffic is constantly going back and forth between the two hosts and will be non-stop as long as the RDP connection exists. If you go to whatismyip.com which it will display your public IP address. Go back to Wireshark and you can see it in the packet capture.
- Stop the packet capture.
<p>
<img src="https://i.imgur.com/vekOvqx.png" height="80%" width="80%" alt="DNS Traffic Steps"/>
</p>
<br />
