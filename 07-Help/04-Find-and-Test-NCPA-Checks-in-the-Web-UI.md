# 04 – Find and Test NCPA Checks in the Web UI

In this doc, I will use the NCPA web interface on **`comp-a-test`** to find the metrics NCPA can report, test them as Nagios checks with warning and critical thresholds, and get the exact command I need for a Nagios service definition.

I'm testing checks in NCPA first because it's faster than editing Nagios config files, running the pre-flight check, restarting Nagios, and waiting for results. Once a check returns what I expect in NCPA, I can copy it into Nagios.

> I ran these examples on **`comp-a-test`** (Windows 11) with **NCPA 3.5.0**.
