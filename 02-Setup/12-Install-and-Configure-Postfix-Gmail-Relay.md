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

Postfix **relays** the mail: it logs in to Gmail on port **587** with the app password, and Gmail delivers it. 

---

## Step 1 – Install the Packages

```bash
sudo apt update
sudo apt install postfix mailutils libsasl2-modules
```

| Package | Why I need it |
|---|---|
| **`postfix`** | The mail server that relays my mail to Gmail |
| **`mailutils`** | Provides the **`mail`** command. Nagios's email commands pipe into it. In [**doc 09**](09-Create-Time-Sensitive-Service-Template.md), I noted **`/bin/mail`** was missing, and this fixes it |
| **`libsasl2-modules`** | The login methods Postfix uses to sign in to Gmail. Without it, Postfix fails with "no mechanism available" |

---

## Step 2 – Answer the Setup Prompts

During install, Postfix asks a few questions:

<img width="1023" height="789" alt="1" src="https://github.com/user-attachments/assets/397cb69f-06e7-47ae-b1da-b33cbe091b63" />





























