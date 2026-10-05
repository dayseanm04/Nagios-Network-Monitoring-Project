# 02 – Install NCPA on Windows Host

I in this doc I will install the **Nagios Cross-Platform Agent (NCPA)** on the Windows host **`comp-a-test`**. NCPA runs as a Windows service, collects system data like CPU, memory, disk, services, and processes, and makes it available to the Nagios server over an HTTPS API on port `5693`.

This doc covers installing and verifying the agent on Windows. 

## Prerequisites

- Administrator rights on **`comp-a-test`**
- An email address to receive the Nagios download link

---

## Step 1 – Download NCPA

1. Go to [**nagios.org/downloads**](https://www.nagios.org/downloads/).
2. Find **Nagios Cross-Platform Agent (NCPA)** and click **Download**.

<img width="926" height="467" alt="1" src="https://github.com/user-attachments/assets/edd9173e-c9a1-4213-97d5-16556f042ab7" />

3. Fill in the **Access Nagios Downloads** form and click **Submit**. Nagios emails you a download link.

<img width="670" height="435" alt="2" src="https://github.com/user-attachments/assets/801a16b8-73b3-4ca4-b5fd-25120242e553" />

<img width="659" height="610" alt="3" src="https://github.com/user-attachments/assets/035f2cf3-4ad6-4252-b481-60d35b6703b0" />

4. Open the email from **`info@nagios.com`** and click **Go to Your Downloads**. Check your promotions and social section of your inbox or spam folder if it doesn't arrive in a few minutes.

<img width="964" height="495" alt="4" src="https://github.com/user-attachments/assets/ba922d68-3a7e-48de-b79f-8a4f1c43ffcc" />

<img width="944" height="674" alt="5" src="https://github.com/user-attachments/assets/e01e3183-ea37-45b8-9eef-411571ae37c6" />

<img width="676" height="265" alt="6" src="https://github.com/user-attachments/assets/d0409557-6bfc-4ae8-82e8-86e83c35b44f" />

The link opens the **Nagios Open Source Download Center** and stays valid for **45 days**. Bookmark it instead of requesting a new link for other Nagios downloads.

<img width="969" height="456" alt="7" src="https://github.com/user-attachments/assets/cdcda666-77cb-403e-a2d0-590a18706f33" />

5. Scroll to **NCPA on Windows** and download the **Windows 64-bit installer** (`ncpa-latest.exe`).

<img width="892" height="706" alt="8" src="https://github.com/user-attachments/assets/427058d2-3290-4a9a-82a6-e97289cc9848" />

---

## Step 2 – Run the Installer

1. Open your **Downloads** folder and double-click **`ncpa-latest.exe`**.
2. Click **Yes** at the User Account Control prompt.

<img width="829" height="221" alt="9" src="https://github.com/user-attachments/assets/6baeb912-b5b2-4b0e-9c0c-d2927b80c715" />

3. On **Welcome to NCPA Setup**, click **Next**.

<img width="497" height="381" alt="10" src="https://github.com/user-attachments/assets/916995ae-8dd7-4a1d-9215-fc23e7b90120" />

4. Read the license agreement (Nagios Software License 1.3) and click **I Agree**.

<img width="496" height="386" alt="11" src="https://github.com/user-attachments/assets/a6452285-6b6c-4583-a92e-7ded5fd4eece" />

---

## Step 3 – Configure the Listener and Token

On the **Listener Configuration** page:
 - Set the log level to warning [keeping the logs small]
 - Configure a token [Required for Nagios to query the agent and to log in to the NCPA web interface]
 - Leave **Bind IP** as **0.0.0.0** [Listens on all network adapters]
 - Leave the **Bind Port** as **5693** [Its the default NCPA port]
 - SSL Minimum is set to **TLSv1_2** [ Blocks older, weaker TLS versions]

> **Treat the token like a password.** Use something long and random, write it down somewhere secure.
  
<img width="498" height="398" alt="12" src="https://github.com/user-attachments/assets/1e56b6e6-5b4c-4bf7-ada7-d0c1c6d6d6d8" />

Click **Next**.

---

## Step 4 – Skip Passive Configuration

On the **Passive Configuration** page, leave **Send passive checks over NRDP** unchecked and click **Next**.

<img width="501" height="389" alt="13" src="https://github.com/user-attachments/assets/e8f9ed38-3945-4b0e-8a38-44f67a30ed90" />

---

## Step 5 – Finish the Install


