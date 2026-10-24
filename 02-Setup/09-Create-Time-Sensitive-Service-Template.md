# 09 – Create Custom Nagios Templates [Time Sensitive Service Template]

In this doc, I will create a separate folder and file for my own Nagios templates, so they're kept apart from the default templates that come with Nagios. Then I'll create my first custom template, **`time-sensitive`**, for high-priority service checks like CPU, memory, and disk that need to be checked more often than the default.

---

## What Is a Template?

A template is a blueprint of settings that hosts and services can inherit with the **`use`** line, so I don't have to repeat the same settings in every definition. 

## Why a Separate File?

The default templates live in **`objects/templates.cfg`**. I could add mine to that file, but keeping them separate means:
- I can tell which templates are mine and which came with Nagios.
- If I ever upgrade or reinstall Nagios, my templates aren't mixed into a file that might get replaced.
- It follows the same idea as **`windows-hosts/`**: my own config is organized in its own place.

---

# Part 1 – Create the Templates Folder and File

## Step 1 – Create the Folder

I make sure I'm in my home directory, then go to the Nagios **`etc`** folder:

```bash
cd
cd /usr/local/nagios/etc
ls -l
```
