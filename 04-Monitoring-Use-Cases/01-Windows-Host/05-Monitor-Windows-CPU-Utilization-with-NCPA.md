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




























