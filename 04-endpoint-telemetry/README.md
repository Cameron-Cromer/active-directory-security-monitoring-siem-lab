# Endpoint Telemetry Collection

## Overview

Windows telemetry from PC01 and DC01 was centralized in Splunk using
Splunk Universal Forwarder and Sysmon.

The telemetry pipeline used in the lab was:

Windows Endpoint  
→ Windows Event Logs / Sysmon  
→ Splunk Universal Forwarder  
→ TCP 9997  
→ Splunk Enterprise

## Splunk Universal Forwarder

Splunk Universal Forwarder was installed on the Windows systems and
configured to send data to the Splunk server at:

`192.168.10.10:9997`

![Universal Forwarder](../assets/screenshots/04-telemetry/01-splunk-universal-forwarder-setup.jpg)

The forwarder was configured to send telemetry to the receiving Splunk
Enterprise instance.

![Forwarder Receiver Configuration](../assets/screenshots/04-telemetry/02-forwarder-receiver-configuration.jpg)

## Sysmon

Microsoft Sysmon was installed on the Windows endpoint to provide
additional endpoint telemetry.

![Sysmon Installation](../assets/screenshots/04-telemetry/04-sysmon-installation.jpg)

The successful installation confirmed that Sysmon and its driver were
running on the Windows system.

![Sysmon Installation Success](../assets/screenshots/04-telemetry/05-sysmon-installation-success.jpg)

## inputs.conf

The Splunk Universal Forwarder was configured to collect:

- Application event logs
- Security event logs
- System event logs
- Sysmon Operational logs

![Splunk Inputs](../assets/screenshots/04-telemetry/06-splunk-inputs-conf.jpg)

The Windows event log inputs were configured to forward the selected
event sources to the centralized Splunk server.

![Windows Event Log Inputs](../assets/screenshots/04-telemetry/07-windows-event-log-inputs.jpg)

The complete configuration is available here:

[View inputs.conf](../configs/splunk/inputs.conf)

## Windows Event Log Verification

An `index=endpoint` search in Splunk confirmed that Windows event data
was successfully being received from the monitored systems.

![Endpoint Events](../assets/screenshots/03-splunk/05-splunk-endpoint-index-events.jpg)

## Sysmon Telemetry Verification

To verify that Sysmon telemetry was successfully reaching the SIEM,
the Sysmon Operational event source was queried in Splunk using:

```spl
index=endpoint source="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
