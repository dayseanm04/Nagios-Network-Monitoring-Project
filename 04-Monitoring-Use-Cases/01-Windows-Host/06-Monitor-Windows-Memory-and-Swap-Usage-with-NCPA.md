# 06 – Monitor Windows Memory and Swap Usage with NCPA

In this doc, I will configure two memory checks for **`comp-a-test`**: one for RAM usage and one for swap (page file) usage. First I'll build and test both checks in the NCPA web interface. Then I'll add them to the host's config file using my **`time-sensitive`** template.

> **Testing and validation for these checks:** [**Click here to see the NCPA memory and swap validation**](../../06-Testing-and-Validation/05-Validate-NCPA-Memory-and-Swap-Checks.md)

---

## Overview

### What These Metrics Mean

NCPA's naming can be confusing, so here's what each one actually measures on Windows:

| NCPA metric | What it really is | Why it matters |
|---|---|---|
| **`memory/virtual`** | **Physical RAM** usage. NCPA uses a Python library (**`psutil`**) that calls RAM "virtual memory" | When RAM runs out, programs slow down or crash |
| **`memory/swap`** | The Windows **page file** on disk, used when RAM is full | Heavy page file use means RAM isn't enough. The machine slows down because disk is much slower than RAM |

Watching both together (High RAM **and** rising swap) means the machine is really running out of memory.

---

# Part 1 – Build the Checks in NCPA

For more on this process, see [**08-Help / 04 – Find and Test NCPA Checks in the Web UI**](../../08-Help/04-Find-and-Test-NCPA-Checks-in-the-Web-UI.md).

## Step 1 – Log In to NCPA

On **`comp-a-test`**, I go to **`https ://localhost:5693`** and log in with the NCPA token.

<img width="969" height="527" alt="1" src="https://github.com/user-attachments/assets/520246ea-b61c-4bff-981d-eba2b20bb85d" />

<img width="947" height="718" alt="2" src="https://github.com/user-attachments/assets/2953fe8a-4689-44cb-9d3c-58eae2ea6bee" />

---

## Check 1 – RAM (memory/virtual)

### Explore the metric

I click **API** and select **memory** → **virtual**.

<img width="948" height="676" alt="3" src="https://github.com/user-attachments/assets/c3f5e1e7-432e-4eec-ad57-3fcc0d1ce2ae" />

### Select percent

In the third dropdown, I select **percent**.

<img width="962" height="580" alt="4" src="https://github.com/user-attachments/assets/f7b97940-7b63-40a5-8bab-2c7d61a35510" />

I left **Unit prefix** blank. Unit prefixes like `Gi` or `Mi` only matter for size values. Percent has no unit to convert.

### Set thresholds

I check **Run as a Nagios check** and test the thresholds.

<img width="966" height="578" alt="5" src="https://github.com/user-attachments/assets/85c199c1-b2ce-46c7-ad6b-9d5919bb0415" />

I tested with a critical of 85% first, then settled on **90%** for the Nagios check. Here's what each one means on a 15.71 GiB machine (**Comp-a-test has 16GB**):

| Threshold | Percent | Memory used | Memory left |
|---|---|---|---|
| Warning | 70% | ~11.0 GiB | ~4.7 GiB |
| Critical (tested) | 85% | ~13.4 GiB | ~2.4 GiB |
| **Critical (used)** | **90%** | **~14.1 GiB** | **~1.6 GiB** |

I went with **90%** because this machine also runs the Nagios VM and regularly sits in the 80s. At **85%**, the check would be CRITICAL almost all the time, and an alert that's always on gets ignored. **90%** leaves about 1.6 GiB, which is when Windows starts leaning hard on the page file.

### Get the command

I click **View in alternate format** → **As active check using check_ncpa.py**.

<img width="635" height="123" alt="6" src="https://github.com/user-attachments/assets/1ce88717-79b5-4362-91c1-e1f64ede6aa6" />

<img width="906" height="558" alt="7" src="https://github.com/user-attachments/assets/e05e463d-fb12-41c4-baf1-32fed959b3e7" />

The part I need for Nagios:

```
-M 'memory/virtual/percent' -w 70 -c 90
```

---

## Check 2 – Swap (memory/swap)

### Explore the metric

I select **memory** → **swap**.

<img width="948" height="581" alt="8" src="https://github.com/user-attachments/assets/4e05638a-7d71-4bfe-8d14-d9e3f506a55d" />

### Select percent and set thresholds

I select **percent**, check **Run as a Nagios check**, and set:

<img width="955" height="533" alt="9" src="https://github.com/user-attachments/assets/cad1141e-beb2-4dc4-bfbe-e097e1571f42" />

| Threshold | Percent | Swap used (of 7 GiB) |
|---|---|---|
| Warning | 50% | ~3.5 GiB |
| Critical | 80% | ~5.6 GiB |

**Why 50/80?** Windows uses the page file regularly even when things are normal. On **`comp-a-test`**, swap usually sits around **15–40%**. Lower thresholds would keep the check in WARNING most of the time. At **50%**, half the page file is in use, which means RAM has been short for a while.

### Get the command

<img width="635" height="123" alt="10" src="https://github.com/user-attachments/assets/1ce88717-79b5-4362-91c1-e1f64ede6aa6" />

<img width="864" height="487" alt="11" src="https://github.com/user-attachments/assets/57348d54-22b3-4bf6-8441-6a6664fe2392" />

The part I need for Nagios:

```
-M 'memory/swap/percent' -w 50 -c 80
```

---

# Part 2 – Add the Services in Nagios

## Step 1 – Open the Host File

```bash
cd /usr/local/nagios/etc/windows-hosts
ls -l
sudo nano comp-a-test.cfg
```

<img width="861" height="299" alt="12" src="https://github.com/user-attachments/assets/36b32d3f-e258-49f8-b597-e2a23890d71b" />

## Step 2 – Add the Memory Section and Swap Check

<img width="941" height="371" alt="13" src="https://github.com/user-attachments/assets/017aab13-074f-4cf3-bc3c-662503b53391" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

I run the pre-flight check:

<img width="962" height="730" alt="14" src="https://github.com/user-attachments/assets/34df8ac6-0e4a-441f-8d99-45e8a91d3f12" />

The service count goes from **9 to 10**, with 0 warnings and 0 errors.

## Step 3 – Add the RAM Check

I open the file again and add the RAM service under the swap service, in the same Memory section:

```bash
sudo nano comp-a-test.cfg
```

<img width="971" height="630" alt="15" src="https://github.com/user-attachments/assets/5d33d71c-a21d-4d78-a5b3-284300f3385a" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

### What each line does

| Line | What it does |
|---|---|
| **`use time-sensitive`** | Checks every 2 minutes, retries every minute, and needs 5 failed checks before alerting |
| **`host_name comp-a-test`** | Attaches the check to the Windows host |



















