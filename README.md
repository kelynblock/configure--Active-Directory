# configure--Active-Directory<p align="center">
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

- Install Active Directory
- Create an Admin & User Account
- Join a client to the Domain
- Run Powershell script to create users

<h2>Deployment and Configuration Steps</h2>

First thing we do is setup our Resources in Azure:
Create a virtual machine (VM) that will be the Domain Controller(DC) using Image Windows Server 2022> Size: Standard_E2s_v3-2vcpu (less than 2 vcpu would run too slow >Make note of the resource group & Virtual Network (Vnet) details >While this VM is being created, start creating another one >Create the client VM with Windows 10 Image >Use the same resource & Vnet as DC VM. >Go back to DC VM >Networking (settings) >network Interface >IP configurations >hit dynamic >change to Static >Save


<p>
<img src="https://i.imgur.com/I4p2OQv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After setting up the Virtual Machines in Azure, remote desktop into the DC fisrt by entering the public IP address (obtained from Azure VM details). Once logged in, you're greeted by the Server Manager Dashboard. 
In server manager >Configure This local server (The #1 option) >Add Roles & Features (The #2 Option) >Role based or feature based Installation >Select a server from the server pool (DC1 is highlighted >select Active Directory Domain Services >next on all dependencies screens >Install >close once complete.
Click on the yellow warning flag sign in top right corner >Promote this server to a domain controller >New Forest >Name the Domain (can be any; mydomain.com for this lab) >Check the DNS & GC box & select your password >In additional options, type domain name (mydomain) >Install. RDP session will end as the system restarts.
Upon restart, log backin w/the domain FQDN (Fully Qualified Domain Name) you setup the the Server with.

</p>
<br />

<p>
<img src="https://i.imgur.com/c1fllux.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Tools (Upper right corner)>Users & PCs
(Organizational Units in Active Directories are like folders) Click on domain >New >OU >Create an OU named “_ADMINS” & another named “_EMPLOYEES” (Done this way so that it stands out to you, but not normally the case)
Click on new OU Admins >New > User> Create an Admin User & make note of the login name (I created Paulie Penne as Admin)
Make this user Admin: Right-click user >Properties >Member of >Add >enter “Domain” >check names >Domain Admins >ok >Apply >Ok.
Click on new OU Employees >New > User> Create a User & make note of the login name (I created Larry Lasagna as a user)
At CMD prompt>logoff Logs off 
Log back in as the Administrator and confirm with CMD Prompt, whoami.

</p>
<br />

<p>
<img src="https://i.imgur.com/CauPgvc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To be part of a domain, you must use the Domain Controller’s DNS server instead of a regular DNS server.
To join a domain, in Azure, get the Private IP address of the Domain Controller (DC-1 in my case) (Networking tab >NIC Private IP >Copy), Go to Client (Client-1 in my case) RDP, Networking >click on network Interface >DNS Servers under settings >Change DNS Servers to custom >enter the NIC Private IP copied from DC-1.
Restart Client-1. Copy Client-1 Public IP & re-launch RDP.
Go to Client-1 system settings (right click Start menu >System >Settings >Rename this PC Advanced >In Computer name Tab, hit Change >Member of >Domain >enter domain name (mydomain.com) >enter credentials >close all tabs so it can restart.
Copy the public IP again & re-launch RDP. Login with Admin credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/PIG8Em9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This script will create 1000 users in the _Employees OU created earlier with Password1 as their default password. Once logged in to the Domain Controller, Search >Powershell_ISE >right-click to run as Admin >Create a new file >Paste the script contents >Run the script. Once complete, we can get the credentials of any of the created users and login as a way to familiarize ourself with the system.

</p>
<br />
