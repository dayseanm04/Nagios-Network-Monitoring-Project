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

**Why a separate Gmail account?** The app password is stored on NetworkVM. If it ever leaked, only this alerts account would be exposed, not my personal email.

| Account | Job |
|---|---|
| Alerts Gmail (**`…alerts@gmail.com`**) | **Sends** the alerts. Postfix logs in as this account |
| My personal email | **Receives** the alerts. Set as my contact email in Nagios ([**doc 13**](13-Configure-Nagios-Email-Notifications.md)) |

### Iver created a new gmail account for sending alerts

<img width="1022" height="478" alt="1" src="https://github.com/user-attachments/assets/1a0b983d-ed97-4f50-8e22-4dd844273ded" />


## Step 1 – Turn On 2-Step Verification
