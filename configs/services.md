# Services Matrix

## dc01

- OS: Windows Server 2025 Standard Evaluation (Desktop Experience)
- Hostname: `dc01`
- Domain: `lab.local`
- Roles:
  - `Active Directory Domain Services`
  - `DNS Server`
  - `DHCP Server`

## app-01

- OS: Ubuntu Server
- Hostname: `app-01`
- Services:
  - `openssh-server`
  - `nginx`
  - `ufw`

## Verified Behaviors

- `app-01` received an address from the Windows DHCP scope
- `app-01` used `172.16.30.10` as DNS on the internal interface
- `dc01` resolved `app-01.lab.local`
- Windows browser successfully loaded the Ubuntu-hosted nginx page

## NAT Access

- `app-01` SSH access from host was enabled through VirtualBox NAT port forwarding
- Example host-side SSH pattern:
  - `ssh <user>@127.0.0.1 -p <host-port>`

## AD Objects Created

- OU: `Servers`
- OU: `Personal`
- Users:
  - `lab.admin`
  - `test.user`
- Group:
  - `WebAdmins`
