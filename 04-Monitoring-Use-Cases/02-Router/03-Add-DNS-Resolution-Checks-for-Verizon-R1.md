# 03 – Add DNS Resolution Checks for Verizon-R1

In this doc, I will add two DNS checks to **`verizon-r1`**. Both ask the router to look up a name, the same way every device on my network does. The first looks up an internet name (**`google.com`**). The second looks up a local device name (**`comp-a-test`**).

My router doesn't support SNMP (see [**11 – Test SNMP Support on Verizon-R1**](../../06-Testing-and-Validation/11-Test-SNMP-Support-on-Verizon-R1.md)), so these checks are one of the ways I can monitor more than whether the router answers ping.

> **Testing and validation for these checks:** [**Click here to see the Verizon-R1 DNS resolution check validation**](../../06-Testing-and-Validation/12-Validate-Verizon-R1-DNS-Resolution-Checks.md)

---

## Why Monitor DNS on the Router?

Every device on my network uses the router (**`192.168.1.1`**) as its DNS server. If the router's DNS stops working, the internet looks "down" to users even though the router still answers ping, so the PING check would stay OK and miss it.

| Check | What it tells me |
|---|---|
| **`google.com`** | The router can resolve internet names. This needs the router's DNS service **and** its upstream connection to Verizon's DNS servers to work |
| **`comp-a-test`** | The router can resolve names of devices on my own network, and gives back the right IP |

---

