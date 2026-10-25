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

---

### `us-holidays`

<img width="935" height="420" alt="3" src="https://github.com/user-attachments/assets/e8aa6362-a1c6-42a0-8801-0462fd29947e" />

This one isn't meant to be used directly. It's a list of holiday **exceptions** that other timeperiods pull in with **`use`**. Each holiday is set to **`00:00-00:00`**, so when another timeperiod inherits it, those days get removed.

**How to read the exception formats**

| Format | Example | Meaning |
|---|---|---|
| **`month day`** | **`january 1`** | That date every year |
| **`weekday N month`** | **`monday 1 september`** | The 1st Monday in September |
| **`weekday N month`** | **`thursday 4 november`** | The 4th Thursday in November |
| **`weekday -N month`** | **`monday -1 may`** | The **last** Monday in May (negative numbers count from the end) |

---

### `24x7_sans_holidays`

<img width="940" height="655" alt="4" src="https://github.com/user-attachments/assets/94b4b245-b0c2-47d7-af94-8182bb7477f6" />

Same as **`24x7`**, except on the holidays in **`us-holidays`**. (**"Sans"** means **"without."**)

**How it works**

1. **`use us-holidays`** pulls in the holiday exceptions, the same way a host uses a template.
2. The weekday lines make every day valid all day.
3. Exceptions (specific dates) take priority over normal weekday lines. So on July 4th, the **`00:00-00:00`** from **`us-holidays`** wins over the **`00:00-24:00`** from the weekday line, and the day is removed.








































