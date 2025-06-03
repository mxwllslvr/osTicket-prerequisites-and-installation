<div align="center">

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">
<br/><br/>

![Github Banner - osTicket Prerequisites and Installation](https://github-production-user-asset-6210df.s3.amazonaws.com/76263416/450453628-5d9e3172-4dff-418d-a651-7801484df0ad.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20250602%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250602T235519Z&X-Amz-Expires=300&X-Amz-Signature=a6bb235da465962759252a3195f3c8aec6baeb1d0ecf1d4e4e6997e91f08f36f&X-Amz-SignedHeaders=host)

<img src="https://github.com/AnderMendoza/AnderMendoza/raw/main/assets/line-neon.gif" width="100%">

</div>

# osTicket: Prerequisites and Installation Guide

This guide walks you through the prerequisites and steps to install osTicket, an open-source help desk ticketing system, on a Windows virtual machine.

## Technologies and Environments

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Connection (mstsc)
- Internet Information Services (IIS)
- MySQL
- PHP
- osTicket

## Operating Systems

- Windows 11 Home (24H2)
- Windows 10 Pro (22H2)

---

## Installation Steps

## Step 1: Create Azure Virtual Machine

### Access Azure Portal and Create VM

![osTicket - Prerequisites and Installation 1](https://github.com/user-attachments/assets/17777f94-9f91-4a0d-b18a-131e1b635216)


1. **Access Azure Portal**: Log into your Azure account and navigate to the Virtual Machines section
2. **Create New VM**: Click "Create" and select "Azure virtual machine"
3. **Configure Basic Settings**:
   - **Subscription**: Select your Azure subscription
   - **Resource Group**: Create new named "RG-osTicket"
   - **Virtual machine name**: osticket-vm
   - **Region**: Choose your preferred region
   - **Image**: Windows 10 Pro, version 22H2
   - **Size**: Standard_D2s_v3 (2 vcpus, 8 GiB memory) - recommended for smooth performance

### Configure Administrator Account

![osTicket - Prerequisites and Installation 2](https://github.com/user-attachments/assets/6b3efcd7-4be6-4529-aa2e-8706f9b4aa67)

1. **Create Administrator Credentials**: Set username and strong password
2. **Licensing**: Check the box "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights"
3. **Networking**: Allow default settings to create new Virtual Network
4. **Review and Create**: Click "Review + Create" then "Create"
5. **Wait for Deployment**: Monitor deployment progress (typically 3-5 minutes)

**Observation**: Your Azure Virtual Machine is now created and ready for remote connection.

---





### Step 2: Download and Extract Installation Files

1. Log into **osTicket-vm** using Remote Desktop Connection (`mstsc`)
2. Open a browser, paste the [osTicket Installation Files](https://drive.usercontent.google.com/download?id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD&export=download&authuser=0&confirm=t&uuid=8cf1d834-66c2-4a02-bec9-baebba57730f&at=ALoNOgmkk8isFnjOIHlNwOyEV9ym%3A1748911963663) link, and download the zip file
3. The file is large; click **Download** and wait for it to complete

After downloading, your **osTicket-Installation-Files** folder should contain:

```
osTicket-Installation-Files/
├── HeidiSQL_12.3.0.6589_Setup.exe
├── mysql-5.5.62-win32.msi  
├── osTicket-v1.15.8.zip
├── php-7.3.8-nts-Win32-VC15-x86.zip
├── PHPManagerForIIS_V1.5.0.msi
├── rewrite_amd64_en-US.msi
└── VC_redist.x86.exe
```

4. In File Explorer, locate the zip file in the Downloads folder
5. Drag the zip file to the Desktop for easy access
6. Right-click the zip file, select **Extract All**, and confirm the Desktop as the destination. The extracted folder will be named **osTicket-Installation-Files**
7. Move the **osTicket-Installation-Files** folder to a prominent spot on the Desktop (e.g., top-right corner). Move the original zip file to the Recycle Bin to avoid confusion

### Step 3: Enable IIS and CGI

1. Open the Start Menu, search for **Control Panel**, and select **Programs and Features**
2. Click **Turn Windows features on or off** on the left
3. In the Windows Features window, check **Internet Information Services**
4. Expand **Internet Information Services** → **World Wide Web Services** → **Application Development Features**
5. Check **CGI**, click **OK**, and wait for the installation to complete. Click **Close** when done
6. Open a browser and navigate to `127.0.0.1`. You should see the default IIS webpage, confirming IIS is enabled

### Step 4: Install PHP and Related Components

1. Open the **osTicket-Installation-Files** folder on the Desktop
2. Install **PHP Manager for IIS** (`PHPManagerForIIS_V1.5.0.msi`). Accept all defaults and complete the installation
3. Install the **Rewrite Module** (`rewrite_amd64_en-US.msi`). Accept defaults and finish the installation
4. In File Explorer, navigate to **C:\** and create a new folder named **PHP**
5. In the **osTicket-Installation-Files** folder, right-click `php-7.3.8-nts-Win32-VC15-x86.zip` and select **Extract All**
6. Click **Browse**, select the **C:\PHP** folder, and extract the files
7. Install the **VC Redistributable** (`VC_redist.x86.exe`) from the **osTicket-Installation-Files** folder. Accept defaults and complete

### Step 5: Install and Configure MySQL

1. From the **osTicket-Installation-Files** folder, run `mysql-5.5.62-win32.msi`
2. Choose **Typical** setup and click **Next**
3. After installation, check **Launch the MySQL Instance Configuration Wizard** and click **Finish**
4. In the wizard, select **Standard Configuration** and click **Next**
5. Keep default settings and click **Next**
6. Set the MySQL username to `root` and password to `ROOT` (all caps). This is critical for the lab, though not secure for production
7. Click **Next**, then **Execute**, and finally **Finish**

### Step 6: Register PHP in IIS

1. Open the Start Menu, search for **IIS**, and run **Internet Information Services (IIS) Manager** as an administrator
2. Double-click **PHP Manager**
3. Click **Register new PHP version**, then browse to **C:\PHP**
4. Select `php-cgi.exe`, click **Open**, and then **OK**
5. In IIS, right-click **osTicket-vm** (under **Connections**), select **Stop**, wait a few seconds, then select **Start** to restart the server

### Step 7: Extract and Configure osTicket

1. In the **osTicket-Installation-Files** folder, right-click `osTicket-v1.15.8.zip` and select **Extract All**. Extract to the **osTicket-Installation-Files** folder
2. Open the extracted **osTicket-v1.15.8** folder, which contains scripts and upload folders
3. In File Explorer, navigate to **C:\inetpub\wwwroot**
4. Drag (do not copy) the **upload** folder from **osTicket-v1.15.8** to **C:\inetpub\wwwroot**
5. Rename the **upload** folder in **wwwroot** to **osTicket** (exact spelling)
6. Restart IIS: In IIS Manager, right-click **osTicket-vm**, select **Stop**, then **Start**

### Step 8: Verify osTicket Installation

1. In IIS Manager, navigate to **osTicket-vm** → **Sites** → **Default Web Site** → **osTicket**
2. Under **Browse Website**, click **Browse *:80 (http)**
3. The osTicket setup page should load, indicating a successful setup so far

### Step 9: Enable PHP Extensions

1. In IIS Manager, navigate to **osTicket-vm** → **Sites** → **Default Web Site** → **osTicket**
2. Double-click **PHP Manager**, then click **Enable or disable an extension**
3. Enable the following extensions by selecting and clicking **Enable**:
   - `php_imap.dll`
   - `php_intl.dll`
   - `php_opcache.dll`
4. Refresh the osTicket setup page in the browser to confirm the extensions are loaded

### Step 10: Configure osTicket Files

1. In File Explorer, navigate to **C:\inetpub\wwwroot\osTicket\include**
2. Locate `ost-sampleconfig.php` and rename it to `ost-config.php` (exact spelling)
3. Right-click `ost-config.php`, select **Properties**, and go to the **Security** tab
4. Click **Advanced**, then **Disable inheritance**
5. Select **Remove all inherited permissions from this object**
6. Click **Add**, then **Select a principal**
7. Type `everyone`, click **Check Names**, and click **OK**
8. Check **Full control**, click **OK**, then **Apply**, and **OK** to close all dialogs

### Step 11: Install HeidiSQL and Configure Database

1. In the **osTicket-Installation-Files** folder, run `HeidiSQL_12.3.0.6589_Setup.exe`
2. Accept defaults, install, and check **Launch HeidiSQL** before clicking **Finish**
3. In HeidiSQL, click **New** to create a session
4. Set the username to `root` and password to `ROOT`. Click **Open**
5. Right-click **Unnamed**, select **Create new** → **Database**
6. Name the database `osTicket` (exact spelling) and click **OK**

### Step 12: Complete osTicket Setup

1. Return to the osTicket setup page in the browser and click **Continue**
2. Fill in the Help Desk details:
   - **Help Desk Name**: Choose a name
   - **Default Email**: Any email (doesn't need to be real)
   - **Admin User**: Use a different email from above
   - **Username**: `adminuser`
   - **Password**: `Password1`
3. Under Database Settings:
   - **MySQL Database**: `osTicket`
   - **MySQL Username**: `root`
   - **MySQL Password**: `ROOT`
4. Click **Install Now**
5. Upon success, note the URLs provided (e.g., Staff Control Panel) for future use

---

![osTicket Success](https://github.com/user-attachments/assets/44e16c30-0cac-440d-aab5-5bd54926b1e3)

---

## Conclusion

You've successfully installed osTicket on a Windows virtual machine! This project demonstrates the setup of a help desk ticketing system, a common tool in IT operations.

**Key Achievements:**
- Configured IIS with PHP support on Windows
- Installed and configured MySQL database
- Successfully deployed osTicket help desk system
- Configured proper file permissions and database connectivity
- Enabled essential PHP extensions for osTicket functionality

In future projects, we'll explore post-installation configurations. Remember to stop your Azure VMs to avoid unnecessary charges.

---

[← Back to Main Page](https://www.github.com/mxwllslvr)
