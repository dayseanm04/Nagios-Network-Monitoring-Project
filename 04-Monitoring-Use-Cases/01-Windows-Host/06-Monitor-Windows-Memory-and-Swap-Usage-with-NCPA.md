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

