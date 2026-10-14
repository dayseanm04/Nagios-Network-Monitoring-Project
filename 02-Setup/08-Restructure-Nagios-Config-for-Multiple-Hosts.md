# 08 – Restructure Nagios Config for Multiple Hosts

I configured the system uptime check for **`comp-a-test`** in the **`windows.cfg`** file. Instead of defining every Windows host and its service checks in that one file, I will create a directory to hold Windows host config files, with one file per host.

This makes it easy to add a new Windows host by adding a new file, instead of adding every Windows host to **`windows.cfg`**. My project scenario is a small company with 12–20 devices [servers, etc], so the config needs to be organized in a way that scales.
