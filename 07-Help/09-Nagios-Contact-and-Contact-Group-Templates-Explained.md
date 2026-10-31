# Nagios Contact and Contact Group Templates (`contacts.cfg`)

This guide explains the default contact and contact group that come with Nagios Core, the **`generic-contact`** template they use, and what each option does.

**File location:** **`/usr/local/nagios/etc/objects/contacts.cfg`**

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
