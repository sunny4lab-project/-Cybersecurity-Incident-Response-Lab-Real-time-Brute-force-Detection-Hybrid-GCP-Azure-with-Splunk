# 🔐 Cybersecurity Incident Response Lab: Real-time Brute-force Detection (Hybrid GCP & Azure) with Splunk

## 🔍 Overview

This project is a real-world hybrid cloud honeynet designed to capture and analyze brute-force login attempts targeting exposed RDP and SSH services. It uses Microsoft Azure and Google Cloud Platform (GCP) to host intentionally vulnerable virtual machines. All security telemetry is ingested into a centralized Splunk SIEM deployed on GCP for monitoring, detection, and forensic analysis.
## ☁️ Cloud Architecture

**Azure:**
- Windows 10 VM (RDP enabled)

**GCP:**
- Ubuntu VM (SSH enabled)
- Splunk Enterprise Server

Real-world external attackers are allowed to interact with the exposed services. No synthetic traffic is generated—this is purely threat actor-driven activity.


![hybrid_cloud_diagram](https://github.com/user-attachments/assets/2a054a08-06b1-43da-9c97-6da242556081)



## 🛠 Tools & Technologies

- **Splunk Enterprise** (GCP)
- **Splunk Universal Forwarder** (Windows/Linux)
- **Sysmon** (for Windows telemetry)
- **Volatility** (for Windows memory forensics)
- **Auditd / UFW / Fail2Ban** (Linux monitoring)
- **Crowd-sourced attackers** (internet-exposed honeypots)

## 🔧 Setup Instructions

### 1. Deploy Virtual Machines
    
  <details><summary>Azure VM creation Details Here</summary>
# Azure VM Creation Guide

This guide provides step-by-step instructions for creating a Virtual Machine (VM) in Azure using the Azure Portal. Each step includes images for better visualization.

## Step 1: Log in to the Azure Portal
1. **Open your web browser** and navigate to the 

2. **Sign in** with your Azure account credentials.

---

## Step 2: Create a New Resource
1. On the Azure Portal dashboard, click on **"Create a resource"** in the left-hand menu.



2. In the "Search the Marketplace" box, type **"Virtual Machine"** and select **"Virtual Machine"** from the dropdown options.

   <img width="689" alt="Screenshot 2024-10-26 181711" src="https://github.com/user-attachments/assets/0194a6e9-9626-4f80-9841-6a45570312a2">


---

## Step 3: Configure Basic Settings
1. **Subscription**: Select the Azure subscription you want to use.
2. **Resource Group**:
   - Create a new resource group by clicking **"Create new"** or select an existing one from the dropdown.
  
   <img width="598" alt="Screenshot 2024-10-26 181619" src="https://github.com/user-attachments/assets/19aa5c91-d26b-4c4b-ac29-45dc83c6e8b2">
   


3. **Virtual Machine Name**: Enter a name for your VM.
4. **Region**: Choose the region where you want your VM to be located (e.g., East US, West Europe).
5. **Availability Options**: Select any availability options based on your needs.
6. **Image**: Choose the operating system you want to use (e.g., Windows Server, Ubuntu).
7. **Size**: Click on the **"Select size"** link to choose the VM size.

<img width="608" alt="Screenshot 2024-10-26 181823" src="https://github.com/user-attachments/assets/eed8f37c-030b-4e6c-abb4-fa41e0ad97b3">

<img width="580" alt="Screenshot 2024-10-26 181920" src="https://github.com/user-attachments/assets/04313f7d-4137-4116-8150-4de708ffd8df">


---

## Step 4: Configure Administrator Account
1. **Username**: Enter the administrator username for your VM.
2. **Password**: Create and confirm a password for the admin account.
3. **Inbound Port Rules**: Select the ports you want to allow.

<img width="583" alt="Screenshot 2024-10-26 191651" src="https://github.com/user-attachments/assets/7544cec1-32af-4b02-877e-e91eaf561f96">

<img width="589" alt="Screenshot 2024-10-26 191900" src="https://github.com/user-attachments/assets/043332d0-a96c-485f-8996-37b3a6fb462c">


---

## Step 5: Configure Networking
1. **Virtual Network**: Choose an existing virtual network or create a new one.
2. **Subnet**: Select a subnet from the dropdown or create a new one.
3. **Public IP**: Ensure you have a public IP assigned to your VM.
4. **Network Security Group**: Select the default option or create a new NSG.

 
<img width="596" alt="Screenshot 2024-10-26 182038" src="https://github.com/user-attachments/assets/131fb72d-030d-41af-95c7-9df1c0b5308c">


---

## Step 6: Configure Management Options
1. **Monitoring**: Choose whether to enable monitoring options, but I will leave it as default.
2. **Identity and Access Management**: If needed, enable managed identity for Azure resources.

   <img width="585" alt="Screenshot 2024-10-26 182108" src="https://github.com/user-attachments/assets/5e0350b8-aa30-423b-8118-004e6e2bc8de">


---


   

---

## Step 7: Review and Create
1. Click on the **"Review + create"** button at the bottom of the page.

<img width="562" alt="Screenshot 2024-10-26 182157" src="https://github.com/user-attachments/assets/5e0114e8-6257-4266-92a2-1fde16061133">


2. Review your settings and click **"Create"** to start the deployment.

---

## Step 8: Wait for Deployment
1. Azure will begin deploying your VM. You will see a progress screen.

   <img width="856" alt="Screenshot 2024-10-26 191157" src="https://github.com/user-attachments/assets/17964e35-4b88-48ae-abff-e58bae0d4182">


2. Once the deployment is complete, you will receive a notification.

---

## Step 9: Access Your VM
1. Go to the **"Virtual machines"** section in the Azure Portal.
2. Click on the VM you just created.
3. To connect to your VM:
   - For Windows VMs, search and click select **"RDP"**.
   - For Linux VMs, search powershell for window and sign-in using your **"SSH"** credentials which include your username and public IP and password

  <img width="304" alt="Screenshot 2024-10-26 182438" src="https://github.com/user-attachments/assets/0cd778b7-9a80-46fa-9320-e362597c8123">

<img width="574" alt="Screenshot 2024-10-26 182352" src="https://github.com/user-attachments/assets/75059cf7-c9d6-4d9a-bbb1-59580cb9ca23">

---

## Conclusion
You have successfully created a Virtual Machine in Azure using the Azure Portal! From here, you can configure your VM further, install applications, or use it for testing and development. 
</details>
#### Azure (Windows)
- Open port 3389 (RDP)
- Set weak credentials for testing

#### GCP (Linux)
<details><summary>GCP VM creation Details Here</summary>

Here’s a step-by-step guide to create a Linux (Ubuntu) VM on Google Cloud Platform (GCP):

✅ Step 1: Set Up a Google Cloud Project
- Go to Google Cloud Console.

- Click on the project drop-down (top-left next to Google Cloud logo).

- Select an existing project or click "New Project".

- Give it a name, optionally set a billing account, then click "Create".

✅ Step 2: Enable Compute Engine API
1. In the GCP Console, navigate to: 

bash 

`` Navigation Menu > Compute Engine > VM instances ``

![Screenshot 2025-05-21 143501](https://github.com/user-attachments/assets/d0ba917b-8bd9-404d-823b-d1eaba2d21a1)

2. Click "Enable" to activate the Compute Engine API if prompted. This may take a few moments.

✅ Step 3: Create a VM Instance
Once you're on the VM instances page, click "Create Instance".

![Screenshot 2025-05-21 143554](https://github.com/user-attachments/assets/bddb843a-7b9a-45cc-972d-5d8839e50d74)


## Configure the VM:
Name: Choose a name (e.g., ubuntu-vm-demo)

![Screenshot 2025-05-21 143619](https://github.com/user-attachments/assets/23b1849d-80ee-4c39-b59d-575be10a0eda)


Region & Zone: Pick your preferred region and zone (e.g., us-central1-a)

Machine Configuration:

Series: E2 (cost-efficient) or N2 if you need more power.

![Screenshot 2025-05-21 143656](https://github.com/user-attachments/assets/0ca813ab-9fa0-48e4-9f65-ab81de7a7535)


Machine type: e2-medium (2 vCPU, 4 GB RAM) is a common choice.

## Boot Disk (OS):
Click on “Change” under the Boot disk section.

![Screenshot 2025-05-21 143743](https://github.com/user-attachments/assets/ae659eb2-75bd-4c6b-891c-04abe061bcc0)


Choose:

Operating System: Ubuntu

![Screenshot 2025-05-21 143755](https://github.com/user-attachments/assets/c9dd2967-d658-40a0-9b12-38a74ee707ce)

Version: Ubuntu 22.04 LTS (or latest available)

Boot disk type: Standard persistent disk (for cost-saving) or SSD (for performance)

Click Select

## Firewall:
Under Firewall, check:

✅ "Allow HTTP traffic" (if you’ll host a web server)

✅ "Allow HTTPS traffic"

![Screenshot 2025-05-21 143838](https://github.com/user-attachments/assets/90c8b511-594e-4e48-a857-007ef965ddee)


✅ Step 4: Create the VM
Click the “Create” button at the bottom.

![Screenshot 2025-07-03 142507](https://github.com/user-attachments/assets/5a2b7b30-4854-453f-98fd-0b7a49736bdb)


GCP will now provision your Ubuntu VM. This may take 30–60 seconds.

✅ Step 5: Connect to the VM (via SSH)
Once your instance is running:

On the VM list, click "SSH" next to your instance.

![Screenshot 2025-07-04 172823](https://github.com/user-attachments/assets/3b484150-4e84-4d68-b5e8-c70fbd76bc22)


It will open a browser-based SSH terminal.


You now have shell access to your Ubuntu VM!

![Screenshot 2025-07-04 172950](https://github.com/user-attachments/assets/f0838dea-2ad7-4793-bb8e-ac4f15215396)


</details>
- Open port 22 (SSH)
- Monitor `/var/log/auth.log`

#
### 2. Install Splunk  
<details><summary>Details Here</summary>

Let's walk through the Splunk installation process step by step. Splunk is a powerful platform for searching, monitoring, and analyzing machine-generated big data. The process can vary slightly depending on your operating system, but I'll guide you through the basic installation on Linux and Windows.

✅ 1. Download Splunk
For Linux (DEB):
- Visit the Splunk download page. ``https://www.splunk.com/en_us/download``
- Sign-in or create an account and it would give you access to the download page.

Choose the version for Linux (RPM/DEB) depending on your system architecture:

But for this Project I will be using DED for Debian-based systems (Ubuntu, etc.)

![Screenshot 2025-07-04 175645](https://github.com/user-attachments/assets/d8ecdf25-4465-4f13-bcd2-90a87963ba22)


DEB-based (Ubuntu, Debian):
- Open a terminal. 
- Paste the deb command you copied : ``  sudo wget -O splunkforwarder-9.4.3-237ebbd22314-linux-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/9.4.3/linux/splunkforwarder-9.4.3-237ebbd22314-linux-amd64.deb" ``
- 
![Screenshot 2025-07-04 175245](https://github.com/user-attachments/assets/388c59c9-e090-4a7c-8e9e-9a2c952ff58f)

  
- Install the downloaded .deb package using dpkg. `` sudo dpkg -i ``

✅ 2. Start Splunk
Linux:
1. After installation, you need to start Splunk from the command line. Go to the directory where Splunk is installed:

``cd /opt/splunk/bin``

2. Start Splunk:

   ``sudo ./splunk start --accept-license``

  - Important: The --accept-license flag ensures that you accept the Splunk license agreement.

  - Splunk will prompt you to create a username and password for the admin account.

    #
✅ 3. Access Splunk Web Interface
Once Splunk is running, you can access the web interface to start configuring and using Splunk.

Open a web browser and navigate to:

For local installation: http://localhost:8000

For remote installation: http://<server-ip>:8000

You should be prompted to log in with the admin username and password you created during the installation process.

    

</details>


#
### 3. Install Splunk Universal Forwarder

#### On Windows (Azure VM):
- Install Sysmon
- Forward Security and Sysmon logs to Splunk

#### On Linux (GCP Ubuntu VM):
- Forward `/var/log/auth.log`

```ini
[monitor:///var/log/auth.log]
sourcetype = linux_secure
index = linux
```

