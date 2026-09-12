# Network Configuration

## Network Design

A VirtualBox NAT network named `CameronLab-NAT` was created using the
following address space:

- Network: `192.168.10.0/24`
- Default Gateway: `192.168.10.1`

![NAT Configuration](../assets/screenshots/02-networking/nat-network-configuration.jpg)

All virtual machines were connected to the CameronLab NAT network.

![VM Network Adapter](../assets/screenshots/02-networking/vm-nat-adapter.jpg)

## Static IP Addressing

| System | IP Address | Role |
|---|---|---|
| DC01 | `192.168.10.7` | Domain Controller / DNS |
| SplunkServer | `192.168.10.10` | Splunk Enterprise |
| PC01 | `192.168.10.100` | Windows Endpoint |
| Kali Linux | `192.168.10.250` | Security Testing |

PC01 was configured to use the Domain Controller at `192.168.10.7`
as its DNS server so that the `cameronlab.local` Active Directory
domain could be resolved.

![PC01 Network Configuration](../assets/screenshots/02-networking/pc01-static-ip-dns.jpg)

## Connectivity Verification

Connectivity between DC01 and the Splunk server was verified using ICMP.

![Connectivity Test](../assets/screenshots/02-networking/dc-splunk-connectivity.jpg)
