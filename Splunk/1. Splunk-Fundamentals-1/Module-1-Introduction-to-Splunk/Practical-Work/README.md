# Module 01 – Introduction to Splunk: Practical Work

## Overview

This folder contains the hands-on labs, a small module-specific project, and technical diagrams related to **Module 01 – Introduction to Splunk**.

The objective is to understand Splunk architecture, components, data collection methods, configuration files, and basic data flow through practical exercises.

All practical activities in this folder are limited to the learning objectives of Module 1.

---

## Folder Structure

```text
Practical-Work/
│
├── README.md
│
├── Labs/
│   ├── Lab-01-Explore-Splunk-Web.md
│   ├── Lab-02-Identify-Different-Log-Types.md
│   ├── Lab-03-Splunk-Architecture-Study.md
│   └── Lab-04-Forwarder-to-Indexer.md
│
├── Projects/
│   └── Project-01-Splunk-Data-Flow-Study.md
│
└── Diagrams/
    ├── Splunk-Architecture.mmd
    ├── Data-Ingestion-Flow.mmd
    └── Linux-Log-Troubleshooting-Flow.mmd
```

---

# 1. Labs

The labs provide step-by-step exercises to practice the concepts covered in Module 1.

| Lab    | Title                        | Objective                                                                         |
| ------ | ---------------------------- | --------------------------------------------------------------------------------- |
| Lab 01 | Explore Splunk Web           | Explore the Splunk interface and identify important sections.                     |
| Lab 02 | Identify Different Log Types | Examine different log sources and understand their purposes.                      |
| Lab 03 | Splunk Architecture Study    | Understand the roles of Forwarders, Indexers, Search Heads, and other components. |
| Lab 04 | Forwarder to Indexer         | Understand and verify basic data forwarding from a Forwarder to an Indexer.       |

## Lab completion expectations

For every lab, document:

1. Objective.
2. Lab environment.
3. Prerequisites.
4. Steps performed.
5. Commands used, where applicable.
6. Observations.
7. Screenshots or evidence.
8. Learning outcomes.

Screenshots should demonstrate actual lab work. Do not include passwords, authentication tokens, private keys, or other sensitive information.

---

# 2. Module-Specific Project

## Project 01 – Splunk Data Flow Study

**File:** `Projects/Project-01-Splunk-Data-Flow-Study.md`

### Objective

Understand and document how machine-generated data moves from a source system into Splunk.

### Scope

The project focuses on:

- Data sources.
- Log generation.
- Universal Forwarder.
- Input configuration.
- Output configuration.
- Receiving configuration.
- Indexer.
- Index.
- Search Head.
- Splunk Web.

### Expected outcome

Document the data flow from a source log file to a searchable event in Splunk.

This is a small learning project for Module 1. It is not a complete SOC investigation project.

---

# 3. Diagrams

The diagrams provide visual explanations of the architecture and workflows covered in this module.

| Diagram                              | Purpose                                                                           |
| ------------------------------------ | --------------------------------------------------------------------------------- |
| `Splunk-Architecture.mmd`            | Shows the main components of Splunk architecture and their relationships.         |
| `Data-Ingestion-Flow.mmd`            | Shows how data moves from a source system through a Forwarder to an Indexer.      |
| `Linux-Log-Troubleshooting-Flow.mmd` | Shows the basic troubleshooting sequence when Linux logs are missing from Splunk. |

The diagrams use Mermaid syntax and can be maintained as `.mmd` files.

---

# 4. Lab Environment

The practical exercises will use the available home lab environment.

### Environment

- Windows 11 host.
- Oracle VirtualBox.
- Ubuntu virtual machine.
- Splunk Enterprise installed on Ubuntu.
- VirtualBox NAT network for internet access.
- Host-only network for private communication between Windows and Ubuntu.

### Basic architecture

```text
Windows 11 Host
      |
      | VirtualBox Host-Only Network
      |
      v
Ubuntu Virtual Machine
      |
      v
Splunk Enterprise
```

Additional virtual machines may be introduced in later modules when their resources and learning objectives are required.

### Resource considerations

The host system has limited memory and processing resources.

Therefore:

- Keep unnecessary virtual machines powered off.
- Use only the resources required for the current exercise.
- Monitor available disk space.
- Avoid downloading unnecessarily large datasets.
- Keep Splunk accessible only through the intended private lab network.
- Do not expose the Splunk management interface to the public internet.

---

# 5. Practical Documentation Standards

For consistent documentation, follow these guidelines.

### Commands

Include commands used during the exercise in Markdown code blocks.

Example:

```bash
sudo /opt/splunk/bin/splunk status
```

### Configuration

Document configuration examples and explain their purpose.

Example:

```ini
[monitor:///var/log/auth.log]
disabled = 0
index = linux_auth
sourcetype = linux_secure
```

Do not copy production credentials, tokens, private keys, or sensitive configuration values into this repository.

### Screenshots

Save screenshots only when they provide useful evidence of completed work.

Use descriptive filenames.

Example:

```text
Lab-01-Splunk-Web-Login.png
Lab-03-Splunk-Architecture.png
Lab-04-Forwarder-Connection.png
```

### Observations

Record what actually happened during the exercise.

Clearly distinguish:

- Expected behavior.
- Observed behavior.
- Errors encountered.
- Troubleshooting performed.
- Final result.

Do not claim that a task succeeded unless it was verified.

---

# 6. Completion Tracker

## Labs

- [ ] Lab 01 – Explore Splunk Web.
- [ ] Lab 02 – Identify Different Log Types.
- [ ] Lab 03 – Splunk Architecture Study.
- [ ] Lab 04 – Forwarder to Indexer.

## Project

- [ ] Project 01 – Splunk Data Flow Study.

## Diagrams

- [ ] Splunk Architecture.
- [ ] Data Ingestion Flow.
- [ ] Linux Log Troubleshooting Flow.

---

# 7. Expected Learning Outcomes

After completing the practical work, I should be able to:

- Navigate the Splunk Web interface.
- Identify common machine-generated log sources.
- Explain the purpose of major Splunk components.
- Describe the basic Splunk data ingestion process.
- Understand the roles of `inputs.conf` and `outputs.conf`.
- Identify the purpose of the Indexer receiving port.
- Explain how a Forwarder sends data to an Indexer.
- Understand common causes of missing data.
- Document practical findings clearly.

---

## Learning Principle

**Understand the concept → Perform the lab → Observe the result → Document the findings → Revise the learning.**
