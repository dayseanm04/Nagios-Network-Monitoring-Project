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

<img width="989" height="566" alt="1" src="https://github.com/user-attachments/assets/ce2e5a0c-602d-43b4-8459-52c794de63eb" />

I create a folder for my templates:

```bash
sudo mkdir custom-templates
ls -l
```

<img width="856" height="243" alt="2" src="https://github.com/user-attachments/assets/651c249b-c884-4350-914e-99c9f2ddf028" />

---

## Step 2 – Copy and Rename the Template File

I go into the new folder:

```bash
cd custom-templates
ls -l
```

<img width="849" height="289" alt="3" src="https://github.com/user-attachments/assets/bb938dec-03a7-440c-a85e-77cf076f4c3e" />

Then I copied the default **`templates.cfg`** as a starting point:

<img width="1034" height="307" alt="4" src="https://github.com/user-attachments/assets/3e45f31a-281b-4445-94ba-a7f8abfd6683" />

I rename it so it's clear that it holds my custom templates:

```bash
sudo mv templates.cfg custom-templates.cfg
ls -l
```

<img width="1032" height="377" alt="5" src="https://github.com/user-attachments/assets/58a17425-75f2-4be4-872f-d09851a34010" />

Copying the default file turned out to cause a problem. See [**Issue 2**](#issue-2--duplicate-definitions).



























