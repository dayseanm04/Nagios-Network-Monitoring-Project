# Generic Host Template (`generic-host`)

This guide explains the **`generic-host`** template that comes with Nagios Core and what each of its options does.

**File location:** **`/usr/local/nagios/etc/objects/`**

<img width="932" height="466" alt="1" src="https://github.com/user-attachments/assets/96ba8e3e-b71e-4b7f-95e4-f84ffaebf3c8" />

---

## What is a template?

A template is an object definition that holds default settings.  It is **not** a real host. Other hosts and templates pull in its settings with the **`use`** option. This saves you from typing the same options on every host.

**`generic-host`** is the base template for hosts. The other host templates, like **`windows-server`** and **`linux-server`**, are built on top of it:

```
your host  →  windows-server / linux-server  →  generic-host
```
