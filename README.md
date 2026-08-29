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

- [ ] Install Active Directory Domain Services on the Windows Server VM
- [ ] Create test user accounts and organizational units
- [ ] Switch VM networking from NAT to Internal/Bridged to enable connectivity testing between VMs
- [ ] Join a client VM to the domain
- [ ] Document basic Group Policy configuration
