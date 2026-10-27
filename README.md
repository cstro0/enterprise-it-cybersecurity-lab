# enterprise-it-cybersecurity-lab

This project simulates a real-world enterprise environment using Active Directory and transitions into a security operations (SOC) lab.

In the future, I may be implementing more tools that I am interested in, but are ultimately outside of the current scope.

I will provide step-by-step documentation for anyone that would like to follow along or recreate the lab.

---

## Project Scope
- Phase 1: Active Directory Setup (Windows Server + Windows 10/11)
- Phase 2: Logging & Detection (Sysmon + Splunk)
- Phase 3: Attack Simulation (Kali + Wireshark)

---

## Skills
- Active Directory & Powershell automation
- Network troubleshooting
- SIEM (Splunk)
- Endpoint logging (Sysmon)
- Traffic analysis (Wireshark)

---

## Network Configuration

This lab uses VirtualBox's Internal Network for isolated VM-to-VM communication. No internet connection is needed as of right now. VMs attached to the same internal network can communicate as if they are connected to the same Ethernet switch while remaining isolated from the host system and external networks (which are rules I need to follow per my school's dorm policy).

The internal network also supports AD DS, endpoint monitoring with Sysmon, log forwarding to Splunk, controlled security testing with Kali Linux, and network traffic analysis with Wireshark (Although I may need to provide a better solution to get Wireshark to be fully operational).

---

**This is still a work in progress, please come back back soon for updates!*