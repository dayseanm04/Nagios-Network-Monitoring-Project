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

---

## Step 1 – Check the Email Commands

Nagios comes with two email commands in **`commands.cfg`**: **`notify-host-by-email`** and **`notify-service-by-email`**.

```bash
grep -n "mail" /usr/local/nagios/etc/objects/commands.cfg
ls -l /bin/mail
```

<img width="1027" height="528" alt="1" src="https://github.com/user-attachments/assets/73a7125a-7dc0-43ce-8dd8-06ac3225fdf8" />

**How they work:**

1. **`printf`** builds the message body.
2. Nagios fills in the **`$MACROS$`**: the host, state, plugin output, and date.
3. The message is piped into **`/bin/mail`** with a subject line.
4. It's sent to **`$CONTACTEMAIL$`**, the **`email`** field of the contact being notified.

When I compiled Nagios, **`mail`** wasn't installed, so the build used **`/bin/mail`** by default. Now that **`mailutils`** is installed, **`/bin/mail`** exists. It's a link to **`/etc/alternatives/mail`**, which points to the **`mailutils`** version.

---

## Step 2 – Set My Contact Email

```bash
sudo nano /usr/local/nagios/etc/objects/contacts.cfg
```

<img width="971" height="752" alt="2" src="https://github.com/user-attachments/assets/1241ff21-c44e-45a7-9712-a93fc1596618" />

I change **`email`** from **`nagios@localhost`** to my personal email:

<img width="871" height="635" alt="3" src="https://github.com/user-attachments/assets/8dc26bff-bf7a-4f61-8b01-a7910177a6f1" />

| Field | Value | Why |
|---|---|---|
| **`email`** | My personal Gmail | Where alerts are **delivered**. It's not the alerts account, which only **sends** |
| **`use generic-contact`** | (unchanged) | Gives this contact 24x7 notifications, the email commands, and the notification options. See [**07-Help / 09 – Nagios Contact and Contact Group Templates**](../07-Help/09-Nagios-Contact-and-Contact-Group-Templates-Explained.md) |

Save and exit: **Ctrl+O**, **Enter**, **Ctrl+X**.

I don't need to change any templates. My host and service templates already notify the **`admins`** contact group, and **`nagiosadmin`** is in it.

---






