# 13 – Validate Postfix Gmail Relay

In this doc, I will test Postfix by itself, before Nagios is involved. If a test email reaches my inbox, the mail side works, and any problem later is on the Nagios side.

> **How Postfix was set up:** [**Click here to see Install and Configure Postfix to Relay Through Gmail**](../02-Setup/12-Install-and-Configure-Postfix-Gmail-Relay.md)

---

## Test Cases

| ID | Test | Expected | Actual | Status |
|---|---|---|---|---|
| TC-01 | Login file is locked down | **`sasl_passwd`** and **`.db`** are **`-rw-------`**, owned by root | **`-rw------- 1 root root`** for both | ✅ PASS |
| TC-02 | Gmail accepts a test email | **`status=sent (250 2.0.0 OK ...)`** in **`mail.log`** | **`relay=smtp.gmail.com[...]:587 ... status=sent (250 2.0.0 OK ...)`** | ✅ PASS |
| TC-03 | Test email reaches my inbox | Email in my inbox | Landed in **Spam**. After I marked it **Not spam**, it goes to my inbox | ✅ PASS |

---

## TC-01 – Login File Is Locked Down

```bash
ls -l /etc/postfix
```

<img width="956" height="532" alt="1" src="https://github.com/user-attachments/assets/1c430bdf-1d1b-4345-870f-c9299b10b9b9" />

**Result:** ✅ PASS. **`sasl_passwd`** and **`sasl_passwd.db`** are both **`-rw-------`** and owned by **`root`**.

---

## TC-02 – Gmail Accepts a Test Email

**Steps:** In a second terminal, I watch the mail log:

```bash
sudo tail -f /var/log/mail.log
```

<img width="1025" height="567" alt="2" src="https://github.com/user-attachments/assets/9e3cc89d-2d56-4360-ac3d-af22e16831e5" />

In the first terminal, I send a test email to my personal Gmail:

```bash
echo "Test from NetworkVM" | mail -s "Postfix relay test" …@gmail.com
```

<img width="1032" height="361" alt="3" src="https://github.com/user-attachments/assets/2a310a6c-145d-4d19-b915-ffb77105f0bc" />

<img width="1035" height="802" alt="4" src="https://github.com/user-attachments/assets/caeb902b-2bc4-4f2b-a879-ef79d3831980" />

**Result:** ✅ PASS. This is the line that matters:

```
to=<…@gmail.com>, relay=smtp.gmail.com[...]:587, ... status=sent (250 2.0.0 OK ... - gsmtp)
```
























