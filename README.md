<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
Hello and welcome! In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

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

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

For this tutorial you will need to create a resource group and two virtual machines (VM) in the Azure portal, one VM with a Windows 10 server (VM1) and the other VM with a Linux (Ubuntu) server (VM2). Make sure you select either (2) or (4) virtual cpu's (VCPUs) for size. Next, remote desktop (RDC) into the Windows VM and download and install the program Wireshark.    
<br />

<p>
<img src="https://i.imgur.com/GHzjkqF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Observe ICMP Traffic</h3>

Internet Control Message Protocol is the protocol used by "ping," to check the connection between two computers.

Open Wireshark and click the blue icon on top left below the File tab to begin capturing "packets." Then, filter for ICMP traffic by entering "icmp" in the filter bar. Remember that ICMP is the protocol that "ping" uses, which is used to check connectivity between two machines/systems; we will check the connection between the two VM's.

<br />
<p>
<img src="https://i.imgur.com/zA2l7ow.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://i.imgur.com/Y8DmN9Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

From the Azure portal, copy the private IP address of the Ubuntu VM. Go back to Windows VM in RDC and open either Command Prompt or Powershell and "ping" the private IP address of VM2. Observe the ICMP traffic in Wireshark.   

<br />
<p>
<img src="https://i.imgur.com/n19x6cO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

Next, initiate a perpetual (non-stop) "ping" (ping <IP address> -t) from V1M to VM2.
<br />
  
<p>
<img src="https://i.imgur.com/r0eRDhl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>You will now change the Firewall settings of VM2 to stop the passing of ICMP traffic: in the Azure portal, search for "network security groups" and click the one for VM2 > click "Inbound security rules" under Settings on the left panel > click Add > select ICMP for Protocol > select Deny for Action > give the rule any name, such as "Deny_ICMP" > Add</p>    

<br />

<p>
<img src="https://i.imgur.com/56ZuMDB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

Observe the activity in Powershell and notice that the ping request begins to fail/time out.

<br />
<p>
<img src="https://i.imgur.com/kG2TEJO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

Now, go back to Azure portal and enable ICMP traffic in the rule you created. Notice the ping request begins to succeed and receive a reply in Powershell. You can then stop the ping by typing control (C) in Powershell.  

<br />

<p>
<img src="https://i.imgur.com/pWTVK3I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<h3>Observe SSH Traffic</h3>

Secure Shell protocol is used when remotely connecting from one computer to another and accessing a command-line.

In Wireshark, filter for SSH traffic by typing "ssh" in the filter bar and clicking the green refresh icon above it.

<br />

<p>
<img src="https://i.imgur.com/Eg9bYIy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

You will now "SSH into" VM2 from VM1 (within RDC) by typing "ssh," the username from VM2 setup and the private IP address of VM2 in Powershell. Answer "yes" for question at the bottom to continue connecting and enter the password created during VM2 setup when prompted (it will not be visible). Notice username in green at bottom, which means connection to VM2 is established. Notice the SSH traffic in Wireshark. You can now close the VM2 connection by typing "exit" and pressing "enter."   

<br />

<p>
<img src="https://i.imgur.com/HujYTOg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Observe DHCP Traffic</h3>

Dynamic Host Configuration Protocol (DHCP) is the protocol that provides a device on the network its IP address. 

In Wireshark, filter for DHCP traffic using same steps from above and attempt to renew the IP address of VM1 within RDC by typing "ipconfig /renew" in Powershell. Notice the DHCP traffic in Wireshark. 

<br />

<p>
<img src="https://i.imgur.com/43vRDUd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Observe DNS Traffic</h3>

Domain Name System is the protocol used by the command-line "nslookup" to retrieve the IP address of websites.

From VM1 in RDC, filter for DNS traffic in Wireshark using steps from above and in Powershell type the command-line "nslookup www.google.com" to retrieve the IP address of google.com. Notice Google's addresses and the DNS traffic in Wireshark.  

<br />

<p>
<img src="https://i.imgur.com/LSnCgoT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Observe RDP Traffic</h3>

Remote Desktop Protocol uses "port number" 3389 and is used when remotely connecting from one computer to another.

In Wireshark, follow same steps from above and filter "tcp.port == 3389." Observe the streaming traffic of "packets" between your actual computer and VM1, which you are accessing via RDP.

<br />

<p>
<img src="https://i.imgur.com/2BGLstI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Cleanup</h3>

You can close your Remote Desktop connection and delete the Resource Group created at the beginning of this tutorial. Verify Resource Group deletion.

This concludes the tutorial. Congrats!

