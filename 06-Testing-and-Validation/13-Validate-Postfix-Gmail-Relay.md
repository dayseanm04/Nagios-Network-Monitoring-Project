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
