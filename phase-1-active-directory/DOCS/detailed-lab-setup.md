# Detailed Lab Setup

The following assumes that you have downloaded VirtualBox and an ISO for both Windows Server 2022 and Windows 10:
---

## 1️⃣ Windows Server VM Setup

- Load Windows Server 2022 ISO into VirtualBox and set up  

From the VirtualBox home screen:
- CTRL + N  
- Name: Windows Server 2022
- Select Windows Server 2022 (64-bit) as the Version  
- Allocate your resources according to your specs and proceed  

Configure Network:
- Go to Settings  
- Click Expert → Network  
- Attached To → Internal Network  
  - This allows VM-to-VM communication  

Continue setup:
- Load VM and select your Windows Server 2022 ISO in the DVD field  
- Complete setup and select Standard Evaluation (Desktop Experience)  
- Click “Custom” and proceed  

- Create and take note of a password, I always choose **@Password1** because it is easy to rememer.  

Rename server:
- Server Manager → Local Server → Computer Name → Change  
- Set name to: **DC01**

---

## 2️⃣ Configure Server Network Settings

- Press Win + R → type: **ncpa.cpl**


- Open your network adapter → Properties  
- Select Internet Protocol Version 4 (TCP/IPv4)  

Set the following:

- IP address: `172.16.0.10`  
- Subnet mask: `255.255.0.0`  
- Default gateway: (leave blank)  

DNS:
- Preferred DNS: `127.0.0.1`  

Explanation:
- This is the loopback interface (server points to itself)  
- Servers running Active Directory also act as DNS servers  
- The server resolves domain names using its own records  

---

## 3️⃣ Install Active Directory & Promote to Domain Controller

From Server Manager:

- Manage → Add Roles & Features  
- Select: Active Directory Domain Services → Add Features  
- Proceed and click Install  

After installation:
- Click “Promote to domain controller”  
- Select: Add a new forest

- Domain name: **[yourname].com**
    - I chose "Craig.com"

- Set password: @Password1


- Complete installation  

---

## 4️⃣ Windows 10 Client VM Setup

From VirtualBox home screen:

- CTRL + N  
- Name: Windows 10 Client

- Allocate resources  

Configure Network:
- Settings → Network → Internal Network  

Continue setup:
- Load Windows 10 ISO  
- Choose: Windows 10 Pro
- Select Custom install  
- Proceed through setup  

When prompted:
- Select **Set up for personal use**  
- Choose:
  - Offline Account  
  - Limited Experience  

Create local account:
- Username: clientuser
- Password: clientuser
- Accept all prompts and finish setup  

---

## 5️⃣ Configure Client Network Settings

- Press Win + R → type: Press Win + R → type: **ncpa.cpl**
- Open network adapter → Properties  
- Select IPv4  

Set:

- IP address: `172.16.0.20`  
- Subnet mask: `255.255.0.0`  
- Default gateway: (leave blank)  

DNS:
- Preferred DNS: `172.16.0.10`  

Configuration Explanation:
- The client must use the Domain Controller as DNS  
- This allows it to locate the domain  
- For Example:
  - Client asks: “Where is Craig.com?”  
  - Server responds with its IP address  
  - Domain join succeeds

---

## 6️⃣ Verify Connectivity

- Open Command Prompt and run: **ipconfig**
- Confirm correct IP configuration

```
{INSERT IMAGE}
```
  
- run **ping 172.16.0.10**
- you are pinging to detect if your host is reachable
- All 4 packets should be received  

If you see: "Destination host unreachable", ensure both VMs are on Internal Network through VirtualBox's network settings

---

## 7️⃣ Join Client to Domain

- Navigate:
  - Settings → System → About  
  - Advanced System Settings  
  - Computer Name → Change  

- Select: Domain

- Enter your domain name, or use the assigned static IP Address of your server 
  - Both work because your DNS is already configured

When prompted:
- Username: administrator
- Password: @Password1

---

## 8️⃣ Verify Domain Join

- On the Domain Controller, open Active Directory Users and Computers  
- Confirm client appears under **Computers**
- If the client appears, you are good to go!
