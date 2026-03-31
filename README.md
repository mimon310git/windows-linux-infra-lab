# Windows-Linux Infra Lab

Two-VM infrastructure lab built on VirtualBox with a Windows Server domain controller and an Ubuntu application server.

## Overview

This lab was built to practice core infrastructure tasks across Windows and Linux:

- Windows Server 2025 as the infrastructure core
- Ubuntu Server as the internal application server
- Active Directory, DNS, and DHCP on the Windows side
- Nginx, SSH, and UFW on the Linux side
- Manual incident simulation and recovery testing

## Lab Topology

- `dc01`
  - Windows Server 2025 Standard Evaluation (Desktop Experience)
  - Roles: `AD DS`, `DNS`, `DHCP`
  - Internal IP: `172.16.30.10/24`
  - Domain: `lab.local`
- `app-01`
  - Ubuntu Server
  - Services: `nginx`, `openssh-server`, `ufw`
  - Internal IP: DHCP lease from `dc01`
  - Observed lease during testing: `172.16.30.100/24`

Additional details are in [docs/topology.md](docs/topology.md) and [configs/ip-plan.md](configs/ip-plan.md).

## What Was Configured

### Windows Server (`dc01`)

- Renamed server to `dc01`
- Created new forest: `lab.local`
- Installed and configured:
  - `Active Directory Domain Services`
  - `DNS Server`
  - `DHCP Server`
- Created a DHCP scope for the internal lab network
- Created basic AD objects for testing:
  - OU: `Servers`
  - OU: `Personal`
  - Users: `lab.admin`, `test.user`
  - Security group: `WebAdmins`

### Ubuntu Server (`app-01`)

- Connected to the same internal lab network
- Received an IP address from Windows DHCP
- Used Windows DNS for `lab.local`
- Installed and enabled:
  - `openssh-server`
  - `nginx`
  - `ufw`
- Served an internal page at `http://app-01.lab.local`

## Services Verified

- DNS resolution from Ubuntu to `dc01.lab.local`
- DNS resolution from Windows to `app-01.lab.local`
- DHCP lease assignment to Ubuntu
- Internal web access from Windows to Ubuntu
- SSH access from host to Ubuntu via NAT port forwarding

Service notes are in [configs/services.md](configs/services.md).

## Incident Scenarios

The lab currently includes three manual incident scenarios:

1. `Nginx service outage on app-01`
2. `DNS service failure on dc01`
3. `Port 80 blocked by UFW on app-01`

Each incident includes symptoms, validation steps, and recovery notes in [docs/incidents.md](docs/incidents.md).

## Results

The final environment successfully demonstrated:

- Windows infrastructure services supporting a Linux host
- Name resolution through `lab.local`
- Automatic IP assignment through Windows DHCP
- Linux web service delivery to a Windows client/server browser
- Clear service outage and firewall failure scenarios

Planned next step:

- Add `win-client-01` to the domain and test basic GPO scenarios

## Suggested Screenshots

Add screenshots to `docs/screenshots/` for:

- healthy app page
- AD users and groups
- DHCP scope
- nginx outage
- DNS failure
- blocked port 80

See [docs/screenshots/README.md](docs/screenshots/README.md).

## Repository Structure

```text
windows-linux-infra-lab/
├─ README.md
├─ .gitignore
├─ configs/
│  ├─ ip-plan.md
│  └─ services.md
└─ docs/
   ├─ topology.md
   ├─ incidents.md
   └─ screenshots/
      └─ README.md
```

## Safe To Publish

This repository is intended to contain:

- lab-only RFC1918 IP addresses
- screenshots from the lab
- documentation written for the project
- non-sensitive service and troubleshooting notes

Do not publish:

- VM exports or disk images
- saved credentials
- private keys
- shell histories
- copied vendor documentation
