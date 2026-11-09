# 13 – Configure Nagios Email Notifications

In this doc, I will point Nagios at my email so alerts actually reach me. Postfix is already set up to send mail through Gmail, so this part is all on the Nagios side.

This is part 3 of setting up email notifications:

| Part | Doc |
|---|---|
| 1 | [**11 – Create a Gmail App Password for Nagios Alerts**](11-Create-Gmail-App-Password-for-Nagios-Alerts.md) |
| 2 | [**12 – Install and Configure Postfix to Relay Through Gmail**](12-Install-and-Configure-Postfix-Gmail-Relay.md) |
| **3** | **13 – Configure Nagios Email Notifications** (this doc) |

> **Testing and validation:** [**Click here to see the Nagios email notification validation**](../06-Testing-and-Validation/14-Validate-Nagios-Email-Notifications.md)

---

## Why This Matters

Before this, Nagios was already "sending" notifications, but they went to **`nagios@localhost`** and stayed on the VM. In [**Troubleshoot Windows Host Down and NCPA Services Unknown**](../05-Troubleshooting/01-Windows-Host/02-Troubleshoot-Windows-Host-Down-and-NCPA-Services-Unknown.md), **`comp-a-test`** was down for hours and I never got an alert. This fixes that.
