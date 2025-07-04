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
- Open port 22 (SSH)
- Monitor `/var/log/auth.log`

### 2. Install Splunk Universal Forwarder

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

