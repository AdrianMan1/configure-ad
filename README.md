<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed within the Cloud (Azure)</h1>
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

- Step 1: Install Active Directory 
- Step 2: Create Domain Admin User within the domain
- Step 3: Join the Client to the Domain
- Step 4: Create users within AD

<h2>Ancillary Information</h2>

- Dealing with account lockouts/resetting passwords
- Enabling/Disabling Accounts 
- Group Policy

<h2>Deployment and Configuration Steps</h2>

<ins>Step 1:Install Active Directory<ins/>

</p>
<p>
Before we begin anything within Active Directory, we first need to install it!
  
  Within the domain controller UI, we will install Active Directory Domain Services using the following steps:
  - Open Server Manager > Dashboard
  - Click Add Roles & Features
  - Click next until the Server Roles tab
  - Click "Active Directory Domain Services" & add features, then continute pressing next 
  - Click "Restart if necessary" button then install.
  
  You have now installed AD! From here we have to promote our Windows Server as the Domain Controller:
  
  - Click the flag on the top right of the server manager dashboard then click *Promote this server to a domain controller*.
  - Add a new forest and name it your desired domain name (ex.mydomain.com). Press next.
  - Create a Directory Services Restore Mode (DSRM) password. (Dont forget to write it down!)
  - Continue through tabs til installation prompt is givem, Install.

Now that the Windows Server has been promoted to DC and have created our domain, we can continue with creating a domain admin user. 

  
  

</p>
<br />

<ins>Step 2: Create Domain Admin User within the domain<ins/>
<p>
textexttext
</p>
<br />

<ins>Step 3:Join the client to the domain<ins/>

</p>
<p>
textexttext
</p>
<br />

<ins>Step 4:Create Users within AD<ins/>

</p>
<p>
textexttext

We do this by using this script:



</p>
<br />


<h2>Useful Knowledge</h2>

<ins>Group Policy<ins/>

<ins>Dealing with Account Lockout/Resetting Passwords<ins/>

<ins>Enabling/Disabling Accounts<ins/> 
