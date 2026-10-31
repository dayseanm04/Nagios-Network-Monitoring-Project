# 09 – Monitor Windows System Event Logs with NCPA

In this doc, I will add a check that watches the Windows **System** event log on **`comp-a-test`** for failure messages. It counts how many System log entries in the last 24 hours mention **"failure," "failed," or "failing,"** and alerts if the count gets too high. I'll build the check in the NCPA web interface, then add it to the host's config file using the default **`generic-service`** template.

> ✅ **Testing and validation for this check:** [**Click here to see the NCPA System event log check validation**](../../06-Testing-and-Validation/08-Validate-NCPA-System-Event-Log-Check.md)

---

## Overview

## Why Monitor the System Log?
