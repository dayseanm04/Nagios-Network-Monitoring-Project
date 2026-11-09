# 11 – Create a Gmail App Password for Nagios Alerts

In this doc, I will create a Gmail account for Nagios alerts and an **app password** that Postfix will use to log in to Gmail and send alert emails.

This is part 1 of setting up email notifications:

| Part | Doc |
|---|---|
| **1** | **11 – Create a Gmail App Password for Nagios Alerts** (this doc) |
| 2 | [**12 – Install and Configure Postfix to Relay Through Gmail**](12-Install-and-Configure-Postfix-Gmail-Relay.md) |
| 3 | [**13 – Configure Nagios Email Notifications**](13-Configure-Nagios-Email-Notifications.md) |

---

## Why an App Password?

Postfix logs in to Gmail to send mail, but Gmail won't accept a normal password from a program like this. It needs an **app password**: a 16-character code made for one app. I can delete it any time without changing my real password.
