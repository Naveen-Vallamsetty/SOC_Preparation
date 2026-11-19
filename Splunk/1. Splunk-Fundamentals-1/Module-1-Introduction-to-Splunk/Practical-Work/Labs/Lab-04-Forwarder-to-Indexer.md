# Lab 04 - Forwarder to Indexer

## 1. Lab Information

| Item       | Details                                                           |
| ---------- | ----------------------------------------------------------------- |
| Module     | Module 01 - Introduction to Splunk                                |
| Lab Number | 04                                                                |
| Difficulty | Beginner                                                          |
| Category   | Data Forwarding and Ingestion                                     |
| Tools      | Splunk Enterprise, Splunk Universal Forwarder, Ubuntu, Kali Linux |
| Status     | Not Started                                                       |

---

## 2. Objective

The objective of this lab is to understand and demonstrate how a Splunk Universal Forwarder collects data and sends it to a Splunk Indexer.

By completing this lab, you will:

- Understand the communication between a Universal Forwarder and an Indexer.
- Enable a receiving port on Splunk Enterprise.
- Install and configure a Universal Forwarder.
- Configure the forwarding destination using `outputs.conf`.
- Configure a log source using `inputs.conf`.
- Verify communication between the Forwarder and Indexer.
- Search for forwarded events in Splunk Web.
- Understand common forwarding and ingestion problems.

---

## 3. Lab Architecture

For this lab, use the following architecture:

```text
                 KALI LINUX VM
              (Universal Forwarder)
                       |
                       |
                 Collects Log Data
                       |
                       |
                 TCP Port 9997
                       |
                       v
                 UBUNTU VM
               (Splunk Enterprise)
                   INDEXER
                       |
                       |
                Receives and
                Indexes Events
                       |
                       v
                 SPLUNK WEB
                 (Search Data)
                       |
                       v
                    ANALYST
```

### Lab Machine Details

| Machine       | Role                | IP Address       |
| ------------- | ------------------- | ---------------- |
| Ubuntu VM     | Splunk Indexer      | 192.168.56.101   |
| Kali Linux VM | Universal Forwarder | To be identified |
| Windows Host  | Access Splunk Web   | 192.168.56.1     |

**Important:** The IP addresses above are based on the existing Ubuntu host-only network setup. Verify the actual addresses before configuring the Forwarder.

---

## 4. Prerequisites

Before starting:

- Complete Lab 01 - Explore Splunk Web.
- Complete Lab 02 - Identify Different Log Types.
- Complete Lab 03 - Splunk Architecture Study.
- Ensure Splunk Enterprise is installed on Ubuntu.
- Ensure Splunk Enterprise is running.
- Ensure Kali Linux is available.
- Ensure both VMs can communicate through the VirtualBox Host-Only Network.

### Resource Considerations

Your host computer has approximately 8 GB RAM and a limited number of CPU cores.

For this lab:

- Run only Ubuntu and Kali Linux.
- Keep Windows and Metasploitable VMs powered off.
- Avoid running unnecessary applications on Windows.
- Do not increase VM memory allocation unnecessarily.

If running both VMs makes the host unstable, stop the lab and reduce resource usage before continuing.

---

## 5. Important Configuration Files

Two configuration files are particularly important in this lab.

| File           | Purpose                                          |
| -------------- | ------------------------------------------------ |
| `inputs.conf`  | Defines which data the Forwarder collects        |
| `outputs.conf` | Defines where the Forwarder sends collected data |

The configuration relationship is:

```text
inputs.conf
     |
     v
What data should be collected?
     |
     v
Universal Forwarder
     |
     v
outputs.conf
     |
     v
Where should data be sent?
     |
     v
Indexer
```

---

## 6. Lab Tasks

### Task 1 - Verify the Ubuntu Indexer

Start the Ubuntu VM.

**Step 1: Check the Ubuntu IP address.**

Run:

```bash
ip -br addr
```

Identify the host-only network interface.

Expected example:

```text
enp0s8    UP    192.168.56.101/24
```

Your interface name or IP address may be different.

**Step 2: Check Splunk Enterprise status.**

Run:

```bash
sudo /opt/splunk/bin/splunk status
```

Expected result:

Splunk should report that its processes are running.

If Splunk is stopped, start it:

```bash
sudo /opt/splunk/bin/splunk start
```

**Step 3: Record the Indexer IP address.**

| Item                     | Observation |
| ------------------------ | ----------- |
| Ubuntu hostname          |             |
| Ubuntu host-only IP      |             |
| Splunk Enterprise status |             |

Keep the Ubuntu VM running for the remaining tasks.

---

### Task 2 - Enable Data Receiving on the Indexer

The Indexer must be configured to receive data from Forwarders.

Splunk commonly uses TCP port `9997` for Splunk-to-Splunk data forwarding.

**Step 1: Open Splunk Web from Windows.**

Navigate to:

```text
http://192.168.56.101:8000
```

Replace the IP if your Ubuntu VM uses a different address.

**Step 2: Open the receiving configuration.**

Navigate to:

```text
Settings
   |
   v
Forwarding and receiving
   |
   v
Configure receiving
```

**Step 3: Create a receiving port.**

Select:

```text
New Receiving Port
```

Enter:

```text
9997
```

Save the configuration.

**Step 4: Verify the port from Ubuntu.**

Run:

```bash
sudo ss -lntp | grep ':9997'
```

Expected observation:

A TCP listening socket should appear for port `9997`.

If no output appears, check whether the receiving port was saved correctly.

**Step 5: Create a dedicated lab index.**

In Splunk Web, navigate to:

```text
Settings
   |
   v
Indexes
   |
   v
New Index
```

Enter the index name:

```text
lab_forwarder
```

Save the index.

This index will store events received from the Universal Forwarder.

**Record your observations:**

| Item                      | Observation |
| ------------------------- | ----------- |
| Receiving port configured |             |
| Port 9997 listening?      |             |
| Lab index created         |             |
| Indexer IP address        |             |

---

### Task 3 - Configure the Kali VM Network

Start Kali Linux.

Ensure the Kali VM has:

- Adapter 1 configured for NAT.
- Adapter 2 configured for the VirtualBox Host-Only Network.
- Cable connected enabled for the host-only adapter.

The host-only adapter must use the same VirtualBox Host-Only Network as Ubuntu.

**Step 1: Identify the Kali IP address.**

Run:

```bash
ip -br addr
```

Identify the address belonging to the `192.168.56.0/24` network.

**Step 2: Test communication with Ubuntu.**

Run:

```bash
ping -c 4 192.168.56.101
```

Replace the address if your Ubuntu IP is different.

Expected result:

You should receive replies from Ubuntu.

**Step 3: Test TCP port 9997.**

If netcat is available, run:

```bash
nc -vz 192.168.56.101 9997
```

Expected result:

The connection should succeed.

If netcat is unavailable, install it using:

```bash
sudo apt update
sudo apt install netcat-openbsd
```

**Important:** Do not expose port 9997 to the public internet or configure router port forwarding.

If Ubuntu's UFW firewall is active, allow the receiving port only from the Kali host-only IP:

```bash
sudo ufw allow from KALI_HOST_ONLY_IP to any port 9997 proto tcp
```

Replace `KALI_HOST_ONLY_IP` with the actual Kali IP address.

**Record your observations:**

| Item                            | Observation |
| ------------------------------- | ----------- |
| Kali hostname                   |             |
| Kali host-only IP               |             |
| Ping to Ubuntu successful?      |             |
| TCP 9997 connection successful? |             |

---

### Task 4 - Install Splunk Universal Forwarder on Kali

The Universal Forwarder must be installed on the machine that will collect and forward data.

For this lab:

- Ubuntu runs Splunk Enterprise and acts as the Indexer.
- Kali runs the Universal Forwarder.

**Step 1: Download the Universal Forwarder package.**

On Kali, open the official Splunk download page and select:

- Product: Universal Forwarder.
- Operating System: Linux.
- Package Type: Debian package (`.deb`).
- Architecture: Linux x86_64.

Use a Universal Forwarder version compatible with your Splunk Enterprise deployment. Matching versions are a convenient choice for this lab where available.

Download the package to Kali.

**Step 2: Open the download directory.**

For example:

```bash
cd ~/Downloads
```

List downloaded files:

```bash
ls -lh
```

Identify the Universal Forwarder `.deb` package.

**Step 3: Install the package.**

Use the actual downloaded filename:

```bash
sudo dpkg -i ./splunkforwarder-<version>-linux-amd64.deb
```

Replace `<version>` with the version in your downloaded filename.

**Step 4: Verify the installation directory.**

Run:

```bash
ls -ld /opt/splunkforwarder
```

Expected result:

The directory should exist.

**Step 5: Start the Universal Forwarder.**

Run:

```bash
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

Follow the prompts.

During the first startup, you may be asked to create an administrator username and password for the Forwarder.

These credentials are for managing the Forwarder. They are separate from the Splunk Enterprise credentials on Ubuntu.

**Step 6: Verify Forwarder status.**

Run:

```bash
sudo /opt/splunkforwarder/bin/splunk status
```

Expected result:

The Universal Forwarder should report that its processes are running.

**Record your observations:**

| Item                           | Observation |
| ------------------------------ | ----------- |
| Universal Forwarder installed? |             |
| Forwarder version              |             |
| Installation directory         |             |
| Forwarder service status       |             |

---

### Task 5 - Create a Sample Log File on Kali

For this exercise, create a small sample log file instead of modifying real authentication logs.

This ensures that the lab does not depend on the availability or permission settings of Kali's operating system logs.

**Step 1: Create a lab directory.**

Run:

```bash
mkdir -p /tmp/splunk-lab
```

**Step 2: Generate sample authentication events.**

Run:

```bash
printf '%s LAB_EVENT type=authentication_failure user=sample_admin src_ip=192.0.2.10\n' "$(date '+%Y-%m-%d %H:%M:%S')" > /tmp/splunk-lab/authentication.log
```

Generate another event:

```bash
printf '%s LAB_EVENT type=authentication_success user=sample_user src_ip=192.0.2.20\n' "$(date '+%Y-%m-%d %H:%M:%S')" >> /tmp/splunk-lab/authentication.log
```

The IP addresses belong to a documentation-only range. These are simulated lab events, not actual login attempts.

**Step 3: Verify the file.**

Run:

```bash
cat /tmp/splunk-lab/authentication.log
```

Expected example:

```text
2026-11-19 10:50:00 LAB_EVENT type=authentication_failure user=sample_admin src_ip=192.0.2.10
2026-11-19 10:50:01 LAB_EVENT type=authentication_success user=sample_user src_ip=192.0.2.20
```

Your timestamps will reflect the actual system time when the commands are executed.

**Record your observations:**

| Item                    | Observation |
| ----------------------- | ----------- |
| Lab directory created?  |             |
| Log file created?       |             |
| Number of sample events |             |
| File path               |             |

---

### Task 6 - Configure inputs.conf

The `inputs.conf` file defines the data sources monitored by the Universal Forwarder.

For this lab, configure the Forwarder to monitor the sample authentication log file.

**Step 1: Create the configuration directory if required.**

Run:

```bash
sudo mkdir -p /opt/splunkforwarder/etc/system/local
```

**Step 2: Create or update inputs.conf.**

Run:

```bash
sudo tee /opt/splunkforwarder/etc/system/local/inputs.conf > /dev/null <<'EOF'
[monitor:///tmp/splunk-lab/authentication.log]
disabled = 0
index = lab_forwarder
sourcetype = lab_auth
EOF
```

**Step 3: Verify the configuration.**

Run:

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

**Configuration explanation:**

| Setting                 | Purpose                                     |
| ----------------------- | ------------------------------------------- |
| `monitor://`            | Tells Splunk to monitor a file              |
| File path               | Identifies the log file                     |
| `disabled = 0`          | Enables the input                           |
| `index = lab_forwarder` | Specifies the destination index name        |
| `sourcetype = lab_auth` | Assigns a sourcetype to the incoming events |

**Important:** The index name must exist on the receiving Splunk Enterprise instance.

---

### Task 7 - Configure outputs.conf

The `outputs.conf` file defines where the Universal Forwarder sends collected data.

**Step 1: Create or update outputs.conf.**

Run:

```bash
sudo tee /opt/splunkforwarder/etc/system/local/outputs.conf > /dev/null <<'EOF'
[tcpout]
defaultGroup = lab_indexers

[tcpout:lab_indexers]
server = 192.168.56.101:9997
EOF
```

Replace `192.168.56.101` if your Ubuntu Indexer has a different host-only IP address.

**Step 2: Verify the configuration.**

Run:

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

**Configuration explanation:**

| Setting                 | Purpose                                             |
| ----------------------- | --------------------------------------------------- |
| `[tcpout]`              | Defines general forwarding settings                 |
| `defaultGroup`          | Identifies the forwarding group used by default     |
| `[tcpout:lab_indexers]` | Defines a forwarding destination group              |
| `server`                | Specifies the Indexer IP address and receiving port |

**Important:** The destination must be the Ubuntu Indexer address, not `127.0.0.1` or the Kali address.

---

### Task 8 - Restart the Universal Forwarder

After changing its configuration, restart the Universal Forwarder.

Run:

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

Wait for the restart to complete.

Check its status:

```bash
sudo /opt/splunkforwarder/bin/splunk status
```

**Expected observation:**

The Forwarder should restart successfully and begin monitoring the configured file.

---

### Task 9 - Verify Forwarder Connectivity

The Universal Forwarder provides a command to check configured forwarding destinations.

On Kali, run:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Authenticate with the Forwarder's administrator credentials if prompted.

**Expected observation:**

The output should show the configured receiving destination.

Look for the destination:

```text
192.168.56.101:9997
```

A successful connection should appear under the active forwarding server list.

If the destination appears as inactive, investigate network connectivity, receiving port configuration, and the Indexer status.

**Record your observations:**

| Item                              | Observation |
| --------------------------------- | ----------- |
| Forwarding destination configured |             |
| Destination IP and port           |             |
| Active forwarding connection?     |             |
| Any error displayed?              |             |

---

### Task 10 - Search Forwarded Events in Splunk

Now verify whether the Indexer received the events.

**Step 1: Open Splunk Web on Windows.**

Navigate to:

```text
http://192.168.56.101:8000
```

**Step 2: Open Search & Reporting.**

**Step 3: Select a suitable time range.**

Use:

```text
Last 24 hours
```

**Step 4: Run the following search.**

```spl
index=lab_forwarder sourcetype=lab_auth
```

Expected observation:

Splunk should return the sample events sent by the Universal Forwarder.

**Step 5: Search for authentication failures.**

```spl
index=lab_forwarder sourcetype=lab_auth authentication_failure
```

**Step 6: Search for successful authentication events.**

```spl
index=lab_forwarder sourcetype=lab_auth authentication_success
```

**Step 7: Inspect a returned event.**

Observe:

- `_time`
- `host`
- `source`
- `sourcetype`
- `index`
- `_raw`

Expected metadata:

| Field        | Expected Value                         |
| ------------ | -------------------------------------- |
| `index`      | `lab_forwarder`                        |
| `sourcetype` | `lab_auth`                             |
| `source`     | `/tmp/splunk-lab/authentication.log`   |
| `host`       | Kali hostname or configured host value |
| `_raw`       | Original sample log record             |

The actual host value may depend on the Forwarder's configuration.

**Important:** The two events are simulated test records. They demonstrate data forwarding and indexing, not real authentication activity.

---

## 7. Final Observation Worksheet

Complete this table using your actual results.

| No. | Activity                         | Actual Observation |
| --- | -------------------------------- | ------------------ |
| 1   | Ubuntu Indexer IP                |                    |
| 2   | Receiving port 9997 enabled      |                    |
| 3   | Dedicated index created          |                    |
| 4   | Kali host-only IP                |                    |
| 5   | Kali-to-Ubuntu ping successful   |                    |
| 6   | TCP 9997 connectivity successful |                    |
| 7   | Universal Forwarder installed    |                    |
| 8   | `inputs.conf` configured         |                    |
| 9   | `outputs.conf` configured        |                    |
| 10  | Forwarder connection active      |                    |
| 11  | Events received by Indexer       |                    |
| 12  | Search returned expected results |                    |

---

## 8. Troubleshooting

### Issue 1 - Forwarder Cannot Connect to Indexer

Possible reasons:

- Incorrect Indexer IP address.
- Receiving port 9997 is not enabled.
- Ubuntu firewall is blocking the connection.
- VMs are not connected to the same host-only network.
- Splunk Enterprise is stopped.

Check from Kali:

```bash
ping -c 4 192.168.56.101
```

Check TCP connectivity:

```bash
nc -vz 192.168.56.101 9997
```

Check the receiving port on Ubuntu:

```bash
sudo ss -lntp | grep ':9997'
```

### Issue 2 - Forwarding Destination Appears Inactive

Check:

```bash
sudo /opt/splunkforwarder/bin/splunk list forward-server
```

Then inspect the Forwarder log:

```bash
sudo tail -n 50 /opt/splunkforwarder/var/log/splunk/splunkd.log
```

Look for connection-related warnings or errors.

### Issue 3 - No Events Appear in Splunk

Verify that the input is configured:

```bash
sudo cat /opt/splunkforwarder/etc/system/local/inputs.conf
```

Verify that the output destination is configured:

```bash
sudo cat /opt/splunkforwarder/etc/system/local/outputs.conf
```

Check the file exists:

```bash
ls -l /tmp/splunk-lab/authentication.log
```

Check the Indexer index name and Splunk time range.

Search again:

```spl
index=lab_forwarder sourcetype=lab_auth
```

### Issue 4 - Index Does Not Exist

Open Splunk Web on Ubuntu and navigate to:

```text
Settings → Indexes
```

Confirm that `lab_forwarder` exists.

The index referenced in `inputs.conf` must be available on the receiving Splunk deployment.

### Issue 5 - Configuration Changes Are Not Applied

Restart the Universal Forwarder:

```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

Then check its status:

```bash
sudo /opt/splunkforwarder/bin/splunk status
```

---

## 9. Evidence to Capture

Save the following screenshots or notes:

1. Ubuntu Indexer IP address.
2. Splunk Enterprise receiving port configuration.
3. Port 9997 listening on Ubuntu.
4. Kali host-only IP address.
5. Successful Kali-to-Ubuntu connectivity test.
6. Universal Forwarder installation and status.
7. `inputs.conf` configuration.
8. `outputs.conf` configuration.
9. Active forwarding destination.
10. Search results showing forwarded events in Splunk.

Do not upload passwords, tokens, private keys, or other sensitive information to GitHub.

---

## 10. Expected Learning Outcomes

After completing this lab, you should be able to:

- Explain how a Universal Forwarder communicates with an Indexer.
- Enable a receiving port on Splunk Enterprise.
- Configure a Universal Forwarder to monitor a log file.
- Configure forwarding destinations using `outputs.conf`.
- Understand the role of `inputs.conf`.
- Verify Forwarder-to-Indexer connectivity.
- Confirm that forwarded events are searchable.
- Troubleshoot common forwarding and ingestion problems.

---

## 11. Completion Checklist

- [ ] Verified the Ubuntu Indexer.
- [ ] Enabled receiving on TCP port 9997.
- [ ] Created the `lab_forwarder` index.
- [ ] Configured Kali host-only networking.
- [ ] Verified connectivity between Kali and Ubuntu.
- [ ] Installed Splunk Universal Forwarder.
- [ ] Created a sample log file.
- [ ] Configured `inputs.conf`.
- [ ] Configured `outputs.conf`.
- [ ] Restarted the Universal Forwarder.
- [ ] Verified the forwarding destination.
- [ ] Searched for forwarded events in Splunk.
- [ ] Completed the observation worksheet.
- [ ] Captured lab evidence.

---

## 12. Lab Conclusion

In this lab, we configured a basic Forwarder-to-Indexer data flow.

The Kali VM generated sample log records, and the Universal Forwarder monitored the file and sent the events to Splunk Enterprise running on Ubuntu.

The Indexer received and stored the events in the `lab_forwarder` index, making them searchable through Splunk Web.

This demonstrates the basic data collection and forwarding process used in distributed Splunk deployments.
