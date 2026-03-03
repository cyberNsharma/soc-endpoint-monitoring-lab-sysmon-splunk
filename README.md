# Enterprise-Style Endpoint Monitoring & Detection Lab  
### Sysmon + Splunk SIEM | SOC Analyst Project

---

## Project Overview

This project simulates a real-world Security Operations Center (SOC) environment by implementing endpoint monitoring, detection engineering, alerting, and dashboard creation using:

- Microsoft Sysmon
- Windows Event Logs
- Splunk SIEM

The goal of this lab is to detect and monitor simulated attacks from a blue-team perspective.

---

##  Objectives

- Ingest Windows & Sysmon logs into Splunk
- Simulate real-world attack scenarios
- Engineer detection rules
- Create SOC alerts
- Build monitoring dashboards
- Map detections to MITRE ATT&CK framework
- Document investigation workflow

---

##  Lab Architecture

Windows Endpoint (Sysmon Installed)  
        ↓  
Windows Event Logs  
        ↓  
Splunk Forwarder  
        ↓  
Splunk SIEM (Index: endpoint)

---

##  Tools Used

- Microsoft Sysmon (Process monitoring)
- Splunk Enterprise (SIEM platform)
- Windows 10 Virtual Machine
- Kali Linux (for brute force simulation)

---

#  Log Source Details

## Sysmon Event ID 1 — Process Creation

Generated whenever a process is created.

Contains:
- Image (process name)
- ParentImage
- CommandLine
- User
- Host
- Hashes

Used for detecting:
- PowerShell abuse
- Malware execution
- Living-off-the-Land attacks

---

## Windows Security Event ID 4625 — Failed Login

Generated when authentication fails.

Used for detecting:
- Brute force attacks
- Password spraying
- Account targeting

---

# Attack Simulations Performed

---

## 1️ Brute Force Attack Simulation

Simulated multiple failed login attempts.

### Detection Query

```spl
index=endpoint EventCode=4625
```

### Advanced Analysis

```spl
index=endpoint EventCode=4625
| stats count by TargetUserName
| sort -count
```

Purpose:
Identifies which user accounts are being targeted.

### Host-Level Analysis

```spl
index=endpoint EventCode=4625
| stats count by host
| sort -count
```

Purpose:
Identifies which endpoint is under attack.

### Timeline Monitoring

```spl
index=endpoint EventCode=4625
| timechart count
```

Purpose:
Detects spikes in failed login attempts.

### MITRE ATT&CK Mapping

T1110 — Brute Force

---

## 2️ PowerShell Execution Monitoring

Executed PowerShell manually to simulate suspicious command execution.

### Detection Query

```spl
index=endpoint EventCode=1 Image="*powershell.exe*"
```

Purpose:
Detect PowerShell execution on endpoint.

### Process Activity Trend

```spl
index=endpoint EventCode=1
| timechart count
```

Purpose:
Monitor process creation activity over time.

### Top Processes Analysis

```spl
index=endpoint EventCode=1
| stats count by Image
| sort -count
| head 10
```

Purpose:
Identify most executed programs and detect anomalies.

### MITRE ATT&CK Mapping

T1059.001 — PowerShell

---

#  Detection Engineering

Two Scheduled Alerts were created.

---

## Alert 1 — Brute Force Detection

**Title:**
SOC-LAB | Brute Force Detection | EventID 4625

Configuration:
- Type: Scheduled
- Run Every: 5 minutes
- Time Range: Last 5 minutes
- Trigger Condition: Number of results > 0
- Action: Add to Triggered Alerts

Purpose:
Detect real-time authentication failures.

---

## Alert 2 — PowerShell Execution Detection

**Title:**
SOC-LAB | PowerShell Execution | Sysmon EventID 1

Configuration:
- Type: Scheduled
- Run Every: 5 minutes
- Time Range: Last 5 minutes
- Trigger Condition: Number of results > 0
- Action: Add to Triggered Alerts

Purpose:
Monitor suspicious command execution.

---

#  SOC Monitoring Dashboard

Dashboard Name:
Endpoint Detection & Monitoring Dashboard

---

## Panel 1 — Failed Login Attempts Over Time

```spl
index=endpoint EventCode=4625
| timechart count
```

Visualization:
Line Chart

Purpose:
Detect brute force activity spikes.

---

## Panel 2 — PowerShell Execution Activity

```spl
index=endpoint EventCode=1 Image="*powershell.exe*"
| timechart count
```

Visualization:
Line Chart

Purpose:
Monitor PowerShell usage trends.

---

## Panel 3 — Top Executed Processes

```spl
index=endpoint EventCode=1
| stats count by Image
| sort -count
| head 10
```

Visualization:
Bar Chart

Purpose:
Identify abnormal process activity.

---

## Panel 4 — Failed Login Attempts by Host

```spl
index=endpoint EventCode=4625
| stats count by host
| sort -count
```

Visualization:
Bar Chart

Purpose:
Identify which endpoint is targeted.

---

#  SOC Knowledge Demonstrated

This project demonstrates understanding of:

- SIEM log ingestion
- Windows Event Log analysis
- Sysmon telemetry
- Detection engineering
- Alert configuration
- SOC dashboard creation
- Attack simulation
- MITRE ATT&CK mapping
- Endpoint monitoring
- Basic threat hunting

---

#  Screenshots Included

- Sysmon logs in Splunk
- Event ID 4625 detection
- Event ID 1 detection
- Alert configuration page
- Triggered alerts view
- SOC dashboard panels

---

#  Learning Outcome

Through this project, I learned:

- How SOC analysts monitor endpoint telemetry
- How to build detections from raw logs
- How to configure alerts in Splunk
- How to interpret authentication failures
- How to analyze process creation events
- How to build visual dashboards for monitoring

---

#  Future Improvements

- Add Sysmon Event ID 3 (Network connections)
- Add encoded PowerShell detection
- Add correlation searches
- Implement incident response playbooks
- Integrate threat intelligence feeds

---

#  Author

Nitin Sharma  
Aspiring SOC Analyst | Blue Team | DFIR Enthusiast  

---

#  Conclusion

This project simulates a basic enterprise SOC detection workflow using endpoint telemetry. It demonstrates foundational blue-team skills required for internship or entry-level SOC analyst roles.



This project simulates a basic enterprise SOC detection workflow using endpoint telemetry. It demonstrates foundational blue-team skills required for internship or entry-level SOC analyst roles.
