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
3) 
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
Security Filtering: removed 'Authenticated Users', added 'Anna (anna.Y@lab.local)' only.

<img src="images/proved-setting-hidecontrolpanel.png" width="40%"/>
Filtering: test Anna user-link it at domain(lab.local)-live for Anna Y.

### Setting password

<img src="images/newRDP-login-lab-client.png" width="30%"/>
<img src="images/lab-client-login.png" width="30%"/>
<img src="images/RDP-network-setting.png" width="30%"/>
<img src="images/network-inbound-port-rules-allow-RDP.png" width="30%"/>
<img src="images/ADUC-account-password-setting.png" width="30%"/>
<img src="images/anna-reset-password.png" width="30%"/>

### Tested login

<img src="images/Login-tested .png" width="30%"/>
Tested: 'WIN+R-mstsc'-Login 'ab.local\anna.Y'
<img src="images/new-connect-login.png" width="30%"/>
Tested by RDP'ing into 'lab-vm' as 'lab\anna.Y' (Refer to section: 12-Trouble shooing)

<img src="images/check-applied-GPO.png" width="30%"/>
'gpresult /r /scope:user' inside the Anna Y session showed 'Hide Control Panel' under 
*Applied Group Policy Objects*.

<img src="images/control-panel-restricted.png" width="30%"/>
Opening Control Panel ('WIN+R' → 'control') returned: "This operation has been cancelled due to
restrictions in effect on this computer. Please contact your system administrator."

## What I Learned: 'gpresult' reporting a GPO as 'Applied" only means the the policy is computed and would take effect - it doesn't mean the 'current' desktop session already reflects it. Administrative template UI restrictions are enforced by 'explorer.exe' at logon, so reconnecting to a saved/remembered RDP session can still show the old, unrestricted state. A full sign-out and fresh logon is required to see the restriction actually apply.



<img src="images/Environment-spunup.png" width="20%" />
Environment-spun up: example of practice with GPO

### 7. Set a Custom Desktop Wallpaper Via GPO
<img src="images/GPO-set-wallpaper.png" width="40%" />
<img src="images/GPO-set-wallpaper-2.png" width="40%" />
<img src="images/GPO-set-wallpaper-3.png" width="40%" />
<img src="images/gpupdate.force.png" width="40%" />
<img src="images/Linked-lab.local-setwallpaper.png" width="40%" />
<img src="images/security.filtering-annaY.png" width="40%" />
<img src="images/wallpaper-whale.png" width="40%" />
<img src="images/wallpaper-whale-2.png" width="40%" />

Used 'User Configuration > Policies > Administrative Templates > Desktop > Desktop' 
→ *Desktop Wallpaper*
to  puch a wallpaper to client machines, following the same GPO creation → security filtering →
link → 'gpupdate /force' → verify pattern as section 6.


### 8. Mapped a Network Drive via GPO
<img src="images/Map-NetworkDrive-1.png" width="40%" />
<img src="images/Map-NetworkDrive-2.png" width="40%" />
<img src="images/Linked-lab.local.png" width="40%" />
<img src="images/security-filtering-anna.png" width="40%" />
<img src="images/gpupdate.force.png" width="40%" />
<img src="images/thisPC-A.png" width="40%" />
Used Group Policy Preferences (Drive Maps) to configure an automatic network drive mapping.
Confirmed the drive appeared in file explorer on the client after login.

### 9. Configured Microsoft Edge Homepage via GPO
<img src="images/Set-EdgeHomepage-1.png" width="40%" />
<img src="images/registry.png" width="40%" />
<img src="images/registry-2.png" width="40%" />
<img src="images/registry-3.png" width="40%" />
<img src="images/Linked-lab.local.png" width="40%" />
<img src="images/security-filtering-annaY.png" width="40%" />
<img src="images/gpupdate-force.png" width="40%" />
<img src="images/browse.png" width="40%" />
Set the default homepage for Microsoft Edge using 'Administrative Template > Microsoft Edge'.
Verified Edge opened to the configured homepage on the client.

*Strongest verification for this one: after 'gpupdate /force', open 'edge://policy' in the browser and confirm the homepage policy show as applied.*

### 10. Security Filtering - ON/Off Comparison
<img src="images/gpupdate-force.png" width="40%" />
<img src="images/gpresult-HideControlPanel.png" width="40%" />
<img src="images/HideControlPanel-remove-annaY.png" width="40%" />
<img src="images/notapplied-HideControlPanel.png" width="40%" />
<img src="images/HIdeControlPanel-add-annaY.png" width="40%" />
<img src="images/gpupdate-force-again.png" width="40%" />
<img src="images/applied-HIdeControlPanel.png" width="40%" />
Rather than requiring a second test account, this demonstrates the filtering mechanism directly on the 'Hide Control Panel' GPO from section 6:
1. With Anna Y in Security Filtering, run 'gpresult /r' as Anna Y → GPO appears under Applied GPOs.
2. Remove Anna Y from Security Filtering.
3. Run 'gpresult /r' again → GPO now appears under *Denied (security filtering)*

This before/after pair is the clearest possible proof that Security Filtering - not just 
the OU link - controls whoa GPO actually applies to.
*Sill to capture: the two gpresult outputs side by side.*
   

### 11. Configured Delegation
Granted a specific user/group permission to manage a GPO without giving full Domain Admin rights, using the *Delegation* tab on the GPO (permission level: 'Edit settings', not 'Edit settings, 
delete, modify security').

<img src="images/TESTGPO-annaY-edit-setting.png" width="40%" />
<img src="images/TESTGPO-annaY-edit-setting-2.png" width="40%" />
In a real environment this would typically go to an IT/helpdesk group rather than an end user - Anna Y is a member of an 'IT workers' group discovered during this lab, which would be a more realistic delegation target than delegation to a restricted end-user account.

*Still to capture: the delegation tab showing the granted permissin, and a login as the delegated account showing it can edit only that GPO.

### 12. Troubleshooting - Getting RDP Access to Work for a Restricted Test User
While testing the 'Hide Control Panel' GPO in section 6, RDP login as 'anna.Y' failed repeatedly. The root cause turned out to be four separate layers, each blocking for a different reason:

**Layer 1 - Azure Network Security Group (network reachability).**
<img src="images/RDP-network-setting.png" width="40%" />

<img src="images/RDP-network-setting.png" width="40%" /> 

<img src="images/.png" width="40%" />
'lab-vm' is an Azure VM reachable over its public IP. The attached NSG ('lab-vm-nsg') had no inbound rule for RDP, so the connections never reached the VM at all.
**Fix:** added an inbound security rule - 'Allow-RDP', TCP, port 3389, source restricted to a specific IP, action Allow.

**Layer 2 - Windows Defender Firewall (OS-level reachability).**
<img src="images/setting.png" width="40%" />

<img src="images/windows-defender-firewall.png" width="40%" />

<img src="images/firewallstate-off.png" width="40%" />

<img src="images/tested-GPO.png" width="40%" />

Even with the NSG open, the in-guest Windows Firewall also needed its inbound "Remote Desktop" rule enabled - a separate layer from the NSG; both have to allow the connection.

**Layer 3 - User Rights Assignment (the real blocker).**
Once the network path was open, login failed with: **"To sign in remotely, you need the right to sign in through Remote Desktop Services. By default, members of the Administrators group have this right."**
<img src="images/Layer3-security-UserRightsAssignment.png" width="40%" />

<img src="images/Layer3-security-UserRightsAssignment-gpupdate-force.png" width="40%" />

Root cause: unlike regular member server, a *Doman controller*
does not grant this right to the 'Remote Desktop Users' group by default - only to 'Administrators'.
**Fix:** 'Default Domain Controllers Policy' → 'Computer configuration > Policies > Windows Settings > Security Settings > Local Policies > User Rights Assignment' → **Allow log on through Remote Desktop Services** → added the 'Remote Desktop users' group, then 'gpupdate /force'.

**Layer 4 - Group membership.**
<img src="images/Layer4-lusrmgr.msc-failed.png" width="40%" />

<img src="images/Layer4-Builtin-add-annaY.png" width="40%" />

Tried adding 'anna.Y' to the local 'Remote Desktop Users' group via 'lusrmgr.msc', which failed with: *"the computer lab-vm is a domain controller. This  snap-in cannot be used on a domain controller. domain accounts are managed with the Active Directory Users and Computers snap-in.* 
A Domain controller has no local SAM database, so 'lusrmgr.nsc' is disabled on it.
**Fix:** Active Directory Users and Computers → 'lab.local' → **Builtin** container → **Remote Desktop Users** → Members → Add → 'Anna Y'

**Layer 5 - Session state.**
<img src="images/Layer5-anndY-signout.png" width="40%" />

<img src="images/Layer5-annaY-signin.png" width="40%" />

<img src="images/Layer5-anndY-gpresult.png" width="40%" />

Even after Layers 1-4 were fixed, the GPO restriction is section 6 didn't visibly apply until a full sign-out-sign-in was performed instead of reconnecting to a saved session.

**Verification of the full fix:**
- Successful RDP login as 'lab\anna.Y' via a fresh "Other user" logon(not a resumed/remembered session).
- 'gpresult /r /scope:user' inside that session showed 'Remote Desktop Users' in Anna Y's grop list and 'Hide Control Panel' under Applied Group Policy Objects.
- Control Panel access attempt returned the restriction error, confirming both the login chain *and* the GPO were working end to end.



## What I Learned
- Learned how Group Policy flows through the AD hierarchy and how 
  conflicting policies are resolved.
- Understood how to create, link, and scope GPOs to specific OUs.
- Practiced real-world admin tasks (firewall, wallpaper, drive mapping, 
  browser config) using GPO instead of manual configuration.
- Learned how Security Filtering narrows a GPO's application beyond its 
  OU link, and that filtering and linking are two independent controls- a GPO needs both to actually apply.
- Learned how Delegation allows non-Domain Admins to manage specific GPOs.
- Practiced troubleshooting GPO application issues using 'gpupdate' and 
  'gpresult', including the difference between a policy being "Applied" per 'gpresult' and actually being reflected in a live desktop session.
- Learned that Domain Controllers handle Remote Desktop access differently from member servers: 'lusrmgr.msc'is disabled on a DC, local group management goes through ADUC's Builtin container instead, and the "Allow log on through Remote Desktop Services" right isn't granted to 'Remote Desktop Users' by default the way it is on a member server.
- Learned to separate network-layer troubleshooting *Azure NSG) from OS-layer troubleshooting (Windows Firewall) from AD-layer troubleshooting (User Rights Assignment, group membership) -
a single "can't connect symptom can have causes at any of these layers.

