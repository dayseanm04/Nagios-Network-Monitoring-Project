# Generic Host Template (`generic-host`)

This guide explains the **`generic-host`** template that comes with Nagios Core and what each of its options does.

**File location:** **`/usr/local/nagios/etc/objects/`**

<img width="932" height="466" alt="1" src="https://github.com/user-attachments/assets/96ba8e3e-b71e-4b7f-95e4-f84ffaebf3c8" />

<img width="924" height="496" alt="2" src="https://github.com/user-attachments/assets/bdc56670-379b-42e0-8805-a44e4fe3e8c1" />


---

## What is a template?

A template is an object definition that holds default settings.  It is **not** a real host. Other hosts and templates pull in its settings with the **`use`** option. This saves you from typing the same options on every host.

**`generic-host`** is the base template for hosts. The other host templates, like **`windows-server`** and **`linux-server`**, are built on top of it:

```
your host  →  windows-server / linux-server  →  generic-host
```

Nagios reads the chain from the bottom up. **`generic-host`** sets the base values, the next template adds its own and overrides anything that overlaps, and your host definition overrides both.

> **Rule:** An option set directly in a host definition always wins over the option inherited from a template.

---

## The template

<img width="936" height="454" alt="generic host template" src="https://github.com/user-attachments/assets/e5f8a4d8-9a6f-4da6-8fab-b8deee9f29e1" />

Most of these options are on/off switches[**binary**]:  **`1`** = on, **`0`** = off. 













