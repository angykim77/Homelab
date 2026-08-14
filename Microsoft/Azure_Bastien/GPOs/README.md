# Group Policy (GPO) - Real-World Configuration Lab

## Overview
A hands-on project demonstrating how to create, link, and apply Group Policy 
Objects (GPOs) in Active Directory, covering common real-world IT tasks such 
as disabling Windows Firewall, setting wallpapers, mapping drives, and 
configuring browser settings, along with security filtering, delegation, 
and troubleshooting.

## Technologies Used
- Windows Server 2022 (Domain Controller)
- Windows 10/11 (Client machine)
- Active Directory Domain Services (AD DS)
- Group Policy Management Console (GPMC)
- Active Directory Users and Computers (ADUC)
- 
## Build Process

### 1. What Is Group Policy?
![Domain Credentials](images/search-GPMC.png)

Reviewed how Group Policy applies settings to users and computers within 
an Active Directory domain, and how policies flow through the AD structure 
(Local, Site, Domain, OU).

### 2. Opened the Group Policy Management Console (GPMC)
![Domain Credentials](images/GPMC.png)
Explored the console layout, including the domain, OU structure, and the 
Group Policy Objects container.

### 3. Created a new GPO
<img src="images/created-GPO.png" width="20%" />
<img src="images/created-GPO-TESTGPO.png" width="20%" />
Created new GPO(TESTGPO) in Group Policy Objects
<img src="images/add-Anna-oneuser.png" width="20%" />
Added one user: Anna Y
<img src="images/remove-authenticatedusers.png" width="20%" />
Remove authenticated users - just security filtering for Anna.
<img src="images/security-advancedsetting-Anna.png" width="20%" />
Advanced setting: allow read, apply group policy for GPO. (Gatekeeper-1)
<img src="images/idchecker-delegation.png" width="20%" />
Security Filtering (Gatekeeper-2)
<img src="images/setting.png" width="20%" />

Created a new Group Policy Object in the Group Policy Objects container.

### 4. Tested the GPO on the client machine
![Domain Credentials](images/tested-GPO.png)

Ran gpupdate /force on the client to apply the policy.
Confirmed the setting took effect on the client machine.

### 5. Disabled Windows Firewall via GPO
<img src="images/newGPO-disable-domain-firewall.png" width="20%" />
Created new GPO: Disable Domain Firewall

<img src="images/windows-defender-firewall.png" width="20%" />
Windows setting
<img src="images/firewallstate-off.png" width="20%" />
Group Policy Management Editor: Firewall state Off

<img src="images/add-computer-DDF.png" width="20%" />
<img src="images/Link-branch1-disablefirewall.png" width="20%" />
<img src="images/linked-branch1.png" width="20%" />
Added computer in security filtering-linked Branch1
Now, GPO is live for LAB_PC
<img src="images/tested-gpresult-computer.png" width="20%" />
Tested gpresult
<img src="images/noted.png" width="20%" />

Configured Computer Configuration > Policies > Windows Settings > Security 
Settings > Windows Defender Firewall to turn off the firewall.
Verified Windows Firewall was disabled on the client after gpupdate.

### 6. Set a custom desktop wallpaper via GPO
<img src="images/add-users-created-hidecontrolpanel.png" width="20%"/>
Added users and created Hide Control Panel

<img src="images/controlpanel-prohibitaccess.png" width="20%"/>
<img src="images/controlpanel-prohibitaccess-proved.png" width="20%"/>
Enabled: Prohibit access to Control Panel and PC settings

<img src="images/filter-add-anna.png" width="20%"/>
<img src="images/link-branch1-anna-hidecontrolpanel.png" width="20%"/>
<img src="images/proved-setting-hidecontrolpanel.png" width="20%"/>
Filtering: test Anna user-link it at domain(lab.local)-live for Anna Y.

<img src="images/newRDP-login-lab-client.png" width="20%"/>
<img src="images/lab-client-login.png" width="20%"/>
WIN+R-mstsc-Login lab.local\anna.Y
<img src="images/RDP-network-setting.png" width="20%"/>
<img src="images/network-inbound-port-rules-allow-RDP.png" width="20%"/>
<img src="images/ADUC-account-password-setting.png" width="20%"/>
<img src="images/anna-reset-password.png" width="20%"/>
<img src="images/Login-successes .png" width="20%"/>
Appeared error: setting networking inbound port rules-Allow RDP
<img src="images/local-users-and-groups.png" width="20%"/>
lusrmgr.msc: local users and groups-Remote Desktop Users-add anna.Y(user)
me able to log in to this VM using anna.Y domain credentials.

Configured User Configuration > Policies > Administrative Templates > 
Desktop > Desktop to push a wallpaper to client machines.
Confirmed the wallpaper was applied on the client after login.

<img src="images/Environment-spunup.png" width="20%" />
Environment-spun up: example of practice with GPO

### 7. Mapped a network drive via GPO
![Domain Credentials](images/.png)
<img src="images/.png" width="20%" />
Used Group Policy Preferences (Drive Maps) to configure an automatic 
network drive mapping.
Confirmed the drive appeared in File Explorer on the client after login.

### 8. Configured Microsoft Edge homepage via GPO
![Domain Credentials](images/.png)
<img src="images/.png" width="20%" />
Set the default homepage for Microsoft Edge using Administrative 
Templates > Microsoft Edge.
Verified Edge opened to the configured homepage on the client.

### 9. Applied Security Filtering
![Domain Credentials](images/.png)
<img src="images/.png" width="20%" />
Restricted which users, computers, or groups the GPO actually applies to, 
beyond just the OU link, using the Security Filtering section of the GPO.

### 10. Configured Delegation
![Domain Credentials](images/.png)
<img src="images/.png" width="20%" />
Granted a specific user/group permission to manage the GPO without giving 
full Domain Admin rights.

### 11. Troubleshot GPO application
![Domain Credentials](images/.png)
<img src="images/.png" width="20%" />
Used gpupdate /force, gpresult /r, and gpresult /h to verify which 
policies applied and diagnose why a policy wasn't taking effect.


## What I Learned
- Learned how Group Policy flows through the AD hierarchy and how 
  conflicting policies are resolved.
- Understood how to create, link, and scope GPOs to specific OUs.
- Practiced real-world admin tasks (firewall, wallpaper, drive mapping, 
  browser config) using GPO instead of manual configuration.
- Learned how Security Filtering narrows a GPO's application beyond its 
  OU link.
- Learned how Delegation allows non-Domain Admins to manage specific GPOs.
- Practiced troubleshooting GPO application issues using gpupdate and 
  gpresult.

