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
<img width="1033" height="747" alt="2" src="https://github.com/user-attachments/assets/e90a3b33-7d1e-469f-a389-10ae680f97d9" />


## Step 1 – Turn On 2-Step Verification

App passwords only exist when 2-Step Verification is on.

In the alerts account, I click **Security** (Sign-in & security) → **2-Step Verification**.

<img width="1039" height="689" alt="3" src="https://github.com/user-attachments/assets/35789768-6757-4165-bc5f-4d06a1caea8d" />

<img width="1031" height="741" alt="4" src="https://github.com/user-attachments/assets/dc335a73-2399-4a30-844e-1ecfbff4f282" />

I set up an **authenticator** app:

<img width="1023" height="615" alt="5" src="https://github.com/user-attachments/assets/9785e693-56ec-4e65-a853-896cdc2a4d9d" />









