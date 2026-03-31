# IP Plan

## Internal Network

- VirtualBox internal network: `infra-lab`
- Subnet: `172.16.30.0/24`

## Static Addresses

- `dc01`: `172.16.30.10/24`

## DHCP Range

- Scope name: `Infra-Lab Scope`
- Start: `172.16.30.100`
- End: `172.16.30.200`
- DNS server handed to clients: `172.16.30.10`
- Gateway: not configured for the internal-only segment

## Observed Lease

- `app-01`: `172.16.30.100/24`

## Notes

- `dc01` uses a static internal address.
- `app-01` was configured to use DHCP on the internal interface.
- NAT remained enabled on Adapter 1 for installation convenience and host access.
