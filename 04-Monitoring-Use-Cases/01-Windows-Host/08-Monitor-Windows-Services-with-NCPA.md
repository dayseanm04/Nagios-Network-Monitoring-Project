# 08 – Monitor Windows Services with NCPA

In this doc, I will add a check that confirms a Windows service is running on **`comp-a-test`**. The first service I'm monitoring is **NCPA** itself, the agent every other check on this host depends on. I'll build the check in the NCPA web interface, then add it to the host's config file using my **`notify-early`** template.

> **Testing and validation for this check:** [**Click here to see the NCPA Windows service check validation**](../../06-Testing-and-Validation/07-Validate-NCPA-Windows-Service-Checks.md)
