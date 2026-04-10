# Phase 1 – Active Directory Environment Setup

## 🧠 Overview
This phase focuses on building a foundational enterprise Active Directory environment using virtual machines. The goal is to simulate a real-world IT infrastructure that will later be expanded with security monitoring and attack simulation.

## 📚 What You Will Learn
- Creating and managing Active Directory objects (Users, OUs, Groups, GPOs)
- Network configuration (IP addressing, DNS, subnetting basics)
- VM configuration and connectivity troubleshooting
- Role-Based Access Control (RBAC) and permissions
- Applying the principle of least privilege

---

## 🎯 Goals

1. **Set Up Virtual Machines**
   - Windows Server 2022 (Domain Controller)
   - Windows 10 (Client)

2. **Network Configuration**
   - Configure static IP addresses
   - Ensure VM-to-VM connectivity
   - Join client to domain

3. **User Management**
   - Create domain users
   - Enforce password change at first login
   - Ensure proper password policies

4. **OU Organization**
   - Create OUs: HR, IT, Sales
   - Assign users accordingly
   - Delegate control to department admins

5. **Security Groups & Permissions**
   - Create:
     - `[Department] Users`
     - `[Department] Managers`
   - Assign permissions:
     - Department Data (read vs modify)
     - Manager-only folders
     - Company-wide read-only folder

6. **Group Policy (GPOs)** *(In Progress)*
   - Password policies
   - Desktop configurations
   - Apply to OUs

7. **Simulated Real-World Tasks** *(In Progress)*
   - Password resets
   - Account lockouts

---

## 🖥️ Lab Setup

### 💻 Virtual Machines
Specs used for MY system (16GB RAM, 1TB Storage, 12 Processors)
| VM | Hostname | RAM | vCPU | Disk |
|----|---------|-----|------|------|
| Windows Server 2022 | DC01 | 3–4 GB | 2 | 60 GB |
| Windows 10 Client | WIN10-CLIENT | 2–3 GB | 1 | 40–60 GB |

---

### 🌐 Network Configuration

#### Domain Controller
- IP Address: `172.16.0.10`
- Subnet Mask: `255.255.0.0`
- DNS: `127.0.0.1`

> The domain controller acts as its own DNS server, allowing it to resolve domain names internally.

#### Client Machine
- IP Address: `172.16.0.20`
- Subnet Mask: `255.255.0.0`
- DNS: `172.16.0.10`

---

### 🔧 Key Setup Steps

- Installed Windows Server 2022 and configured static IP
- Installed Active Directory Domain Services (AD DS)
- Promoted server to Domain Controller
- Created domain: `Craig.com`
- Installed Windows 10 client
- Configured networking on client
- Joined client to domain

---

## 🚶 Walkthrough (Work in Progress)

This section demonstrates administrative tasks performed within the domain environment, including images and steps taken.

### ✅ Completed
- Created Organizational Units (HR, IT, Sales)
- Created domain users
- Configured initial password policies

### ⏳ Not Documented Yet
- Group Policy Object (GPO) configuration
- Security group implementation
- Folder permissions and access control
- Password reset simulations
- Account lockout testing

---

## 🔍 What I Learned

### 🧩 System Compatibility & Troubleshooting
- Encountered Windows 11 hardware requirement issues (TPM, Secure Boot)
- Attempted registry bypass methods
- Learned differences between registry hives
- Switched to Windows 10 for efficiency and stability

---

### ⚙️ Resource Management
- Identified time inefficiencies during troubleshooting
- Adjusted approach to prioritize progress over perfection

---

### 🌐 Networking & Connectivity

- Used `ipconfig` and `ping` for troubleshooting
- Encountered: **“Destination host unreachable”**
- Discovered that:
  - NAT = internet access but isolated VMs
  - Internal Network = VM-to-VM communication

✅ Solution:
- Switched both VMs to **Internal Network**
- Successfully established connectivity between client and domain controller

---

## 🚀 Next Steps

In the next phase, this environment will be expanded with security tools:

- Sysmon (endpoint logging)
- Splunk (log aggregation and analysis)
- Wireshark (network traffic analysis)
- Kali Linux (attack simulation)

This will transform the lab into a **mini Security Operations Center (SOC)** environment.

---
