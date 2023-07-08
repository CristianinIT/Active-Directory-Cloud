<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up Azure enviroment
- Create Active Directory domain controller VM (Windows Server) and a client VM (Windows 10)
- Create users using a PowerShell script
- Log into client VM and test AD 

<h2>Deployment and Configuration Steps</h2>

<p>
Start the tutorial by creating the domain controller (DC) first using Windows Server for operating system and 2 vcpus for processing power. Next, create the client side using windows 10 OS and 2 vcpus for processing power as well. Set the DC's NIC private IP address from dynamic to private to ensure DC's private IP address won't change. Once everything is set up you can confirm if it's set up properly by viewing the network topology and verify both VMs are in the same Vnet.
</p>
<br />
<p>
<img <img width="960" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/6e4209a5-7783-42de-922e-e4df177a6faa">
</p>

<p>
Let's ensure that the there is connectvity between the client and domain controller. Login to client VM with RDP and ping DC's private address with "ping -t IP address" this is a perpetual ping. You will notice a series of "Request timed out" messages. Next, enable ICMPv4 traffic on local Windows Firewall on DC-1. Check back on the client's VM to see if there's connectivity now.
</p>
<br />
<p>
<img <img width="674" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/9212187d-75bf-4e0b-a3ef-1d9244290796">
</p>
<p>
<img width="883" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/5e1ac4c8-04ca-42ec-bf5f-674bffd40bac">
<p>
<img <img width="666" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/76c387aa-7247-4caa-bc2e-ece47b722ca4">

<p>
Now we can finally install Active Directory within the DC's VM. 
</p>
<br />
