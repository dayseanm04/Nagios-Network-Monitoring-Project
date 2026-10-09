# 04 – Find and Test NCPA Checks in the Web UI

In this doc, I will use the NCPA web interface on **`comp-a-test`** to find the metrics NCPA can report, test them as Nagios checks with warning and critical thresholds, and get the exact command I need for a Nagios service definition.

I'm testing checks in NCPA first because it's faster than editing Nagios config files, running the pre-flight check, restarting Nagios, and waiting for results. Once a check returns what I expect in NCPA, I can copy it into Nagios.

> I ran these examples on **`comp-a-test`** (Windows 11) with **NCPA 3.5.0**.

## Related Docs
- [**04-Monitoring-Use-Cases / 01-Windows-Host / 02 – Install NCPA on Windows Host**](../04-Monitoring-Use-Cases/01-Windows-Host/02-Install-NCPA-on-Windows-Host.md)
- [**02-Setup / 07 – Set Up NCPA Check Command in Nagios**](../02-Setup/07-Set-Up-NCPA-Check-Command-in-Nagios.md)
- [**07-Help / 03 – Nagios Host and Service Definitions Explained**](03-Nagios-Host-and-Service-Definitions-Explained.md)

---

## Step 1 – Log In to NCPA

1. On **`comp-a-test`**, I opened a browser and went to:

```
https://localhost:5693/
```

<img width="1215" height="852" alt="1" src="https://github.com/user-attachments/assets/69c93bea-7e6f-42d9-8af7-376e54bda45b" />

2. At **Your connection is not private**, I click **Advanced** then **Proceed to localhost (unsafe)**. NCPA uses a self-signed certificate, so this warning is expected.

3. I enter my NCPA token and click **Log In**.

<img width="1150" height="552" alt="2" src="https://github.com/user-attachments/assets/66cb8430-d3c0-4703-bee0-ba392041024d" />

The **Dashboard** loads. **Check Results** shows how many checks NCPA has answered in the last 30 days.


