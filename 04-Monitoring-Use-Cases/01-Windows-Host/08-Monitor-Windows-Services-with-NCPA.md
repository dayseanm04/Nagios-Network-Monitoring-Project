# 08 – Monitor Windows Services with NCPA

In this doc, I will add a check that confirms a Windows service is running on **`comp-a-test`**. The first service I'm monitoring is **NCPA** itself, the agent every other check on this host depends on. I'll build the check in the NCPA web interface, then add it to the host's config file using my **`notify-early`** template.

> **Testing and validation for this check:** [**Click here to see the NCPA Windows service check validation**](../../06-Testing-and-Validation/07-Validate-NCPA-Windows-Service-Checks.md)

---

## Overview

## Why Monitor NCPA?

NCPA is the agent that answers every NCPA check on **`comp-a-test`**. If it stops, Nagios loses CPU, memory, disk, and uptime monitoring for this host all at once. A check named **`NCPA Service Status`** makes that failure easy to spot, instead of leaving me to figure out why five checks broke at the same time.

### Why notify-early?

This check uses **`notify-early`** instead of **`time-sensitive`**:

| Setting | `time-sensitive` | `notify-early` |
|---|---|---|
| Check interval | 2 min | **1 min** |
| Max check attempts | 5 | **1** |
| Notification interval | 15 min | **10 min** |

A stopped service doesn't fix itself, so there's no reason to wait for 5 attempts the way I do for a CPU spike. With 1 attempt, the first failed check goes straight to HARD and alerts.

### What happens if NCPA stops

This check asks NCPA whether NCPA is running. While NCPA is up, the answer is always "running." If NCPA stops, nothing answers, so Nagios won't see **"NCPA is stopped."** It'll get a **connection error** instead, and every other NCPA check on this host will fail at the same time.

So for this check, the alert is a connection failure, not a **"stopped"** message. 

---

# Part 1 – Build the Check in NCPA

For more on this process, see [**08-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../08-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md).

## Step 1 – Log In to NCPA

On **`comp-a-test`**, I go to `https://localhost:5693` and log in with the NCPA token.

<img width="969" height="527" alt="1" src="https://github.com/user-attachments/assets/520246ea-b61c-4bff-981d-eba2b20bb85d" />

<img width="959" height="726" alt="2" src="https://github.com/user-attachments/assets/092c6260-d49d-4c1a-a06c-8e1906497418" />

## Step 2 – Select the services Endpoint

I click **API**, then under **API Endpoint** select **services**. NCPA lists every Windows service on the machine and whether it's **`running`** or **`stopped`**.

<img width="960" height="583" alt="3" src="https://github.com/user-attachments/assets/acf8cf71-e19e-4835-9db6-555c045836c2" />

## Step 3 – Filter to the NCPA Service

I set:

| Setting | Value | Why |
|---|---|---|
| Service Filter(s) | **`NCPA`** | The Windows service name for the agent |
| Filter match type | **`exact match`** | Matches only a service named exactly `NCPA`, not anything with "NCPA" in its name |
| Status | **`running`** | The state I expect. The check is OK when the service matches this |

<img width="956" height="573" alt="4" src="https://github.com/user-attachments/assets/f624f299-ece4-4f4a-a01b-5ccf20d0ea54" />

> **Service name vs. display name:** In the Windows Services app, NCPA shows up as **Nagios Cross-Platform Agent**. That's the *display name*. NCPA's filter uses the *service name*, which is **`NCPA`**. To find a service name, open **Services**, double-click the service, and look at **Service name** at the top of the General tab.

<img width="413" height="477" alt="5" src="https://github.com/user-attachments/assets/da331f99-90b0-4d92-b249-15bb09cc2f2d" />

## Step 4 – Run as a Nagios Check

I check **Run as a Nagios check** and click **Reload**.

<img width="946" height="557" alt="6" src="https://github.com/user-attachments/assets/1a290a0a-22ac-4e3c-be9c-df57b4ecf4ad" />


## Step 5 – Get the Command

I click **View in alternate format** → **As active check using check_ncpa.py**.

<img width="635" height="123" alt="7" src="https://github.com/user-attachments/assets/1ce88717-79b5-4362-91c1-e1f64ede6aa6" />

<img width="897" height="586" alt="7" src="https://github.com/user-attachments/assets/de250877-bac4-4194-a979-bf6a8f1d46f1" />

```
./check_ncpa.py -H localhost -t '<your token>' -M 'services' -q 'service=NCPA,status=running'
```

The part I need for Nagios:

```
-M 'services' -q 'service=NCPA,status=running'
```

| Part | What it does |
|---|---|
| **`-M 'services'`** | Asks NCPA for Windows service status |
| **`-q 'service=NCPA,status=running'`** | Narrows it to the **`NCPA`** service and says `running` is the expected state |

---

# Part 2 – Add the Service in Nagios

## Step 1 – Open the Host File

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
sudo nano comp-a-test.cfg
```

<img width="926" height="579" alt="1" src="https://github.com/user-attachments/assets/62095afc-1fe2-485f-bde3-6e7ca8761284" />

## Step 2 – Add a Services Section and the Check

<img width="949" height="624" alt="2" src="https://github.com/user-attachments/assets/b500d74b-0bfb-4cd2-affc-26ce11901165" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

### What each line does

| Line | What it does |
|---|---|


