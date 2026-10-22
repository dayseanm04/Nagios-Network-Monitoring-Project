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

