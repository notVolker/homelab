# Home Lab Project: Virtualized Windows/Linux Server Environment

A personal home lab built using Oracle VirtualBox to practice core IT support and sysadmin skills — server installation, Linux CLI fundamentals, Windows Server administration, and (upcoming) Active Directory Domain Services — on consumer hardware.

## Overview

| | |
|---|---|
| **Hypervisor** | Oracle VirtualBox |
| **Host Specs** | 8GB RAM (constraint: VMs run one at a time) |
| **VMs Built** | Ubuntu Server 26.04 LTS, Windows Server 2022 Standard |
| **Status** | Both VMs installed and functional — Active Directory setup in progress |

## Why This Project

As a Computer Science graduate targeting IT support, helpdesk, and sysadmin-adjacent roles, I built this home lab to get hands-on with the tools and concepts that come up daily in those jobs — user/permission management, server administration, troubleshooting, and (soon) Active Directory — rather than only learning them in theory.

## VM 1: Ubuntu Server

**Specs:** 1536 MB RAM · 25 GB disk · Ubuntu Server 26.04 LTS

**What I did:**
- Installed using the guided installer with "use entire disk" (LVM partitioning for `/` and `/boot`)
- Set up a user account and hostname (`ubuntu-server`)
- Enabled OpenSSH server during setup for remote access
- Ran a full system update: `sudo apt update && sudo apt upgrade -y`
- Practiced core CLI commands for navigation, networking, and system monitoring:
  - Navigation: `pwd`, `ls`, `cd`, `cat`
  - Networking: `ip a`, `ping`, `hostname`
  - Permissions: `sudo`, `whoami`, `adduser`, `passwd`
  - Monitoring: `top`, `df -h`, `free -h`, `systemctl status`

**Network config:** NAT (internal IP `10.0.2.15`)

<img width="1280" height="800" alt="VirtualBox_Ubuntu-Server_25_08_2026_12_49_44" src="https://github.com/user-attachments/assets/a0eb103e-a1d1-4695-b68c-234961adc2b4" />

<img width="1280" height="800" alt="VirtualBox_Ubuntu-Server_25_08_2026_12_57_54" src="https://github.com/user-attachments/assets/86c1dd72-4884-4de3-981c-92b7fd3c0e8b" />

<img width="1280" height="800" alt="VirtualBox_Ubuntu-Server_25_08_2026_12_58_16" src="https://github.com/user-attachments/assets/93869e2e-1d8b-41d7-b31d-d81cadda8d46" />



## VM 2: Windows Server

**Specs:** 3072 MB RAM · 2 vCPUs · 50 GB disk · Windows Server 2022 Standard (Desktop Experience, Evaluation)

**What I did:**
- Performed a clean custom installation (not upgrade) with the Desktop Experience edition for a full GUI
- **Troubleshot a boot loop issue**: an initial interrupted install left partition remnants that caused Windows Setup to repeatedly detect a false "upgrade" scenario. Resolved by deleting all existing partitions on the disk at the installer's disk-selection screen before reinstalling clean.
- Set the local Administrator password
- Confirmed successful boot into Server Manager

**Next milestone:** Installing the Active Directory Domain Services role and configuring a domain controller.

<img width="1024" height="768" alt="VirtualBox_WinServer_29_08_2026_19_57_42" src="https://github.com/user-attachments/assets/e6fef942-3022-48c0-a149-c1ee237e080c" />

<img width="1024" height="768" alt="VirtualBox_WinServer_29_08_2026_22_17_59" src="https://github.com/user-attachments/assets/99708c72-8955-4611-867c-e90d05561e19" />

<img width="1024" height="768" alt="VirtualBox_WinServer_29_08_2026_22_20_15" src="https://github.com/user-attachments/assets/3bc06b2e-a5ac-49d8-a988-e6dd4d3e9f21" />

## Constraints & Lessons Learned

- **8GB host RAM** meant both VMs could not run simultaneously — sized each VM's memory allocation individually and made a habit of shutting one down cleanly (`sudo shutdown now` on Ubuntu) before starting the other.
- **Snapshots matter**: took a VirtualBox snapshot right after the Windows Server install succeeded, so future configuration mistakes (e.g. during AD setup) can be rolled back without a full reinstall.
- **Clean installs vs. upgrades**: learned firsthand how leftover partition data can confuse the Windows installer, and how to force a clean install by wiping partitions manually.

## Roadmap

- [X] Install Active Directory Domain Services on the Windows Server VM
- [ ] Create test user accounts and organizational units
- [ ] Switch VM networking from NAT to Internal/Bridged to enable connectivity testing between VMs
- [ ] Join a client VM to the domain
- [ ] Document basic Group Policy configuration


## Day 2: Active Directory Domain Services

**Goal:** Turn the Windows Server VM into a functioning domain controller and practice core AD administration tasks.

**What I did:**
- Installed the **Active Directory Domain Services (AD DS)** role via Server Manager's "Add Roles and Features" wizard
- Promoted the server to a **domain controller**, creating a new forest with the domain `homelab.local`
- Set a Directory Services Restore Mode (DSRM) password (kept separate from the Administrator password)
- Accepted the expected DNS delegation warning (normal for an internal lab domain with no parent DNS)
- Verified the promotion by confirming domain login format (`HOMELAB\Administrator`) and the new AD DS role appearing in Server Manager
- Opened **Active Directory Users and Computers** and created an **Organizational Unit (OU)** to hold test accounts
- Created a **test user account** inside the OU, applying Windows' default password complexity rules (8+ characters, 3 of 4 character types, can't contain the username)

<img width="1024" height="768" alt="VirtualBox_WinServer_31_08_2026_14_09_40" src="https://github.com/user-attachments/assets/3ee02f62-f125-4679-9b14-3c2c63eeaa95" />

<img width="1024" height="768" alt="VirtualBox_WinServer_31_08_2026_14_35_15" src="https://github.com/user-attachments/assets/a1b7c710-978c-4f35-b2f6-7982590745c5" />

<img width="1024" height="768" alt="VirtualBox_WinServer_31_08_2026_14_55_09" src="https://github.com/user-attachments/assets/cfc7195b-1b85-4046-bdf5-31889f3152f1" />

**Troubleshooting / What I learned:**
- Attempted to log in locally as the new test user and hit: *"The sign-in method you're trying to use is not allowed."*
- Root cause: Domain Controllers restrict interactive/local logon to privileged groups (Administrators, Server Operators, etc.) by default — regular domain users are intentionally blocked from logging directly into the DC as a security measure.
- Learned that in a real environment, regular users log into a **domain-joined client machine**, not the DC itself — confirms the account/domain setup is correct, just tested from the wrong machine.

**Next milestone:** Add a Windows 10/11 client VM, join it to `homelab.local`, and properly test user login from the client. Also plan to create a security group and practice group-based permissions.

<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/b9dad4a5-9557-4d4f-8421-4e8c8772a614" />


