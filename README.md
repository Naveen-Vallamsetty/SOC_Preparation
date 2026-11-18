# SOC Preparation

A structured cybersecurity learning and practical training repository documenting my journey toward becoming a SOC Analyst.

This repository contains my learning notes, hands-on labs, commands, practical exercises, Splunk searches, security investigations, projects, diagrams, and interview preparation materials.

## Learning Roadmap

My SOC preparation is organized into the following areas:

1. **TryHackMe** – Cybersecurity fundamentals and practical security learning.
2. **Splunk** – Splunk fundamentals, Search Processing Language (SPL), security monitoring, detection engineering, and SOC projects.
3. **Practical SOC Projects** – Real-world security monitoring and investigation scenarios.
4. **Interview Preparation** – Technical concepts, troubleshooting scenarios, and SOC Analyst interview questions.

---

# Repository Structure

```text
SOC_Preparation/
│
├── README.md
│
├── TryHackMe/
│   ├── README.md
│   │
│   └── Pre-Security/
│       ├──Pre-Security-Notes.md
│
│
├── Splunk/
│   ├── README.md
│   │
│   ├── Part-1-Splunk-Fundamentals-1/
│   │   ├── README.md
│   │   │
│   │   ├── Module-01-Introduction-to-Splunk/
│   │   │   ├── README.md
│   │   │   ├── Labs/
│   │   │   │   ├── Lab-01-Splunk-Interface.md
│   │   │   │   ├── Lab-02-Identify-Log-Types.md
│   │   │   │   ├── Lab-03-Splunk-Architecture.md
│   │   │   │   └── Lab-04-Forwarder-to-Indexer.md
│   │   │   ├── Diagrams/
│   │   │   │   ├── Splunk-Architecture.mmd
│   │   │   │   └── Data-Ingestion-Flow.mmd
│   │   │   └── Interview-Questions/
│   │   │       └── SOC-L1-L2-Questions.md
│   │   │
│   │   ├── Module-02-Basic-Searching/
│   │   │   ├── README.md
│   │   │   ├── Labs/
│   │   │   ├── Examples/
│   │   │   └── Interview-Questions/
│   │   │
│   │   ├── Module-03-Using-Search-Results/
│   │   │   ├── README.md
│   │   │   ├── Labs/
│   │   │   └── Interview-Questions/
│   │   │
│   │   ├── Module-04-Saving-and-Scheduling-Searches/
│   │   │   ├── README.md
│   │   │   ├── Labs/
│   │   │   └── Interview-Questions/
│   │   │
│   │   ├── Module-05-Working-with-Fields/
│   │   │   ├── README.md
│   │   │   ├── Labs/
│   │   │   └── Interview-Questions/
│   │   │
│   │   └── Module-06-Dashboards-and-Visualizations/
│   │       ├── README.md
│   │       ├── Labs/
│   │       ├── Dashboards/
│   │       └── Interview-Questions/
│   │
│   ├── Part-2-Splunk-Fundamentals-2/
│   │   ├── README.md
│   │   │
│   │   ├── Module-01-Transformation-Commands/
│   │   │   ├── README.md
│   │   │   ├── SPL-Examples/
│   │   │   └── Interview-Questions/
│   │   │
│   │   ├── Module-02-Eval-Command/
│   │   │   ├── README.md
│   │   │   ├── SPL-Examples/
│   │   │   └── Interview-Questions/
│   │   │
│   │   ├── Module-03-Multivalue-Fields-and-Lookups/
│   │   │   ├── README.md
│   │   │   ├── Sample-Lookups/
│   │   │   └── Interview-Questions/
│   │   │
│   │   ├── Module-04-Advanced-Dashboards-and-Reports/
│   │   │   ├── README.md
│   │   │   ├── Labs/
│   │   │   └── Interview-Questions/
│   │   │
│   │   └── Module-05-Data-Models-and-Pivot/
│   │       ├── README.md
│   │       ├── Labs/
│   │       └── Interview-Questions/
│   │
│   ├── Part-3-Splunk-SPL-for-SOC/
│   │   ├── README.md
│   │   │
│   │   ├── Module-01-Filtering-and-Searching/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-02-Aggregation-and-Grouping/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-03-Where-and-Result-Filtering/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-04-Ordering-and-Formatting/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-05-Time-Series-Analysis/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-06-String-Manipulation-and-Rex/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-07-Event-Correlation/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-08-Lookups-and-Threat-Intelligence/
│   │   │   ├── README.md
│   │   │   └── Queries.md
│   │   │
│   │   ├── Module-09-Detection-Engineering-and-Alerts/
│   │   │   ├── README.md
│   │   │   └── Detection-Queries.md
│   │   │
│   │   └── Module-10-Threat-Hunting-and-Optimization/
│   │       ├── README.md
│   │       └── Hunting-Queries.md
│   │
│   ├── Part-4-SOC-Practical-Projects/
│   │   ├── README.md
│   │   │
│   │   ├── Project-01-Linux-Security-Monitoring/
│   │   │   ├── README.md
│   │   │   ├── SPL-Queries/
│   │   │   ├── Dashboards/
│   │   │   └── Investigation-Reports/
│   │   │
│   │   ├── Project-02-Edge-Infrastructure-Monitoring/
│   │   │   ├── README.md
│   │   │   ├── Architecture-Diagrams/
│   │   │   ├── SPL-Queries/
│   │   │   └── Dashboards/
│   │   │
│   │   ├── Project-03-Brute-Force-Investigation/
│   │   │   ├── README.md
│   │   │   ├── Detection-Queries/
│   │   │   ├── Sample-Logs/
│   │   │   └── Incident-Reports/
│   │   │
│   │   ├── Project-04-VPN-and-Linux-Correlation/
│   │   │   ├── README.md
│   │   │   ├── SPL-Queries/
│   │   │   └── Investigation-Reports/
│   │   │
│   │   ├── Project-05-Threat-Intelligence-Investigation/
│   │   │   ├── README.md
│   │   │   ├── Sample-Threat-Intel/
│   │   │   ├── SPL-Queries/
│   │   │   └── Investigation-Reports/
│   │   │
│   │   └── Project-06-Complete-SOC-Monitoring-Dashboard/
│   │       ├── README.md
│   │       ├── Dashboard-Exports/
│   │       ├── Detection-Queries/
│   │       ├── Security-Incidents/
│   │       └── Documentation/
│   │
│   └── Resources/
│       ├── Splunk-Documentation.md
│       ├── Syslog-Fundamentals.md
│       └── SPL-Cheat-Sheet.md
│
└── Progress-Tracker.md
```

---

# 1. TryHackMe

This section contains cybersecurity fundamentals and practical learning completed through TryHackMe.

## Learning Path: Pre-Security

The Pre-Security learning path builds foundational knowledge required before moving into more advanced cybersecurity topics.

Topics covered:

- Computer Fundamentals
- Operating System Basics
- Software Basics
- Network Fundamentals
- How the Web Works
- Attacks and Defenses
- Offensive Security Fundamentals
- Defensive Security Fundamentals

Study materials:

| Resource              | Location                           |
| --------------------- | ---------------------------------- |
| Pre-Security Overview | `TryHackMe/Pre-Security/README.md` |
| Complete Study Notes  | `TryHackMe/Pre-Security/Notes.md`  |

Future learning paths can be added under the TryHackMe directory as separate folders.

---

# 2. Splunk

This section documents my Splunk learning, from fundamentals to practical SOC applications.

## Part 1: Splunk Fundamentals 1

Focus areas:

- Introduction to Splunk.
- Splunk architecture.
- Splunk interface.
- Data ingestion.
- Forwarders and indexers.
- Basic searching.
- Search results.
- Saving and scheduling searches.
- Working with fields.
- Dashboards and visualizations.

## Part 2: Splunk Fundamentals 2

Focus areas:

- Transformation commands.
- Eval command.
- Multivalue fields.
- Lookups.
- Advanced dashboards.
- Reports.
- Data models.
- Pivot.

## Part 3: Splunk SPL for SOC

Focus areas:

- Filtering and searching.
- Aggregation and grouping.
- Where and result filtering.
- Ordering and formatting.
- Time-series analysis.
- String manipulation and regular expressions.
- Event correlation.
- Lookups and threat intelligence.
- Detection engineering.
- Alerts.
- Threat hunting.
- Search optimization.

## Part 4: SOC Practical Projects

This section contains practical projects designed to apply Splunk knowledge to security monitoring and investigation scenarios.

### Project 1: Linux Security Monitoring

Objectives:

- Monitor Linux authentication events.
- Identify failed and successful logins.
- Detect unusual login activity.
- Analyze system logs.
- Create security dashboards.

### Project 2: Edge Infrastructure Monitoring

Objectives:

- Monitor infrastructure events.
- Analyze network-related logs.
- Identify availability issues.
- Monitor device activity.
- Build infrastructure monitoring dashboards.

### Project 3: Brute-Force Investigation

Objectives:

- Detect repeated failed authentication attempts.
- Identify suspicious source IP addresses.
- Identify targeted usernames.
- Correlate authentication events.
- Prepare an incident investigation report.

### Project 4: VPN and Linux Correlation

Objectives:

- Analyze VPN authentication logs.
- Correlate VPN activity with Linux authentication events.
- Identify unusual access patterns.
- Investigate suspicious login activity.

### Project 5: Threat Intelligence Investigation

Objectives:

- Analyze IP addresses and domains.
- Review indicators of compromise.
- Correlate threat intelligence with available logs.
- Identify suspicious activity.
- Document investigation findings.

### Project 6: Complete SOC Monitoring Dashboard

Objectives:

- Create a centralized SOC monitoring dashboard.
- Monitor authentication activity.
- Monitor network activity.
- Track security alerts.
- Visualize important security metrics.
- Document security incidents.

---

# 3. Resources

This section contains useful documentation and reference materials.

| Resource             | Purpose                                               |
| -------------------- | ----------------------------------------------------- |
| Splunk Documentation | Official Splunk documentation                         |
| Syslog Fundamentals  | Understanding Syslog and log collection               |
| SPL Cheat Sheet      | Quick reference for Splunk Search Processing Language |

Additional resources and references can be added as the learning progresses.

---

# 4. Progress Tracker

The `Progress-Tracker.md` file tracks completed topics, ongoing learning, practical labs and projects.

Learning progress will be updated as each module and project is completed.

---

# Learning Approach

My learning approach focuses on:

1. Understanding fundamentals before moving to advanced topics.
2. Writing organized technical notes.
3. Practising concepts through hands-on labs.
4. Building practical projects.
5. Understanding how tools are used in real SOC environments.
6. Developing troubleshooting and investigation skills.
7. Preparing for SOC Analyst L1/L2 responsibilities.
8. Documenting findings and explaining technical concepts clearly.
