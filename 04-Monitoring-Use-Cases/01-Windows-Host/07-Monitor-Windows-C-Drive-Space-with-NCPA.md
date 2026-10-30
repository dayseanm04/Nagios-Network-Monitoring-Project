# 07 – Monitor Windows C: Drive Space with NCPA

In this doc, I will add a disk space check for the C: drive on **`comp-a-test`**. First I'll build and test the check in the NCPA web interface. Then I'll add it to the host's config file using my **`time-sensitive`** template.

> **Testing and validation for this check:** [**Click here to see the NCPA C: drive space validatio**n](../../06-Testing-and-Validation/06-Validate-NCPA-C-Drive-Space-Check.md)

---

## Overview

## Why Monitor the C: Drive?

C: is where Windows, its updates, and most programs live. When it fills up, Windows updates fail, programs can't save files, and the page file can't grow. That's why I'm watching C: first.

**`comp-a-test`** has two other drives that I'm not monitoring right now:

| Drive | What it is | Monitored? |
|---|---|---|
| **C:** (Windows) | System drive | Yes |
| **D:** (Test-Drive) | Extra local drive for testing | Not yet |
| **G:** (Google Drive) | Cloud drive synced from Google | No. Its space is managed by Google, not by this machine |

---

# Part 1 – Build the Check in NCPA

For more on this process, see [**08-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../08-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md).

## Step 1 – Log In to NCPA

On **`comp-a-test`**, I go to **`https://localhost:5693`** and log in with the NCPA token.

<img width="969" height="527" alt="1" src="https://github.com/user-attachments/assets/520246ea-b61c-4bff-981d-eba2b20bb85d" />

<img width="957" height="726" alt="2" src="https://github.com/user-attachments/assets/bc1a1b5f-6711-45e6-b4b2-e0979170d1ad" />

## Step 2 – Select the Disk Endpoint

I click **API**, then under **API Endpoint** select **disk**. The second dropdown has three options:

<img width="928" height="408" alt="3" src="https://github.com/user-attachments/assets/84600aaa-a0a3-4359-83f3-00eda73d3868" />

| Option | What it shows |
|---|---|
| **`mount`** | Every mounted drive and its settings |
| **`logical`** | Drives by letter (C:, D:, G:), with size, used, and free space |
| **`physical`** | Raw read/write activity on the physical disks |

I select **logical**, then **C:|** in the third dropdown.

<img width="959" height="717" alt="4" src="https://github.com/user-attachments/assets/1efb16bd-654c-4a75-a5ac-072c2161e39a" />

> **Why `C:|` and not `C:\`?** NCPA uses **`|`** in place of **`\`** in drive paths, because **`\`** has a special meaning in URLs. In the Nagios command, the metric has to stay inside single quotes. In Linux, **`|`** is a pipe, so without the quotes the shell would cut the command in half.

## Step 3 – Select used_percent

In the last dropdown, I select **used_percent**.

<img width="959" height="724" alt="5" src="https://github.com/user-attachments/assets/9f0df2a9-27b0-4265-a9d4-88de66372a51" />

I leave **Unit prefix** blank, since a percentage has no unit to convert.

## Step 4 – Set Thresholds

<img width="949" height="593" alt="6" src="https://github.com/user-attachments/assets/fa00fdbb-de12-4b37-b5c8-f524346d2939" />

| Threshold | Percent | Space used (of 482 GiB) | Space left |
|---|---|---|---|
| Warning | 70% | ~338 GiB | ~145 GiB |
| Critical | 90% | ~434 GiB | ~48 GiB |

I chose **70%** as an early warning. It gives me time to clean up before the drive gets close to full. At **90%**, there's only about **48 GiB** left, which can go fast with Windows updates, VM files, and downloads.

> C: was already at **70.1%** when I set this up, so the check went into **WARNING** right away. That turned out to be useful: it pushed me to clean up the drive. See the [**validation doc**](../../06-Testing-and-Validation/06-Validate-NCPA-C-Drive-Space-Check.md).

## Step 5 – Compare with Windows

To double-check NCPA's numbers, I open **File Explorer**, right-click **Windows (C:)**, and click **Properties**.

<img width="719" height="518" alt="9" src="https://github.com/user-attachments/assets/80228ab2-291e-43f3-ab8c-5452c0abb4f5" />

| Source | Capacity | Used | Free |
|---|---|---|---|
| NCPA | 482.63 GiB | 338.44 GiB (70.1%) | 144.18 GiB |
| Windows Properties | 482 GB | 322 GB (~67%) | 160 GB |

The capacity matches. The used and free numbers are different because the two screenshots were taken at different times, and I deleted files in between.

## Step 6 – Get the Command

I check **Run as a Nagios check** and set:

I click **View in alternate format** → **As active check using check_ncpa.py**.

<img width="635" height="123" alt="7" src="https://github.com/user-attachments/assets/1ce88717-79b5-4362-91c1-e1f64ede6aa6" />

<img width="948" height="572" alt="8" src="https://github.com/user-attachments/assets/536c3bd7-4aac-4909-88b3-1bb2716c403d" />

The part I need for Nagios:

```
-M 'disk/logical/C:|/used_percent' -w 70 -c 90
```

---

# Part 2 – Add the Service in Nagios

## Step 1 – Open the Host File

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
sudo nano comp-a-test.cfg
```

<img width="981" height="452" alt="9" src="https://github.com/user-attachments/assets/2d418423-5760-4147-9de5-5954db9a4228" />

<img width="861" height="299" alt="10" src="https://github.com/user-attachments/assets/36b32d3f-e258-49f8-b597-e2a23890d71b" />


## Step 2 – Add a Disks Section and the Service

Below the Memory section, I add a new section header and the C: drive service:

<img width="893" height="633" alt="11" src="https://github.com/user-attachments/assets/7110c779-2e1f-4cb1-9638-4b92635e09ec" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

### What each line does

| Line | What it does |
|---|---|
| **`use time-sensitive`** | Checks every 2 minutes, retries every minute, and needs 5 failed checks before alerting |
| **`host_name comp-a-test`** | Attaches the check to the Windows host |
| **`service_description C: Drive Space utilization`** | The name shown on the Nagios **Services** page |
| **`check_command check_ncpa!...`** | Runs `check_ncpa` and passes the metric and thresholds as `$ARG1$` |

## Step 3 – Run the Pre-Flight Check

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="1019" height="847" alt="12" src="https://github.com/user-attachments/assets/110ae452-0d00-4bd1-b225-8f17c7190058" />

The service count goes from **11 to 12**, which confirms Nagios picked up the new check.

## Step 4 – Reload Nagios

```bash
sudo systemctl reload nagios
```

<img width="1015" height="278" alt="13" src="https://github.com/user-attachments/assets/8c1b4d84-21b3-4ca1-b37e-e01fd789ba40" />

---




