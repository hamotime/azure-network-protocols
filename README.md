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
<b>3. Configuring a Firewall (Network Security Group)</b>

- Open the Network Security Group (NSG) of the Linux VM in Azure and disable inbound and outbound ICMP traffic. To do this open the VM in Azure -> Network Settings -> Observe the Network Security Group Panel, click Create port rule -> Configure the settings to Block ICMP Traffic to and from the Linux VM (Refer to screenshot for this step if not sure what to configure), click Add.
- Back in the Windows VM ping the Linux VM. You will see IN Wirehsark "No response found!" messages to the ping requests. Also observe the "Request timed out" messages in command line or Powershell. This demonstrates that we successfully created a virtual firewall rule to block incoming ICMP traffic to the Linux VM.
- Open NSG of the Linux VM in Azure and delete the block ICMP rule that we created. Click the trash can icon that is next to it to delete it.
- Go back to the Windows 10 VM and ping the Linux VM. Observe Wirehsark and you should see request and reply communication between the two VMs again. You should also see successful replies from the Linux VM in Powershell from the ping request. We can successfully communicate with the Linux VM again over ICMP.
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
