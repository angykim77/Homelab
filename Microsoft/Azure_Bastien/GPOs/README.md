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

## **Note:** All GPO testing in this lab was performed by RDP'ing directly into the Domain Controller as a restricted test user - no separate client VM was provisioned. This meant tools normally used on a member client (like 'lusrmgr.msc') aren't available on a DC, See section 12 for how that was worked around.

## Build Process

### 1. What Is Group Policy?
<img src="images/search-GPMC.png" width="50%" />
Reviewed how Group Policy applies settings to users and computers within 
an Active Directory domain, and how policies flow through the AD structure 
(Local, Site, Domain, OU).

### 2. Opened the Group Policy Management Console (GPMC)
<img src="images/GPMC.png" width="50%" />
Explored the console layout, including the domain, OU structure, and the 
Group Policy Objects container.

### 3. Created a new GPO
<img src="images/created-GPO.png" width="40%" />
<img src="images/created-GPO-TESTGPO.png" width="40%" />
Created new GPO(TESTGPO) in Group Policy Objects This GPO has no actual settings defined - it exists purely to practice the permission mechanics below, not to test an applied setting.
<img src="images/add-Anna-oneuser.png" width="40%" />
Added one user to Security Filtering: 'Anna Y'
<img src="images/remove-authenticatedusers.png" width="40%" />
Remove 'authenticated users' from Security filtering so the GPO scope narrows to Anna only.
<img src="images/security-advancedsetting-Anna.png" width="40%" />
<img src="images/idchecker-delegation.png" width="40%" />
Advanced setting: allow 'read'+'apply group policy': Verified the same result appears in two place
1) the 'Security Filtering' section on the scope tab
2) the underlying 'Advanced Security Settings' (Delegation tab-Advanced)
<img src="images/setting.png" width="40%" />



### 4. Tested the GPO Application
<img src="images/tested-GPO.png" width="50%" />
Command Prompt: used 'gpupdate /force' and 'gpresult /r /scope:computer'
-Basic verification workflow before testing a GPO with a real setting.

### 5. Disabled the Domain Firewall Profile via GPO
<img src="images/newGPO-disable-domain-firewall.png" width="40%" />
Created new GPO: Disable Domain Firewall

<img src="images/windows-defender-firewall.png" width="40%" />
Configured: 'Computer Configuration > Policies > Windows Settings > Security Setting > 
Windows Defender Firewall with Advanced Security'
<img src="images/firewallstate-off.png" width="40%" />
Domain Profile → Firewall state: off
<img src="images/add-computer-DDF.png" width="40%" />
<img src="images/Link-branch1-disablefirewall.png" width="40%" />
Security Filtering scoped to the computer object 'LAB-PC'
(a computer, not a user, since this is a Computer Configuration policy).
<img src="images/linked-branch1.png" width="40%" />
Linked to the 'Branch1' OU.
<img src="images/tested-gpresult-computer.png" width="40%" />
Verified with 'gpupdate /force' and 'gpresult', and confirmed the Domain Profile firewall was 
disabled on the target computer.

<img src="images/noted.png" width="40%" />
remind me the progress.

### 6. Restricted control panel Access - A User-Scoped GPO

**Goal:** prove a GPO can be scoped to a single user via Security Filtering, and that the restriction actually takes effect on that user's session.

<img src="images/add-users-created-hidecontrolpanel.png" width="40%"/>
Created a new GPO: 'Hide Control Panel'
<img src="images/controlpanel-prohibitaccess.png" width="40%"/>
<img src="images/controlpanel-prohibitaccess-proved.png" width="40%"/>
Configured: 'Users configuration > Police > Administrative Templates > Control Panel'
→Prohibit access to Control Panel and PC settings = *Enabled*
<img src="images/filter-add-anna.png" width="40%"/>
<img src="images/link-branch1-anna-hidecontrolpanel.png" width="40%"/>
Linked the GPO at the domain root (lab.local)
SEcurity Filtering: removed 'Authenticated Users', added 'Anna (anna.Y@lab.local)' only.
<img src="images/proved-setting-hidecontrolpanel.png" width="40%"/>
Filtering: test Anna user-link it at domain(lab.local)-live for Anna Y.

##Trouble Shooting (1): Setting password
<img src="images/newRDP-login-lab-client.png" width="40%"/>
<img src="images/lab-client-login.png" width="40%"/>
<img src="images/RDP-network-setting.png" width="40%"/>
<img src="images/network-inbound-port-rules-allow-RDP.png" width="40%"/>
<img src="images/ADUC-account-password-setting.png" width="40%"/>
<img src="images/anna-reset-password.png" width="40%"/>
<img src="images/Login-tested .png" width="40%"/>
Tested: 'WIN+R-mstsc'-Login 'ab.local\anna.Y'
Appeared error: setting networking inbound port rules-Allow RDP
<img src="images/local-users-and-groups.png" width="40%"/>
'lusrmgr.msc': local users and groups-Remote Desktop Users-add anna.Y(user)
me able to log in to this VM using anna.Y domain credentials.


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

