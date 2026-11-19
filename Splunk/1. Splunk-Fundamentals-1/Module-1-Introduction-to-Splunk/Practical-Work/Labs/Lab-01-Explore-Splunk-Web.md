# Lab 01 – Explore Splunk Web

## 1. Objective

Explore the Splunk Web interface and understand its main sections.

By completing this lab, I should be able to:

- Access Splunk Web through a browser.
- Log in to Splunk Enterprise.
- Identify the Search & Reporting application.
- Understand the purpose of the Search interface.
- Identify important Splunk navigation sections.
- Locate the internal Splunk index.
- Perform a basic search.
- Understand how Splunk displays search results.

---

## 2. Lab Environment

| Component       | Details                      |
| --------------- | ---------------------------- |
| Host OS         | Windows 11                   |
| Virtualization  | Oracle VirtualBox            |
| Virtual Machine | Ubuntu                       |
| Splunk Product  | Splunk Enterprise            |
| Access Method   | Web Browser                  |
| Splunk Web Port | TCP 8000                     |
| Network         | VirtualBox Host-Only Network |

---

## 3. Prerequisites

Before starting, ensure:

- Ubuntu VM is powered on.
- Splunk Enterprise is installed.
- Splunk Enterprise service is running.
- Splunk Web is accessible from the Windows host.
- You have the Splunk administrator credentials.

Check Splunk status inside Ubuntu:

```bash
sudo /opt/splunk/bin/splunk status
```

If Splunk is not running, start it:

```bash
sudo /opt/splunk/bin/splunk start
```

Check the Ubuntu IP address:

```bash
ip -br addr
```

Identify the IP address assigned to the Host-Only network interface.

---

## 4. Step 1 – Access Splunk Web

Open a browser on the Windows host.

Enter:

```text
http://<ubuntu-host-only-ip>:8000
```

Example:

```text
http://192.168.56.101:8000
```

The Splunk login page should appear.

Log in using the administrator credentials created during the initial Splunk startup.

### Expected Result

The Splunk Web interface should open successfully.

### Observation

Record:

- Whether the login page opened.
- Whether authentication succeeded.
- Whether any browser or connection error appeared.

---

## 5. Step 2 – Explore the Splunk Home Page

After logging in, explore the Splunk Home page.

Identify the available applications and navigation options.

Look for:

- Search & Reporting.
- Settings.
- Activity or related navigation options.
- Available applications.
- Splunk interface navigation.

The exact layout may vary depending on the Splunk version and installed applications.

### Expected Result

I should understand where to access the main Splunk applications and settings.

---

## 6. Step 3 – Open Search & Reporting

Open the **Search & Reporting** application.

This application is commonly used to:

- Search indexed data.
- Review events.
- Analyze fields.
- Create reports.
- Build dashboards.
- Investigate operational and security-related activity.

Identify the following areas:

1. Search bar.
2. Time Range Picker.
3. Search button.
4. Events tab.
5. Fields sidebar.
6. Search results.
7. Timeline visualization.

### Expected Result

I should be able to identify the main sections of the Search & Reporting interface.

---

## 7. Step 4 – Perform the First Search

Enter the following SPL search:

```spl
index=_internal
```

Click Search.

This search retrieves events from the internal Splunk index, subject to the selected time range and user permissions.

### Understand the Search

| Element        | Meaning                             |
| -------------- | ----------------------------------- |
| `index`        | Search field identifying the index. |
| `_internal`    | Internal Splunk index.              |
| Search results | Events matching the search.         |

### Expected Result

If internal events are available within the selected time range, Splunk should display matching events.

If no events appear, verify the time range, permissions, and whether Splunk has generated internal events.

---

## 8. Step 5 – Explore an Individual Event

Select an event from the search results.

Examine the event details.

Look for commonly available fields such as:

- `_time`
- `_raw`
- `host`
- `source`
- `sourcetype`
- `index`

### Understand the Fields

| Field        | Meaning                                |
| ------------ | -------------------------------------- |
| `_time`      | Event timestamp assigned by Splunk.    |
| `_raw`       | Raw event content.                     |
| `host`       | Host associated with the event.        |
| `source`     | Source of the event data.              |
| `sourcetype` | Type or format of the event data.      |
| `index`      | Logical storage location of the event. |

Not every event will contain the same extracted fields.

### Expected Result

I should be able to inspect an event and identify its raw content and important metadata.

---

## 9. Step 6 – Explore the Fields Sidebar

On the Search & Reporting page, examine the Fields sidebar.

Look at:

- Selected Fields.
- Interesting Fields.

Field availability depends on the events returned by the search and the field extraction configuration.

Try selecting a field to understand how Splunk displays its values and related events.

### Expected Result

I should understand that Splunk uses fields to organize and analyze event information.

---

## 10. Step 7 – Explore the Time Range Picker

Locate the Time Range Picker.

Select different time ranges, such as:

- Last 15 minutes.
- Last 60 minutes.
- Last 24 hours.
- Last 7 days.
- All time.

Run the search again after changing the time range.

Observe how the number of returned events changes.

### Important Point

The Time Range Picker controls which event timestamps are included in the search.

A search may return zero events if the selected time range does not contain matching data.

---

## 11. Step 8 – Explore Splunk Internal Logs

Run the following searches one at a time.

### Search 1 – Internal events

```spl
index=_internal
```

### Search 2 – Splunk errors

```spl
index=_internal error
```

### Search 3 – Splunk warnings

```spl
index=_internal WARN
```

### Search 4 – Splunk service logs

```spl
index=_internal sourcetype=splunkd
```

### Search 5 – Splunk Web access logs

```spl
index=_internal sourcetype=splunk_web_access
```

The exact sourcetypes available depend on the Splunk version and environment.

### Expected Result

I should be able to run basic searches and observe different types of internal Splunk events.

---

## 12. Step 9 – Record the Results

Complete the following table based on the actual lab observations.

| Activity                   | Result  | Observation                            |
| -------------------------- | ------- | -------------------------------------- |
| Opened Splunk Web          | Pending | Record whether the login page opened.  |
| Logged in successfully     | Pending | Record the authentication result.      |
| Opened Search & Reporting  | Pending | Record whether the application opened. |
| Executed `index=_internal` | Pending | Record whether events appeared.        |
| Opened an individual event | Pending | Record the fields observed.            |
| Explored Fields sidebar    | Pending | Record the available fields.           |
| Changed time range         | Pending | Record how the results changed.        |
| Searched internal errors   | Pending | Record the returned events, if any.    |

Replace `Pending` with the actual result after performing each activity.

---

## 13. Troubleshooting

### Issue 1 – Splunk Web is not opening

Check Splunk status:

```bash
sudo /opt/splunk/bin/splunk status
```

Check whether port 8000 is listening:

```bash
sudo ss -lntp | grep 8000
```

Check the Ubuntu IP address:

```bash
ip -br addr
```

Verify that Windows can reach the Ubuntu Host-Only IP.

### Issue 2 – Login page opens, but authentication fails

Check:

- Whether the correct username is being used.
- Whether the password is correct.
- Whether the account is valid.
- Whether the account has been locked or disabled.

Do not share passwords in screenshots or GitHub documentation.

### Issue 3 – Search returns zero events

Check:

1. Whether the search syntax is correct.
2. Whether the selected time range is appropriate.
3. Whether the internal index is available.
4. Whether the logged-in user has permission to search the index.
5. Whether Splunk is running correctly.

### Issue 4 – Fields are not visible

Check whether the returned events contain the expected information.

Some fields may not be extracted or available for every sourcetype.

---

## 14. Evidence and Screenshots

Save screenshots only after completing the corresponding activity.

Suggested filenames:

```text
Lab-01-01-Splunk-Web-Login.png
Lab-01-02-Splunk-Home-Page.png
Lab-01-03-Search-and-Reporting.png
Lab-01-04-Internal-Index-Search.png
Lab-01-05-Event-Details.png
Lab-01-06-Fields-Sidebar.png
```

Before uploading screenshots to GitHub, ensure that they do not expose credentials, tokens, private information, or sensitive system details.

---

## 15. Learning Outcomes

After completing this lab, I should be able to:

- Access Splunk Web.
- Navigate the Splunk Home page.
- Open Search & Reporting.
- Execute a basic SPL search.
- Search the `_internal` index.
- Inspect individual events.
- Identify important event metadata.
- Explore the Fields sidebar.
- Understand the Time Range Picker.
- Recognize basic Splunk internal logs.

---

## 16. Completion Checklist

- [ ] Splunk Enterprise service verified.
- [ ] Splunk Web accessed successfully.
- [ ] Logged in successfully.
- [ ] Explored Splunk Home page.
- [ ] Opened Search & Reporting.
- [ ] Executed `index=_internal`.
- [ ] Explored individual event details.
- [ ] Identified important metadata fields.
- [ ] Explored the Fields sidebar.
- [ ] Changed the Time Range Picker.
- [ ] Executed additional internal searches.
- [ ] Recorded observations.
- [ ] Captured relevant screenshots.

---

## 17. Final Notes

This lab introduces the Splunk Web interface and basic event exploration.

The purpose is not to perform advanced SPL searches or security investigations yet.

Those topics will be covered in subsequent modules.

**Learning approach:**

Understand the interface → Execute a search → Inspect an event → Identify fields → Record observations.
