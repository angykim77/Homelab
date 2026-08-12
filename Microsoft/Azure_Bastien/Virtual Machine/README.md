Azure Bastion & Virtual Machine Project
Overview
A hands-on project setting up secure remote access to an Azure VM using Azure Bastion, without exposing a public IP.

Technologies Used
Azure Virtual Machine (Windows Server 2022)
Azure Bastion
Azure Virtual Network (VNet)
Build Process
1. Deployed a Windows Server Virtual Machine
<img width="1538" height="746" alt="01-create-vm" src="https://github.com/user-attachments/assets/7a4460b2-79c2-4e8a-bc5d-29b08855aace" />

Create VM Created the VM using the Standard_D2s_v3 size.

2. IP Configurations
<img width="1442" height="861" alt="02-ipconfiguration" src="https://github.com/user-attachments/assets/ae6b932b-c614-48d0-98cb-e1a4367e27eb" />

Set a static private IP for the domain controller in my lab environment.

3. Connect the Windows Server vm through Azure Bastion
<img width="1920" height="775" alt="03-bastion-setup" src="https://github.com/user-attachments/assets/5440d5ca-df2b-46de-b384-44e8fe9c55d6" />

Set up Azure Bastion to allow secure connection without exposing a public IP.

4. Install roles and features (adds)_Active Directory
<img width="1711" height="773" alt="04-adds" src="https://github.com/user-attachments/assets/3bf2f115-c041-4721-8008-c5a47f029dee" />


What I Learned
Learned how Bastion enables secure remote access without a public IP.
Understood the importance of Network Security Group (NSG) configuration.
