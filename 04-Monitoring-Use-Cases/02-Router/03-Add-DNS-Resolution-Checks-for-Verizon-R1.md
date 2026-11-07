# 03 – Add DNS Resolution Checks for Verizon-R1

In this doc, I will add two DNS checks to **`verizon-r1`**. Both ask the router to look up a name, the same way every device on my network does. The first looks up an internet name (**`google.com`**). The second looks up a local device name (**`comp-a-test`**).

My router doesn't support SNMP (see [**11 – Test SNMP Support on Verizon-R1**](../../06-Testing-and-Validation/11-Test-SNMP-Support-on-Verizon-R1.md)), so these checks are one of the ways I can monitor more than whether the router answers ping.
