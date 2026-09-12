# Lessons Learned

## Overview

This project provided hands-on experience building, integrating, and monitoring
a small Windows Active Directory environment from the ground up.

Rather than working with isolated tools, the lab demonstrated how networking,
identity management, endpoint telemetry, and SIEM analysis all depend on one
another within a functioning enterprise environment.

---

## Active Directory and Identity Management

One of the key takeaways from the project was understanding how Active
Directory components work together.

The lab reinforced practical experience with:

- Deploying Active Directory Domain Services
- Promoting Windows Server 2022 to a Domain Controller
- Creating the `cameronlab.local` domain
- Creating Organizational Units for separate departments
- Provisioning domain user accounts
- Joining a Windows 11 endpoint to the domain
- Authenticating to a domain-joined endpoint with Active Directory accounts
- Verifying user and OU placement using command-line tools such as `whoami /fqdn`

This helped connect Active Directory concepts with their actual implementation
inside a Windows environment.

---

## DNS and Network Configuration

The project also reinforced the importance of correct network and DNS
configuration in an Active Directory environment.

Static IPv4 addresses were assigned to the major systems in the lab, including
the Domain Controller, Splunk server, Windows endpoint, and Kali Linux
workstation.

A particularly important lesson was that domain-connected Windows systems need
to use the Domain Controller as their DNS server in order to properly resolve
and communicate with the Active Directory domain.

The lab demonstrated that even when systems are powered on and connected to the
same virtual network, incorrect DNS or IP configuration can prevent domain
services from functioning correctly.

---

## Centralized Logging and Telemetry

Another major takeaway was understanding how endpoint activity becomes
searchable SIEM data.

Splunk Universal Forwarder was configured to collect Windows telemetry and send
it to the centralized Splunk Enterprise server.

Sysmon was also installed to provide additional endpoint visibility.

The telemetry pipeline used throughout the lab was:

```text
Endpoint Activity
      ↓
Windows Event Logs / Sysmon
      ↓
Splunk Universal Forwarder
      ↓
TCP 9997
      ↓
Splunk Enterprise
      ↓
Search and Investigation
```

Working through this process helped reinforce the difference between:

- **Generating telemetry**
- **Collecting telemetry**
- **Forwarding telemetry**
- **Indexing telemetry**
- **Searching and analyzing telemetry**

The `inputs.conf` and `outputs.conf` files were especially useful for
understanding how the Splunk Universal Forwarder determines what data to
collect and where that data should be sent.

---

## Windows Security Event Analysis

The authentication-testing portion of the lab demonstrated how activity on an
endpoint becomes visible through Windows Security Event Logs.

The Hydra RDP test generated both failed and successful authentication
activity, which could later be investigated in Splunk.

Two important Windows Security Event IDs were observed:

| Event ID | Meaning |
|---|---|
| `4625` | Failed account logon |
| `4624` | Successful account logon |

This reinforced the importance of understanding what individual event IDs
represent rather than relying only on raw log volume.

---

## Event Correlation and Investigation

One of the most valuable lessons from the project was learning how multiple
pieces of telemetry can be correlated during an investigation.

For example, the Kali Linux workstation was configured with the static IP
address:

`192.168.10.250`

During the Splunk investigation, the Windows authentication event metadata
identified:

- **Workstation Name:** `kali`
- **Source Network Address:** `192.168.10.250`

Because the source address matched the known Kali Linux system, the
authentication events could be correlated back to the workstation that
generated the activity.

This demonstrated how security investigations often rely on combining
information from multiple sources rather than looking at a single event in
isolation.

---

## Recognizing Automated Authentication Activity

The failed logon events generated during the Hydra test occurred within a very
short period of time.

This timing pattern helped distinguish automated brute-force password-guessing
activity from normal interactive authentication behavior.

The lab reinforced that useful indicators are not limited to the contents of
individual events. Timing, frequency, source address, account name, and
workstation information can all provide important context during an
investigation.

---

## Troubleshooting

Building the environment also required troubleshooting across several
different technologies.

Issues involving networking, DNS, virtual machines, Splunk forwarding, and
Windows domain configuration demonstrated the importance of validating each
layer separately.

A useful troubleshooting approach throughout the project was:

```text
Verify configuration
      ↓
Test connectivity
      ↓
Confirm service operation
      ↓
Confirm log generation
      ↓
Confirm log forwarding
      ↓
Verify ingestion in Splunk
```

This step-by-step approach made it easier to isolate problems rather than
troubleshooting the entire environment at once.

---

## Key Takeaways

The project strengthened my understanding of:

- Active Directory administration
- Windows Server configuration
- Organizational Units and domain identities
- DNS dependencies in Active Directory
- Static IPv4 networking
- Windows domain joins
- Splunk Enterprise deployment
- Splunk Universal Forwarder configuration
- `inputs.conf` and `outputs.conf`
- Sysmon telemetry collection
- Windows Security Event Logs
- Event IDs `4624` and `4625`
- SPL searching
- Brute-force authentication detection
- Event timeline analysis
- Source IP identification
- Security event correlation

The most important takeaway was understanding the complete relationship
between infrastructure, endpoint activity, telemetry collection, and security
analysis.

Instead of treating Active Directory, Sysmon, Splunk, and Kali Linux as
separate technologies, this lab demonstrated how they can work together as
part of a complete security-monitoring workflow.

---

## Final Reflection

The project progressed from building the environment to generating security
activity and finally investigating that activity through centralized telemetry.

The complete workflow was:

**Build Infrastructure → Configure Identity → Collect Telemetry → Generate Test Activity → Investigate Events → Correlate the Source**

This end-to-end process provided a much clearer understanding of how enterprise
systems can be monitored and how authentication activity can be investigated
using SIEM data.
