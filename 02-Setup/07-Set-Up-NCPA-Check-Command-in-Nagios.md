# 07 – Set Up NCPA Check Command in Nagios

In this doc I will prepares the Nagios server (**`NetworkVM`**) to talk to the NCPA agent installed on **`comp-a-test`**. I will install the **`check_ncpa.py`** plugin, stores the NCPA token securely, and creates the **`check_ncpa`** command definition.

No service checks are defined yet. That's covered in the next guide. By the end of this one, Nagios has everything it needs to run NCPA checks; it just hasn't been told which ones.

## Prerequisites
- NCPA installed and running on **`comp-a-test`** (see [**02 – Install NCPA on Windows Host**](../04-Monitoring-Use-Cases/01-Windows-Host/02-Install-NCPA-on-Windows-Host.md))
