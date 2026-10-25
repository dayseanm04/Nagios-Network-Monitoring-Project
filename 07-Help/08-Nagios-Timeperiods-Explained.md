# Nagios Timeperiods (`timeperiods.cfg`)

This guide explains the timeperiods that come with Nagios Core, how they're written, and where they get used.

**File location:** `/usr/local/nagios/etc/objects/timeperiods.cfg`

---

## What is a timeperiod?

A timeperiod is a named block of time, like "all day every day" or "Monday to Friday, 9 to 5." Nagios uses timeperiods to decide **when** it's allowed to run checks and **when** it's allowed to send alerts.
