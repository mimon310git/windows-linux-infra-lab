# Topology

## VM Layout

```text
Windows Host
│
├─ VirtualBox NAT
│  ├─ dc01
│  │  └─ Windows Server 2025
│  └─ app-01
│     └─ Ubuntu Server
│
└─ Internal Network: infra-lab (172.16.30.0/24)
   ├─ dc01   -> 172.16.30.10
   └─ app-01 -> DHCP lease from dc01
```

## Role Mapping

### dc01

- Domain controller for `lab.local`
- DNS server for the lab
- DHCP server for the internal segment

### app-01

- Internal Ubuntu application server
- Nginx-hosted internal page
- SSH-managed Linux host

## Traffic Flow

### Normal State

1. `app-01` requests an address from Windows DHCP.
2. `dc01` assigns an internal lease.
3. `app-01` uses `dc01` as DNS for `lab.local`.
4. Windows resolves `app-01.lab.local`.
5. Browser request reaches nginx on Ubuntu.

### Failure States Tested

1. `nginx` stopped on `app-01`
2. `DNS Server` service stopped on `dc01`
3. `ufw` blocked `80/tcp` on `app-01`
