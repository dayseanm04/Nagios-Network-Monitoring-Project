# 03 – Nagios Host and Service Definitions Explained

A line-by-line breakdown of the two most common Nagios configuration blocks: the **host** definition and the **service** definition. Use this guide to understand what each line does before adding or editing devices and checks in Nagios.

It covers:

- **Part 1** – Host definitions
- **Part 2** – Service definitions using NSClient++ (`check_nt`)
- **Part 3** – Service definitions using NCPA (`check_ncpa`)

---

## Hosts vs. Services

In Nagios, a **service** is simply the label for any individual metric or health check you want to track.
