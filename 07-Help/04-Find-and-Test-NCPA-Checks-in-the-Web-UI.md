# 04 – Find and Test NCPA Checks in the Web UI

In this doc, I will use the NCPA web interface on **`comp-a-test`** to find the metrics NCPA can report, test them as Nagios checks with warning and critical thresholds, and get the exact command I need for a Nagios service definition.

I'm testing checks in NCPA first because it's faster than editing Nagios config files, running the pre-flight check, restarting Nagios, and waiting for results. Once a check returns what I expect in NCPA, I can copy it into Nagios.

> I ran these examples on **`comp-a-test`** (Windows 11) with **NCPA 3.5.0**.

## Related Docs
- [**04-Monitoring-Use-Cases / 01-Windows-Host / 02 – Install NCPA on Windows Host**](../04-Monitoring-Use-Cases/01-Windows-Host/02-Install-NCPA-on-Windows-Host.md)
- [**02-Setup / 07 – Set Up NCPA Check Command in Nagios**](../02-Setup/07-Set-Up-NCPA-Check-Command-in-Nagios.md)
- [**07-Help / 03 – Nagios Host and Service Definitions Explained**](03-Nagios-Host-and-Service-Definitions-Explained.md)

---

## Step 1 – Log In to NCPA

1. On **`comp-a-test`**, I opened a browser and went to:

```
https://localhost:5693/
```

<img width="1215" height="852" alt="1" src="https://github.com/user-attachments/assets/69c93bea-7e6f-42d9-8af7-376e54bda45b" />

2. At **Your connection is not private**, I click **Advanced** then **Proceed to localhost (unsafe)**. NCPA uses a self-signed certificate, so this warning is expected.

3. I enter my NCPA token and click **Log In**.

<img width="1150" height="552" alt="2" src="https://github.com/user-attachments/assets/66cb8430-d3c0-4703-bee0-ba392041024d" />

The **Dashboard** loads. **Check Results** shows how many checks NCPA has answered in the last 30 days.

<img width="1207" height="765" alt="3" src="https://github.com/user-attachments/assets/a0ef73c2-21b0-4def-8a39-c35afd1e0192" />

<img width="945" height="523" alt="4" src="https://github.com/user-attachments/assets/2cf36bb6-6e76-41e2-8703-1bce22f677c9" />

---

## Step 2 – Browse the API

I click **API** in the top menu. With no endpoint selected, it shows the **root** of the API: every metric NCPA can report, all at once.

<img width="950" height="537" alt="5" src="https://github.com/user-attachments/assets/0fee89b9-3385-462b-9193-613c474ebd3c" />

I open the **API Endpoint** dropdown to see the categories:

<img width="950" height="553" alt="6" src="https://github.com/user-attachments/assets/a301119a-ffce-4e28-8200-a436fd6aaae2" />

<br/>

| Endpoint | What it covers |
|---|---|
| **`cpu`** | Core count, usage percent, idle and system time |
| **`disk`** | Physical and logical disks, used/free space |
| **`plugins`** | Custom plugin scripts installed on the agent |
| **`interface`** | Network adapters: status, bytes sent/received, errors |
| **`memory`** | Physical (virtual) memory and swap usage |
| **`processes`** | Running processes |
| **`services`** | Windows services and their status |
| **`system`** | Hostname, OS version, uptime, agent version |
| **`user`** | Logged-in users |
| **`logs`** | Windows Event Log entries |
| **`windowscounters`** | Windows Performance Counters |

---

## Step 3 – Explore an Endpoint

**Example: `system`**

I select **system** from the first dropdown. The right side shows everything NCPA knows about **`comp-a-test`**:

<img width="942" height="622" alt="7" src="https://github.com/user-attachments/assets/a9bd38a7-5f7c-4f6a-bbbc-4a1211bdf8ea" />

The URL above the results (e.g. **`https://localhost:5693/api/system`**) shows the API path. That path is what I'll put after **`-M`** in a check, without the **`api/`** part.

---

## Step 4 – Turn a Metric into a Nagios Check

**Example: `system/uptime`**

1. I select **system** in the first dropdown and **uptime** in the second.
2. I check **Run as a Nagios check**.

<img width="945" height="468" alt="8" src="https://github.com/user-attachments/assets/e20e3a1f-c703-4641-b562-0e52985b5c3b" />

3. I click **Reload**. Instead of raw data, NCPA now returns a Nagios check result.

---

## Step 5 – Get the Command

1. At the bottom left, I click **View in alternate format**.

<img width="948" height="278" alt="9" src="https://github.com/user-attachments/assets/7df02571-4d52-47d9-b397-f0de783dbff8" />

2. There are two options:

<img width="908" height="158" alt="10" src="https://github.com/user-attachments/assets/dc7d0d67-9114-4d2e-aa38-8213cbc0a676" />

| Option | Use it for |
|---|---|
| **As active check using check_ncpa.py** | The command Nagios runs to ask NCPA for data. **This is what I use in this project** |
| **As passive check config definition** | Config for NCPA to push results to Nagios on its own (requires NRDP) |

3. I click **As active check using check_ncpa.py**.

<img width="603" height="286" alt="11" src="https://github.com/user-attachments/assets/5524c93e-944a-473d-97e8-51aa8dae9a77" />

---

The command NCPA shows is written for running by hand **on the monitored host**. In Nagios, my **`check_ncpa`** command definition already fills in the host, token, and port (see **`02-Setup/07`**):

```
command_line    $USER1$/check_ncpa.py -H $HOSTADDRESS$ -t '$USER5$' -P 5693 $ARG1$
```

So I only copy the part **after the token** into the service's **`check_command`**:

| NCPA shows | Where it goes in Nagios |
|---|---|
