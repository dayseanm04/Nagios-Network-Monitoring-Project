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
