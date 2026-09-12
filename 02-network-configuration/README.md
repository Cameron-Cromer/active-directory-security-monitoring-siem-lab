# Network Configuration

## Network Design

A VirtualBox NAT network named `CameronLab-NAT` was created using the
following address space:

- Network: `192.168.10.0/24`
- Default Gateway: `192.168.10.1`

![NAT Configuration](../assets/screenshots/02-networking/01-cameronlab-nat-network.jpg)

All virtual machines were connected to the CameronLab NAT network.

![VM Network Adapter](../assets/screenshots/02-networking/02-vm-nat-network-adapter.jpg)

## Static IP Addressing

| System | IP Address | Role |
|---|---|---|
| DC01 | `192.168.10.7` | Domain Controller / DNS |
| SplunkServer | `192.168.10.10` | Splunk Enterprise |
| PC01 | `192.168.10.100` | Windows Endpoint |
| Kali Linux | `192.168.10.250` | Security Testing |

The Splunk server was configured with a static IP address.

![Splunk Server Static IP](../assets/screenshots/02-networking/03-splunk-server-static-ip.jpg)

DC01 was configured with a static IP address to provide consistent
domain and DNS services within the lab.

![Domain Controller Static IP](../assets/screenshots/02-networking/04-domain-controller-static-ip.jpg)

PC01 was configured to use the Domain Controller at `192.168.10.7`
as its DNS server so that the `cameronlab.local` Active Directory
domain could be resolved.

![PC01 Network Configuration](../assets/screenshots/02-networking/06-pc01-static-ip-dns.jpg)

Kali Linux was assigned the static IP address `192.168.10.250`
for use as the security-testing workstation.

![Kali Linux Static IP](../assets/screenshots/02-networking/07-kali-linux-static-ip.jpg)

## Connectivity Verification

Connectivity between DC01 and the Splunk server was verified using ICMP.

![Connectivity Test](../assets/screenshots/02-networking/05-dc01-splunk-connectivity-test.jpg)
