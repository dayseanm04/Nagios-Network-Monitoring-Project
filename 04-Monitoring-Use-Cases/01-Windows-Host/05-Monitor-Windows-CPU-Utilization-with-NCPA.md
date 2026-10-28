# 05 – Monitor Windows CPU Utilization with NCPA

In this doc, I will configure a CPU utilization check for **`comp-a-test`**. First I'll build and test the check in the NCPA web interface, then I'll add it to the host's config file using my **`time-sensitive`** template so CPU problems are noticed quickly.

> **Testing and validation for this check:** [**Click here to see the NCPA CPU utilization check validation**](../../06-Testing-and-Validation/04-Validate-NCPA-CPU-Utilization-Check.md)

---

## Why Monitor CPU?

High CPU usage slows down everything running on the machine and can point to a stuck process, malware, or a machine that's undersized for its workload. Short spikes are normal, so the goal is to catch CPU that **stays** high, not every spike.

That's why this check uses the `time-sensitive` template from [**02-Setup / 09 – Create Time-Sensitive Service Template**](../../02-Setup/09-Create-Time-Sensitive-Service-Template.md):

| Setting | Value | Why |
|---|---|---|
| Check interval | 2 min | High CPU gets noticed within 2 minutes |
| Retry interval | 1 min | Problems are rechecked quickly |
| Max check attempts | 5 | CPU has to stay high for about 4 minutes before alerting, so a short spike won't trigger a false alarm |

---

# Part 1 – Build the Check in NCPA

I build and test the check in the NCPA web interface before adding it to Nagios. For more on this process, see [**08-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../08-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md).

## Step 1 – Log In to NCPA

On **`comp-a-test`**, I go to **`https ://localhost:5693`** and log in with the NCPA token.

<img width="969" height="527" alt="1" src="https://github.com/user-attachments/assets/520246ea-b61c-4bff-981d-eba2b20bb85d" />

<img width="963" height="723" alt="2" src="https://github.com/user-attachments/assets/8124d543-4374-427e-bab0-ab1520f6530f" />

## Step 2 – Select the CPU Metric

I click **API**, then select **cpu** and **percent** under **API Endpoint**.

<img width="961" height="553" alt="3" src="https://github.com/user-attachments/assets/78366be9-d769-432d-bf18-666e3b6a946a" />

NCPA returns one value per CPU core. **`comp-a-test`** has 8 cores, so it shows 8 numbers ranging from 20.6% to 75.7%.

## Step 3 – Average the Cores

Under **Data Settings**, I set **Aggregate** to **average** and click **Reload**.

<img width="960" height="552" alt="4" src="https://github.com/user-attachments/assets/1b5f3236-e704-46e0-8cfb-8888c6f50fd6" />

Now it returns a single number (**41.05%** at that moment), and the URL changes to **`cpu/percent?aggregate=avg`**.

> **Why average?** Without it, Nagios would compare the thresholds against each core separately, so one busy core could trigger an alert even if the machine overall is fine. Averaging gives one number that represents the whole CPU.

## Step 4 – Set Thresholds

Under **Check Settings**, I check **Run as a Nagios check** and set:

| Threshold | Value | Meaning |
|---|---|---|
| Warning | 70 | Average CPU above 70% |
| Critical | 85 | Average CPU above 85% |

<img width="962" height="540" alt="5" src="https://github.com/user-attachments/assets/18198719-7416-47f1-ab9d-e89526d0bc6f" />

I chose **70%** as the warning point because sustained use at that level means the machine is working hard but still has room. **85%** leaves very little room, so that's when it becomes critical.

## Step 5 – Get the Command

I click **View in alternate format** → **As active check using check_ncpa.py**.

<img width="635" height="123" alt="6" src="https://github.com/user-attachments/assets/1ce88717-79b5-4362-91c1-e1f64ede6aa6" />

<img width="920" height="544" alt="7" src="https://github.com/user-attachments/assets/0773b2ae-0fa1-4016-b516-d63feff77690" />


```
./check_ncpa.py -H localhost -t '<your token>' -M 'cpu/percent' -w 70 -c 85 -q 'aggregate=avg'
```

My **`check_ncpa`** command in Nagios already handles the host, token, and port, so I only need the part starting at **`-M`**:

```
-M 'cpu/percent' -w 70 -c 85 -q 'aggregate=avg'
```

<img width="693" height="252" alt="8" src="https://github.com/user-attachments/assets/94dc4581-7088-4bd0-8f14-6d3e7ea64902" />

---

# Part 2 – Add the Service in Nagios

## Step 1 – Open the Host File

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
sudo nano comp-a-test.cfg
```

## Step 2 – Add a CPU Section

Below the existing **`system`** section (the uptime check), I add a new section header so each type of check is grouped:

<img width="889" height="596" alt="9" src="https://github.com/user-attachments/assets/5bb5dbff-7f4b-40c6-88bf-aa53f9ae20d0" />

## Step 3 – Add the Service

<img width="960" height="667" alt="10" src="https://github.com/user-attachments/assets/f4835f59-3dd7-40e0-8757-83193b4c13de" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

### What each line does

| Line | What it does |
|---|---|
| **`use time-sensitive`** | Checks every 2 minutes, retries every minute, and needs 5 failed checks before alerting |
| **`host_name comp-a-test`** | Attaches the check to the Windows host |
| **`service_description CPU Utilization`** | The name shown on the Nagios **Services** page |
| **`check_command check_ncpa!...`** | Runs `check_ncpa` and passes the metric, thresholds, and average setting as `$ARG1$` |































