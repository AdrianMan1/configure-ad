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


<img src="https://github.com/user-attachments/assets/5de0f701-202e-49f7-8887-d396ae81a416" width="650" height="400"/> 

  - Click next until the Server Roles tab
  - Click "Active Directory Domain Services" & add features, then continute pressing next


<img src="https://github.com/user-attachments/assets/e407907c-fdff-4749-a4c5-1c6680ae951b" width="650" height="400"/>


  - Click "Restart if necessary" button then install.
  
  You have now installed AD! From here we have to promote our Windows Server as the Domain Controller:
  
  - Click the flag on the top right of the server manager dashboard then click *Promote this server to a domain controller*.
![image](https://github.com/user-attachments/assets/a81fc6df-f2a2-467d-a79d-f088a71dd94d)

  - Add a new forest and name it your desired domain name (ex.mydomain.com). Press next.
<img src="https://github.com/user-attachments/assets/43d8bbb8-05db-4314-a3ba-5a1d766305df" width="600" height="500">


  - Create a Directory Services Restore Mode (DSRM) password. (Dont forget to write it down!)
  - Continue through tabs til installation prompt is givem, Install.

Now that the Windows Server has been promoted to DC and have created our domain, we can continue with creating a domain admin user.

</p>
<br />

<ins>Step 2: Create Domain Admin User within the domain<ins/>
<p>
 Domain Admins are required to manage users, groups, devices and permissions to a domain while also making changes that affect the domain entirely. We will now need to create a Domain Admin user within the domain that will be created for anyone who requires administrator permissions within the network.

 To Create a Domain Admin User:
 - Create an Organizational Unit (OU) named "EMPLOYEES" within Active Directory Users & Computers (ADUC)
 - Create a new OU named "ADMINS"
 - Within ADMINS, create a new employee named *PREFERRED NAME*, give them a username and then a password.
      - It is recommended to require them to create their own password when they login for security. This can be done by checking the "User must change password at next login" button when
        creating the user's password.
        

<img src="https://github.com/user-attachments/assets/71d4a78c-479f-4963-bc22-9741148e2f5e" width="500" height="500"/> 


 - Now add your new admin user to the "Domain Admins" security group. You can do this by:

     *ADMINS > -Admin User- > Properties > Member of > Check names for "Domain Admins" > Apply

  
<img src="https://github.com/user-attachments/assets/4e245a58-347b-4452-b6b7-9a6f4ee03067" width="750" height="500"/> 

You can now log into your Domain Controller machine using your admin user. Join using the framework: *domain.com/DomainUsername*. gro
  


  
</p>
<br />

<ins>Step 3:Join the client to the domain<ins/>

</p>
<p>
At this point, we want to allow our client(s) to join the domain we created in the last step. To do this, we need to start by logging into our client as the local admin and join it to the domain. To do this we will need to:
                    
- Log into our client computer
- Open settings > System > About > Rename this PC (Advanced)
- Click "Change" to change its domain
- Verify it is "Member of: Domain: *your domain*"

<img src="https://github.com/user-attachments/assets/7af9f7c7-181a-4aba-a1aa-8d8080ce2889" width="750" height="500"/> 



- Afterwards, log into admin user account using appropriate cridentals (computer will restart)

Since you have joined the client to the domain, we now need to log into our DC and verify the client is in ACUC. 

<img src="https://github.com/user-attachments/assets/8bb1f5cb-7527-459e-92a7-7274cad96ff0" width="500" height="250"/> 

Once you have verified the client is in the domain, you will now be able to utilize admin permissions via the domain admin login. I also, personally, reccomend creating a new OU for your clients called "CLIENTS" for organizational purposes. :)




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
