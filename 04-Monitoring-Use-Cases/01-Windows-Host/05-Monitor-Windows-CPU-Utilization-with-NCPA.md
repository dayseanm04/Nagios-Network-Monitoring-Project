# 05 – Monitor Windows CPU Utilization with NCPA

In this doc, I will configure a CPU utilization check for **`comp-a-test`**. First I'll build and test the check in the NCPA web interface, then I'll add it to the host's config file using my **`time-sensitive`** template so CPU problems are noticed quickly.

> **Testing and validation for this check:** [**Click here to see the NCPA CPU utilization check validation**](../../06-Testing-and-Validation/04-Validate-NCPA-CPU-Utilization-Check.md)

---

## Why Monitor CPU?

High CPU usage slows down everything running on the machine and can point to a stuck process, malware, or a machine that's undersized for its workload. 

