# Module 01 – Introduction to Splunk

## Module Overview

This module introduces Splunk, its architecture, major components, data collection methods, and basic data flow.

The objective is to understand how machine-generated data moves from different systems into Splunk and becomes available for searching, monitoring, and investigation.

### Learning Objectives

By the end of this module, I should be able to:

- Explain what Splunk is and why organizations use it.
- Understand the difference between logs, events, and machine-generated data.
- Explain SIEM and how Splunk supports security monitoring.
- Identify the major components of Splunk architecture.
- Understand the responsibilities of Forwarders, Indexers, and Search Heads.
- Differentiate Universal Forwarders and Heavy Forwarders.
- Understand Splunk indexes and data ingestion.
- Identify common data collection methods.
- Understand the purpose of `inputs.conf` and `outputs.conf`.
- Explain common Splunk ports.
- Describe the end-to-end flow of data from a source system to Splunk Web.
- Perform basic troubleshooting when data is not appearing in Splunk.

---

# 1. Introduction to Splunk

## 1.1 What is Splunk?

Splunk is a platform used to collect, index, search, monitor, analyze, and visualize machine-generated data.

Machine-generated data is information automatically produced by computers, operating systems, applications, network devices, and other IT systems.

Examples include:

- Linux system logs.
- Windows Event Logs.
- Application logs.
- Firewall logs.
- Router and switch logs.
- VPN authentication logs.
- Web server access logs.
- Database logs.
- Security alerts.
- Cloud platform logs.

Splunk helps organizations collect data from different sources and analyze it through a centralized platform.

Instead of manually logging in to every server or device and checking individual log files, an analyst can search data from multiple systems using Splunk.

## 1.2 Why do organizations use Splunk?

Organizations generate large amounts of machine data every day.

Manually reviewing individual logs becomes difficult when an environment contains hundreds or thousands of systems.

Splunk helps organizations:

1. Collect logs from different systems.
2. Store and index machine-generated data.
3. Search for specific events.
4. Monitor infrastructure and applications.
5. Identify errors and unusual activities.
6. Investigate operational and security incidents.
7. Create reports, alerts, and dashboards.
8. Analyze events from multiple data sources.
9. Support troubleshooting and root-cause analysis.
10. Improve visibility across IT environments.

## 1.3 Splunk in a real-world environment

Consider an organization that has:

- Linux servers.
- Windows servers.
- Firewalls.
- Routers and switches.
- VPN gateways.
- Web applications.
- Endpoint security tools.
- Cloud infrastructure.

Each system generates logs describing its activities.

Splunk can collect these logs and make them searchable from a centralized interface.

For example, a SOC Analyst can investigate:

- Repeated failed login attempts.
- Successful logins from unusual IP addresses.
- Suspicious network connections.
- Firewall-denied traffic.
- Unexpected administrative activity.
- Changes to important system configurations.
- Possible malware-related events.

An infrastructure operations engineer can investigate:

- Application errors.
- Server availability.
- Network connectivity problems.
- Service failures.
- Disk space issues.
- Device communication problems.
- Unexpected system restarts.

Splunk can support both operational monitoring and security monitoring. The data sources and investigation objectives determine how it is used.

---

# 2. SIEM and Splunk

## 2.1 What is SIEM?

SIEM stands for **Security Information and Event Management**.

A SIEM solution collects and analyzes security-related information from different systems to help organizations detect, investigate, and respond to security incidents.

SIEM solutions commonly provide:

- Security log collection.
- Event analysis.
- Threat detection.
- Event correlation.
- Alert generation.
- Incident investigation.
- Security reporting.
- Security monitoring dashboards.

## 2.2 What is the purpose of SIEM?

Consider an organization with 500 employees.

Employees access different systems using:

- Windows accounts.
- Linux accounts.
- VPN connections.
- Web applications.
- Cloud services.

Security events are generated across these systems.

Without centralized monitoring, security analysts may need to check multiple systems individually.

A SIEM solution helps collect and analyze relevant security events in one place.

For example:

1. A user attempts to log in to a Linux server several times.
2. The authentication system records failed login events.
3. The VPN gateway records connection activity.
4. The SIEM receives events from both systems.
5. The analyst investigates the activity and checks whether the events are related.

## 2.3 Is Splunk a SIEM?

Splunk is a data platform with several capabilities.

Splunk Enterprise provides capabilities such as:

- Data collection.
- Indexing.
- Searching.
- Monitoring.
- Reporting.
- Dashboards.
- Data analysis.

**Splunk Enterprise Security (ES)** is a security-focused solution built on the Splunk platform. It adds SIEM capabilities such as security monitoring, correlation searches, risk-based alerting, and investigation workflows.

Understand the difference:

| Term                       | Meaning                                                                                       |
| -------------------------- | --------------------------------------------------------------------------------------------- |
| Splunk                     | Data platform and product family.                                                             |
| Splunk Enterprise          | Platform for collecting, indexing, searching, and analyzing data.                             |
| Splunk Enterprise Security | Security-focused solution that provides SIEM capabilities.                                    |
| SIEM                       | Security Information and Event Management; a category of security solutions and capabilities. |

**Important:** A basic Splunk Enterprise installation does not automatically include every Splunk Enterprise Security feature.

---

# 3. Logs, Events, and Machine-Generated Data

## 3.1 What is machine-generated data?

Machine-generated data is information automatically produced by computers, applications, operating systems, and network devices.

Examples:

| Source                 | Example Data                    |
| ---------------------- | ------------------------------- |
| Linux server           | SSH login attempts              |
| Windows server         | Windows Security Event Logs     |
| Firewall               | Allowed and denied connections  |
| Router                 | Interface status changes        |
| VPN gateway            | VPN login and connection events |
| Web server             | HTTP access logs                |
| Database               | Database connection logs        |
| Endpoint security tool | Malware detection alerts        |
| DNS server             | DNS query logs                  |
| DHCP server            | IP address assignment logs      |

## 3.2 What is a log?

A log is a record generated by a system, application, or device describing an activity, event, or condition.

Example Linux authentication log:

```text
Nov 18 10:15:22 server01 sshd[1234]: Failed password for invalid user admin from 192.168.1.50 port 52341 ssh2
```

This log contains information about:

- Date and time.
- Hostname.
- Service name.
- Process ID.
- Authentication result.
- Username.
- Source IP address.
- Source port.
- Protocol.

A single log record can provide useful information for operational troubleshooting or security investigation.

## 3.3 What is an event in Splunk?

An event is an individual record represented and processed by Splunk.

When a log record is ingested and processed by Splunk, it becomes searchable as an event.

Example:

```text
Nov 18 10:15:22 server01 sshd[1234]: Failed password for invalid user admin from 192.168.1.50 port 52341 ssh2
```

This individual record can be returned by a Splunk search.

Depending on the data format and configuration, Splunk may extract fields such as:

- User.
- Source IP.
- Destination IP.
- Process name.
- Action.
- Status.

Not every field is automatically available for every type of data.

## 3.4 Difference between log and event

| Log                                             | Event                                                  |
| ----------------------------------------------- | ------------------------------------------------------ |
| Record generated by a system or application.    | Individual record represented and processed by Splunk. |
| May exist in a file or external logging system. | Available for searching after ingestion and indexing.  |
| Example: An entry in `/var/log/auth.log`.       | The corresponding record returned by a Splunk search.  |

In everyday SOC discussions, people sometimes use the words log and event interchangeably. However, understanding the distinction is useful.

## 3.5 Common log sources

### Linux logs

Common Linux log files include:

| Log File                      | Purpose                                                                                         |
| ----------------------------- | ----------------------------------------------------------------------------------------------- |
| `/var/log/auth.log`           | Authentication and authorization-related messages on Debian-based systems.                      |
| `/var/log/secure`             | Authentication and security-related messages on some distributions, such as RHEL-based systems. |
| `/var/log/syslog`             | General system messages on some Debian-based systems.                                           |
| `/var/log/messages`           | General system messages on some distributions.                                                  |
| `/var/log/kern.log`           | Kernel-related messages on systems that use this file.                                          |
| `/var/log/apache2/access.log` | Apache web server access logs on common Debian-based installations.                             |
| `/var/log/apache2/error.log`  | Apache web server error logs on common Debian-based installations.                              |

Log file names and locations vary by Linux distribution and configuration.

Some systems use `systemd-journald` and may store logs in the system journal rather than conventional text files.

### Windows logs

Common Windows Event Log categories include:

| Log Category | Purpose                                                            |
| ------------ | ------------------------------------------------------------------ |
| Security     | Authentication, logon, account, and security-related activity.     |
| System       | Operating system and system component events.                      |
| Application  | Events generated by applications.                                  |
| PowerShell   | PowerShell activity, when appropriate logging is enabled.          |
| Sysmon       | Detailed system activity, when Sysmon is installed and configured. |

### Network and security device logs

Examples include:

- Firewall logs.
- Router logs.
- Switch logs.
- VPN authentication logs.
- IDS/IPS alerts.
- DNS query logs.
- DHCP logs.
- Proxy logs.
- Wireless Access Point logs.

### Application logs

Examples include:

- Web application logs.
- Database logs.
- Authentication service logs.
- API logs.
- Application error logs.
- Transaction logs.

---

# 4. Splunk Architecture

Splunk uses different components to collect, process, store, and search machine-generated data.

The main components to understand are:

1. Forwarder.
2. Indexer.
3. Search Head.
4. Deployment Server.
5. License Manager.
6. Cluster Manager.

The Forwarder, Indexer, and Search Head are the most important components when learning the basic data flow.

## 4.1 Basic Splunk architecture

```text
+-----------------------+
|      Data Sources     |
|                       |
| Linux / Windows       |
| Firewalls / Routers   |
| Applications / VPN    |
+-----------+-----------+
            |
            v
+-----------------------+
|       Forwarder       |
|                       |
| Collects and sends    |
| configured data       |
+-----------+-----------+
            |
            v
+-----------------------+
|        Indexer        |
|                       |
| Processes and stores  |
| indexed events        |
+-----------+-----------+
            |
            v
+-----------------------+
|      Search Head      |
|                       |
| Coordinates searches  |
+-----------+-----------+
            |
            v
+-----------------------+
|       Splunk Web      |
|                       |
| Search and analysis   |
| Dashboards and reports|
+-----------+-----------+
            |
            v
+-----------------------+
|        Analyst        |
+-----------------------+
```

This diagram shows a simplified architecture. Real deployments may contain multiple Forwarders, Indexers, Search Heads, and intermediate components.

## 4.2 Forwarder

A Forwarder collects data from a source system and sends it to another Splunk component, usually an Indexer or an intermediate Forwarder.

Examples of systems where a Forwarder may be installed:

- Linux servers.
- Windows servers.
- Application servers.
- Virtual machines.

Responsibilities depend on the type and configuration of the Forwarder.

A Forwarder may:

- Monitor configured log files.
- Collect configured event data.
- Read data from supported inputs.
- Send collected data to a receiving Splunk instance.

## 4.3 Universal Forwarder (UF)

The Universal Forwarder is a lightweight Splunk agent installed on systems where data needs to be collected.

Responsibilities include:

- Monitoring configured log files.
- Collecting configured event data.
- Forwarding collected data to a receiving Splunk instance.
- Using relatively few system resources compared with a full Splunk Enterprise installation.

Example:

A Universal Forwarder is installed on a Linux server and monitors:

```text
/var/log/auth.log
```

It forwards the collected data to a Splunk Indexer.

The Indexer processes and stores the data so that it can be searched.

**Important:**

A Universal Forwarder does not provide the full Splunk Web search interface and does not perform the same indexing role as an Indexer.

## 4.4 Heavy Forwarder (HF)

A Heavy Forwarder is a full Splunk Enterprise instance configured to forward data.

It can perform additional processing before forwarding data.

Capabilities include:

- Parsing data.
- Filtering events.
- Routing data.
- Applying transformations.
- Forwarding data to downstream Splunk components.

A Heavy Forwarder generally uses more system resources than a Universal Forwarder.

## 4.5 Universal Forwarder vs Heavy Forwarder

| Feature               | Universal Forwarder      | Heavy Forwarder                                           |
| --------------------- | ------------------------ | --------------------------------------------------------- |
| Installation          | Lightweight agent        | Full Splunk Enterprise instance configured for forwarding |
| Resource usage        | Low                      | Higher                                                    |
| Log collection        | Yes                      | Yes                                                       |
| Parsing capability    | Limited                  | Full parsing capabilities                                 |
| Filtering and routing | Limited                  | More advanced capabilities                                |
| Splunk Web            | No full search interface | Available                                                 |
| Common use            | Collect and forward data | Process, route, and forward data                          |

## 4.6 Intermediate Forwarder

An Intermediate Forwarder receives data from one or more Forwarders and forwards it to downstream Splunk components.

It can be a Universal Forwarder or Heavy Forwarder, depending on the architecture and required processing.

Example:

```text
Linux Servers
      |
      v
Universal Forwarders
      |
      v
Intermediate Forwarder
      |
      v
Indexer
```

Organizations may use intermediate Forwarders to centralize data routing or forwarding.

## 4.7 Indexer

The Indexer is responsible for processing incoming data and storing it in a searchable form.

Main responsibilities include:

- Receiving incoming data.
- Parsing and processing events.
- Applying configured metadata and indexing rules.
- Storing indexed data.
- Making data available for searching.

Example:

A Linux server sends authentication logs to an Indexer.

The Indexer processes and stores events such as:

- Successful SSH logins.
- Failed SSH logins.
- Invalid usernames.
- Session disconnections.

An analyst can search these events using SPL.

## 4.8 Index

An index is a logical storage location in Splunk where indexed events are stored.

Examples of index names:

```text
main
_internal
linux_auth
windows
firewall
network
```

Example search:

```spl
index=linux_auth
```

This search retrieves events from the `linux_auth` index, subject to the user's permissions and selected time range.

**Important:**

An index is not the same as an operating system directory or a source log file. It is a logical data storage location managed by Splunk.

## 4.9 Search Head

The Search Head provides the interface and search-processing environment for users to search and analyze indexed data.

Main responsibilities include:

- Executing searches.
- Coordinating searches against Indexers.
- Displaying search results.
- Creating reports.
- Creating dashboards.
- Supporting alerts and other search-based workflows.

Example:

A SOC Analyst opens Splunk Web and searches:

```spl
index=linux_auth "Failed password"
```

The Search Head coordinates the search and displays matching events.

In a distributed environment, a Search Head can coordinate searches across multiple Indexers.

## 4.10 Splunk Web

Splunk Web is the browser-based user interface for interacting with Splunk.

Users can:

- Search data.
- View events.
- Create dashboards.
- Create reports.
- Manage alerts.
- Configure certain Splunk settings, depending on permissions.

The default Splunk Web port is:

```text
TCP 8000
```

Example:

```text
http://<splunk-server-ip>:8000
```

The actual port can be changed by an administrator.

## 4.11 Deployment Server

The Deployment Server centrally manages configuration deployment to supported Splunk Forwarders.

It can distribute:

- Inputs configurations.
- Outputs configurations.
- Application packages.
- Other supported Forwarder configurations.

Example:

An organization has 100 Linux servers with Universal Forwarders.

Instead of manually configuring every Forwarder, an administrator can use a Deployment Server to distribute the required configurations.

**Important:** The Deployment Server manages configuration deployment. It is not the same component as an Indexer or Search Head.

## 4.12 License Manager

The License Manager manages Splunk license usage and license-related information.

It helps administrators monitor whether Splunk deployments are operating within their licensing limits.

## 4.13 Cluster Manager

The Cluster Manager coordinates an Indexer cluster.

In an Indexer cluster, it helps manage:

- Cluster configuration.
- Replication.
- Search-factor requirements.
- Replication-factor requirements.
- Cluster health.

It is relevant to distributed Splunk environments rather than a basic single-server installation.

---

# 5. Splunk Data Flow

## 5.1 Basic data flow

The basic Splunk data flow is:

```text
Data Source
    |
    v
Forwarder
    |
    v
Indexer
    |
    v
Search Head
    |
    v
Splunk Web
    |
    v
Analyst
```

## 5.2 Step-by-step explanation

### Step 1 – Data generation

A system generates machine data.

Example:

A Linux server generates SSH authentication logs.

### Step 2 – Data collection

A Universal Forwarder monitors the configured log file.

Example:

```text
/var/log/auth.log
```

### Step 3 – Data forwarding

The Forwarder sends the collected data to a receiving Splunk component.

A commonly used Splunk-to-Splunk receiving port is:

```text
TCP 9997
```

### Step 4 – Data indexing

The Indexer receives, processes, and stores the events.

### Step 5 – Searching

The Search Head coordinates searches against indexed data.

### Step 6 – Analysis

The analyst searches, reviews, and investigates the returned events.

## 5.3 Example from a SOC environment

**Scenario:** A Linux server experiences repeated failed SSH login attempts.

The log contains:

```text
Failed password for invalid user admin from 192.168.1.50 port 52341 ssh2
```

The investigation flow:

1. Linux generates the authentication log.
2. Universal Forwarder monitors the authentication log.
3. Forwarder sends the event to the Indexer.
4. Indexer processes and stores the event.
5. Analyst searches the event through Splunk Web.
6. Analyst reviews the source IP, username, time, and number of failed attempts.
7. Analyst determines whether further investigation is required.

---

# 6. Data Collection Methods

Splunk supports different methods of collecting machine-generated data.

## 6.1 Universal Forwarder

A Universal Forwarder is installed on a source machine and configured to monitor files or other supported inputs.

Example:

```text
Linux Server
    |
    v
Universal Forwarder
    |
    v
Splunk Indexer
```

This is a common approach for collecting operating system and application logs.

## 6.2 Syslog

Syslog is a standard protocol used to transmit log messages from devices and applications to a logging server.

Common examples:

- Firewalls.
- Routers.
- Switches.
- Linux systems.
- Network appliances.

Common ports and conventions:

| Protocol        | Common Port |
| --------------- | ----------: |
| Syslog over UDP |         514 |
| Syslog over TCP |         514 |
| Syslog over TLS |        6514 |

These are commonly used port assignments, not mandatory settings for every environment.

A network device can send Syslog messages to a central Syslog server.

Example:

```text
Firewall
    |
    v
Syslog Server
    |
    v
Splunk Indexer
```

The Syslog server can be configured to forward received messages to Splunk.

**Important:** A device sending Syslog messages does not automatically mean Splunk is receiving them. The Syslog receiver and Splunk ingestion configuration must be set up correctly.

## 6.3 HTTP Event Collector (HEC)

HTTP Event Collector (HEC) is a Splunk feature that allows applications and supported data sources to send events to Splunk over HTTP or HTTPS.

HEC uses an authentication token to authorize data submission.

Common HEC port:

```text
TCP 8088
```

Example data flow:

```text
Application
    |
    v
HTTP / HTTPS
    |
    v
Splunk HEC
    |
    v
Indexer
```

HEC is useful when an application can send data through an HTTP-based interface.

## 6.4 API-based data collection

Some applications and services expose APIs that allow data to be retrieved programmatically.

Splunk can collect such data through supported integration methods, add-ons, or custom scripts.

Example:

A monitoring application exposes an API containing alert information.

A configured data collection process retrieves the information and sends it to Splunk.

**Important:** API collection requires appropriate authentication, permissions, configuration, and a supported integration or custom collection method.

## 6.5 Monitor inputs

Splunk can monitor files and directories for new data.

Example:

```text
/var/log/auth.log
```

A suitable input configuration can instruct Splunk to monitor this file.

When new data is written to the monitored file, Splunk can read and ingest it according to the input configuration.

## 6.6 Scripted inputs

A scripted input runs a configured script or command to collect data.

Example:

A script retrieves system information or queries an application and produces output that Splunk can ingest.

Scripted inputs should be configured carefully because they can affect system resources and security.

## 6.7 Windows Event Logs

Windows Event Logs can be collected using the Splunk Universal Forwarder with the appropriate Windows event log input configuration.

Examples include:

- Security.
- System.
- Application.

Security analysts commonly investigate Windows authentication, account, and system activity using these logs.

## 6.8 Data collection methods summary

| Collection Method       | Common Use                                                     |
| ----------------------- | -------------------------------------------------------------- |
| Universal Forwarder     | Collecting operating system and application logs from servers. |
| Syslog                  | Collecting logs from network and security devices.             |
| HTTP Event Collector    | Receiving application or service events over HTTP/HTTPS.       |
| API integration         | Collecting data from applications and external platforms.      |
| Monitor input           | Monitoring files and directories.                              |
| Scripted input          | Collecting information through scripts or commands.            |
| Windows Event Log input | Collecting Windows event data.                                 |

---

# 7. Important Splunk Configuration Files

Splunk uses configuration files to control data collection, forwarding, and other behavior.

Two important files for understanding basic data flow are:

- `inputs.conf`
- `outputs.conf`

## 7.1 inputs.conf

The `inputs.conf` file defines what data Splunk should collect and how it should collect it.

It can configure inputs such as:

- Files.
- Directories.
- Network ports.
- Windows Event Logs.
- Scripted inputs.
- Other supported input types.

Example:

```ini
[monitor:///var/log/auth.log]
disabled = 0
index = linux_auth
sourcetype = linux_secure
```

Explanation:

| Configuration                   | Meaning                                                                              |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| `[monitor:///var/log/auth.log]` | Monitors the specified log file.                                                     |
| `disabled = 0`                  | Enables the input.                                                                   |
| `index = linux_auth`            | Sends events to the specified index when processed by the receiving Splunk instance. |
| `sourcetype = linux_secure`     | Assigns the specified source type.                                                   |

The exact configuration and behavior depend on where the input runs and the deployment architecture.

## 7.2 outputs.conf

The `outputs.conf` file defines where a Forwarder sends its data.

Example:

```ini
[tcpout]
defaultGroup = primary_indexer

[tcpout:primary_indexer]
server = 192.168.56.101:9997
```

Explanation:

| Configuration              | Meaning                                             |
| -------------------------- | --------------------------------------------------- |
| `[tcpout]`                 | Defines TCP output settings.                        |
| `defaultGroup`             | Specifies the default forwarding group.             |
| `[tcpout:primary_indexer]` | Defines a forwarding group named `primary_indexer`. |
| `server`                   | Specifies the receiving Splunk server and port.     |

In this example, the Forwarder sends data to:

```text
192.168.56.101:9997
```

## 7.3 Indexer receiving configuration

The receiving Splunk instance must be configured to accept incoming Splunk-to-Splunk data.

A commonly used receiving port is:

```text
TCP 9997
```

Receiving can be enabled through Splunk Web or through the appropriate configuration.

Example configuration:

```ini
[splunktcp://9997]
disabled = 0
```

This is an example of a receiving input configuration. The receiving port must also be permitted by the network and host firewall.

## 7.4 Difference between inputs.conf and outputs.conf

| Configuration File | Main Purpose                                        | Simple Question         |
| ------------------ | --------------------------------------------------- | ----------------------- |
| `inputs.conf`      | Defines what data to collect and how to collect it. | What should I collect?  |
| `outputs.conf`     | Defines where collected data should be sent.        | Where should I send it? |

Remember:

- `inputs.conf` = What to collect.
- `outputs.conf` = Where to send.
- Receiving configuration = Which port should accept incoming data.

## 7.5 Common configuration locations

For a typical Splunk Enterprise installation on Linux:

```text
$SPLUNK_HOME/etc/system/local/
```

For example:

```text
/opt/splunk/etc/system/local/
```

Splunk apps can also contain configuration files under their own directories.

For a Universal Forwarder, a common installation location is:

```text
/opt/splunkforwarder/
```

Its configuration files are generally located under:

```text
$SPLUNK_HOME/etc/
```

**Important:** Splunk configuration can come from multiple locations, including system, app, and deployment-managed configurations. The effective configuration may be influenced by configuration precedence and context.

---

# 8. Important Splunk Ports

Understanding ports is important for troubleshooting connectivity between Splunk components.

| Port | Protocol | Purpose                                 |
| ---- | -------- | --------------------------------------- |
| 8000 | TCP      | Splunk Web default port.                |
| 8089 | TCP      | Splunk management port and REST API.    |
| 9997 | TCP      | Common Splunk-to-Splunk receiving port. |
| 8088 | TCP      | HTTP Event Collector default port.      |
| 514  | UDP/TCP  | Common Syslog port.                     |
| 6514 | TCP      | Common Syslog over TLS port.            |

These are default or commonly used ports. Administrators can configure different ports.

## 8.1 Port 8000 – Splunk Web

Used to access the browser-based Splunk interface.

Example:

```text
http://192.168.56.101:8000
```

## 8.2 Port 8089 – Management Port

Used for Splunk management communication and REST API access.

It is not the normal browser interface port.

## 8.3 Port 9997 – Splunk Receiving Port

Commonly used for Splunk-to-Splunk data forwarding.

Example:

```text
Universal Forwarder
       |
       | TCP 9997
       v
Splunk Indexer
```

## 8.4 Port 8088 – HTTP Event Collector

Used by applications and supported clients to send events to Splunk through HEC.

## 8.5 Syslog Ports

Common conventions:

- UDP 514.
- TCP 514.
- TCP 6514 for Syslog over TLS.

---

# 9. Splunk Metadata: Source, Sourcetype, Host, and Index

Splunk associates metadata with events to help organize and search data.

Four important metadata fields are:

- `index`
- `host`
- `source`
- `sourcetype`

## 9.1 Index

The index identifies the logical storage location of an event.

Example:

```text
index=linux_auth
```

Meaning:

Search events stored in the `linux_auth` index, subject to the user's permissions and time range.

## 9.2 Host

The `host` field identifies the host associated with an event.

Example:

```text
host=server01
```

The host value may be a hostname, IP address, or another configured identifier.

## 9.3 Source

The `source` field identifies the source of the data.

For file-based inputs, it commonly contains the file path.

Example:

```text
source=/var/log/auth.log
```

## 9.4 Sourcetype

The `sourcetype` field identifies the format or type of the data.

It helps Splunk interpret events and apply appropriate event processing.

Examples:

```text
linux_secure
access_combined
syslog
```

The actual sourcetype depends on the configuration, add-ons, and data source.

## 9.5 Difference between source and sourcetype

| Field        | Meaning                                            | Example             |
| ------------ | -------------------------------------------------- | ------------------- |
| `source`     | Where the data came from.                          | `/var/log/auth.log` |
| `sourcetype` | What type or format of data it represents.         | `linux_secure`      |
| `host`       | Which host is associated with the event.           | `server01`          |
| `index`      | Which logical storage location contains the event. | `linux_auth`        |

Easy way to remember:

- Host = Which machine?
- Source = Which file or input?
- Sourcetype = What kind of data?
- Index = Where is the event stored in Splunk?

---

# 10. Splunk in Infrastructure Operations and SOC Monitoring

Splunk can be used for both infrastructure operations and security monitoring.

The same data source can provide information for different operational and security purposes.

## 10.1 Operational monitoring

Operational monitoring focuses on the health, availability, and performance of systems and services.

Examples:

- Server availability.
- CPU and memory utilization.
- Disk usage.
- Application errors.
- Network interface status.
- Service failures.
- Device communication problems.

Example:

A network device becomes unreachable.

An operations engineer may investigate:

1. Device availability.
2. Network connectivity.
3. Interface status.
4. Upstream connectivity.
5. Related system alerts.
6. Event timestamps.

The goal is to restore normal service and identify the operational cause.

## 10.2 Security monitoring

Security monitoring focuses on identifying suspicious activities, potential threats, and security incidents.

Examples:

- Failed authentication attempts.
- Unknown administrative logins.
- Unusual source IP addresses.
- Suspicious network connections.
- Unexpected configuration changes.
- Privilege-related activity.
- Malware detection alerts.
- Possible data exfiltration.

Example:

An unknown IP address attempts to access a network device's administrative interface repeatedly.

A SOC Analyst may investigate:

1. Source IP address.
2. Target device.
3. Username used.
4. Authentication result.
5. Number of attempts.
6. Time of activity.
7. Whether the activity succeeded.
8. Whether the source IP is expected or approved.
9. Related events from other systems.

The goal is to determine whether the activity is suspicious and whether an incident response process is required.

## 10.3 NOC vs SOC

| NOC                                                 | SOC                                                  |
| --------------------------------------------------- | ---------------------------------------------------- |
| Network Operations Center.                          | Security Operations Center.                          |
| Focuses on network and infrastructure availability. | Focuses on security monitoring and threat detection. |
| Investigates connectivity and performance issues.   | Investigates suspicious and malicious activities.    |
| Handles operational incidents.                      | Handles security incidents.                          |
| Example: Router interface down.                     | Example: Suspicious access to router administration. |

Some organizations have separate NOC and SOC teams.

Other organizations use hybrid operational and security monitoring responsibilities, depending on their structure and processes.

## 10.4 Example: Wireless Access Point monitoring

Consider a Wireless Access Point (WAP) deployed at a remote location.

### Operational monitoring

An operations engineer may check:

- Device reachability.
- Power status.
- Network connectivity.
- Upstream connectivity.
- Satellite or WAN connectivity, where applicable.
- Interface status.
- Service availability.

### Security monitoring

A security analyst may investigate:

- Unknown administrative logins.
- Repeated authentication failures.
- Unknown wireless clients.
- Unexpected configuration changes.
- Suspicious traffic patterns.
- Unauthorized access attempts.

### Important observation

One root cause may generate multiple alerts.

For example, a network connectivity failure may cause several downstream devices to report communication problems.

Multiple alerts do not automatically mean multiple independent incidents.

An analyst should correlate events and identify whether they share a common cause.

---

# 11. Splunk Troubleshooting: Missing Data

One important practical skill is understanding how to investigate missing data.

**Scenario:** A Linux server is expected to send authentication logs to Splunk, but no events are appearing in the search results.

A structured troubleshooting approach helps identify where the data flow has failed.

## 11.1 End-to-end troubleshooting flow

```text
Source System
     |
     v
Are logs being generated?
     |
     v
Is Forwarder running?
     |
     v
Is the input configured?
     |
     v
Is the Forwarder connected
to the receiving server?
     |
     v
Is the receiving port enabled
and reachable?
     |
     v
Is the data being received
and indexed?
     |
     v
Are the index and time range correct?
     |
     v
Are the events visible?
```

## 11.2 Step 1 – Verify the source log

First, check whether the source system is generating logs.

Example:

```bash
sudo tail -f /var/log/auth.log
```

This command displays new entries as they are written to the file.

For systems that use `/var/log/secure`, check that file instead.

Questions:

- Does the log file exist?
- Is the file receiving new entries?
- Is the expected activity generating logs?
- Does the Forwarder have permission to read the file?

If no logs are being generated, investigate the source system before troubleshooting Splunk forwarding.

## 11.3 Step 2 – Check the Forwarder service

Check whether the Universal Forwarder is running.

For a typical Universal Forwarder installation:

```bash
sudo /opt/splunkforwarder/bin/splunk status
```

For Splunk Enterprise:

```bash
sudo /opt/splunk/bin/splunk status
```

The installation path depends on the type of Splunk installation.

Questions:

- Is the Forwarder running?
- Did the service stop unexpectedly?
- Are there startup errors?
- Is the system operating normally?

## 11.4 Step 3 – Verify input configuration

Check whether the expected log file is configured as an input.

Example command:

```bash
sudo /opt/splunkforwarder/bin/splunk btool inputs list --debug
```

This command helps display effective input configuration and the files from which the settings originated.

Check:

- Is the file path correct?
- Is the input enabled?
- Is the correct index configured?
- Is the correct sourcetype configured?
- Does the Forwarder have permission to read the file?

## 11.5 Step 4 – Verify output configuration

Check where the Forwarder is configured to send data.

Example:

```bash
sudo /opt/splunkforwarder/bin/splunk btool outputs list --debug
```

Check:

- Is the receiving server IP correct?
- Is the receiving port correct?
- Is the correct forwarding group configured?
- Is the expected receiving destination present?

## 11.6 Step 5 – Check Forwarder connections

The following command can display configured forwarding connections:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

This helps identify configured receiving destinations and their connection status.

Interpret the output carefully. A configured destination does not by itself prove that the expected log data is successfully indexed.

## 11.7 Step 6 – Check network connectivity

Check whether the Forwarder can reach the receiving server on the configured port.

Example:

```bash
nc -vz 192.168.56.101 9997
```

Meaning:

- `nc` = Netcat.
- `-v` = Verbose output.
- `-z` = Check connection without sending application data.
- `192.168.56.101` = Example receiving server IP.
- `9997` = Example Splunk receiving port.

A successful connection indicates that the TCP connection could be established.

It does not prove that the data was successfully parsed or indexed.

If the command fails, investigate:

- Network connectivity.
- Routing.
- Firewall rules.
- Receiving port configuration.
- Whether the receiving Splunk service is running.

## 11.8 Step 7 – Verify the receiving configuration

On the receiving Splunk instance, verify that the expected receiving port is enabled.

Check:

- Is the receiving port configured?
- Is the Splunk service running?
- Is the server listening on the expected port?
- Is the host firewall allowing the connection?
- Are there receiving or ingestion errors?

A common command to check listening TCP ports on Linux is:

```bash
sudo ss -lntp
```

To filter for common Splunk ports:

```bash
sudo ss -lntp | grep -E '8000|8088|8089|9997'
```

## 11.9 Step 8 – Verify the index

Check whether the event is being sent to the expected index.

For example:

```spl
index=linux_auth
```

If the index name in the configuration is different, the search must use the correct index.

Do not assume the data is missing before verifying that the correct index is being searched.

## 11.10 Step 9 – Verify time range and permissions

Events may exist but remain invisible because of:

- Incorrect time range.
- Incorrect index name.
- Incorrect search terms.
- User permissions.
- Wrong sourcetype.
- Unexpected event timestamps.

For example, a search covering only the last 15 minutes will not return an event generated yesterday.

Check the time range and user permissions before concluding that ingestion has failed.

## 11.11 Step 10 – Check Splunk internal logs

Splunk internal logs may provide information about ingestion, forwarding, parsing, and other internal processes.

Example:

```spl
index=_internal
```

Search for error messages:

```spl
index=_internal error
```

Search for Forwarder-related messages:

```spl
index=_internal forwarder
```

The exact message and sourcetype depend on the component and issue being investigated.

## 11.12 Troubleshooting checklist

| Layer                | What to Verify                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Source system        | Are logs being generated?                                          |
| Log file             | Does the file exist and receive new entries?                       |
| Permissions          | Can the Forwarder read the file?                                   |
| Forwarder service    | Is the Forwarder running?                                          |
| Input configuration  | Is the expected input configured and enabled?                      |
| Output configuration | Is the correct receiving server configured?                        |
| Network              | Can the Forwarder reach the receiving host and port?               |
| Receiving instance   | Is the receiving port enabled and listening?                       |
| Indexing             | Is the data being processed and indexed?                           |
| Search               | Are the correct index, time range, and search criteria being used? |
| Permissions          | Does the analyst have access to the index?                         |

**Operational rule:** Do not make configuration changes on production systems without the required approval and change-management process.

---

# 12. Splunk Internal Logs

Splunk generates its own internal logs to record information about Splunk processes and activities.

These internal logs can help administrators and analysts troubleshoot Splunk itself.

The common internal index is:

```text
_internal
```

## 12.1 Why are internal logs useful?

Internal logs can help investigate:

- Splunk service issues.
- Forwarding problems.
- Input and ingestion issues.
- Search-related activity.
- Splunk Web access.
- Management API activity.
- Internal warnings and errors.

## 12.2 Common internal log sourcetypes

| Sourcetype          | General Purpose                                               |
| ------------------- | ------------------------------------------------------------- |
| `splunkd`           | Splunk daemon and internal application messages.              |
| `splunkd_access`    | Access requests to Splunk management interfaces and services. |
| `splunkd_ui_access` | Splunk Web UI access activity.                                |

The exact internal sourcetypes available depend on the Splunk version and configuration.

## 12.3 Examples of basic searches

Search all events from the internal index:

```spl
index=_internal
```

Search for error messages:

```spl
index=_internal error
```

Search daemon logs:

```spl
index=_internal sourcetype=splunkd
```

Search management access logs:

```spl
index=_internal sourcetype=splunkd_access
```

Search Splunk Web access logs:

```spl
index=_internal sourcetype=splunkd_ui_access
```

These are introductory examples. More advanced searching will be covered in later modules.

---

# 13. Understanding HTTP Status Codes in Splunk Access Logs

When reviewing Splunk access logs, HTTP status codes help indicate the result of an HTTP request.

Common status codes include:

| Status Code | General Meaning                                           |
| ----------- | --------------------------------------------------------- |
| 200         | OK – Request successfully processed.                      |
| 201         | Created – A resource was created.                         |
| 301         | Moved Permanently – Resource has a permanent redirect.    |
| 302         | Found – Temporary redirect.                               |
| 400         | Bad Request – Request is invalid or malformed.            |
| 401         | Unauthorized – Authentication is required or failed.      |
| 403         | Forbidden – Access is denied.                             |
| 404         | Not Found – Requested resource was not found.             |
| 500         | Internal Server Error – Server encountered an error.      |
| 503         | Service Unavailable – Service is temporarily unavailable. |

The meaning of a status code should be interpreted in the context of the application and request.

**Important security distinction:**

An HTTP 200 response means the request was successfully processed. It does not automatically mean the activity was legitimate or safe.

For example, an unauthorized action may be successfully executed if an attacker has valid credentials.

A security investigation should consider:

- User identity.
- Source IP address.
- Requested resource.
- Request method.
- Authentication context.
- Timestamp.
- Whether the activity was expected.

---

# 14. Important Concepts to Remember

## 14.1 Forwarder

Collects configured data from a source system and forwards it to another Splunk component.

## 14.2 Universal Forwarder

Lightweight agent commonly used to collect and forward data.

## 14.3 Heavy Forwarder

Full Splunk Enterprise instance that can perform additional processing and forwarding.

## 14.4 Intermediate Forwarder

A Forwarder that receives data from upstream sources and forwards it downstream.

## 14.5 Indexer

Processes and stores indexed events.

## 14.6 Index

Logical storage location where indexed events are stored.

## 14.7 Search Head

Coordinates searches and provides search and analysis capabilities.

## 14.8 Splunk Web

Browser-based user interface for searching, monitoring, and managing Splunk.

## 14.9 Deployment Server

Centrally manages configuration deployment to supported Forwarders.

## 14.10 License Manager

Manages Splunk license usage and related information.

## 14.11 Cluster Manager

Coordinates an Indexer cluster.

## 14.12 inputs.conf

Defines what data Splunk should collect and how it should collect it.

## 14.13 outputs.conf

Defines where a Forwarder sends collected data.

## 14.14 Receiving Port

The port on which a receiving Splunk instance accepts incoming data.

## 14.15 Syslog

A standard protocol used to transmit log messages from systems and network devices.

## 14.16 HEC

HTTP Event Collector allows supported clients and applications to send events to Splunk over HTTP or HTTPS.

---

# 15. Quick Revision

| Question                         | Answer                                                                                                         |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| What is Splunk?                  | A platform for collecting, indexing, searching, monitoring, analyzing, and visualizing machine-generated data. |
| What does SIEM stand for?        | Security Information and Event Management.                                                                     |
| What is a log?                   | A record generated by a system, application, or device.                                                        |
| What is an event?                | An individual record represented and processed by Splunk.                                                      |
| What is a Forwarder?             | A component that collects and forwards data.                                                                   |
| What is UF?                      | Universal Forwarder.                                                                                           |
| What is HF?                      | Heavy Forwarder.                                                                                               |
| What is an Indexer?              | A component that processes and stores indexed events.                                                          |
| What is an index?                | A logical storage location for indexed events.                                                                 |
| What is a Search Head?           | A component that coordinates searches and presents search results.                                             |
| What is Splunk Web?              | The browser-based Splunk interface.                                                                            |
| What is `inputs.conf`?           | Configuration defining data inputs.                                                                            |
| What is `outputs.conf`?          | Configuration defining forwarding destinations.                                                                |
| What is Deployment Server?       | A component that manages Forwarder configuration deployment.                                                   |
| What is Syslog?                  | A standard protocol for transmitting log messages.                                                             |
| What is HEC?                     | HTTP Event Collector.                                                                                          |
| What is port 8000?               | Default Splunk Web port.                                                                                       |
| What is port 8089?               | Default Splunk management port.                                                                                |
| What is port 9997?               | Common Splunk-to-Splunk receiving port.                                                                        |
| What is port 8088?               | Default HEC port.                                                                                              |
| What is the `_internal` index?   | An index containing Splunk internal logs.                                                                      |
| What does `host` identify?       | The host associated with an event.                                                                             |
| What does `source` identify?     | The source of the data, often a file path.                                                                     |
| What does `sourcetype` identify? | The format or type of data.                                                                                    |
| What does `index` identify?      | The logical storage location of an event.                                                                      |

---

# 16. Module 1 Completion Checklist

Use this checklist to review the concepts covered in this module.

- [ ] Understand what Splunk is.
- [ ] Understand why organizations use Splunk.
- [ ] Understand SIEM fundamentals.
- [ ] Understand logs and events.
- [ ] Identify common log sources.
- [ ] Understand Splunk architecture.
- [ ] Understand Universal Forwarder.
- [ ] Understand Heavy Forwarder.
- [ ] Understand Intermediate Forwarder.
- [ ] Understand Indexer and Index.
- [ ] Understand Search Head and Splunk Web.
- [ ] Understand Deployment Server.
- [ ] Understand License Manager.
- [ ] Understand Cluster Manager.
- [ ] Explain basic Splunk data flow.
- [ ] Understand Syslog collection.
- [ ] Understand HTTP Event Collector.
- [ ] Understand API-based collection.
- [ ] Understand `inputs.conf`.
- [ ] Understand `outputs.conf`.
- [ ] Identify common Splunk ports.
- [ ] Differentiate `host`, `source`, `sourcetype`, and `index`.
- [ ] Understand operational monitoring vs security monitoring.
- [ ] Explain the difference between NOC and SOC responsibilities.
- [ ] Understand the missing-data troubleshooting process.
- [ ] Understand the purpose of Splunk internal logs.

---

# 17. Final Summary

Splunk provides centralized collection, indexing, searching, and analysis of machine-generated data.

A basic understanding of Splunk architecture is essential before learning advanced SPL and security investigations.

The most important data flow to remember is:

```text
Data Source
    |
    v
Forwarder
    |
    v
Indexer
    |
    v
Search Head
    |
    v
Splunk Web
    |
    v
Analyst
```

The most important configuration files are:

```text
inputs.conf  = What to collect
outputs.conf = Where to send
```

The most important metadata fields are:

```text
host       = Which machine?
source     = Which file or input?
sourcetype = What type of data?
index      = Where is the event stored?
```

A good Splunk user should understand not only how to search data, but also how that data is collected, forwarded, processed, stored, and made available for investigation.

**Learning principle:**

Understand the architecture → Understand the data flow → Identify the data source → Practice collection and troubleshooting → Apply the knowledge in SOC scenarios.
