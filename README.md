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

- Group Policy
- Dealing with account lockouts/resetting passwords
- Enabling/Disabling Accounts 


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
  - Leave DNS options unchecked
  - Continue through tabs til installation prompt is given, Install.

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

Now that we have our Active Directory set up, we should now create users for our employees to be assigned. There are two main ways to go about doing so. First, we can individually create users within our domain by using the following path:

*  ADUC > mydomain.com > _EMPLOYEES > New > User*

<img src="https://github.com/user-attachments/assets/16a9ef9a-6769-480e-b2bc-b425d026945b" width="300" height="250"/> <img src="https://github.com/user-attachments/assets/dc05fb3b-813c-4807-9e26-69d9c02c44ee" width="300" height="250"/> 



From here we would create the user, assign it a password then continue with other configuration. This way work fine for small organizations, but not every Domain User has time to manually create from tens to hundreds of user profiles. Thats where the second method comes into play, which is automatically creating users. We can do that by using a script!

This script will create 10 (or however many needed) users under the "_EMPLOYEES" tab within the domain:

      # ----- Edit these Variables for your own Use Case ----- #
    $PASSWORD_FOR_USERS   = "Password1"
    $NUMBER_OF_ACCOUNTS_TO_CREATE = 10
    # ------------------------------------------------------ #

    Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

    }

    $count = 1
    while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
    }

This script can be pasted into a text file, then ran through Powershell ISE. If youre curious on how to do so, the path is simple:

- Open Powershell ISE as an administrator
- Click the arrow on the top right with the word "script" next to it.
- Paste the script into the area with the white background
- Press the "Run Script"/green arrow on the top of the screen
- Witness how 10 users are created with the same cridentials, waiting to be configured to your liking.

<img src="https://github.com/user-attachments/assets/9a56451c-39e1-478b-a159-ffca44f7b20e" width="750" height="500"/>



Now that your users are created, you may configure them with their groups and permissions, then assign them to staff as needed. 

**‼️Remember to check that the user needs to change their password when they login for the first time...its necessary to be able to keep our system secure :)‼️**

</p>
<br />


<h2>Useful Knowledge</h2>

<ins>Group Policy<ins/>

<ins>Dealing with Account Lockout/Resetting Passwords<ins/>

Now that we have our group policy configured, there will be times when there is someone in our domain who needs to reset their password, or gets locked out of their account. This is an extremely common issue that end users have everywhere, so we as IT professionals will most likely come across this issue more than once so its good to know how to fix it. Both of these can be done directly within the ADUC interface. 

Use this path to unlock an account:



and use the following to reset a password:

<ins>Enabling/Disabling Accounts<ins/> 

Enabling & disabling an account is just as simple as the previous process. If someone were to ask you to enable their account, it is always a good idea to check in with your system administrator first before enabling someone's account. It might have been disabled for a specific reason and its always good to be a bit paranoid when it comes to security for our network!

To enable or disable an account follow the path below:


