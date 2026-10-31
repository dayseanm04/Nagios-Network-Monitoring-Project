# Nagios Contact and Contact Group Templates (`contacts.cfg`)

This guide explains the default contact and contact group that come with Nagios Core, the **`generic-contact`** template they use, and what each option does.

**File location:** **`/usr/local/nagios/etc/objects/templates.cfg`**

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

<img width="938" height="394" alt="3" src="https://github.com/user-attachments/assets/22184aa3-19e4-42df-bb5e-dd26ac11d3df" />

| Option | Value | What it does |
|---|---|---|
| `contact_name` | **`nagiosadmin`** | Short name for the contact. This is what contact groups, hosts, and services refer to. |









