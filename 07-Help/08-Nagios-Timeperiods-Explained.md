# Nagios Timeperiods (`timeperiods.cfg`)

This guide explains the timeperiods that come with Nagios Core, how they're written, and where they get used.

**File location:** `/usr/local/nagios/etc/objects/timeperiods.cfg`

---

## What is a timeperiod?

A timeperiod is a named block of time, like **"all day every day"** or **"Monday to Friday, 9 to 5."** Nagios uses timeperiods to decide **when** it's allowed to run checks and **when** it's allowed to send alerts.

Hosts, services, and contacts don't list hours directly. They point to a timeperiod by name:

---

## The default timeperiods

### `24x7`

<img width="940" height="557" alt="1" src="https://github.com/user-attachments/assets/d59d94b1-4f8f-4c6b-8166-81452837b5d5" />

Every hour of every day is valid. This is the most-used timeperiod. **`generic-host`**, **`generic-service`**, and **`windows-server`** all use it.

---

### `workhours`

<img width="926" height="600" alt="2" src="https://github.com/user-attachments/assets/0d23669c-c0a3-4268-8a0c-8356bf285f75" />

Monday to Friday, 9 AM to 5 PM. No weekends. The **`linux-server`** template uses this as its **`notification_period`**, so Linux hosts only send alerts during business hours.

---

### `none`

No days and no time ranges, so it's **never** valid. Setting **`notification_period none`** on a host or service is one way to make sure it never sends alerts.



















