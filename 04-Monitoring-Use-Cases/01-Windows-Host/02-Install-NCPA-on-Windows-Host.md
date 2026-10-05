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
