# 12 – Install and Configure Postfix to Relay Through Gmail

In this doc, I will install **Postfix** on NetworkVM and set it up to send mail through Gmail. Nagios doesn't send email itself. It hands each alert to the **`mail`** command, and Postfix delivers it.

This is part 2 of setting up email notifications:

| Part | Doc |
|---|---|
| 1 | [**11 – Create a Gmail App Password for Nagios Alerts**](11-Create-Gmail-App-Password-for-Nagios-Alerts.md) |
| **2** | **12 – Install and Configure Postfix to Relay Through Gmail** (this doc) |
| 3 | [**13 – Configure Nagios Email Notifications**](13-Configure-Nagios-Email-Notifications.md) |

> **Testing and validation:** [**Click here to see the Postfix Gmail relay validation**](../06-Testing-and-Validation/13-Validate-Postfix-Gmail-Relay.md)

---

## How the Mail Gets Sent

```
Nagios → /bin/mail → Postfix (NetworkVM) → smtp.gmail.com:587 (login + TLS) → my inbox
```
