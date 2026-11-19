# Project 01 - Splunk Data Flow Study

## 1. Project Information

| Item           | Details                                                           |
| -------------- | ----------------------------------------------------------------- |
| Module         | Module 01 - Introduction to Splunk                                |
| Project Number | 01                                                                |
| Project Name   | Splunk Data Flow Study                                            |
| Difficulty     | Beginner                                                          |
| Category       | Splunk Architecture and Data Ingestion                            |
| Tools          | Splunk Enterprise, Splunk Universal Forwarder, Ubuntu, Kali Linux |
| Status         | Not Started                                                       |

---

## 2. Project Overview

In this project, we will study and document how log data moves from a source system to Splunk.

We will use our home lab environment to understand the basic data flow between a Universal Forwarder and a Splunk Enterprise Indexer.

The project will use the following setup:

- Kali Linux generates sample log records.
- Splunk Universal Forwarder monitors the sample log file.
- Ubuntu runs Splunk Enterprise and receives the forwarded events.
- Splunk Web is used to search and verify the received events.

This project brings together the concepts covered in Module 01 and demonstrates the basic working architecture of a Splunk deployment.

---

## 3. Project Objectives

The objectives are to:

- Identify the source system generating log data.
- Understand how a log file is monitored by a Universal Forwarder.
- Understand how the Forwarder communicates with the Indexer.
- Identify the role of `inputs.conf`.
- Identify the role of `outputs.conf`.
- Understand where received events are indexed.
- Verify that forwarded events are searchable in Splunk.
- Document the complete data flow.

---

## 4. Project Architecture

The project uses the following architecture:

```text
                 KALI LINUX VM
                 Data Source
                      |
                      |
             Sample Log File
                      |
                      v
             SPLUNK UNIVERSAL
                 FORWARDER
                      |
                      |
                TCP PORT 9997
                      |
                      v
                 UBUNTU VM
              SPLUNK ENTERPRISE
                      |
             +--------+--------+
             |                 |
             v                 v
          INDEXER          SEARCH HEAD
             |                 |
             |                 |
        Store Events       Run SPL Searches
             |                 |
             +--------+--------+
                      |
                      v
                 SOC ANALYST
```

**Architecture note:**

In this home lab, Indexer and Search Head responsibilities are handled by the same Splunk Enterprise installation on Ubuntu.

The diagram separates these responsibilities to explain their logical roles. They are not separate machines in this project.

---

## 5. Project Environment

| Component       | Role                               | IP Address       |
| --------------- | ---------------------------------- | ---------------- |
| Windows 11 Host | Access Splunk Web                  | 192.168.56.1     |
| Ubuntu VM       | Splunk Enterprise / Indexer        | 192.168.56.101   |
| Kali Linux VM   | Universal Forwarder / Data Source  | Record actual IP |
| Receiving Port  | Forwarder-to-Indexer communication | TCP 9997         |

**Important:**

Verify the actual IP addresses before using them.

The Ubuntu and Kali VMs should communicate through the VirtualBox Host-Only Network.

Do not expose Splunk receiving port 9997 to the public internet.

---

## 6. Prerequisites

Before starting this project, complete:

- Lab 01 - Explore Splunk Web.
- Lab 02 - Identify Different Log Types.
- Lab 03 - Splunk Architecture Study.
- Lab 04 - Forwarder to Indexer.

The following should be available:

- Splunk Enterprise installed on Ubuntu.
- Splunk Enterprise running.
- Receiving port 9997 enabled.
- A dedicated index named `lab_forwarder`.
- Universal Forwarder installed on Kali.
- A sample log file created on Kali.
- `inputs.conf` configured.
- `outputs.conf` configured.
- Forwarder connected to the Indexer.

If you have not configured the Forwarder yet, complete Lab 04 before starting the implementation portion of this project.

---

## 7. Project Implementation

### Step 1 - Identify the Data Source

The first step is to identify where the events originate.

For this project, the data source is a sample authentication log file on Kali Linux.

File path:

```text
/tmp/splunk-lab/authentication.log
```

Run the following command on Kali:

```bash
cat /tmp/splunk-lab/authentication.log
```

Example records:

```text
2026-11-19 10:50:00 LAB_EVENT type=authentication_failure user=sample_admin src_ip=192.0.2.10
2026-11-19 10:50:01 LAB_EVENT type=authentication_success user=sample_user src_ip=192.0.2.20
```

Your actual timestamps will differ.

These are simulated records for the project, not real authentication events.

Record the following:

| Item                    | Observation |
| ----------------------- | ----------- |
| Source system           |             |
| Hostname                |             |
| Log file path           |             |
| Number of sample events |             |
| Type of sample events   |             |

### Step 2 - Understand the Input Configuration

The Universal Forwarder needs to know which file it should monitor.

Open the configuration file on Kali:

```bash
sudo cat /opt/splunkforwarder/etc/system/local/inputs.conf
```

Expected configuration:

```ini
[monitor:///tmp/splunk-lab/authentication.log]
disabled = 0
index = lab_forwarder
sourcetype = lab_auth
```

Record the following:

| Setting      | Value                                | Purpose                         |
| ------------ | ------------------------------------ | ------------------------------- |
| Monitor path | `/tmp/splunk-lab/authentication.log` | Identifies the file             |
| Disabled     | `0`                                  | Enables the input               |
| Index        | `lab_forwarder`                      | Specifies the destination index |
| Sourcetype   | `lab_auth`                           | Identifies the event format     |

**Project observation:**

The Universal Forwarder is configured to monitor the sample authentication log file.

### Step 3 - Understand the Output Configuration

The next step is to identify where the Universal Forwarder sends the collected data.

On Kali, run:

```bash
sudo cat /opt/splunkforwarder/etc/system/local/outputs.conf
```

Expected configuration:

```ini
[tcpout]
defaultGroup = lab_indexers

[tcpout:lab_indexers]
server = 192.168.56.101:9997
```

Replace the example IP address with your actual Ubuntu Indexer address if necessary.

Record the following:

| Setting          | Value          | Purpose                              |
| ---------------- | -------------- | ------------------------------------ |
| Forwarding group | `lab_indexers` | Identifies the forwarding group      |
| Destination      | Ubuntu IP      | Identifies the receiving server      |
| Receiving port   | `9997`         | Identifies the Splunk receiving port |

**Project observation:**

The Universal Forwarder uses `outputs.conf` to identify the destination that receives its forwarded data.

### Step 4 - Verify the Receiving Configuration

Open Splunk Web on Ubuntu.

Navigate to:

```text
Settings → Forwarding and receiving → Configure receiving
```

Verify that receiving port `9997` is configured.

On Ubuntu, run:

```bash
sudo ss -lntp | grep ':9997'
```

Record:

| Item                      | Observation |
| ------------------------- | ----------- |
| Receiving port configured |             |
| Port 9997 listening       |             |
| Indexer IP address        |             |

### Step 5 - Verify Forwarder Connectivity

On Kali, run:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Authenticate with the Forwarder administrator credentials if requested.

Identify whether the configured Indexer appears under active or inactive forwarding servers.

Record:

| Item                   | Observation |
| ---------------------- | ----------- |
| Forwarder status       |             |
| Configured destination |             |
| Active connection      |             |
| Error messages, if any |             |

**Expected result:**

The Universal Forwarder should show an active connection to the configured receiving destination.

An active connection confirms connectivity, but event ingestion must still be verified separately.

### Step 6 - Verify the Forwarded Events

Open Splunk Web on Ubuntu.

Navigate to:

```text
Apps → Search & Reporting
```

Run:

```spl
index=lab_forwarder
```

Select a time range that includes when the sample events were generated.

Then run:

```spl
index=lab_forwarder sourcetype=lab_auth
```

Inspect the returned events.

Expected sample records include:

```text
type=authentication_failure user=sample_admin src_ip=192.0.2.10
```

And:

```text
type=authentication_success user=sample_user src_ip=192.0.2.20
```

**Expected observation:**

The events generated on Kali should be available for searching in Splunk on Ubuntu.

If no events appear, use the troubleshooting section before proceeding.

### Step 7 - Inspect Event Metadata

Select one of the returned events.

Inspect the following fields:

| Field        | Expected Information                  |
| ------------ | ------------------------------------- |
| `_time`      | Event timestamp interpreted by Splunk |
| `host`       | Host associated with the event        |
| `source`     | Monitored log file path               |
| `sourcetype` | `lab_auth`                            |
| `index`      | `lab_forwarder`                       |
| `_raw`       | Original event text                   |

Use the following SPL search:

```spl
index=lab_forwarder sourcetype=lab_auth
| table _time host source sourcetype index _raw
```

Record the actual values:

| Field        | Actual Observation |
| ------------ | ------------------ |
| `_time`      |                    |
| `host`       |                    |
| `source`     |                    |
| `sourcetype` |                    |
| `index`      |                    |
| `_raw`       |                    |

**Important:**

The actual `host` value may depend on the Forwarder's configuration.

The `_time` field represents the event timestamp interpreted by Splunk. It is not necessarily the exact time the event was generated if timestamps are missing, delayed, or interpreted differently.

### Step 8 - Document the Complete Data Flow

Based on your implementation, document the data flow in the correct order.

| Sequence | Component            | Activity                                                |
| -------- | -------------------- | ------------------------------------------------------- |
| 1        | Kali Linux           | Generates sample log events                             |
| 2        | Sample log file      | Stores the generated records                            |
| 3        | Universal Forwarder  | Monitors the configured file                            |
| 4        | `inputs.conf`        | Defines the monitored input and metadata                |
| 5        | `outputs.conf`       | Defines the forwarding destination                      |
| 6        | TCP 9997             | Carries forwarded data to the receiving Splunk instance |
| 7        | Splunk Enterprise    | Receives and processes incoming data                    |
| 8        | Indexer function     | Indexes and stores the events                           |
| 9        | Search Head function | Executes search requests                                |
| 10       | SOC Analyst          | Searches and reviews the events                         |

---

## 8. Project Validation

Complete the following validation checks.

### Validation 1 - Source File Exists

On Kali:

```bash
ls -l /tmp/splunk-lab/authentication.log
```

Expected result:

The sample log file should exist.

### Validation 2 - Forwarder Is Running

On Kali:

```bash
sudo /opt/splunkforwarder/bin/splunk status
```

Expected result:

The Universal Forwarder should be running.

### Validation 3 - Forwarder Has an Active Destination

On Kali:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Expected result:

The configured receiving server should appear as active.

### Validation 4 - Indexer Is Listening

On Ubuntu:

```bash
sudo ss -lntp | grep ':9997'
```

Expected result:

The receiving port should be listening.

### Validation 5 - Events Are Searchable

In Splunk Web:

```spl
index=lab_forwarder sourcetype=lab_auth
```

Expected result:

The sample events should appear in the search results.

### Validation Summary

| Validation                       | Expected Result | Actual Result |
| -------------------------------- | --------------- | ------------- |
| Source file exists               | Yes             |               |
| Forwarder running                | Yes             |               |
| Forwarding destination active    | Yes             |               |
| Indexer receiving port listening | Yes             |               |
| Events indexed                   | Yes             |               |
| Events searchable                | Yes             |               |

---

## 9. Troubleshooting

### Problem 1 - Forwarder Is Not Running

Check:

```bash
sudo /opt/splunkforwarder/bin/splunk status
```

Start it if necessary:

```bash
sudo /opt/splunkforwarder/bin/splunk start
```

### Problem 2 - Forwarder Cannot Connect to the Indexer

Check the Ubuntu IP address:

```bash
ip -br addr
```

Check communication from Kali:

```bash
ping -c 4 UBUNTU_INDEXER_IP
```

Check the receiving port:

```bash
nc -vz UBUNTU_INDEXER_IP 9997
```

Replace `UBUNTU_INDEXER_IP` with the actual Ubuntu host-only IP address.

### Problem 3 - Forwarder Connection Is Inactive

Review:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Inspect the Forwarder log:

```bash
sudo tail -n 50 /opt/splunkforwarder/var/log/splunk/splunkd.log
```

Look for forwarding and connection-related messages.

### Problem 4 - No Events Appear in Splunk

Check whether the input file exists:

```bash
ls -l /tmp/splunk-lab/authentication.log
```

Verify the input configuration:

```bash
sudo cat /opt/splunkforwarder/etc/system/local/inputs.conf
```

Verify the output configuration:

```bash
sudo cat /opt/splunkforwarder/etc/system/local/outputs.conf
```

Confirm that:

- The monitored file path is correct.
- The input is enabled.
- The destination is correct.
- Receiving port 9997 is available.
- The target index exists.
- The time range includes the event timestamps.

### Problem 5 - Configuration Changes Are Not Applied

Restart the Universal Forwarder:

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

Then verify the configuration and connection again.

---

## 10. Project Deliverables

Save the following items as project evidence:

| No. | Deliverable             | Description                                 |
| --- | ----------------------- | ------------------------------------------- |
| 1   | Source Log Evidence     | Screenshot or output of the sample log file |
| 2   | Input Configuration     | `inputs.conf` contents                      |
| 3   | Output Configuration    | `outputs.conf` contents                     |
| 4   | Forwarder Status        | Universal Forwarder service status          |
| 5   | Receiving Configuration | Indexer receiving port evidence             |
| 6   | Connectivity Evidence   | Active forwarding destination               |
| 7   | Splunk Search Results   | Forwarded events displayed in Splunk        |
| 8   | Metadata Evidence       | Event fields displayed in Splunk            |
| 9   | Data Flow Documentation | Completed data flow table                   |
| 10  | Project Summary         | Findings and observations                   |

Do not upload credentials, passwords, authentication tokens, private keys, or other sensitive information to GitHub.

---

## 11. Final Project Report

Complete this section after implementation.

### 11.1 Environment Details

| Item                        | Actual Value |
| --------------------------- | ------------ |
| Source system               |              |
| Source system IP            |              |
| Indexer system              |              |
| Indexer IP                  |              |
| Universal Forwarder version |              |
| Splunk Enterprise version   |              |
| Receiving port              |              |
| Destination index           |              |

### 11.2 Implementation Summary

Write a short explanation of what you configured.

```text
Write your implementation summary here.
```

### 11.3 Results

| Activity                          | Result |
| --------------------------------- | ------ |
| Sample log file created           |        |
| Universal Forwarder installed     |        |
| Input configured                  |        |
| Output configured                 |        |
| Forwarding connection established |        |
| Events received by Splunk         |        |
| Events searchable                 |        |

### 11.4 Challenges Faced

Document any problems you encountered and how you resolved them.

```text
Problem:

Root Cause:

Resolution:
```

### 11.5 Final Conclusion

Write a short summary of what you learned from this project.

```text
Write your project conclusion here.
```

---

## 12. Project Completion Checklist

- [ ] Reviewed the project architecture.
- [ ] Identified the source system.
- [ ] Created and inspected the sample log file.
- [ ] Verified `inputs.conf`.
- [ ] Verified `outputs.conf`.
- [ ] Enabled Indexer receiving.
- [ ] Verified Forwarder connectivity.
- [ ] Confirmed the Forwarder was running.
- [ ] Searched for forwarded events.
- [ ] Inspected event metadata.
- [ ] Documented the complete data flow.
- [ ] Captured project evidence.
- [ ] Completed the final project report.

---

## 13. Project Conclusion

This project demonstrates the basic flow of data from a source system to Splunk.

We generated sample events on Kali Linux, configured a Universal Forwarder to monitor the log file, and configured the forwarding destination to send data to Splunk Enterprise running on Ubuntu.

The Indexer received and stored the events, and we verified their availability through Splunk Web.

This project provides practical experience with the basic components involved in Splunk data collection and ingestion.
