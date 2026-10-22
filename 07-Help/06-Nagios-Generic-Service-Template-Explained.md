# Generic Service Template (`generic-service`)

This guide explains the **`generic-service`** template that comes with Nagios Core and what each of its options does.

**File location:** `/usr/local/nagios/etc/objects/templates.cfg`

<img width="932" height="466" alt="1" src="https://github.com/user-attachments/assets/96ba8e3e-b71e-4b7f-95e4-f84ffaebf3c8" />

<img width="924" height="496" alt="2" src="https://github.com/user-attachments/assets/bdc56670-379b-42e0-8805-a44e4fe3e8c1" />

---

## What it's for

**`generic-service`** is the base template for services. A service is anything Nagios checks on a host, like uptime, CPU, disk space, or whether a website responds.

When a service definition has **`use generic-service`**, it gets all of the settings below. Any option you set directly in the service definition overrides the template's value.

## The template

<img width="937" height="595" alt="generic-service" src="https://github.com/user-attachments/assets/9aa4593d-70dd-4d60-bcd3-c401f5b84da8" />

---

## Options explained

### How checks run

| Option | Value | What it does |
|---|---|---|
| **`active_checks_enabled`** | **`1`** | Nagios runs the check itself on a schedule. NCPA checks through **`check_ncpa`** are active checks. |
| **`passive_checks_enabled`** | `1` | Nagios also accepts results sent to it from somewhere else, like NCPA passive checks through NRDP. Doesn't affect active checks. |
| **`parallelize_check`** | **`1`** | Left over from older versions. It isn't in the Nagios 4 object definitions anymore, because Nagios 4 runs checks in parallel on its own. |
| **`obsess_over_service`** | **`1`** | Used in distributed monitoring, where one Nagios server forwards results to a central one. Also needs **`obsess_over_services=1`** and an **`ocsp_command`** in **`nagios.cfg`**. Does nothing in a single-server setup. |
| **`check_freshness`** | **`0`** | For passive checks. If on, Nagios notices when a passive result hasn't come in for a while (set with **`freshness_threshold`**) and runs an active check itself. |
| **`is_volatile`** | **`0`** | A volatile service sends an alert on **every** non-OK result, not just when the state changes. Useful for event-type checks, like **"a failed login showed up in the log."** Leave at **`0`** for normal checks. |
| **`check_period`** | **`24x7`** | When Nagios is allowed to run the check. |

### Check timing

| Option | Value | What it does |
|---|---|---|
| **`check_interval`** | **`10`** | Minutes between normal checks. |
| **`retry_interval`** | **`2`** | Minutes between rechecks after the service leaves the OK state. |
| **`max_check_attempts`** | **`3`** | How many times Nagios checks before it treats a problem as real (a **"hard"** state) and sends an alert. Setting it to **`1`** alerts right away with no retries. |

> The time options are in minutes because **`interval_length=60`** in **`nagios.cfg`** by default. If that value were changed, the units would change too.

**How these three work together**

1. The service is checked every 10 minutes (**`check_interval`**).
2. A check fails. The service is now in a **"soft"** problem state.
3. Nagios rechecks every 2 minutes (**`retry_interval`**).
4. If it's still failing after 3 total attempts (**`max_check_attempts`**), the problem becomes **"hard"** and an alert goes out.

With these defaults, a problem can take up to about 14 minutes to reach you or an admin: up to 10 minutes before the first failed check, then two retries 2 minutes apart. That's fine for something like uptime, but probably too slow for a critical service. Choose these values per check based on how quickly you need to know.

### Alerts

| Option | Value | What it does |
|---|---|---|











