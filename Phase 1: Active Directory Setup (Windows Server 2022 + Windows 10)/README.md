# Phase 1 – Active Directory Environment Setup

## Overview
This phase focuses on building a foundational enterprise Active Directory environment using virtual machines. The goal is to simulate a real-world IT infrastructure that will later be expanded with security monitoring and attack simulation.

## What You Will Learn
- Creating and managing Active Directory objects (Users, OUs, Groups, GPOs)
- Network configuration (IP addressing, DNS, subnetting basics)
- VM configuration and connectivity troubleshooting
- Role-Based Access Control (RBAC) and permissions
- Applying the principle of least privilege

---

## Goals

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

6. **Group Policy (GPOs)** 
   - Password policies
   - Account lockout thresholds
   - Apply to OUs

7. **Simulated Real-World Tasks** 
   - Password resets
   - Account lockouts

. **User Creation with Powershell Automation** *IN PROGRESS*
   - Password resets
   - Account lockouts
---

## Lab Setup

### Virtual Machines
These are the specs that I for the VMs in my system (16GB RAM, 1TB Storage, 12 Processors)
| VM | Hostname | RAM | vCPU | Disk |
|----|---------|-----|------|------|
| Windows Server 2022 | DC01 | 3–4 GB | 2 | 60 GB |
| Windows 10 Client | WIN10-CLIENT | 2–3 GB | 1 | 40 GB |

---

### Network Configuration

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

#  Walkthrough (Work in Progress)

This section provides a complete walkthrough of the lab after you have already set up the VMs

In Progress
- GPOs
- Real-world Tasks
- Powershell Automation

## 1️⃣Creating OUs & User Creation

Organizational Units in Active Directory are containers used to organize objects like users,
groups, and computers into logical structures like departments. They are needed to efficiently
apply Group Policies and delegate administrative control without affecting the entire domain.

In this lab, we will create 4 OUs:

Parent OU

1. Our corporation's OU, containing the 3 departments of our business

Child OUs

2. Department #1 - Sales

3. Department #2 - IT

4. Department #3 - HR

The following demonstrates how to create Organizational Units from your Windows Server 2022
VM.

---

### <ins>Creating your corporation's OU</ins>

1.  Navigate to the Server Manager on your server VM  
2. Click Tools -> Active Directory Users and Computers  
3. Right-click your domain  
4. Click New -> Organizational Unit  
5. Because my name is Craig, I will title mine "Craigs Corporation" (Leave the box
checked to prevent accidental deletion)

Here is what my parent OU looks like:

```
{INSERT IMAGE}
```

Now that we have created a parent OU for our organization, we need to fill it with OUs for our
departments (Sales, IT, and HR)

---

### <ins>Creating the department OUs</ins>

1. Right-click your corporation OU  
2. Click New -> Organizational Unit  
3. Create 3 separate OUs for HR, IT, and Sales using this process. It should look
like this:

```
{INSERT IMAGE}
```

Structuring our OUs in this manner will allow for more scalable Group Policy Object (GPO)
deployment (more on that later), since general GPOs can be applied to the entire corporation's
OU and department-specific GPOs can be applied to a single department.

After creating these OUs, we need to fill them with our employees! For the sake of simplicity, we
will create the users within their respective department OUs instead of creating them and then
adding them.

---

### <ins>Creating Users in OUs</ins>

For this example we will be creating the users Tom Brady (our Sales manager) and
Patrick Mahomes (our Sales staff) into the Sales department's OU according to our
domain user table.

1. Right-click on the Sales OU  
2. Click New -> User  
3. Enter Patrick Mahomes' according to the domain user table  
   - First name: Patrick  
   - Last name: Mahomes  
   - User logon name: PMahomes  
4. Click Next  
5. Set this password to something simple (e.g. @Password1), because this is only
the initial password.  
6. Make sure "User must change password at next logon" box is checked!  
   - Not doing so would be a violation of security, because the admin (you)
would know the user's password!  
7. Leave "User cannot change password", "Password never expires", and "Account
is disabled" unchecked.  
   - IT should not restrict normal users from performing a password change,
   this could create user authentication issues and slow down business
   operations  
   - Passwords should ALWAYS expire for normal users in the event of an
   undetected breach or password leak. This limits the attacker's window to
   be able to use the illegally obtained employee credentials.  
   - The account should not be disabled, this is unnecessary in the scenario  
8. Click Next -> Finish  
9. Repeat the previous steps to create every user in their respective department
using their credentials according to the domain user table

After you create your users in their department OU, it should look like this:

```
{INSERT IMAGE}
```

Associating our domain users with their departments keeps things organized, and makes
configs much easier.

---

## 2️⃣Creating Security Groups & Adding Users

We need to add our users to Security Groups so that configurations to authorization and
permissions are scalable and applicable to many users at once.

We will apply these Security Group objects to our Sales department users as an example,
having one for every Sales User and another for our Sales Manager.

---

### <ins>Creating the Security Groups</ins>

1. Navigate to the Server manager on your Server VM  
2. Click Tools -> Active Directory Users and Computers  
3. Find the Sales OU that you created inside of your Corporation OU and right-click
it.  
4. Click New -> Group  
5. Type in Sales Users for the Name, and click OK  
6. Repeat this process again, instead creating a Security Group for Sales Managers

The inside of your department OU should look something like this as of now:

```
{INSERT IMAGE}
```

---

### <ins>Adding Users to Security Groups</ins>

1. Double-click on Sales Users and navigate to the Members tab  
2. Click Add  
3. Enter the object name of the user that you want to add to the security group (This
is case-sensitive, be careful), in this case, it is Patrick Mahomes  
4. Left-click "Check Names" to verify that you are adding the correct user.
   - You will have performed this correctly if the correct email auto-fills within the object name
field  
5. Click OK -> Add -> Apply  
6. Try repeating this process by adding Tom Brady to the Sales Managers group,
and applying the same process to the other departments

To verify that the correct user was added to the correct Security Group, simply double-click one
of the Security Groups, and click the Members tab. The Members tab of both of the Sales
Security Groups should look similar to this.

```
{INSERT IMAGE}
```

Now that you have security groups, you can create files and apply permissions to entire groups
of users!

---

## 3️⃣File Sharing & Access Control Implementation

All of our employees need to access files so that business operations can run smoothly.
However, one employee might not need to view or modify the same data as another employee
within the same department to fulfill their job role. Additionally, some data might be sensitive,
only intended for certain types of employees. These are why we have permissions, role-based
access, and least privilege. Role-based access assigns permissions based on job
responsibilities, making access management more efficient and organized. The principle of least
privilege ensures users only have the minimum access needed, reducing security risks and
potential damage from mistakes or attacks. We will be creating folders with data for each type of
employee in each department to further demonstrate this concept.

In this example, we will:

1. Create a Sales folder with a text document file inside intended for the viewing of the
ENTIRE Sales department  
2. Allow Sales Manager to modify the file in addition to reading it  
3. Deny access to all of the Sales Department's files for anyone not within the sales
department (HR and IT)

---

### <ins>Creating the Folder and Text Document File</ins>

1. From the Server VM desktop, create a new folder named "Sales Data"  
2. Double-click on Sales the Data folder and create a new Text Document within the
folder  
3. Name the document "Sales Reports"

```
{INSERT IMAGE}
```

We only want our Sales employees to be able to VIEW the data, and for our Sales Manager to
READ, WRITE, and MODIFY the Sales Reports file.

Similar to OUs and Security Groups, every permission that we apply to the parent folder will also
affect the child documents. With that in mind, we will set permissions for the Sales Data folder.

---

### <ins>Set Permissions</ins>

1. Right-click the Sales Data folder and click Properties -> Security -> Edit -> Add  
2. Add Sales Users security group. Apply the default perms as they appear (Read &
Execute)  
3. Add Sales Managers security group. Apply the permissions to Write and Modify  
4. Share the Folder With the Network

Because we applied the permissions to security groups, any user added to a security group will
inherit those same permissions.

---

As of now, the folder only exists on the server, we need to push it to our client PC so that our
domain users can interact with it.

### <ins>Sharing the File with Network</ins>

1. Right-click folder and click Properties -> Sharing -> Advanced Sharing  
2. Check "Share this folder"  
3. Click Permissions
4. Add Domain Users and give them Full Control

Now that we made the sales file, assigned read/write/modify permissions, and pushed it to the
network, we should test the results on the accounts of our sales manager, sales users, and
non-sales users.

Test permissions by going on different client accounts. Use Win +R and enter:

```
\\172.16.0.10
```

```
{INSERT IMAGE}
```

Navigate to the .txt file in the Sales Data folder. Notice how:

1. Sales User can read Sales Reports.txt  
```
{INSERT IMAGE}
```

2. Sales Manager can write on Sales Reports.txt  
```
{INSERT IMAGE}
```

3. Non-sales user can't perform read or write on Sales Reports.txt  
```
{INSERT IMAGE}
```

As you can see, only employees that need the reports for operations can view it, and those that
don't, don't. Additionally the manager can edit the file if need be.


---

## Here's Everything That I Learned

### System Compatibility & Troubleshooting
- Encountered Windows 11 hardware requirement issues (TPM, Secure Boot)
- Attempted registry bypass methods
- Learned differences between registry hives
- Switched to Windows 10 for efficiency and stability

---

### Resource Management
- Identified time inefficiencies during troubleshooting
- Adjusted approach to prioritize progress over perfection

---

### Networking & Connectivity

- Used `ipconfig` and `ping` for troubleshooting
- Encountered: **“Destination host unreachable”**
- Discovered that:
  - NAT = internet access but isolated VMs
  - Internal Network = VM-to-VM communication

Solution:
- Switched both VMs to **Internal Network**
- Successfully established connectivity between client and domain controller

---

## Next Steps

In the next phase, this environment will be expanded with security tools:

- Sysmon (endpoint logging)
- Splunk (log aggregation and analysis)
- Wireshark (network traffic analysis)
- Kali Linux (attack simulation)

This will transform the lab into a **mini Security Operations Center (SOC)** environment.

---
