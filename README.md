<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


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
Now we can finally install Active Directory within the DC's VM. We will have to some post-deployment configuration and promote the server as the domain controller. Create a new forest and the root domain name can be anything you decide to use. After that, restart DC and login under user "mydomain.com\labuser".
</p>
<br />
<p>
<img <img width="890" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/710b1fcd-a1d0-4171-9af1-426c26f17233">
<p>
<img <img width="674" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/df78a3fe-89ec-4c14-bc55-3ae00af04a0e">
<p>
Now it's time to create an Admin and normal user accounts in AD. We will create some Organizational Units by going through the Server Management Dashboard and click on tools to click on Active Directory Users and Computers. From here you want to right click on mydomain.com and create new OUs with the names "_ADMINS" and "_EMPLOYEES"
</p>
<p>
<img <img width="569" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/db60923a-bf05-4a20-86d7-b7ef6c798fc0">
<p>
Next, you will create a new employee in the _ADMINS OU. For this example the new employee's name is John Shelby and the username will be "johnshelby" and make sure to add this person to the "Domain Admins" group. Log out of DC and log back in as "mydomain.com\johnshelby".
</p>
<p>
<img <img width="708" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/d40ef562-9426-4aac-9f54-b4295cd55e75">
<p>
Now we will join client to the domain (mydomain.com). From the Azure portal we will client's DNS settings to DC's private address. Restart client and log back in as labuser join the domain. Go to settings, scroll down to About, and on the right side select Raname this PC (advanced). Change domain to "mydomain.com" and when prompted enter "mydomain.com\johnshelby" for the username and the password that is associated with the John Shelby. A welcome to the domain message should appear. 
</p>
<p>
<img <img width="485" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/d83efb31-b9e5-40c3-a477-3222c08aec58">
<p>
<img <img width="882" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/06662d93-f4c5-4372-a959-eb6120776129">
<p>
We will now verify if client is on the domain by logging back in DC, go to Active Directory Users and Computers, and check the computers folder for Client.
</p>
<p>
<img <img width="563" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/5ff19bbe-85ed-4469-a64b-77ae3c848492">
<p>
We will now set up Remote Desktop for non-administrative users on client. Right click the Windows button, select System, Remote Desktop, Select users that can remotely access this PC. Select add and type "Domain Users" and click "Check names". You can now log in client as a normal non-administrative user.
</p>
<p>
<img <img width="949" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/6d4c5fdf-2f09-4015-a732-137040e558b5">
<p>
Finally we will create a bunch of users and attempt to log in client with one of the users. Log into DC as an administrative and open PowerShell_ise as an administrator. Open this file https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 and copy and paste the script into the file. Run the script and observe the accounts being created. While the accounts are being created, go to "Active Directory Users and Computers" and open up the "_EMPLOYEES" OU and observe the accounts.
</p>
<p>
<img <img width="890" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/306d358e-4ad8-4ab8-ab17-60ffba84408f">
<p>
<img <img width="884" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/d22723e1-91bf-4db6-9571-b5abfa99d36e">
<p>
For the final step, pick one of the users and attempt to log in client with the chosen account. The password for the accounts can be found in the top of the script. Open Command Prompt and use command "whoami" in the command line and this confirm if you did the steps correctly. Congratulations on creating an on-premises Active Directory within Azure VM.
</p>
<p>
<img <img width="665" alt="image" src="https://github.com/CristianinIT/Active-Directory-Cloud/assets/138620922/1d3f656d-786c-411a-845b-84834d3bd249">
