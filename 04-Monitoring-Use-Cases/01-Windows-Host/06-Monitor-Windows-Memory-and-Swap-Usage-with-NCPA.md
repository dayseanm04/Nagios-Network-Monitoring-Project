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




















