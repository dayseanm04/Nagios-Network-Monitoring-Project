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

---

## Step 3 – Rename the Folder

I go back to **`etc`** and shorten the folder name from **`custom-templates`** to **`templates`**:

```bash
cd ..
sudo mv custom-templates templates
ls -l
```

<img width="910" height="227" alt="6" src="https://github.com/user-attachments/assets/165dfc53-a8dd-428c-8c48-031b385be5b1" />

---

## Step 4 – Set Ownership and Permissions

Since I created the folder and copied the file with **`sudo`**, they're owned by **`root`**. I change them to match the rest of the Nagios config:

```bash
sudo chown -R nagios:nagios /usr/local/nagios/etc/templates
sudo chmod 775 /usr/local/nagios/etc/templates
sudo chmod 664 /usr/local/nagios/etc/templates/*.cfg
ls -l
cd templates
ls -l
```

<img width="968" height="412" alt="7" src="https://github.com/user-attachments/assets/105c8d2d-517c-4aae-a34d-cd0ac3d17ff6" />

| Command | What it does |
|---|---|
| **`chown -R nagios:nagios`** | Makes the **`nagios`** user and group the owner of the folder and everything in it |
| **`chmod 775`** (folder) | Owner and group can read, write, and open the folder. Everyone else can read and open it |
| **`chmod 664`** (file) | Owner and group can read and write. Everyone else can only read |

---

## Step 5 – Load the File in nagios.cfg

```bash
cd ..
sudo nano /usr/local/nagios/etc/nagios.cfg
```

<img width="839" height="195" alt="8" src="https://github.com/user-attachments/assets/9d358090-0776-4685-b409-bc6c8cf51711" />

<img width="1020" height="848" alt="9" src="https://github.com/user-attachments/assets/d0c1242e-4ac5-4b8a-bff9-b017c197f576" />

I scroll to the **`cfg_file=`** lines and add my template file right after the default one, with comments so it's clear which is which:

```
# nagios default templates
cfg_file=/usr/local/nagios/etc/objects/templates.cfg

# my custom templates
cfg_file=/usr/local/nagios/etc/templates/custom-templates.cfg
```

<img width="802" height="528" alt="10" src="https://github.com/user-attachments/assets/b1e3b9c6-9f12-4c8b-8011-5fb31a9d422a" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

> I placed it **after** the default templates on purpose. My templates inherit from the defaults (like **`generic-service`**), so it's clear to load the defaults first.

---

## Step 6 – Run the Pre-Flight Check

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

This check failed twice before it passed.

### Issue 1 – Wrong File Path

<img width="953" height="574" alt="11" src="https://github.com/user-attachments/assets/9fb28953-aff2-4d14-bdf3-8f14d3e4eb5b" />

**Cause:** I copied the default **`cfg_file=`** line and edited it, but left **`objects/`** in the path. My folder is at **`etc/templates/`**, not **`etc/objects/templates/`**.

**Fix:** I corrected the line in `nagios.cfg`:

```
# Wrong
cfg_file=/usr/local/nagios/etc/objects/templates/custom-templates.cfg

# Correct
cfg_file=/usr/local/nagios/etc/templates/custom-templates.cfg
```

<img width="753" height="500" alt="12" src="https://github.com/user-attachments/assets/3587d80c-9291-46a2-b922-2ada197f7fa9" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

**Run the Pre-Flight Check again**

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="1026" height="626" alt="13" src="https://github.com/user-attachments/assets/44979575-9f75-44d6-a0d8-6d38ad866339" />


### Issue 2 – Duplicate Definitions

**Cause:** Because I copied the default **`templates.cfg`**, my file contained every default template (**`generic-contact`**, **`generic-host`**, **`generic-service`**, **`windows-server`**, and so on). Nagios was now loading each of them twice: once from **`objects/templates.cfg`** and once from **`custom-templates.cfg`**. Every object can only be defined once.

**Fix:** I cleared the copied templates out of **`custom-templates.cfg`**, keeping only a header:

```bash
cd templates
sudo nano custom-templates.cfg
```

<img width="966" height="699" alt="14" src="https://github.com/user-attachments/assets/be59276d-409e-4a53-a0e5-864195c8feb4" />


To delete a large block of text in nano:

1. Move the cursor to the start of the text to delete.
2. Press **Alt+A** to set a mark.
3. Use the **down arrow** to highlight everything to delete.

<img width="1030" height="645" alt="15" src="https://github.com/user-attachments/assets/99fb4f65-c03b-426c-86fc-fbbaf6e9ac94" />

4. Press **Ctrl+K** to cut it.

<img width="1018" height="378" alt="16" src="https://github.com/user-attachments/assets/0e4612de-2b51-433b-b507-ea2047f4b2d5" />

Save: **Ctrl+O**, **Enter**, **Ctrl+X**.

> **Lesson learned:** I didn't need to copy **`templates.cfg`** at all. The default templates were already loaded. Starting with an empty file would have avoided this error

**Run the Pre-Flight Check again**

```bash
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

<img width="1032" height="838" alt="17" src="https://github.com/user-attachments/assets/9d64b24b-27e0-4e28-9e7e-dae916a45dbf" />

### Pre-Flight Check Passes
























