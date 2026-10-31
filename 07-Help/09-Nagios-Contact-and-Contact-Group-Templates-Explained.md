# Nagios Contact and Contact Group Templates (`contacts.cfg`)

This guide explains the default contact and contact group that come with Nagios Core, the **`generic-contact`** template they use, and what each option does.

**File location:** **`/usr/local/nagios/etc/objects/templates.cfg`**
**File location:** **`/usr/local/nagios/etc/objects/contacts.cfg`**

<img width="932" height="466" alt="1" src="https://github.com/user-attachments/assets/eaf9f53c-ec42-4814-b363-8db5d5cc46ec" />

<img width="928" height="500" alt="2" src="https://github.com/user-attachments/assets/3785a43b-9bee-4e62-8645-007f7af6b3d6" />

---

## What are contacts and contact groups?

- A **contact** is a person (or account) Nagios can alert when something goes wrong.
- A **contact group** is a list of contacts, so you can alert several people at once by naming the group.

Hosts and services decide **who** gets alerted by pointing to contacts or contact groups:

```
contact_groups      admins        ; alert everyone in the "admins" group
contacts            nagiosadmin   ; alert one specific person
```

The contact's own settings then decide **when** and **how** that person gets alerted.

---

## How it all connects

```
host / service  →  contact_groups admins  →  members nagiosadmin  →  use generic-contact
   (what broke)       (which group)            (which people)          (when + how to alert)
```

---

## The default contact: `nagiosadmin`

<img width="942" height="389" alt="3" src="https://github.com/user-attachments/assets/ae3449db-c2e2-4162-a99d-bec7bbc9b94b" />

| Option | Value | What it does |
|---|---|---|
| `contact_name` | **`nagiosadmin`** | Short name for the contact. This is what contact groups, hosts, and services refer to. |
| `use` | **`generic-contact`** | Inherits the rest of its settings from the `generic-contact` template in `templates.cfg`. |
| `alias` | **`Nagios Admin`** | Longer, readable name shown in the web UI. |

> **Why the name `nagiosadmin` matters:** It matches the web UI login you made during install. The web UI uses the logged-in username to figure out which contact you are, and shows you the hosts and services you're a contact for. Full admin access comes from the **`authorized_for_*`** lines in **`cgi.cfg`**.

---

## The template behind it: `generic-contact`

**`nagiosadmin`** only sets four options. Everything else comes from **`generic-contact`** in **`/usr/local/nagios/etc/objects/templates.cfg`**. On a default install it looks like this:

<img width="938" height="394" alt="3" src="https://github.com/user-attachments/assets/22184aa3-19e4-42df-bb5e-dd26ac11d3df" />

| Option | Value | What it does |
|---|---|---|
| `name` | **`generic-contact`** | The template's name, used with `use generic-contact`. |
| `service_notification_period` | **`24x7`** | When this contact can get **service** alerts. Points to a timeperiod in `timeperiods.cfg`. |
| `host_notification_period` | **`24x7`** | When this contact can get **host** alerts. |
| `service_notification_options` | **`w,u,c,r,f,s`** | Which service states this contact wants to hear about (see the letter table below). |
| `host_notification_options` | **`d,u,r,f,s`** | Which host states this contact wants to hear about. |
| `service_notification_commands` | **`notify-service-by-email`** | The command that sends service alerts. Defined in `commands.cfg`. |
| `host_notification_commands` | **`notify-host-by-email`** | The command that sends host alerts. Defined in `commands.cfg`. |
| `register` | **`0`** | Marks this as a template, not a real contact. |

**Notification option letters**

| Letter | Host meaning | Service meaning |
|---|---|---|
| **`d`** | DOWN | — |
| **`u`** | UNREACHABLE | UNKNOWN |
| **`w`** | — | WARNING |
| **`c`** | — | CRITICAL |

