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
- **Windows Server 2022** (Domain Controller)  
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

## STEP 1: Azure Account & Resource Organization

### Purpose
Organize all lab resources into a single resource group for easy management and cleanup.

### Actions
1. Sign in to Azure Portal  
2. Go to **Resource Groups → Create**
3. Configure:
   - Name: `IT-Lab-RG`
   - Region: Canada Central (or East US)
4. Click **Create**

**Expected Result:** A dedicated resource group for all lab components.

📸 *Screenshot:* Resource group overview page

---

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

📸 *Screenshot:* VNet address space and subnet configuration

---

## STEP 3: Deploy Domain Controller (DC01)

### VM Configuration
- Name: `DC01`
- OS: Windows Server 2022 Datacenter
- Size: Standard B2s
- Authentication: Username & Password
- VNet: `IT-Lab-VNet`
- Subnet: `IT-Subnet`
- Public Ports: RDP (3389)

📸 *Screenshot:* VM overview page

---

## STEP 4: Assign Static Private IP (CRITICAL)

### Why
Active Directory requires consistent DNS records.

### Actions
1. DC01 → Networking → Network Interface  
2. IP Configuration  
3. Change **Dynamic → Static**  
4. Set IP: `10.0.1.4`

📸 *Screenshot:* Static IP configuration

---

## STEP 5: Install Active Directory Domain Services

### Actions
1. RDP into DC01  
2. Server Manager → Add Roles  
3. Select **Active Directory Domain Services**  
4. Install

📸 *Screenshot:* AD DS installation confirmation

---

## STEP 6: Promote Server to Domain Controller

### Actions
1. Server Manager → **Promote this server**  
2. Add a new forest  
3. Domain: `lab.local`  
4. Set DSRM password  
5. Reboot

📸 *Screenshot:* Domain promotion summary

---

## STEP 7: Create Organizational Units and Users

### Actions
1. Open **Active Directory Users and Computers**
2. Create OUs:
   - IT
   - HR
3. Create Users:
   - jsmith
   - adoe
   - helpdesk1

📸 *Screenshot:* ADUC showing OUs and users

---

## STEP 8: Deploy Client Machine (CLIENT01)

### VM Configuration
- Name: `CLIENT01`
- OS: Windows 10 / 11
- Size: Standard B2s
- VNet: `IT-Lab-VNet`

### DNS (CRITICAL)
Set DNS to: `10.0.1.4`

📸 *Screenshot:* Client DNS settings

---

## STEP 9: Join Client to Domain

### Actions
1. Log into CLIENT01  
2. System → Rename this PC → Domain  
3. Domain: `lab.local`  
4. Authenticate  
5. Restart

📸 *Screenshot:* Successful domain join

---

## Troubleshooting Notes
- Domain join failures → check DNS settings  
- Login issues → verify time sync and user permissions  
- Static IP is critical for Domain Controller reliability  


## Learning Outcomes
After completing this project, you will have learned:  
- How to deploy and configure an Active Directory Domain Services environment in Azure  
- How to plan and configure Azure Virtual Networks and subnets for secure communication  
- How to deploy Windows Server VMs as Domain Controllers and configure static IPs  
- How to install AD DS, promote a Domain Controller, and create a new forest  
- How to create Organizational Units and users for enterprise-like structure  
- How to deploy a client VM and join it to the domain  
- How to troubleshoot domain join and DNS-related issues  
- Practical understanding of enterprise-level IT administration and helpdesk-ready skills


## Author

**Sandip Dhungana**  
- Diploma: Computer System Technician – Networking  
- Certifications: CompTIA A+, Zendesk Customer Service Professional  

---


