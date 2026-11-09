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

<img width="1030" height="708" alt="2" src="https://github.com/user-attachments/assets/20fe8067-cbcf-4a6c-bdb6-525cdc84dcc1" />

<img width="1019" height="657" alt="3" src="https://github.com/user-attachments/assets/d768efa0-930f-4b46-a166-ed600d6f1e64" />

| Prompt | My answer | Why |
|---|---|---|
| General mail configuration type | **Satellite system** | Sends all mail to one relay server and doesn't receive mail from outside |
| System mail name | **`NetworkVM`** | Used for local addresses like **`nagios@NetworkVM`** |
| SMTP relay host | **`[smtp.gmail.com]:587`** | Where all outgoing mail goes |
| Everything else | Default | |

<img width="1024" height="811" alt="4" src="https://github.com/user-attachments/assets/f03b23c5-4c39-4164-a8e4-49d0c7fe64ff" />

The output shows Postfix set **`relayhost`** to **`[smtp.gmail.com]:587`** and **`inet_interfaces`** to **`loopback-only`** for me. 

---

## Step 3 – Configure Postfix to Log In to Gmail

I use **`postconf -e`** to write each setting into **`/etc/postfix/main.cf`**, which avoids typos from editing the file by hand:

<img width="918" height="326" alt="5" src="https://github.com/user-attachments/assets/68683476-0da9-47c4-937c-305105ce503f" />

| Setting | What it does |
|---|---|
| **`relayhost = [smtp.gmail.com]:587`** | Send all mail to Gmail's SMTP server on port 587. The **square brackets** tell Postfix to connect to that exact host instead of looking up its MX record |
| **`smtp_sasl_auth_enable = yes`** | Turns on logging in to the relay. SASL is the login part of SMTP |
| **`smtp_sasl_password_maps`** | Where Postfix finds the username and password (Step 4) |
| **`smtp_sasl_tls_security_options = noanonymous`** | Only log in with a username and password, never anonymously |
| **`smtp_tls_security_level = encrypt`** | Requires TLS. If encryption fails, Postfix won't send, so the password never crosses the network in plain text |
| **`inet_interfaces = loopback-only`** | Postfix only listens on **`127.0.0.1`**, so nothing else on my network can use NetworkVM to send mail |

### Check the CA certificates

Postfix needs to know where the certificates are to verify it's really talking to Gmail:

```bash
postconf smtp_tls_CApath smtp_tls_CAfile
```

<img width="798" height="300" alt="6" src="https://github.com/user-attachments/assets/7212ff88-2685-418c-8fe5-268027e44f28" />

**`smtp_tls_CAfile`** is already set to **`/etc/ssl/certs/ca-certificates.crt`**, so I don't need to change anything.

---

## Step 4 – Store the Gmail Login

```bash
sudo nano /etc/postfix/sasl_passwd
```

I add one line with the alerts account and the app password from [**doc 11**](11-Create-Gmail-App-Password-for-Nagios-Alerts.md), with no spaces in the password:

```
[smtp.gmail.com]:587 emailgoes@here.com:<16-character app password>
```

<img width="968" height="167" alt="7" src="https://github.com/user-attachments/assets/b34d8961-b752-46c4-b4a5-fd91e929ddc5" />

Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

---

## Step 5 – Lock Down the File and Build the Lookup Table

<img width="956" height="532" alt="8" src="https://github.com/user-attachments/assets/39b96458-2561-458e-9222-f65cb8bec8b2" />


















