# 06 – Validate NCPA C: Drive Space Check

In this doc, I will verify that the **`C: Drive Space utilization`** check for **`comp-a-test`** works. The check went **WARNING** as soon as it was added, because C: was above **70%**. I deleted files and folders to free up space, and the check went back to **OK**. That makes this a full test: the alert fired, I fixed the problem, and Nagios confirmed it.
