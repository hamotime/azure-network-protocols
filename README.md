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
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create our Virtual Machines using Azure
- Observe ICMP Traffic
- Configuring a Firewall (Network Security Group)
- Observe SSH Traffic
- Observee DHCP Traffic
- Observer DNS Traffic
- Observer RDP Traffic

<h2>Actions and Observations</h2>

<p>
<b>1. Create our Virtual Machines</b>

- Create a Resource Group
- Create a Windows 10 Virtual Machine. When creating, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet
- Create a Linux (Ubuntu) VM. While creating, select the previously created Resource Group and the Virtual Network you created when creating the Windows 10 VM. <b> THE VIRTUAL NETWORK MUST BE THE SAME FOR BOTH VMS! </b>
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
- From the Windows 10 VM, open command line or Powershell and attempt to ping a public website and observe the traffic in Wireshark.<b>Can also see 8 packets were captured when pinging google.com showing request and reply ICMP communication between Windows 10 VM and the google.com web server. Furthermore, we know that we have successfully reached the destination over ICMP.</b>
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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
