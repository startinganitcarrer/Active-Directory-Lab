# Project 1 — Active Directory Lab

[![Azure](https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com/) 
[![Windows Server](https://img.shields.io/badge/Windows_Server-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://www.microsoft.com/en-us/windows-server) 
[![Active Directory](https://img.shields.io/badge/AD_DS-4CAF50?style=for-the-badge)](https://learn.microsoft.com/en-us/windows-server/identity/active-directory-domain-services)  

---

## Objective
- To design, deploy, and document a production-style Active Directory environment in Microsoft Azure, simulating a real enterprise domain with centralized authentication and user management. This project mirrors what entry-level IT professionals support daily in corporate environments.
---

## Skills Demonstrated
- Active Directory Domain Services (AD DS)
- DNS configuration and troubleshooting
- Domain join process
- User and Organizational Unit (OU) management
- Azure virtual networking fundamentals
- Windows Server administration

## Tools & Technologies
- **Microsoft Azure** (Free Tier)  
- **Windows Server 2025** (Domain Controller)  
- **Windows 10/11** (Client VM)  
- **Active Directory Domain Services (AD DS)**  
- **Active Directory Users and Computers (ADUC)**  
- **Azure Virtual Network (VNet)**  

---

## Overview
This lab demonstrates a complete Active Directory deployment in Azure, including:  
- Domain Controller setup and promotion  
- Organizational Unit and user creation  
- DNS configuration and client domain join  
- Network and authentication troubleshooting  
- Step-by-step documentation with screenshots


---
## 🏗️ Lab Architecture

```text
Azure Virtual Network (10.0.0.0/16)
│
├── Subnet: IT-Subnet (10.0.1.0/24)
│     ├── DC01 — Windows Server 2022 (10.0.1.4)
│     └── CLIENT01 — Windows 10/11
│
└── DNS → Domain Controller (10.0.1.4)
```

## STEP 1: Azure Account & Resource Organization

### Purpose
Organize all lab resources into a single resource group for easy management and cleanup.

### Actions
1. Sign in to Azure Portal  
2. Go to **Resource Groups → Create**
3. Configure:
   - Name: `HomeLab`
   - Region: Canada Central (or East US)
4. Click **Create**

**Expected Result:** A dedicated resource group for all lab components.
<p align="center">
  <img src="https://github.com/user-attachments/assets/fd62a641-4e92-4e44-9fa7-30bcbf8485ed" width="750">
  <br>
  <em>Resource Group Overview</em>
</p>



## STEP 2: Create Virtual Network (VNet)

### Purpose
Provide internal networking and DNS communication between machines.

### Actions
1. Go to **Virtual Networks → Create**
2. Configure:
   - Name: `IT-Lab-VNet`
   - Address Space: `10.0.0.0/16`
   - Subnet: `IT-Subnet`
   - Subnet Range: `10.0.1.0/24`
3. Review and Create

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/0b94dcab-17c7-4e19-a109-728d172f80ec" width="750">
  <br>
  <em>VNet Address Space and Subnet Configuration</em>
</p>



## STEP 3: Deploy Domain Controller (DC01)

### VM Configuration
- Name: `DC01`
- OS: Windows Server 2022 Datacenter
- Size: Standard B2s
- Authentication: Username & Password
- VNet: `IT-Lab-VNet`
- Subnet: `IT-Subnet`
- Public Ports: RDP (3389)

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/c4ce9495-03f7-4bed-9049-0345c7d6594d" width="750">
  <br>
  <em>DC01 Virtual Machine Overview</em>
</p>


---

## STEP 4: Assign Static Private IP (CRITICAL)

### Why
Active Directory requires consistent DNS records.

### Actions
1. DC01 → Networking → Network Interface  
2. IP Configuration  
3. Change **Dynamic → Static**  
4. Set IP: `172.16.0.4`

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/b379be44-63b4-4127-ae65-02820d998b23" width="750">
  <br>
  <em>Static Private IP Assignment</em>
</p>

---

## STEP 5: Install Active Directory Domain Services

### Actions
1. RDP into DC01  
2. Server Manager → Add Roles  
3. Select **Active Directory Domain Services**  
4. Install

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/e0f8132b-d65e-471d-a5c1-fa67c62813ff" width="750">
  <br>
  <em>Active Directory Domain Services Installation</em>
</p>

---

## STEP 6: Promote Server to Domain Controller

### Actions
1. Server Manager → **Promote this server**  
2. Add a new forest  
3. Domain: `homelab.local`  
4. Set DSRM password  # This is the recovery password in case you want to login to the domain controller.
5. Reboot

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/f8d365c6-8d36-43e2-a4a3-cf3e0df90a59" width="750">
  <br>
  <em>Domain Controller Promotion Summary</em>
</p>

---

## STEP 7: Create Organizational Units and Users

### Actions
1. Open **Active Directory Users and Computers (ADUC)**  
2. Create Organizational Units (OUs):  
   - IT  
   - HR  
3. Create Users inside the appropriate OUs:  
   - jsmith  
   - adoe  
   - helpdesk1  

---

## 🧠 Learning Outcome: Creating AD Users with PowerShell

In addition to using ADUC, I also created users using **PowerShell**, which is the preferred method in real enterprise environments for automation and bulk user creation.
```
Powershell Scripts:
Step 1 — Import the Active Directory module
-Import-Module ActiveDirectory
Step 2 — View the syntax for creating users
-Get-Command New-ADUser -Syntax
Step 3 — Create a new user (example)
-New-ADUser -Name "jsmith" `
  -GivenName "John" `
  -Surname "Smith" `
  -SamAccountName "jsmith" `
  -UserPrincipalName "jsmith@homelab.local" `
  -Path "OU=IT,DC=homelab,DC=local" `
  -AccountPassword (Read-Host -AsSecureString "Enter Password") `
  -Enabled $true
```
📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/f4816c4a-6013-4602-af29-54a27b0ef461" width="750">
  <br>
  <em>Active Directory Users and Computers — OUs and Users</em>
</p>


---

## STEP 8: Deploy Client Machine (CLIENT01)

### VM Configuration
- Name: `CLIENT01`
- OS: Windows 10 / 11
- Size: Standard B2s
- VNet: `IT-Lab-VNet`

### DNS (CRITICAL)
Set DNS to: `172.18.0.4`

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/e941c9da-3301-4097-83bb-3db0560734a3" width="750">
  <br>
  <em>Client Machine DNS Configuration</em>
</p>


---

## STEP 9: Join Client to Domain

### Actions
1. Log into CLIENT01  
2. System → Rename this PC → Domain  
3. Domain: `lab.local`  
4. Authenticate  
5. Restart

📸 
<p align="center">
  <img src="https://github.com/user-attachments/assets/5ee9afe8-d562-481d-a71f-f339209acf4c" width="750">
  <br>
  <em>Successful Domain Join</em>
</p>

---

## Troubleshooting Notes
- Domain join failures → check DNS settings  
- Login issues → verify time sync and user permissions  
- Static IP is critical for Domain Controller reliability  


## Learning Outcomes
After completing this project, I have learned:  
- Deploying Windows Server in Azure
-Configuring AD DS, DNS, and domain authentication
-Creating OUs and users
-Joining clients to a domain
-Troubleshooting DNS & networking issues
-Using PowerShell for AD automation
-Understanding enterprise-level IT administration


## Author

**Sandip Dhungana**  
- Diploma: Computer System Technician – Networking  
- Certifications: CompTIA A+, Zendesk Customer Service Professional  

---



