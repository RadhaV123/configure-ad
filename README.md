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

- Step 1. Setup Resources in Azure
- Step 2. Ensure Connectivity between the client and Domain Controller
- Step 3. Install Active Directory
- Step 4. Create an Admin and Normal User Account in AD
- Step 5. Join Client-1 to your domain (mydomain.com)
- Step 6. Setup Remote Desktop for non-administrative users on Client-1
- Step 7. Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Setup Resourses in Azure
  
- Create a resourse group
- Create the Domain Controller VM (Windows Server 2022) named “DC-1” Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Set Domain Controller’s NIC Private IP address to be static
- Virtual Machines -> DC-1 -> Networking -> Network Interface -> IP configurations -> ipconfig1 -> change from Dynamic to Static -> Save
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in DC-1 - Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
  
Note: make sure you let DC-1 VM finish deploying, otherwise, when creating the second VM, it wont show up.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Ensure Connectivity between the client and Domain Controller

- Login to Client-1 with Remote Desktop and ping DC-1’s private IP address
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Open Command Line -> type in pinng -t (perpetual ping) and the private IP address -> enter.
- Observe how the comman did not work
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
- Minimize Client-1 VM window and go to DC-1 VM
- Once inside DC-1 VM go to finder and type in "Windows Defender firewall with advance security" and open it and expand it
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click on Inbound Rules -> Protocol -> look for "Core Networking - Destination unreachable Fragmentation Needed (ICMPv4)
Right click on the 2 rules directly under it and click "enable rule"

</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Minimize DC-1 VM and go back to Client-1 VM
- Observer how the ping started working again after enabling the ICPv4 request on the windows firewall.
- Press Control C and close the command line window.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Install Active Directory
  
- Minimize Client 1 VM and Go to DC-1
- Open up Server manager
- Click Add roles and features -> click next until you get to Server Roles -> Click on "Active Directory Domain Services" -> Add feature -> click next until it says install -> install -> click close after its done
</p>
<p>
- Click on the Yellow exclamation point icon next to the flag on top
- Click on "Promote thi server to a domain controller"
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Click on Add a new forest
- Type in "mydomain.com"
- Click next to type in a password and put which ever password you choose
- Click next to everything and install

</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After it finishes installing, it be automatically sign you out
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Open it back up but instead of using the username you originally put in for the VM, type in the username you put in the root domain name \ your VM username
- In this case, it is mydomain.com\Derek15. Reason for this is because we turned this VM into a Domain controller
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
4. Create an Admin and Normal User Account in AD
- In DC-1 VM, Open Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
- Right Click mydomain.com -> New -> Organizational Unit -> Name it _EMPLOYEES -> Ok
- Right Click mydomain.com -> New -> Organizational Unit -> name it _ADMIN
- Right click mydomain.com and click Refresh it

</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Right click _ADMIN -> New -> User and name it Jane Doe and set a username and password
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- After you created the user, go to the _ADMINS folder
 Right click Jane Doe -> properties -> Add -> Type in "domain" -> Check names -> Domain Admin -> Ok -> Apply, then click Ok
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log out of DC-1 VM and log back in DC-1 VM as the user we just created.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- After your logged in, If you want to make sure you are logged in as Jane Doe, Open up command line and type in "whoami" or "hostname" and it user you are logged in as will pop up
5. Join Client-1 to your domain (mydomain.com)
  
- Minimize DC-1 VM
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
- Go to DC-1 VM(in azure), Click networkig and get its private IP
- Go to Client-1(in azure), Click netwoking -> Network interface -> DNS servers -> click "custom" and put int DC-1's private IP address
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go to Client-1 overview page, click Restart
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Log back in to Client-1 VM in Microsoft Remote Desktop
- Right click Start -> System -> Name this PC (Advanced) -> Change -> Click Domain and type in domain.com
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- It will ask to type in username and password
- Type in the username and password for DC-1
- A window will pop up asking if you want to restart the computer now. Click restart now
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
6. Setup Remote Desktop for non-administrative users on Client-1
Open Client-1 VM again. Instead of using the credentials you created for Client-1 VM, put in the username and password for DC-1
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once inside, right click start -> system -> Remote Desktop -> Select users that can remotely access this PC -> Add -> type in "domain users" -> check names -> Ok
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Minimize Client-1 VM and go to DC-1 VM
Open Active directory users and computes and click mydomain.com -> users -> Domain Users -> Members
You will see all the user accounts that have been created that are allowed to remotely login to Client-1
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
7. Create a bunch of additional users and attempt to log into client-1 with one of the users
  
- In DC-1, Open PowerShell ISE As an administrator (rigth click powershell)
- Copy the files from this link (https://github.com/DereHz/Generate-Names-Created-Users/blob/main/README.md)
- Paste it on PowerShell
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
- Attempt to log into Client-1 with one of the accounts (take note of the password in the script)
- For example, go to ADUC and click on mydomain.com -> _EMPLOYEES -> pick a random user recently created -> Account -> and copy the username
- Go to Client-1 and log off of it
- Log back in with the username of one of the users you picked
- mydomain.com\bec.huvo
  
You have successfully configured Active Directory

That Concludes This Project.
</p>
<br />
