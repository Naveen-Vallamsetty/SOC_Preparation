# Lab 03 - Splunk Architecture Study

## 1. Lab Information

| Item       | Details                            |
| ---------- | ---------------------------------- |
| Module     | Module 01 - Introduction to Splunk |
| Lab Number | 03                                 |
| Difficulty | Beginner                           |
| Category   | Splunk Architecture                |
| Tool       | Splunk Enterprise, Ubuntu          |
| Status     | Not Started                        |

---

## 2. Objective

The objective of this lab is to understand the architecture of Splunk Enterprise and the roles of its major components.

By completing this lab, you will:

- Identify the main components of Splunk architecture.
- Understand the responsibilities of Forwarders, Indexers, and Search Heads.
- Understand the difference between Universal Forwarder and Heavy Forwarder.
- Understand how data travels from a source system to Splunk.
- Understand the basic data processing stages.
- Identify the architecture used by your local Splunk installation.
- Understand the difference between standalone and distributed deployments.

---

## 3. Lab Environment

| Component                    | Purpose                                  |
| ---------------------------- | ---------------------------------------- |
| Windows 11 Host              | Access Splunk Web                        |
| Ubuntu VM                    | Run Splunk Enterprise                    |
| Splunk Enterprise            | Study the Splunk architecture            |
| VirtualBox Host-Only Network | Communication between Windows and Ubuntu |

**Note:** This lab uses your existing Ubuntu Splunk installation. You do not need to install additional Splunk components or run multiple virtual machines.

---

## 4. Prerequisites

Before starting:

- Complete Lab 01 - Explore Splunk Web.
- Complete Lab 02 - Identify Different Log Types.
- Ensure Splunk Enterprise is installed on Ubuntu.
- Ensure Splunk Enterprise can start successfully.
- Confirm that Splunk Web is accessible from your Windows host.

---

## 5. Understanding Splunk Architecture

Splunk can be deployed as a single-instance installation or as a distributed deployment.

In a distributed deployment, different Splunk components perform different responsibilities.

The three major components are:

1. Forwarder
2. Indexer
3. Search Head

### 5.1 Forwarder

A Forwarder collects data from source systems and sends it to another Splunk instance for processing and indexing.

Examples of data sources:

- Linux servers.
- Windows servers.
- Application servers.
- Network devices.
- Security appliances.

There are two commonly used Forwarder types.

#### Universal Forwarder (UF)

The Universal Forwarder is a lightweight agent installed on source systems.

Its primary responsibilities include:

- Collecting data from configured sources.
- Monitoring log files.
- Collecting selected event data.
- Forwarding collected data to a configured Splunk destination.

A Universal Forwarder performs limited data processing compared with a Heavy Forwarder.

#### Heavy Forwarder (HF)

A Heavy Forwarder is a full Splunk Enterprise instance configured to forward data.

Depending on its configuration, it can perform additional processing before forwarding data.

For example:

- Parsing data.
- Filtering events.
- Routing events.
- Handling inputs that require additional processing.

**Important:** A Heavy Forwarder is not the same as a Universal Forwarder. The two have different resource requirements and capabilities.

### 5.2 Indexer

The Indexer receives data and processes it so that Splunk can store and search the events.

Its responsibilities include:

- Receiving forwarded data.
- Processing incoming events.
- Extracting or assigning relevant event metadata.
- Creating index structures.
- Storing indexed data.
- Responding to search requests.

The Indexer is a major component of Splunk data storage and search operations.

### 5.3 Search Head

The Search Head provides the interface and search capabilities used by analysts and Splunk users.

Its responsibilities include:

- Providing Splunk Web.
- Accepting search requests.
- Processing Search Processing Language (SPL) searches.
- Coordinating searches against indexers in distributed deployments.
- Presenting search results.
- Supporting reports, dashboards, and alerts.

In a distributed deployment, a Search Head can send search requests to multiple Indexers.

### 5.4 Additional Splunk Components and Roles

Larger environments may use additional Splunk components to manage and scale their deployments.

| Component / Role   | General Purpose                                                           |
| ------------------ | ------------------------------------------------------------------------- |
| Deployment Server  | Manages configuration deployment to supported Splunk instances            |
| License Manager    | Manages license usage and license-related information                     |
| Cluster Manager    | Coordinates an Indexer Cluster                                            |
| Monitoring Console | Monitors the health and performance of Splunk infrastructure              |
| Deployer           | Distributes apps and configuration bundles to Search Head Cluster members |

These components are not required in every Splunk deployment.

For this beginner lab, focus primarily on Forwarders, Indexers, and Search Heads.

---

## 6. Splunk Architecture Diagram

Review the following simplified distributed architecture.

```text
                DATA SOURCES
                     |
        +------------+------------+
        |            |            |
     Linux        Windows      Network
     Server       Server       Devices
        |            |            |
        +------------+------------+
                     |
                     v
              FORWARDERS
          +-------------------+
          | Universal         |
          | Forwarder         |
          |                   |
          | Heavy Forwarder   |
          +-------------------+
                     |
                     v
                 INDEXERS
          +-------------------+
          | Receive Data      |
          | Process Events    |
          | Index Data        |
          | Store Data        |
          +-------------------+
                     |
                     |
                     v
               SEARCH HEAD
          +-------------------+
          | SPL Queries       |
          | Search Results    |
          | Reports           |
          | Dashboards        |
          +-------------------+
                     |
                     v
                 ANALYST
```

This diagram represents a simplified distributed architecture.

In a real deployment, multiple Forwarders, Indexers, and Search Heads may exist. The exact design depends on data volume, availability, performance, and organizational requirements.

**Important:** The diagram illustrates the logical flow of data and searches. Actual search communication between Search Heads and Indexers is separate from the forwarder-to-indexer ingestion path.

---

## 7. Lab Tasks

### Task 1 - Understand the Responsibilities of Each Component

Review Sections 5 and 6.

Create the following table in your lab notes.

| Component           | Main Responsibility | Example |
| ------------------- | ------------------- | ------- |
| Universal Forwarder |                     |         |
| Heavy Forwarder     |                     |         |
| Indexer             |                     |         |
| Search Head         |                     |         |

**Expected observation:**

| Component           | Main Responsibility                                              | Example                                |
| ------------------- | ---------------------------------------------------------------- | -------------------------------------- |
| Universal Forwarder | Collects and forwards data                                       | Reads Linux authentication logs        |
| Heavy Forwarder     | Collects and can perform additional processing before forwarding | Filters or routes selected events      |
| Indexer             | Processes, indexes, and stores events                            | Stores searchable Linux log events     |
| Search Head         | Provides search and analysis capabilities                        | Runs SPL searches and displays results |

---

### Task 2 - Identify Your Local Splunk Deployment

You have installed Splunk Enterprise on your Ubuntu VM.

In this lab, you will identify whether your installation is operating as a standalone Splunk instance.

**Step 1: Start your Ubuntu VM.**

Open the terminal.

**Step 2: Check Splunk status.**

Run:

```bash
sudo /opt/splunk/bin/splunk status
```

Expected result:

Splunk should report that its processes are running if the service has started successfully.

If Splunk is not running, start it using:

```bash
sudo /opt/splunk/bin/splunk start
```

Follow any startup instructions displayed in the terminal.

**Step 3: Identify the Splunk installation directory.**

Run:

```bash
ls -ld /opt/splunk
```

Expected result:

The command should display information about the Splunk installation directory.

**Step 4: Check the Splunk version.**

Run:

```bash
sudo /opt/splunk/bin/splunk version
```

Record the version displayed.

**Step 5: Access Splunk Web.**

Open a browser on your Windows host and navigate to:

```text
http://192.168.56.101:8000
```

Replace the IP address if your Ubuntu VM has a different host-only IP address.

Log in to Splunk Web.

**Expected observation:**

You should be able to access the Splunk interface hosted by your Ubuntu VM.

In your current setup, the same Splunk Enterprise installation provides the main Splunk services, including indexing and search capabilities.

This is a standalone deployment rather than a separate Forwarder-to-Indexer-to-Search-Head distributed deployment.

**Record your observations:**

| Item                     | Actual Observation |
| ------------------------ | ------------------ |
| Splunk installation path |                    |
| Splunk version           |                    |
| Splunk service status    |                    |
| Ubuntu VM IP address     |                    |
| Splunk Web accessible?   |                    |
| Deployment type          |                    |

---

### Task 3 - Identify Splunk Network Ports

Splunk uses different network ports for different services.

Review the following common defaults:

| Port | Protocol | Purpose                     |
| ---- | -------- | --------------------------- |
| 8000 | TCP      | Splunk Web                  |
| 8089 | TCP      | Splunk management interface |
| 9997 | TCP      | Splunk-to-Splunk receiving  |
| 8088 | TCP      | HTTP Event Collector (HEC)  |

These are common default ports. They may be changed depending on the deployment configuration.

**Step 1: Check listening ports on Ubuntu.**

Run:

```bash
sudo ss -lntp
```

This displays TCP ports currently in the listening state.

**Step 2: Filter for common Splunk ports.**

Run:

```bash
sudo ss -lntp | grep -E ':8000|:8089|:9997|:8088'
```

**Step 3: Observe which ports are listening.**

Complete the table:

| Port | Expected Purpose     | Listening? |
| ---- | -------------------- | ---------- |
| 8000 | Splunk Web           |            |
| 8089 | Management interface |            |
| 9997 | Forwarder receiving  |            |
| 8088 | HTTP Event Collector |            |

**Expected observation:**

In your local installation, ports 8000 and 8089 are commonly expected to be listening when Splunk is running.

Ports 9997 and 8088 may not be listening because receiving and HEC services are not necessarily enabled by default.

Do not assume that every standard Splunk port must be open.

---

### Task 4 - Understand the Splunk Data Flow

Study how information travels from a source system to Splunk.

Consider the following example:

A Linux server generates an SSH authentication failure.

The event is written to a Linux authentication log.

A Universal Forwarder installed on the Linux server monitors the configured log file.

The Forwarder sends the data to an Indexer.

The Indexer processes and indexes the event.

A SOC analyst searches for the event using a Search Head.

**Simplified data flow:**

```text
Linux Server
     |
     | Generates authentication event
     v
/var/log/auth.log
     |
     | Monitored by Universal Forwarder
     v
Universal Forwarder
     |
     | Sends event
     v
Indexer
     |
     | Processes and indexes event
     v
Search Head
     |
     | Analyst runs SPL search
     v
Search Results
```

**Your activity:**

1. Identify where the event originates.
2. Identify which component collects the event.
3. Identify which component receives and indexes the event.
4. Identify which component allows the analyst to search for the event.
5. Write the data flow in your own words.

**Expected observation:**

The event originates on the Linux system.

The Forwarder collects and sends it.

The Indexer processes and stores it.

The Search Head provides the search interface and coordinates the search.

In your standalone deployment, these responsibilities are provided by the same Splunk Enterprise instance.

---

### Task 5 - Understand Splunk Data Processing Stages

Splunk processes incoming data through different stages.

For this lab, focus on three main stages.

#### 1. Input Stage

Splunk identifies and reads data from configured inputs.

Examples:

- Monitoring a log file.
- Receiving forwarded data.
- Collecting events through HEC.

#### 2. Parsing Stage

Splunk processes incoming event data.

Depending on the deployment and configuration, this stage can involve:

- Identifying event boundaries.
- Extracting timestamps.
- Assigning metadata.
- Applying configured transformations.

Heavy Forwarders and Indexers can perform parsing-related processing depending on configuration.

#### 3. Indexing Stage

Splunk prepares and stores processed events in its indexing system.

This makes events searchable through SPL.

**Your activity:**

Review this sample event:

```text
Nov 19 10:15:22 ubuntu sshd[1234]: Failed password for invalid user admin from 192.168.56.105 port 54321 ssh2
```

Identify the following:

| Item                  | Your Observation |
| --------------------- | ---------------- |
| Original source       |                  |
| Application / process |                  |
| Timestamp             |                  |
| Event message         |                  |
| Source IP             |                  |
| Event type            |                  |

**Expected observation:**

| Item                  | Expected Value           |
| --------------------- | ------------------------ |
| Original source       | Linux authentication log |
| Application / process | sshd                     |
| Timestamp             | Nov 19 10:15:22          |
| Event message         | Failed password          |
| Source IP             | 192.168.56.105           |
| Event type            | Authentication failure   |

The sample event demonstrates the type of information Splunk can process and make searchable.

---

### Task 6 - Understand the Basic Configuration Files

Splunk uses configuration files to control its behavior.

Review the following important files:

| Configuration File | General Purpose                                    |
| ------------------ | -------------------------------------------------- |
| `inputs.conf`      | Defines data inputs                                |
| `outputs.conf`     | Defines forwarding destinations                    |
| `server.conf`      | Controls core Splunk instance settings             |
| `props.conf`       | Defines data processing and field-related behavior |
| `transforms.conf`  | Defines reusable data transformation rules         |

For this lab, understand the basic relationship:

- `inputs.conf` determines what data to collect.
- `outputs.conf` determines where forwarded data should be sent.
- `server.conf` contains core instance configuration.
- `props.conf` and `transforms.conf` support data processing and transformations.

**Note:** Configuration files may exist at different locations and configuration layers. Effective settings can depend on configuration precedence.

Do not modify any production configuration as part of this introductory lab.

**Your activity:**

Complete the table:

| Configuration File | What Does It Control? |
| ------------------ | --------------------- |
| `inputs.conf`      |                       |
| `outputs.conf`     |                       |
| `server.conf`      |                       |
| `props.conf`       |                       |
| `transforms.conf`  |                       |

**Expected observation:**

You should be able to explain the basic purpose of each configuration file and identify which files are associated with data collection, forwarding, and processing.

---

### Task 7 - Compare Standalone and Distributed Deployments

Review the following comparison.

| Feature          | Standalone Deployment                | Distributed Deployment          |
| ---------------- | ------------------------------------ | ------------------------------- |
| Splunk instances | One main instance                    | Multiple instances              |
| Indexing         | Performed by the standalone instance | Performed by dedicated Indexers |
| Searching        | Performed by the same instance       | Coordinated by Search Heads     |
| Forwarders       | Optional                             | Commonly used                   |
| Infrastructure   | Simpler                              | More complex                    |
| Typical Use      | Learning and smaller environments    | Larger enterprise environments  |

**Your activity:**

Identify which architecture you currently use.

Record your answer:

| Item                                    | Your Observation |
| --------------------------------------- | ---------------- |
| Current deployment                      |                  |
| Separate Universal Forwarder installed? |                  |
| Separate Indexer installed?             |                  |
| Separate Search Head installed?         |                  |
| Main Splunk services hosted on Ubuntu?  |                  |

**Expected observation:**

Your current environment uses a standalone Splunk Enterprise installation on Ubuntu.

The Indexing and Search Head responsibilities are handled by the same Splunk instance.

You have not yet configured a separate Universal Forwarder, dedicated Indexer, or dedicated Search Head.

---

## 8. Final Observation Worksheet

Complete this table after finishing the lab.

| No. | Activity                      | Observation |
| --- | ----------------------------- | ----------- |
| 1   | Splunk service status         |             |
| 2   | Splunk version                |             |
| 3   | Splunk installation directory |             |
| 4   | Splunk Web availability       |             |
| 5   | Listening network ports       |             |
| 6   | Current deployment type       |             |
| 7   | Forwarder responsibility      |             |
| 8   | Indexer responsibility        |             |
| 9   | Search Head responsibility    |             |
| 10  | Data processing stages        |             |

---

## 9. Troubleshooting

### Issue 1 - Splunk service is not running

Check:

```bash
sudo /opt/splunk/bin/splunk status
```

Start Splunk:

```bash
sudo /opt/splunk/bin/splunk start
```

### Issue 2 - Splunk Web is not accessible

Check the Ubuntu IP address:

```bash
ip -br addr
```

Check whether port 8000 is listening:

```bash
sudo ss -lntp | grep ':8000'
```

Verify that you are using the correct Ubuntu host-only IP address.

### Issue 3 - A particular Splunk port is not listening

Not all Splunk ports are enabled automatically.

Check whether the corresponding service is configured before treating the absence of a listening port as a problem.

### Issue 4 - Splunk command returns an error

Verify the command path:

```bash
ls -l /opt/splunk/bin/splunk
```

Run Splunk commands with the required permissions.

---

## 10. Evidence to Capture

Capture the following screenshots or notes:

1. Output of the Splunk status command.
2. Output of the Splunk version command.
3. Splunk Web interface.
4. Listening network ports.
5. Completed architecture component table.
6. Completed deployment comparison.
7. Completed final observation worksheet.

Do not expose credentials, authentication tokens, private keys, or sensitive configuration values in GitHub screenshots.

---

## 11. Expected Learning Outcomes

After completing this lab, you should be able to:

- Explain the purpose of Forwarders, Indexers, and Search Heads.
- Distinguish Universal Forwarders from Heavy Forwarders.
- Explain the basic data flow in Splunk.
- Understand the input, parsing, and indexing stages.
- Identify common Splunk configuration files.
- Identify your current Splunk deployment type.
- Understand the difference between standalone and distributed deployments.

---

## 12. Completion Checklist

- [ ] Reviewed the major Splunk architecture components.
- [ ] Understood Universal Forwarder responsibilities.
- [ ] Understood Heavy Forwarder responsibilities.
- [ ] Understood Indexer responsibilities.
- [ ] Understood Search Head responsibilities.
- [ ] Reviewed the Splunk architecture diagram.
- [ ] Checked Splunk service status.
- [ ] Identified the Splunk version.
- [ ] Checked the listening network ports.
- [ ] Understood the Splunk data flow.
- [ ] Reviewed the data processing stages.
- [ ] Reviewed the basic Splunk configuration files.
- [ ] Identified the current deployment type.
- [ ] Completed the observation worksheet.
- [ ] Captured available evidence.

---

## 13. Lab Conclusion

In this lab, we studied the major components of Splunk architecture and understood how data moves from source systems to Splunk.

We also examined the responsibilities of Forwarders, Indexers, and Search Heads, reviewed the basic data processing stages, and identified the architecture used by our local Splunk installation.

Understanding Splunk architecture is important because SOC analysts need to know where data originates, how it reaches Splunk, and which components are responsible for making it searchable.
