# AZ-104 Azure Administrator Certification - Complete 2-Week Learning Path


## **Week 1: Core Azure Fundamentals + Identity + Compute + Storage**

---

### **Day 1-2: Azure Fundamentals & Identity (Entra ID)**

#### **📚 THEORY**

**What is Azure?**
- Microsoft's cloud computing platform
- Think of it as renting computers, storage, and services over the internet instead of buying physical servers
- You pay only for what you use (like paying for electricity)

**Azure Account Hierarchy:**
      ```
          Management Group (optional)
              ↓
          Subscription (billing boundary)
              ↓
          Resource Group (logical container)
              ↓
          Resources (VMs, Storage, etc.)
      ```

**Key Concepts:**
1. **Subscription**: Your billing account - like a credit card for Azure
2. **Resource Group**: A folder that holds related resources (VM + its disk + network)
3. **Region**: Physical location of data centers (e.g., East US, West Europe)
4. **Availability Zones**: Separate buildings within a region for high availability

**Microsoft Entra ID (formerly Azure AD):**
- Identity service for Azure (like your company's employee directory)
- **Users**: Individual accounts (john@company.com)
- **Groups**: Collections of users
- **Roles**: What users can do (Owner, Contributor, Reader)

**RBAC (Role-Based Access Control):**
- **Owner**: Full control including assigning permissions
- **Contributor**: Can create/manage resources but can't assign permissions
- **Reader**: Can only view resources

---

#### **🛠️ HANDS-ON: Day 1-2**

**Exercise 1: Create Free Azure Account**

1. Go to https://azure.microsoft.com/free
2. Click "Start free"
3. Sign in with Microsoft account (create one if needed)
4. Enter credit card (won't be charged, just verification)
5. Complete verification

**Exercise 2: Navigate Azure Portal**

1. Go to https://portal.azure.com
2. Click the hamburger menu (≡) on top left
3. Explore these sections:
   - **Home**: Dashboard
   - **All resources**: Everything you create
   - **Resource groups**: Your containers
   - **Subscriptions**: Your billing accounts

**Exercise 3: Create Your First Resource Group**

1. In Azure Portal, search "Resource Groups" in top search bar
2. Click **+ Create**
3. Fill in:
   - **Subscription**: Select your free subscription
   - **Resource group name**: `rg-learning-eastus`
   - **Region**: East US
4. Click **Review + Create** → **Create**

**Exercise 4: Create Users in Entra ID**

1. Search "Microsoft Entra ID" in top search bar
2. Click **Users** in left menu
3. Click **+ New user** → **Create new user**
4. Fill in:
   - **User principal name**: testuser1
   - **Display name**: Test User One
   - **Password**: Click "Auto-generate" and COPY IT
5. Click **Create**
6. Repeat to create `testuser2`

**Exercise 5: Assign RBAC Roles**

1. Go to your resource group `rg-learning-eastus`
2. Click **Access control (IAM)** in left menu
3. Click **+ Add** → **Add role assignment**
4. **Role tab**: Select "Reader"
5. **Members tab**: Click "+ Select members"
6. Search for `testuser1`, select it
7. Click **Select** → **Review + assign**

**Exercise 6: Verify Access**

1. Open an Incognito/Private browser window
2. Go to https://portal.azure.com
3. Sign in as testuser1 (use the password you copied)
4. Try to view the resource group (should work)
5. Try to create a new resource (should fail - Reader role)

---

### **Day 3-4: Virtual Machines & Compute**

#### **📚 THEORY**

**Virtual Machines (VMs):**
- A computer running inside Azure's data center
- You control the operating system, software, and configuration
- Like renting a computer instead of buying one

**VM Components:**
1. **OS Disk**: Where Windows/Linux is installed (like C: drive)
2. **Data Disks**: Additional storage you can attach (like D:, E: drives)
3. **Network Interface (NIC)**: Connects VM to network
4. **Public IP**: Address to access VM from internet
5. **Network Security Group (NSG)**: Firewall rules

**VM Sizes:**
      ```
      B-Series: Burstable, low cost (testing, dev)
      D-Series: General purpose (balanced CPU/RAM)
      E-Series: Memory optimized (databases)
      F-Series: CPU optimized (computation)
      ```

Example: **Standard_B2s**
- Standard = Tier
- B = B-series (burstable)
- 2 = 2 vCPUs
- s = SSD storage

**Availability Options:**
1. **Single VM**: One instance (99.9% SLA with Premium SSD)
2. **Availability Set**: VMs spread across racks in one datacenter (99.95% SLA)
3. **Availability Zone**: VMs spread across different buildings (99.99% SLA)

**VM States:**
- **Running**: VM is on, you're being charged for compute
- **Stopped**: VM is off, still charged for storage/IP
- **Deallocated**: VM is off, only charged for storage

---

#### **🛠️ HANDS-ON: Day 3-4**

**Exercise 1: Create a Windows VM**

1. Search "Virtual machines" in Azure Portal
2. Click **+ Create** → **Azure virtual machine**
3. **Basics tab:**
   - **Subscription**: Your subscription
   - **Resource group**: rg-learning-eastus
   - **Virtual machine name**: `vm-windows-test`
   - **Region**: East US
   - **Availability options**: No infrastructure redundancy required
   - **Image**: Windows Server 2022 Datacenter
   - **Size**: Click "See all sizes" → Select **Standard_B2s** (2 vCPUs, 4GB RAM)
   - **Username**: azureuser
   - **Password**: Create a strong password (WRITE IT DOWN!)
   - **Public inbound ports**: Allow selected ports
   - **Select inbound ports**: RDP (3389)

4. **Disks tab:**
   - **OS disk type**: Standard SSD
   - Leave other defaults

5. **Networking tab:**
   - **Virtual network**: (new) rg-learning-eastus-vnet
   - **Subnet**: (new) default (10.0.0.0/24)
   - **Public IP**: (new) vm-windows-test-ip
   - **NIC network security group**: Basic
   - **Public inbound ports**: Allow selected ports (RDP)

6. **Management tab:**
   - Scroll to **Auto-shutdown**
   - **Enable auto-shutdown**: Yes
   - **Shutdown time**: 7:00 PM (to save costs)

7. Click **Review + create** → **Create**
8. Wait 3-5 minutes for deployment

**Exercise 2: Connect to Windows VM**

1. Go to the VM → Click **Connect** → **RDP**
2. Click **Download RDP File**
3. Open the downloaded file
4. Enter username: `azureuser` and your password
5. Accept certificate warning
6. You're now inside a Windows server in Azure!
7. Open Notepad and type "I'm in Azure!"
8. Close RDP connection

**Exercise 3: Create a Linux VM**

1. Virtual machines → **+ Create**
2. **Basics:**
   - **Resource group**: rg-learning-eastus
   - **VM name**: `vm-linux-web`
   - **Image**: Ubuntu Server 22.04 LTS
   - **Size**: Standard_B2s
   - **Authentication type**: Password
   - **Username**: azureuser
   - **Password**: (same as before for consistency)
   - **Inbound ports**: SSH (22), HTTP (80)

3. **Disks**: Standard SSD
4. **Networking**: Same VNet as Windows VM
5. **Management**: Enable auto-shutdown
6. **Review + create** → **Create**

**Exercise 4: Connect to Linux VM and Install Web Server**

1. Go to Linux VM overview page
2. Copy the **Public IP address**
3. On Windows: Open PowerShell or Command Prompt
   - On Mac/Linux: Open Terminal

4. Type: `ssh azureuser@<YOUR-VM-IP>`
   - Example: `ssh azureuser@20.121.45.67`
5. Type `yes` to accept fingerprint
6. Enter your password

7. **Install Nginx web server:**
      ```bash
      # Update package list
      sudo apt update
      
      # Install Nginx
      sudo apt install nginx -y
      
      # Verify it's running
      sudo systemctl status nginx
      ```

8. Press `q` to exit status view

9. **Create a simple web page:**
    ```bash
    # Edit the default page
    echo "<h1>Hello from Azure VM!</h1>" | sudo tee /var/www/html/index.html
    ```

10. Open a browser and go to: `http://<YOUR-VM-IP>`
    - You should see "Hello from Azure VM!"

**Exercise 5: Manage VM Disks**

1. Go to your Linux VM → **Stop** it (click Stop button)
2. Wait until status shows "Stopped (deallocated)"
3. Click **Disks** in left menu
4. Click **+ Create and attach a new disk**
5. **Disk name**: `vm-linux-web-data01`
6. **Size**: Change to 32 GiB
7. **Encryption**: (Default) Platform-managed key
8. Click **Save**
9. **Start** the VM again

**Exercise 6: Use the Disk in Linux**

1. SSH back into the Linux VM
2. List disks:
```bash
lsblk
```
You'll see something like:
```
NAME   SIZE
sda     30G  (OS disk)
sdb      8G  (temp disk)
sdc     32G  (new data disk - unmounted)
```

3. **Format and mount the new disk:**
```bash
# Create partition
sudo fdisk /dev/sdc
# Type: n (new partition)
# Type: p (primary)
# Press Enter 3 times (accept defaults)
# Type: w (write changes)

# Format it
sudo mkfs.ext4 /dev/sdc1

# Create mount point
sudo mkdir /datadisk

# Mount it
sudo mount /dev/sdc1 /datadisk

# Verify
df -h
```

4. **Make it permanent (survive reboots):**
```bash
# Get the disk UUID
sudo blkid /dev/sdc1

# Edit fstab
sudo nano /etc/fstab

# Add this line at the end (replace UUID with yours):
UUID=your-uuid-here /datadisk ext4 defaults 0 0

# Save: Ctrl+X, Y, Enter
```

**Exercise 7: Create Custom VM Image**

1. In SSH, prepare the VM:
```bash
# Install additional software
sudo apt install htop -y

# Clean up
sudo waagent -deprovision+user -force
```

2. Exit SSH: `exit`

3. In Azure Portal:
   - Go to the VM → **Stop** → **Deallocate**
   - Click **Capture** (top menu)
   - **Resource group**: rg-learning-eastus
   - **Image name**: `img-ubuntu-nginx`
   - **Automatically delete this VM**: No
   - Click **Review + create** → **Create**

---

### **Day 5-6: Networking**

#### **📚 THEORY**

**Virtual Network (VNet):**
- Private network in Azure (like your office network)
- Resources in same VNet can communicate privately
- Isolated from other VNets by default

**IP Address Concepts:**
- **Private IP**: Internal address (10.x.x.x, 192.168.x.x)
- **Public IP**: Internet-facing address
- **Static**: Never changes
- **Dynamic**: Can change when resource restarts

**Subnets:**
- Segments within a VNet
- Example: 10.0.0.0/16 VNet divided into:
  - 10.0.1.0/24 - Web tier (256 addresses)
  - 10.0.2.0/24 - App tier
  - 10.0.3.0/24 - Database tier

**Network Security Group (NSG):**
- Firewall rules for subnets or NICs
- Rules have:
  - **Priority**: Lower number = higher priority (100 before 200)
  - **Source/Destination**: IP, Service Tag, or Any
  - **Port**: 80 (HTTP), 443 (HTTPS), 22 (SSH), 3389 (RDP)
  - **Action**: Allow or Deny

**VNet Peering:**
- Connects two VNets so they can communicate
- Traffic stays on Microsoft backbone (secure, fast)

**Azure DNS:**
- Name resolution service
- Private DNS zones for VNet resources

---

#### **🛠️ HANDS-ON: Day 5-6**

**Exercise 1: Create Custom VNet Architecture**

1. Search "Virtual networks" → **+ Create**
2. **Basics:**
   - **Resource group**: Create new: `rg-networking-lab`
   - **Name**: `vnet-hub-eastus`
   - **Region**: East US

3. **IP Addresses tab:**
   - **IPv4 address space**: Delete default, add `10.1.0.0/16`
   - **+ Add subnet**:
     - **Name**: `subnet-web`
     - **Address range**: `10.1.1.0/24`
   - **+ Add subnet**:
     - **Name**: `subnet-db`
     - **Address range**: `10.1.2.0/24`

4. **Review + create** → **Create**

**Exercise 2: Create Second VNet for Peering**

1. Create another VNet:
   - **Name**: `vnet-spoke-eastus`
   - **Address space**: `10.2.0.0/16`
   - **Subnet**: `subnet-apps` (10.2.1.0/24)

**Exercise 3: Configure VNet Peering**

1. Go to `vnet-hub-eastus`
2. Click **Peerings** → **+ Add**
3. **This virtual network:**
   - **Peering link name**: `hub-to-spoke`
   - **Allow traffic to remote virtual network**: Yes
   - **Allow forwarded traffic**: Yes

4. **Remote virtual network:**
   - **Peering link name**: `spoke-to-hub`
   - **Virtual network**: vnet-spoke-eastus
   - **Allow traffic from remote**: Yes

5. Click **Add**
6. Wait 30 seconds - Status should show "Connected"

**Exercise 4: Create and Configure NSG**

1. Search "Network security groups" → **+ Create**
2. **Resource group**: rg-networking-lab
3. **Name**: `nsg-web-tier`
4. **Region**: East US
5. **Create**

6. Go to the NSG → **Inbound security rules**
7. **+ Add** rule:
   - **Source**: Any
   - **Source port ranges**: *
   - **Destination**: Any
   - **Service**: HTTP
   - **Action**: Allow
   - **Priority**: 100
   - **Name**: `AllowHTTP`

8. **+ Add** another rule:
   - **Service**: HTTPS
   - **Priority**: 110
   - **Name**: `AllowHTTPS`

9. **+ Add** deny rule:
   - **Source**: Any
   - **Destination port ranges**: 22
   - **Protocol**: TCP
   - **Action**: Deny
   - **Priority**: 200
   - **Name**: `DenySSH`

**Exercise 5: Associate NSG with Subnet**

1. In the NSG, click **Subnets** → **+ Associate**
2. **Virtual network**: vnet-hub-eastus
3. **Subnet**: subnet-web
4. Click **OK**

**Exercise 6: Test Network Connectivity**

1. Create a VM in each VNet:
   - VM1: `vm-hub` in vnet-hub-eastus, subnet-web
   - VM2: `vm-spoke` in vnet-spoke-eastus, subnet-apps
   - Use Ubuntu, Standard_B1s, password auth

2. SSH into vm-hub
3. Ping vm-spoke's private IP:
```bash
ping <vm-spoke-private-ip>
```
Should work because of VNet peering!

4. Install curl and test:
```bash
sudo apt update && sudo apt install curl -y
curl http://<vm-spoke-private-ip>
```

**Exercise 7: Create Azure Bastion (Secure Access)**

1. Go to vnet-hub-eastus
2. Click **Subnets** → **+ Subnet**
3. **Name**: `AzureBastionSubnet` (MUST be exactly this)
4. **Address range**: `10.1.3.0/26` (minimum /26 required)
5. **Save**

6. Search "Bastions" → **+ Create**
7. **Resource group**: rg-networking-lab
8. **Name**: `bastion-hub`
9. **Region**: East US
10. **Virtual network**: vnet-hub-eastus
11. **Subnet**: AzureBastionSubnet (auto-selected)
12. **Public IP**: Create new: `bastion-hub-ip`
13. **Review + create** (takes ~10 minutes)

14. After deployment, go to vm-hub → **Connect** → **Bastion**
15. Enter username and password
16. Connect - you're now in the VM without RDP/SSH!

---

### **Day 7: Storage Accounts**

#### **📚 THEORY**

**Azure Storage Account:**
- Massively scalable cloud storage
- Unique namespace: `mystorageaccount.blob.core.windows.net`

**Storage Services:**
1. **Blob Storage**: Object storage (files, images, videos)
   - Block blobs: General purpose
   - Append blobs: Log files
   - Page blobs: VM disks

2. **File Storage**: SMB file shares (mountable in VMs)

3. **Queue Storage**: Message queuing

4. **Table Storage**: NoSQL key-value store

**Blob Access Tiers:**
- **Hot**: Frequently accessed (highest storage cost, lowest access cost)
- **Cool**: Infrequently accessed, stored 30+ days
- **Archive**: Rarely accessed, stored 180+ days (must rehydrate to access)

**Redundancy Options:**
- **LRS**: Locally redundant (3 copies in one datacenter) - 99.999999999%
- **ZRS**: Zone redundant (3 copies across availability zones)
- **GRS**: Geo-redundant (6 copies across 2 regions)
- **GZRS**: Geo-zone redundant

**Security:**
- **Shared Access Signature (SAS)**: Time-limited access token
- **Managed Identity**: Azure resources access without keys
- **Access Keys**: Full access (like password)

---

#### **🛠️ HANDS-ON: Day 7**

**Exercise 1: Create Storage Account**

1. Search "Storage accounts" → **+ Create**
2. **Basics:**
   - **Resource group**: Create new: `rg-storage-lab`
   - **Storage account name**: `salearnYYYYMMDD` (must be globally unique, lowercase, no hyphens)
   - **Region**: East US
   - **Performance**: Standard
   - **Redundancy**: Locally-redundant storage (LRS)

3. **Advanced tab:**
   - **Allow enabling public access**: Checked
   - **Minimum TLS version**: Version 1.2

4. **Review + create** → **Create**

**Exercise 2: Create Blob Container and Upload Files**

1. Go to the storage account → **Containers**
2. **+ Container**
   - **Name**: `website-files`
   - **Public access level**: Blob (anonymous read access)
3. **Create**

4. Click the container → **Upload**
5. Click "Browse for files"
6. Create a simple HTML file on your computer:

**index.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Azure Website</title>
</head>
<body>
    <h1>Hello from Azure Blob Storage!</h1>
    <p>This file is stored in the cloud.</p>
</body>
</html>
```

7. Upload it to the container
8. Click on the uploaded file → Copy the **URL**
9. Open URL in browser - you should see your webpage!

**Exercise 3: Generate SAS Token**

1. Go to storage account → **Shared access signature** (left menu)
2. **Allowed services**: Check Blob
3. **Allowed resource types**: Check Container, Object
4. **Allowed permissions**: Read, List
5. **Start and expiry date/time**: Set expiry 24 hours from now
6. **Generate SAS and connection string**
7. **Copy the Blob service SAS URL**

8. Create another container:
   - **Name**: `private-files`
   - **Public access level**: Private

9. Upload a text file to `private-files`
10. Try to access its URL directly - should get error
11. Append the SAS token to the URL: `<blob-url>?<sas-token>`
12. Now it should work!

**Exercise 4: Enable Static Website Hosting**

1. Storage account → **Static website** (under Data management)
2. **Static website**: Enabled
3. **Index document name**: `index.html`
4. **Error document path**: `404.html`
5. **Save**
6. Copy the **Primary endpoint** URL

7. Go to **Containers** - notice new `$web` container
8. Upload your `index.html` to `$web`
9. Visit the primary endpoint URL - your site is live!

**Exercise 5: Configure Lifecycle Management**

1. Storage account → **Lifecycle management**
2. **+ Add rule**
3. **Rule name**: `move-to-cool`
4. **Rule scope**: Limit blobs with filters
5. **Blob type**: Block blobs
6. **Blob subtype**: Base blobs

7. **Next** → **Base blobs:**
   - **If base blobs were last modified**: More than 30 days ago
   - **Then**: Move to cool storage
8. **Add**

**Exercise 6: Create File Share and Mount in VM**

1. Storage account → **File shares** → **+ File share**
2. **Name**: `shared-data`
3. **Tier**: Transaction optimized
4. **Create**

5. Click the share → **Upload** some files

6. Click **Connect** (top menu)
7. **Operating system**: Linux
8. Copy the script shown

9. SSH into one of your Linux VMs
10. Paste and run the script:
```bash
sudo mkdir /mnt/shareddata
sudo mount -t cifs //salearn....file.core.windows.net/shared-data /mnt/shareddata -o vers=3.0,username=salearn...,password=<key>,dir_mode=0777,file_mode=0777
```

11. Verify:
```bash
ls /mnt/shareddata
```

12. Create a file:
```bash
echo "Created from VM" | sudo tee /mnt/shareddata/test.txt
```

13. Check in Azure Portal - file should appear!

**Exercise 7: Use AzCopy for Bulk Operations**

1. Download AzCopy:
```bash
# On Linux VM
wget https://aka.ms/downloadazcopy-v10-linux
tar -xvf downloadazcopy-v10-linux
sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
```

2. Create SAS token for storage account (like Exercise 3)

3. Copy files:
```bash
# Upload directory
azcopy copy "/local/path/*" "https://salearn...blob.core.windows.net/container?<SAS-token>" --recursive

# Download
azcopy copy "https://salearn...blob.core.windows.net/container?<SAS-token>" "/local/path" --recursive
```

---

## **Week 2: Advanced Topics + Monitoring + Governance + Practice**

---

### **Day 8-9: Azure App Service & Databases**

#### **📚 THEORY**

**Azure App Service:**
- Platform-as-a-Service (PaaS) for web apps
- You don't manage VMs - just deploy code
- Supports: .NET, Node.js, Python, PHP, Java
- Auto-scaling, SSL certificates, deployment slots

**App Service Plans:**
- **Free/Shared**: For dev/test, shared resources
- **Basic**: Dedicated VMs, no auto-scale
- **Standard**: Auto-scale, staging slots
- **Premium**: Better performance, VNet integration

**Deployment Slots:**
- Separate environments (staging, production)
- Swap slots for zero-downtime deployments

**Azure SQL Database:**
- Managed SQL Server database
- Microsoft handles backups, patching, HA
- **DTU model**: Bundled compute/storage
- **vCore model**: Separate compute/storage

**Connection Security:**
- **Firewall rules**: Allow specific IPs
- **VNet integration**: Private connectivity
- **Private endpoints**: No public internet exposure

---

#### **🛠️ HANDS-ON: Day 8-9**

**Exercise 1: Create App Service (Web App)**

1. Search "App Services" → **+ Create**
2. **Basics:**
   - **Resource group**: Create new: `rg-webapp-lab`
   - **Name**: `webapp-learn-YYYYMMDD` (globally unique)
   - **Publish**: Code
   - **Runtime stack**: Python 3.11
   - **Region**: East US
   - **Pricing plan**: Click "Explore pricing plans"
     - Select **Free F1** → **Select**

3. **Review + create** → **Create**

4. Go to the App Service → **URL** (at top) → Click it
   - Should see "Your web app is running and waiting for your content"

**Exercise 2: Deploy Sample Application**

1. In App Service → **Deployment Center**
2. **Source**: Local Git
3. **Save**
4. Copy the **Git Clone Uri**

5. On your local computer (install Git if needed):
```bash
# Create sample app
mkdir my-azure-app
cd my-azure-app

# Create requirements.txt
echo "Flask==2.3.0" > requirements.txt

# Create app.py
cat > app.py << 'EOF'
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return '<h1>Hello from Azure App Service!</h1><p>Deployed via Git</p>'

if __name__ == '__main__':
    app.run()
EOF
```

6. Initialize Git and push:
```bash
git init
git add .
git commit -m "Initial commit"

# Get deployment credentials
# Go to App Service → Deployment Center → FTPS credentials
# Copy username and password

git remote add azure <Git-Clone-Uri>
git push azure master
```

7. Visit your app URL - should see your Flask app!

**Exercise 3: Create Deployment Slot**

1. App Service → **Deployment slots** → **+ Add Slot**
2. **Name**: staging
3. **Clone settings from**: (your production slot)
4. **Add**

5. Click on **staging** slot
6. Modify app.py locally:
```python
@app.route('/')
def hello():
    return '<h1>STAGING VERSION</h1><p>Testing new features</p>'
```

7. Commit and push to staging:
```bash
git commit -am "Staging changes"
git remote add staging <staging-git-url>
git push staging master
```

8. Visit staging URL
9. In portal, click **Swap** to promote staging to production
10. Visit production URL - changes are live!

**Exercise 4: Create Azure SQL Database**

1. Search "SQL databases" → **+ Create**
2. **Basics:**
   - **Resource group**: rg-webapp-lab
   - **Database name**: `sqldb-customers`
   - **Server**: Create new:
     - **Server name**: `sqlserver-learn-YYYYMMDD`
     - **Location**: East US
     - **Authentication method**: Use SQL authentication
     - **Server admin login**: sqladmin
     - **Password**: (strong password)
   - **Want to use elastic pool**: No
   - **Compute + storage**: Configure database
     - Select **Basic** (5 DTU) → **Apply**

3. **Networking tab:**
   - **Connectivity method**: Public endpoint
   - **Firewall rules**:
     - **Allow Azure services**: Yes
     - **Add current client IP**: Yes

4. **Review + create** → **Create** (takes 3-5 minutes)

**Exercise 5: Connect to SQL Database**

1. Go to the SQL Database → **Query editor** (left menu)
2. Login with:
   - **SQL server authentication**
   - **Login**: sqladmin
   - **Password**: (your password)

3. Run these SQL commands:
```sql
-- Create table
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY IDENTITY(1,1),
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    Email NVARCHAR(100),
    CreatedDate DATETIME DEFAULT GETDATE()
);

-- Insert data
INSERT INTO Customers (FirstName, LastName, Email)
VALUES 
    ('John', 'Doe', 'john@example.com'),
    ('Jane', 'Smith', 'jane@example.com'),
    ('Bob', 'Johnson', 'bob@example.com');

-- Query data
SELECT * FROM Customers;
```

**Exercise 6: Connect Web App to SQL Database**

1. SQL Database → **Connection strings** (left menu)
2. Copy the **ADO.NET** connection string
3. Replace `{your_password}` with your actual password

4. Go to App Service → **Configuration** → **Connection strings**
5. **+ New connection string**:
   - **Name**: `SQLCONNECTION`
   - **Value**: (paste connection string)
   - **Type**: SQL Azure
6. **Save**

7. Update your Flask app to use the database:
```python
import pyodbc
from flask import Flask
import os

app = Flask(__name__)

@app.route('/')
def hello():
    return '<h1>Customer List</h1><a href="/customers">View Customers</a>'

@app.route('/customers')
def customers():
    # Get connection string from environment
    conn_str = os.environ.get('SQLCONNECTION')
    
    # Connect to database
    conn = pyodbc.connect(conn_str)
    cursor = conn.cursor()
    
    # Query customers
    cursor.execute("SELECT FirstName, LastName, Email FROM Customers")
    rows = cursor.fetchall()
    
    # Build HTML
    html = '<h1>Customers</h1><ul>'
    for row in rows:
        html += f'<li>{row[0]} {row[1]} - {row[2]}</li>'
    html += '</ul>'
    
    conn.close()
    return html
```

8. Update requirements.txt:
```
Flask==2.3.0
pyodbc==4.0.39
```

9. Commit and push - visit `/customers` route!

---

### **Day 10: Monitoring & Diagnostics**

#### **📚 THEORY**

**Azure Monitor:**
- Centralized monitoring platform
- Collects metrics and logs from all Azure resources

**Log Analytics Workspace:**
- Centralized log storage
- Query using KQL (Kusto Query Language)

**Application Insights:**
- Application performance monitoring
- Tracks requests, failures, dependencies

**Metrics vs Logs:**
- **Metrics**: Numeric time-series data (CPU %, memory)
- **Logs**: Event-based data (errors, warnings)

**Alerts:**
- Notifications based on conditions
- **Action Groups**: Who gets notified (email, SMS, webhook)

**Diagnostic Settings:**
- Route platform logs to:
  - Log Analytics workspace
  - Storage account
  - Event Hub

---

#### **🛠️ HANDS-ON: Day 10**

**Exercise 1: Create Log Analytics Workspace**

1. Search "Log Analytics workspaces" → **+ Create**
2. **Resource group**: Create new: `rg-monitoring-lab`
3. **Name**: `law-monitoring-eastus`
4. **Region**: East US
5. **Review + create** → **Create**

**Exercise 2: Enable Diagnostics for VM**

1. Go to one of your VMs (e.g., vm-linux-web)
2. Click **Diagnostic settings** (under Monitoring)
3. **+ Add diagnostic setting**
4. **Diagnostic setting name**: `vm-logs-to-law`
5. **Logs**: Select all categories
6. **Metrics**: Check "AllMetrics"
7. **Destination details**: Check "Send to Log Analytics workspace"
8. **Log Analytics workspace**: law-monitoring-eastus
9. **Save**

**Exercise 3: Query Logs with KQL**

1. Go to Log Analytics workspace → **Logs**
2. Close the "Queries" popup
3. Run these queries:

**Query 1: Recent Events**
```kql
// All events from last 24 hours
Event
| where TimeGenerated > ago(24h)
| take 100
```

**Query 2: Performance Metrics**
```kql
// CPU usage over time
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated > ago(1h)
| summarize AvgCPU = avg(CounterValue) by bin(TimeGenerated, 5m)
| render timechart
```

**Query 3: Syslog Errors**
```kql
// Linux system errors
Syslog
| where SeverityLevel == "err"
| project TimeGenerated, Computer, Facility, SyslogMessage
| order by TimeGenerated desc
```

**Query 4: Heartbeat (VM Health)**
```kql
// VMs that haven't sent heartbeat in last hour
Heartbeat
| summarize LastHeartbeat = max(TimeGenerated) by Computer
| where LastHeartbeat < ago(1h)
```

**Exercise 4: Create Metric Alert**

1. Go to one of your VMs → **Alerts** (under Monitoring)
2. **+ Create** → **Alert rule**
3. **Condition**:
   - Click **Add condition**
   - **Signal name**: Percentage CPU
   - **Threshold**: Static
   - **Operator**: Greater than
   - **Threshold value**: 80
   - **Check every**: 1 minute
   - **Lookback period**: 5 minutes
   - **Done**

4. **Actions**:
   - Click **Create action group**
   - **Resource group**: rg-monitoring-lab
   - **Action group name**: `ag-email-alerts`
   - **Display name**: Email Alerts
   - **Next: Notifications**
   - **Notification type**: Email/SMS message/Push/Voice
   - **Name**: Email Admin
   - **Email**: (your email)
   - **Review + create** → **Create**

5. **Details**:
   - **Alert rule name**: `High CPU Alert`
   - **Severity**: 2 - Warning
   - **Enable upon creation**: Yes

6. **Review + create** → **Create**

**Exercise 5: Test the Alert**

1. SSH into the Linux VM
2. Generate CPU load:
```bash
# Install stress tool
sudo apt install stress -y

# Create CPU load (80%+ for 5 minutes)
stress --cpu 4 --timeout 300
```

3. Wait 5-10 minutes
4. Check your email - you should receive an alert!
5. In Azure Portal → VM → **Alerts** - you'll see the fired alert

**Exercise 6: Enable Application Insights for Web App**

1. Go to your App Service (webapp-learn-...)
2. Click **Application Insights** (under Settings)
3. **Turn on Application Insights**
4. **Create new resource**:
   - **Name**: appinsights-webapp-learn
   - **Log Analytics Workspace**: law-monitoring-eastus
5. **Apply** → **Yes** (to restart app)

6. Wait 2-3 minutes, then visit your web app URL multiple times
7. Go to **Application Insights** → **Live Metrics**
   - You'll see real-time requests, dependencies, and performance

**Exercise 7: Analyze Application Performance**

1. In Application Insights → **Performance**
2. Click on your operation (GET /)
3. View:
   - **Duration**: How long requests take
   - **Dependencies**: Database calls, external APIs
   - **Samples**: Individual request traces

4. Click **Failures** to see errors
5. Click **Application map** to visualize architecture

**Exercise 8: Create Custom Log Query**

1. Application Insights → **Logs**
2. Query recent requests:
```kql
// Requests in last hour
requests
| where timestamp > ago(1h)
| project timestamp, name, url, duration, resultCode
| order by timestamp desc
```

**Query slow requests:**
```kql
// Requests taking more than 1 second
requests
| where duration > 1000
| summarize Count = count(), AvgDuration = avg(duration) by name
| order by AvgDuration desc
```

**Exercise 9: Create Availability Test**

1. Application Insights → **Availability**
2. **+ Add Standard test**
3. **Test name**: `Homepage Availability`
4. **URL**: (your web app URL)
5. **Test frequency**: 5 minutes
6. **Test locations**: Select 3-5 locations worldwide
7. **Success criteria**:
   - **HTTP status code**: 200
   - **Test timeout**: 30 seconds
8. **Alerts**: Enable
9. **Create**

10. Wait 10-15 minutes, then check the availability chart

**Exercise 10: Set Up Cost Alerts**

1. Search "Cost Management + Billing"
2. Click **Cost Management** → **Budgets**
3. **+ Add**
4. **Scope**: Select your subscription
5. **Budget details**:
   - **Name**: `Monthly Budget`
   - **Reset period**: Monthly
   - **Budget amount**: $50 (or your limit)
6. **Next**
7. **Alert conditions**:
   - **Type**: Actual
   - **% of budget**: 80
   - **Action group**: ag-email-alerts
8. Add another alert at 100%
9. **Create**

---

### **Day 11: Azure Backup & Recovery**

#### **📚 THEORY**

**Azure Backup:**
- Managed backup service
- Backs up VMs, files, SQL databases, etc.
- Stored in **Recovery Services Vault**

**Recovery Services Vault:**
- Container for backup data
- Geo-redundant storage by default
- Point-in-time recovery

**VM Backup Concepts:**
- **Full backup**: Complete VM snapshot
- **Incremental backup**: Only changed blocks
- **Retention**: How long to keep backups (days/weeks/months/years)
- **Backup policy**: Schedule + retention rules

**Azure Site Recovery (ASR):**
- Disaster recovery solution
- Replicates VMs to another region
- Failover during outages

**Soft Delete:**
- Deleted backup data kept for 14 days
- Protects against accidental deletion

---

#### **🛠️ HANDS-ON: Day 11**

**Exercise 1: Create Recovery Services Vault**

1. Search "Recovery Services vaults" → **+ Create**
2. **Resource group**: Create new: `rg-backup-lab`
3. **Vault name**: `rsv-backup-eastus`
4. **Region**: East US
5. **Review + create** → **Create**

**Exercise 2: Configure Backup for VM**

1. Go to the Recovery Services Vault
2. Click **+ Backup**
3. **Where is your workload running?**: Azure
4. **What do you want to backup?**: Virtual machine
5. **Backup**

6. **Backup policy**: Create new policy
   - **Policy name**: `daily-backup-policy`
   - **Backup schedule**:
     - **Frequency**: Daily
     - **Time**: 2:00 AM
     - **Timezone**: (your timezone)
   - **Instant Restore**: Retain snapshots for 2 days
   - **Retention of daily backup**: 30 days
   - **Retention of weekly backup**: 12 weeks (Sunday)
   - **Retention of monthly backup**: 12 months (First Sunday)
   - **OK**

7. **Add** virtual machines:
   - Select one of your VMs (e.g., vm-linux-web)
   - **OK**

8. **Enable Backup** (takes 5-10 minutes)

**Exercise 3: Trigger On-Demand Backup**

1. Go to Recovery Services Vault → **Backup items** → **Azure Virtual Machine**
2. Click on your VM
3. **Backup now**
4. **Retain Backup Till**: 30 days from today
5. **OK**

6. Click **Backup Jobs** to monitor progress (takes 10-20 minutes)

**Exercise 4: Restore Files from Backup**

1. After backup completes, go to the VM's backup item
2. Click **File Recovery**
3. **Select recovery point**: Choose the backup you just created
4. **Download Executable** (for Windows) or **Download Script** (for Linux)

5. On the VM:
```bash
# For Linux VMs
chmod +x script.sh
sudo ./script.sh
```

6. Script will mount the backup as a drive
7. Browse and copy files:
```bash
# List mounted backups
df -h | grep backup

# Navigate to mounted backup
cd /backup_mount_point/

# Copy files
cp important_file.txt ~/recovered_file.txt
```

8. After copying, unmount:
```bash
sudo umount /backup_mount_point
```

**Exercise 5: Restore Entire VM**

1. Go to Recovery Services Vault → **Backup items** → **Azure Virtual Machine**
2. Click on your VM
3. **Restore VM**
4. **Restore point**: Select a backup
5. **Restore configuration**: Create new
6. **Restore Type**: Create new virtual machine
7. **Virtual machine name**: `vm-restored-test`
8. **Resource group**: rg-backup-lab
9. **Virtual network**: Select existing VNet
10. **Subnet**: Select a subnet
11. **Staging Location**: Select a storage account (or create new)
12. **Restore**

13. Monitor the restore job (takes 15-30 minutes)
14. Once complete, verify the new VM exists

**Exercise 6: Configure Geo-Redundant Backup**

1. Go to Recovery Services Vault → **Properties**
2. Under **Backup Configuration**, click **Update**
3. **Storage replication type**: Geo-redundant
4. **Save**

> **Note**: This setting cannot be changed if backup items exist. For demo, create a new vault or understand the concept.

**Exercise 7: Set Up Azure Site Recovery**

1. Create a new VM in East US region (source)
2. Create a new Recovery Services Vault in **West US** region:
   - **Name**: `rsv-dr-westus`
   - **Region**: West US

3. Go to the new vault → **Site Recovery**
4. **Prepare infrastructure**:
   - **Protection goal**: Azure to Azure
   - **Source location**: East US
   - **Azure VM deployment model**: Resource Manager
   - **Source subscription**: Your subscription
   - **Source resource group**: (VM's resource group)
   - **Next**

5. **Target location**: West US
6. **Next** → **Next** (accept defaults)
7. **Prepare infrastructure**

8. Go to **Replicate**:
   - **Source**: East US
   - **Virtual machines**: Select your VM
   - **Next**

9. **Replication settings**:
   - **Target location**: West US
   - Accept defaults for resource group, network, storage
   - **Next**

10. **Replication policy**:
    - **Name**: `24-hour-retention-policy`
    - **Recovery point retention**: 24 hours
    - **App-consistent snapshot frequency**: 4 hours
    - **OK**

11. **Enable replication** (takes 30-60 minutes to complete initial replication)

**Exercise 8: Test Failover**

1. After replication is complete (90%+), go to **Replicated items**
2. Click on your VM
3. **Test failover**
4. **Recovery point**: Latest (latest app-consistent)
5. **Azure virtual network**: Select a VNet in West US
6. **OK**

7. Monitor the failover (takes 10-15 minutes)
8. Once complete, a test VM will be created in West US
9. Verify the test VM works
10. **Cleanup test failover** to remove the test VM

---

### **Day 12: Governance & Policy**

#### **📚 THEORY**

**Azure Policy:**
- Enforce organizational standards
- Examples:
  - Allowed VM sizes
  - Required tags
  - Allowed regions
  - Must use managed disks

**Policy Effects:**
- **Deny**: Block resource creation
- **Audit**: Log non-compliant resources
- **Append**: Add properties (tags)
- **DeployIfNotExists**: Auto-deploy resources (e.g., monitoring agent)
- **Modify**: Modify resource properties

**Initiatives:**
- Collection of related policies
- Example: "CIS Microsoft Azure Foundations Benchmark"

**Resource Tags:**
- Metadata key-value pairs
- Use cases:
  - Cost tracking (Department: IT)
  - Environment (Env: Production)
  - Owner (Owner: john@company.com)

**Resource Locks:**
- **CanNotDelete**: Prevent deletion
- **ReadOnly**: Prevent modifications

**Management Groups:**
- Organize subscriptions hierarchically
- Apply policies at scale

**Azure Blueprints:**
- Package of:
  - Policies
  - Role assignments
  - ARM templates
  - Resource groups

---

#### **🛠️ HANDS-ON: Day 12**

**Exercise 1: Create and Assign Tags**

1. Go to any resource group (e.g., rg-learning-eastus)
2. Click **Tags**
3. Add tags:
   - **Name**: `Environment`, **Value**: `Learning`
   - **Name**: `Department`, **Value**: `IT`
   - **Name**: `CostCenter`, **Value**: `CC-001`
4. **Apply**

5. Go to a VM in that resource group → **Tags**
6. Click **Inherit tags from resource group**
7. **Save**

**Exercise 2: Create Custom Policy - Require Tags**

1. Search "Policy" → **Definitions**
2. **+ Policy definition**
3. **Definition location**: Your subscription
4. **Name**: `Require-Environment-Tag`
5. **Category**: Create new: `Custom`
6. **Policy rule**:
```json
{
  "mode": "Indexed",
  "policyRule": {
    "if": {
      "field": "tags['Environment']",
      "exists": "false"
    },
    "then": {
      "effect": "deny"
    }
  }
}
```
7. **Save**

**Exercise 3: Assign the Policy**

1. Go to **Assignments** → **Assign policy**
2. **Scope**: Select a resource group
3. **Policy definition**: Search for "Require-Environment-Tag"
4. **Assignment name**: Require Environment Tag
5. **Review + create** → **Create**

**Exercise 4: Test the Policy**

1. Try to create a storage account in that resource group WITHOUT an Environment tag
   - Should be **denied**!

2. Create it again WITH the tag:
   - Add tag: `Environment = Production`
   - Should succeed

**Exercise 5: Use Built-in Policy - Allowed Locations**

1. Policy → **Assignments** → **Assign policy**
2. **Scope**: Your subscription
3. **Policy definition**: Search "Allowed locations"
4. Select **Allowed locations** (Built-in)
5. **Parameters**:
   - **Allowed locations**: Select only East US and West US
6. **Review + create** → **Create**

7. Try creating a resource in "North Europe" - should be denied

**Exercise 6: Create Policy Initiative**

1. Policy → **Definitions** → **+ Initiative definition**
2. **Definition location**: Your subscription
3. **Name**: `Resource-Governance-Standards`
4. **Category**: Custom
5. **Add policy definitions**:
   - Search and add: "Allowed locations"
   - Search and add: "Require-Environment-Tag" (your custom policy)
   - Search and add: "Allowed virtual machine size SKUs"
6. **Save**

7. Assign this initiative to a resource group

**Exercise 7: Create Resource Lock**

1. Go to a critical resource (e.g., your production VM)
2. Click **Locks** (under Settings)
3. **+ Add**
4. **Lock name**: `prevent-deletion`
5. **Lock type**: Delete
6. **Notes**: "Production VM - do not delete"
7. **OK**

8. Try to delete the VM - you'll get an error!
9. Remove the lock to proceed with deletion if needed

**Exercise 8: View Policy Compliance**

1. Policy → **Compliance**
2. View compliance state:
   - **Compliant**: Resources following policies
   - **Non-compliant**: Resources violating policies
3. Click on a non-compliant policy
4. See which resources are non-compliant
5. **Create Remediation Task** to fix them

**Exercise 9: Use Azure Cost Management**

1. Search "Cost Management + Billing"
2. **Cost analysis**
3. View costs by:
   - **Resource group**
   - **Resource**
   - **Service name**
   - **Tag** (Department, Environment)

4. Create a custom view:
   - **Group by**: Tag (Environment)
   - **Granularity**: Daily
   - **Chart type**: Column (stacked)
   - **Save** as "Cost by Environment"

5. **Export** the data as CSV for reporting

**Exercise 10: Create Azure Advisor Recommendations**

1. Search "Advisor"
2. View recommendations in categories:
   - **Cost**: Underutilized VMs, unattached disks
   - **Security**: Enable disk encryption
   - **Reliability**: Use availability zones
   - **Operational Excellence**: Service health alerts
   - **Performance**: Upgrade to faster disks

3. Click on a recommendation
4. **Postpone** or **Dismiss** if not applicable
5. **Download as CSV** for action items

---

### **Day 13: Automation & DevOps**

#### **📚 THEORY**

**Infrastructure as Code (IaC):**
- Define infrastructure in code files
- Version control, repeatability, consistency

**ARM Templates:**
- Azure Resource Manager templates
- JSON format
- Declarative (describe what you want, not how)

**Bicep:**
- Simplified language for ARM templates
- Compiles to ARM JSON
- Easier to read and write

**Azure CLI:**
- Command-line interface
- Cross-platform (Windows, Mac, Linux)
- Scripting and automation

**Azure PowerShell:**
- PowerShell modules for Azure
- Object-oriented (vs text-based CLI)

**Azure Automation:**
- **Runbooks**: Automated scripts (PowerShell, Python)
- **Update Management**: Patch VMs
- **State Configuration**: DSC for configuration management

**Azure DevOps:**
- **Repos**: Git repositories
- **Pipelines**: CI/CD
- **Boards**: Work tracking
- **Artifacts**: Package management

---

#### **🛠️ HANDS-ON: Day 13**

**Exercise 1: Install and Use Azure CLI**

1. On your local computer or Cloud Shell:
   - Azure Portal → Click Cloud Shell icon (>_) at top
   - Choose **Bash**

2. Verify installation:
```bash
az --version
```

3. Login (skip if using Cloud Shell):
```bash
az login
```

**Exercise 2: Manage Resources with CLI**

```bash
# List subscriptions
az account list --output table

# Set active subscription
az account set --subscription "Your Subscription Name"

# List resource groups
az group list --output table

# Create resource group
az group create \
  --name rg-cli-demo \
  --location eastus

# List VMs
az vm list --output table

# Get VM details
az vm show \
  --resource-group rg-learning-eastus \
  --name vm-linux-web \
  --output json

# Start VM
az vm start \
  --resource-group rg-learning-eastus \
  --name vm-linux-web

# Stop (deallocate) VM
az vm deallocate \
  --resource-group rg-learning-eastus \
  --name vm-linux-web

# Get VM status
az vm get-instance-view \
  --resource-group rg-learning-eastus \
  --name vm-linux-web \
  --query instanceView.statuses[1].displayStatus
```

**Exercise 3: Create Resources with CLI**

```bash
# Create storage account
az storage account create \
  --name saclidemo$(date +%s) \
  --resource-group rg-cli-demo \
  --location eastus \
  --sku Standard_LRS

# Create a VM
az vm create \
  --resource-group rg-cli-demo \
  --name vm-cli-created \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --public-ip-sku Standard

# Open port 80
az vm open-port \
  --resource-group rg-cli-demo \
  --name vm-cli-created \
  --port 80

# Clean up
az group delete --name rg-cli-demo --yes --no-wait
```

**Exercise 4: Create ARM Template**

1. Create file `storage-template.json`:
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the storage account"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2023-01-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true
      },
      "tags": {
        "Environment": "Learning",
        "DeployedBy": "ARM-Template"
      }
    }
  ],
  "outputs": {
    "storageAccountId": {
      "type": "string",
      "value": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]"
    }
  }
}
```

2. Create parameters file `storage-parameters.json`:
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "value": "saarmtemplateXXXX"
    }
  }
}
```

3. Deploy the template:
```bash
# Create resource group
az group create --name rg-arm-demo --location eastus

# Deploy template
az deployment group create \
  --resource-group rg-arm-demo \
  --template-file storage-template.json \
  --parameters storage-parameters.json
```

**Exercise 5: Create Bicep Template**

1. Create file `storage.bicep`:
```bicep
@description('Name of the storage account')
param storageAccountName string

@description('Location for the storage account')
param location string = resourceGroup().location

@allowed([
  'Standard_LRS'
  'Standard_GRS'
  'Standard_ZRS'
])
param storageSKU string = 'Standard_LRS'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: storageSKU
  }
  kind: 'StorageV2'
  properties: {
    supportsHttpsTrafficOnly: true
    minimumTlsVersion: 'TLS1_2'
  }
  tags: {
    Environment: 'Learning'
    DeployedBy: 'Bicep'
  }
}

output storageAccountId string = storageAccount.id
output primaryEndpoint string = storageAccount.properties.primaryEndpoints.blob
```

2. Deploy Bicep:
```bash
az deployment group create \
  --resource-group rg-arm-demo \
  --template-file storage.bicep \
  --parameters storageAccountName=sabicepXXXX
```

**Exercise 6: Create Multi-Resource Bicep Template**

Create `web-infrastructure.bicep`:
```bicep
param location string = resourceGroup().location
param environmentName string = 'dev'

var vmName = 'vm-${environmentName}-web'
var vnetName = 'vnet-${environmentName}'
var nsgName = 'nsg-${environmentName}-web'

// Virtual Network
resource vnet 'Microsoft.Network/virtualNetworks@2023-05-01' = {
  name: vnetName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.0.0.0/16'
      ]
    }
    subnets: [
      {
        name: 'subnet-web'
        properties: {
          addressPrefix: '10.0.1.0/24'
          networkSecurityGroup: {
            id: nsg.id
          }
        }
      }
    ]
  }
}

// Network Security Group
resource nsg 'Microsoft.Network/networkSecurityGroups@2023-05-01' = {
  name: nsgName
  location: location
  properties: {
    securityRules: [
      {
        name: 'AllowHTTP'
        properties: {
          priority: 100
          direction: 'Inbound'
          access: 'Allow'
          protocol: 'Tcp'
          sourcePortRange: '*'
          destinationPortRange: '80'
          sourceAddressPrefix: '*'
          destinationAddressPrefix: '*'
        }
      }
      {
        name: 'AllowSSH'
        properties: {
          priority: 110
          direction: 'Inbound'
          access: 'Allow'
          protocol: 'Tcp'
          sourcePortRange: '*'
          destinationPortRange: '22'
          sourceAddressPrefix: '*'
          destinationAddressPrefix: '*'
        }
      }
    ]
  }
}

output vnetId string = vnet.id
output subnetId string = vnet.properties.subnets[0].id
```

Deploy it:
```bash
az deployment group create \
  --resource-group rg-arm-demo \
  --template-file web-infrastructure.bicep \
  --parameters environmentName=production
```

**Exercise 7: Create Azure Automation Account**

1. Search "Automation Accounts" → **+ Create**
2. **Resource group**: Create new: `rg-automation-lab`
3. **Name**: `aa-learning-automation`
4. **Region**: East US
5. **Create**

**Exercise 8: Create PowerShell Runbook**

1. Go to Automation Account → **Runbooks**
2. **+ Create a runbook**
3. **Name**: `Stop-DeallocateVMs`
4. **Runbook type**: PowerShell
5. **Runtime version**: 7.2
6. **Description**: "Stops and deallocates all VMs in a resource group"
7. **Create**

8. In the editor, paste:
```powershell
param(
    [Parameter(Mandatory=$true)]
    [string]$ResourceGroupName
)

# Connect to Azure with system-assigned managed identity
Connect-AzAccount -Identity

# Get all VMs in the resource group
$vms = Get-AzVM -ResourceGroupName $ResourceGroupName

if ($vms.Count -eq 0) {
    Write-Output "No VMs found in resource group: $ResourceGroupName"
    return
}

Write-Output "Found $($vms.Count) VM(s) in resource group: $ResourceGroupName"

# Stop each VM
foreach ($vm in $vms) {
    Write-Output "Stopping VM: $($vm.Name)"
    Stop-AzVM -ResourceGroupName $ResourceGroupName -Name $vm.Name -Force
    Write-Output "VM $($vm.Name) stopped successfully"
}

Write-Output "All VMs in $ResourceGroupName have been stopped"
```

9. **Save** → **Publish**

**Exercise 9: Enable Managed Identity and Assign Permissions**

1. Automation Account → **Identity**
2. **System assigned** tab → **Status**: On
3. **Save** → **Yes**
4. Copy the **Object (principal) ID**

5. Go to the resource group with VMs → **Access control (IAM)**
6. **+ Add** → **Add role assignment**
7. **Role**: Virtual Machine Contributor
8. **Members**: Select **Managed identity**
9. **+ Select members** → **Automation Account** → Select your automation account
10. **Review + assign**

**Exercise 10: Run the Runbook**

1. Go to the runbook → **Start**
2. **Parameters**:
   - **ResourceGroupName**: (name of a resource group with VMs)
3. **OK**

4. Monitor the job output
5. Verify VMs are stopped in the Azure Portal

**Exercise 11: Schedule the Runbook**

1. In the runbook → **Schedules** → **+ Add a schedule**
2. **Schedule**: Create new schedule
3. **Name**: `nightly-vm-shutdown`
4. **Starts**: Tomorrow at 11:00 PM
5. **Recurrence**: Recurring → Every 1 day
6. **Create**

7. **Parameters**:
   - **ResourceGroupName**: (your resource group)
8. **OK**

Now VMs will automatically shut down every night!

---

### **Day 14: Final Review & Practice Exam**

#### **📚 COMPREHENSIVE REVIEW**

**Core AZ-104 Domains:**

1. **Manage Azure Identities and Governance (15-20%)**
   - Entra ID users, groups, roles
   - RBAC and custom roles
   - Azure Policy and initiatives
   - Resource locks and tags
   - Management groups
   - Cost management

2. **Implement and Manage Storage (15-20%)**
   - Storage accounts and redundancy
   - Blob storage (tiers, lifecycle)
   - File shares
   - Access control (SAS, keys, RBAC)
   - Azure Backup

3. **Deploy and Manage Azure Compute Resources (20-25%)**
   - Virtual machines and availability
   - VM Scale Sets
   - Azure App Service
   - Containers (overview)

4. **Configure and Manage Virtual Networking (20-25%)**
   - VNets and subnets
   - NSGs and Azure Firewall
   - VNet peering
   - Private endpoints
   - DNS and load balancing

5. **Monitor and Maintain Azure Resources (10-15%)**
   - Azure Monitor and alerts
   - Log Analytics and KQL
   - Network Watcher
   - Azure Backup and Site Recovery

---

#### **🛠️ HANDS-ON: Final Lab - Build Complete Infrastructure**

**Scenario**: Deploy a secure, monitored, production-ready web application

**Exercise: End-to-End Deployment**

```bash
# Step 1: Create resource group with tags
az group create \
  --name rg-final-project \
  --location eastus \
  --tags Environment=Production Department=IT CostCenter=CC-100

# Step 2: Create VNet with multiple subnets
az network vnet create \
  --resource-group rg-final-project \
  --name vnet-production \
  --address-prefix 10.10.0.0/16 \
  --subnet-name subnet-web \
  --subnet-prefix 10.10.1.0/24

```bash
az network vnet subnet create \
  --resource-group rg-final-project \
  --vnet-name vnet-production \
  --name subnet-db \
  --address-prefix 10.10.2.0/24

az network vnet subnet create \
  --resource-group rg-final-project \
  --vnet-name vnet-production \
  --name AzureBastionSubnet \
  --address-prefix 10.10.3.0/26

# Step 3: Create NSG for web tier
az network nsg create \
  --resource-group rg-final-project \
  --name nsg-web

az network nsg rule create \
  --resource-group rg-final-project \
  --nsg-name nsg-web \
  --name AllowHTTP \
  --priority 100 \
  --source-address-prefixes '*' \
  --destination-port-ranges 80 \
  --access Allow \
  --protocol Tcp

az network nsg rule create \
  --resource-group rg-final-project \
  --nsg-name nsg-web \
  --name AllowHTTPS \
  --priority 110 \
  --source-address-prefixes '*' \
  --destination-port-ranges 443 \
  --access Allow \
  --protocol Tcp

# Associate NSG with subnet
az network vnet subnet update \
  --resource-group rg-final-project \
  --vnet-name vnet-production \
  --name subnet-web \
  --network-security-group nsg-web

# Step 4: Create public IP for load balancer
az network public-ip create \
  --resource-group rg-final-project \
  --name pip-lb-web \
  --sku Standard \
  --zone 1 2 3

# Step 5: Create load balancer
az network lb create \
  --resource-group rg-final-project \
  --name lb-web \
  --sku Standard \
  --public-ip-address pip-lb-web \
  --frontend-ip-name frontend-web \
  --backend-pool-name backend-web

# Create health probe
az network lb probe create \
  --resource-group rg-final-project \
  --lb-name lb-web \
  --name health-probe-http \
  --protocol tcp \
  --port 80

# Create load balancing rule
az network lb rule create \
  --resource-group rg-final-project \
  --lb-name lb-web \
  --name rule-http \
  --protocol tcp \
  --frontend-port 80 \
  --backend-port 80 \
  --frontend-ip-name frontend-web \
  --backend-pool-name backend-web \
  --probe-name health-probe-http

# Step 6: Create VM Scale Set
az vmss create \
  --resource-group rg-final-project \
  --name vmss-web \
  --image Ubuntu2204 \
  --upgrade-policy-mode automatic \
  --instance-count 2 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --vnet-name vnet-production \
  --subnet subnet-web \
  --lb lb-web \
  --backend-pool-name backend-web \
  --vm-sku Standard_B2s \
  --zones 1 2 \
  --public-ip-address "" \
  --custom-data cloud-init.txt

# Step 7: Create cloud-init.txt for auto-configuration
cat > cloud-init.txt << 'EOF'
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - path: /var/www/html/index.html
    content: |
      <!DOCTYPE html>
      <html>
      <head><title>Production Web App</title></head>
      <body>
        <h1>Hello from Azure VMSS!</h1>
        <p>Instance: $(hostname)</p>
        <p>Deployed via cloud-init</p>
      </body>
      </html>
runcmd:
  - systemctl start nginx
  - systemctl enable nginx
EOF

# Step 8: Configure autoscaling
az monitor autoscale create \
  --resource-group rg-final-project \
  --resource vmss-web \
  --resource-type Microsoft.Compute/virtualMachineScaleSets \
  --name autoscale-web \
  --min-count 2 \
  --max-count 5 \
  --count 2

az monitor autoscale rule create \
  --resource-group rg-final-project \
  --autoscale-name autoscale-web \
  --condition "Percentage CPU > 70 avg 5m" \
  --scale out 1

az monitor autoscale rule create \
  --resource-group rg-final-project \
  --autoscale-name autoscale-web \
  --condition "Percentage CPU < 30 avg 5m" \
  --scale in 1

# Step 9: Create storage account for logs
STORAGE_NAME="safinalproject$(date +%s)"
az storage account create \
  --resource-group rg-final-project \
  --name $STORAGE_NAME \
  --sku Standard_LRS \
  --kind StorageV2 \
  --access-tier Hot

# Step 10: Create Log Analytics workspace
az monitor log-analytics workspace create \
  --resource-group rg-final-project \
  --workspace-name law-production

# Step 11: Enable diagnostics for load balancer
LB_ID=$(az network lb show --resource-group rg-final-project --name lb-web --query id -o tsv)
LAW_ID=$(az monitor log-analytics workspace show --resource-group rg-final-project --workspace-name law-production --query id -o tsv)

az monitor diagnostic-settings create \
  --resource $LB_ID \
  --name lb-diagnostics \
  --workspace $LAW_ID \
  --logs '[{"category":"LoadBalancerAlertEvent","enabled":true},{"category":"LoadBalancerProbeHealthStatus","enabled":true}]' \
  --metrics '[{"category":"AllMetrics","enabled":true}]'

# Step 12: Create alert for unhealthy backend
az monitor metrics alert create \
  --resource-group rg-final-project \
  --name alert-unhealthy-backend \
  --description "Alert when backend pool has unhealthy instances" \
  --scopes $LB_ID \
  --condition "avg DipAvailability < 90" \
  --window-size 5m \
  --evaluation-frequency 1m

# Step 13: Get the public IP to test
az network public-ip show \
  --resource-group rg-final-project \
  --name pip-lb-web \
  --query ipAddress -o tsv
```

**Test the deployment:**
```bash
# Get load balancer IP
LB_IP=$(az network public-ip show --resource-group rg-final-project --name pip-lb-web --query ipAddress -o tsv)

# Test the application
curl http://$LB_IP

# Test multiple times to see different instances
for i in {1..10}; do curl http://$LB_IP 2>/dev/null | grep Instance; done
```

---

#### **📝 PRACTICE EXAM QUESTIONS**

**Practice these scenario-based questions:**

**Question 1: Identity & Access**
You need to ensure that users in the Marketing department can only create VMs in the East US region and only with Standard_B2s size. What should you do?

<details>
<summary>Answer</summary>

1. Create a custom RBAC role with VM creation permissions
2. Create an Azure Policy initiative with:
   - "Allowed locations" policy (East US only)
   - "Allowed virtual machine size SKUs" policy (Standard_B2s only)
3. Assign the policy initiative to the Marketing resource group
4. Assign the custom RBAC role to the Marketing group
</details>

**Question 2: Networking**
You have two VNets: VNet-A (10.1.0.0/16) and VNet-B (10.2.0.0/16). VMs in VNet-A cannot communicate with VMs in VNet-B. What should you check?

<details>
<summary>Answer</summary>

1. Verify VNet peering is configured in BOTH directions (bidirectional)
2. Check NSG rules on both subnets/NICs
3. Verify route tables aren't blocking traffic
4. Ensure "Allow forwarded traffic" is enabled if needed
5. Check if VNet address spaces don't overlap
</details>

**Question 3: Storage**
You need to store 500 GB of archived documents that will be accessed once per year. The solution must minimize costs. What should you do?

<details>
<summary>Answer</summary>

1. Create a storage account with LRS redundancy
2. Create a blob container
3. Set access tier to **Archive**
4. Upload files
5. (Optional) Create lifecycle management policy to automatically move blobs to archive tier after X days
</details>

**Question 4: Backup**
Your company requires all VMs to be backed up daily and retained for 30 days, with weekly backups retained for 12 weeks. What should you configure?

<details>
<summary>Answer</summary>

1. Create Recovery Services Vault
2. Create custom backup policy:
   - Schedule: Daily at specific time
   - Daily retention: 30 days
   - Weekly retention: 12 weeks (select day of week)
3. Enable backup for VMs using this policy
</details>

**Question 5: Monitoring**
You need to be notified when a VM's CPU usage exceeds 85% for more than 10 minutes. What should you create?

<details>
<summary>Answer</summary>

1. Create an action group with email/SMS notification
2. Create a metric alert:
   - Signal: Percentage CPU
   - Threshold: Static, Greater than 85
   - Aggregation: Average
   - Period: 10 minutes
   - Frequency: 5 minutes
3. Assign the action group to the alert
</details>

**Question 6: Scaling**
You have a web application that experiences high traffic between 9 AM - 5 PM. You need to automatically scale out during these hours and scale in after hours. What should you configure?

<details>
<summary>Answer</summary>

1. Deploy application to VM Scale Set
2. Create two autoscale rules:
   - Scale-out rule: CPU > 70% for 5 minutes
   - Scale-in rule: CPU < 30% for 5 minutes
3. (Optional) Create schedule-based autoscale profiles:
   - Business hours profile (9 AM - 5 PM): Min 3, Max 10 instances
   - After hours profile: Min 1, Max 3 instances
</details>

**Question 7: Security**
You need to allow VMs in subnet-web to access Azure SQL Database without the traffic going over the internet. What should you implement?

<details>
<summary>Answer</summary>

1. Create a Private Endpoint for the Azure SQL Database
2. Place the private endpoint in subnet-web (or a dedicated subnet)
3. Configure Private DNS zone for database.windows.net
4. Link the DNS zone to the VNet
5. Remove public access from SQL Database firewall
</details>

**Question 8: Cost Management**
You notice several deallocated VMs still incurring costs. What resources should you check?

<details>
<summary>Answer</summary>

Check for:
1. **Managed disks** attached to deallocated VMs (charged for storage)
2. **Public IP addresses** with Static allocation (charged even if not in use)
3. **Reserved IP addresses**
4. **Snapshots** of VM disks
5. **Backup data** in Recovery Services Vault

Solution: Delete unused disks, change public IPs to dynamic or delete them, remove old snapshots
</details>

**Question 9: Disaster Recovery**
You need to implement disaster recovery for VMs in East US to West US with a 1-hour recovery point objective (RPO). What should you use?

<details>
<summary>Answer</summary>

1. Create Recovery Services Vault in West US
2. Enable Azure Site Recovery for VMs
3. Configure replication policy:
   - Recovery point retention: 24 hours
   - App-consistent snapshot frequency: 1 hour (meets RPO)
4. Enable replication from East US to West US
5. Perform test failover to validate
</details>

**Question 10: Governance**
You need to ensure all resources created in your subscription have an "Owner" tag. If the tag is missing, it should be automatically added with a default value. What should you configure?

<details>
<summary>Answer</summary>

1. Create Azure Policy with **Modify** effect:
```json
{
  "if": {
    "field": "tags['Owner']",
    "exists": "false"
  },
  "then": {
    "effect": "modify",
    "details": {
      "roleDefinitionIds": ["/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"],
      "operations": [{
        "operation": "add",
        "field": "tags['Owner']",
        "value": "Unassigned"
      }]
    }
  }
}
```
2. Assign policy to subscription
3. Create remediation task for existing resources
</details>

---

#### **🎯 EXAM PREPARATION CHECKLIST**

**2 Days Before Exam:**
- [ ] Review all hands-on labs
- [ ] Practice KQL queries
- [ ] Memorize RBAC built-in roles (Owner, Contributor, Reader)
- [ ] Understand VM sizing tiers (B, D, E, F series)
- [ ] Review storage redundancy options (LRS, ZRS, GRS, RA-GRS)
- [ ] Understand NSG rule priority (lower number = higher priority)
- [ ] Review backup retention policies
- [ ] Practice creating resources via CLI and Portal

**1 Day Before Exam:**
- [ ] Review Microsoft Learn AZ-104 path
- [ ] Take practice exam: https://learn.microsoft.com/certifications/exams/az-104
- [ ] Review any weak areas
- [ ] Get good sleep!

**Exam Day:**
- [ ] Arrive 15 minutes early (or prepare your testing space if online)
- [ ] Have valid ID ready
- [ ] Read questions carefully - look for keywords like "minimize cost", "most secure", "least administrative effort"
- [ ] Flag difficult questions and return to them
- [ ] Manage your time: ~2 minutes per question

---

#### **🔑 KEY EXAM TIPS**

**1. Common Gotchas:**
- **Stopped vs Deallocated**: Stopped VMs still incur compute charges; Deallocated VMs don't
- **NSG vs Azure Firewall**: NSG is free and basic; Firewall costs money but has advanced features
- **VNet Peering vs VPN Gateway**: Peering for Azure-to-Azure; VPN for on-premises
- **Hot vs Cool vs Archive**: Archive requires rehydration (can take hours)
- **RBAC scope**: Applied at subscription > resource group > resource (inherited downward)

**2. Least Privilege Principle:**
- Always choose the minimum permissions needed
- Reader > Contributor > Owner (choose the least powerful that works)

**3. Cost Optimization:**
- Use managed disks (Standard HDD is cheapest)
- Deallocate VMs when not in use
- Use auto-shutdown schedules
- Reserved Instances for long-term workloads
- LRS redundancy unless HA is required

**4. Time-Saving Exam Strategies:**
- If you see "migrate on-premises to Azure" → think Azure Migrate or Site Recovery
- If you see "secure communication without internet" → think Private Endpoint
- If you see "automatic scaling" → think VM Scale Sets or App Service
- If you see "enforce organizational standards" → think Azure Policy
- If you see "centralized logging" → think Log Analytics Workspace

**5. Scenario Keywords to Watch:**
- **"Minimize cost"** → Choose cheapest tier (Basic, Standard HDD, LRS, B-series VMs)
- **"Most secure"** → Private endpoints, managed identities, disable public access
- **"Least administrative effort"** → Use PaaS over IaaS, automation over manual
- **"High availability"** → Availability Zones, geo-redundancy, load balancers
- **"Disaster recovery"** → Azure Site Recovery, geo-replication

---

#### **📚 FINAL STUDY RESOURCES**

**Official Microsoft Learning:**
1. Microsoft Learn AZ-104 Path: https://learn.microsoft.com/certifications/exams/az-104
2. Azure Documentation: https://learn.microsoft.com/azure
3. Azure Architecture Center: https://learn.microsoft.com/azure/architecture

**Practice Exams:**
1. Microsoft Official Practice Assessment
2. MeasureUp practice tests
3. Whizlabs AZ-104 practice tests

**Cheat Sheets:**
```
COMMON PORTS:
22  - SSH
80  - HTTP
443 - HTTPS
3389 - RDP (Windows)
1433 - SQL Server
3306 - MySQL
5432 - PostgreSQL

VM STATES:
Running    - Charged for compute + storage
Stopped    - Charged for compute + storage
Deallocated - Charged for storage only

RBAC ROLES:
Owner       - Full access + assign roles
Contributor - Full access, cannot assign roles
Reader      - View only
User Access Administrator - Manage access only

STORAGE TIERS:
Hot     - Frequent access, high storage cost, low access cost
Cool    - Infrequent access, 30+ days, medium costs
Archive - Rare access, 180+ days, low storage cost, high access cost, rehydration required

REDUNDANCY:
LRS  - 3 copies, 1 datacenter, 11 nines
ZRS  - 3 copies, 3 zones, 12 nines
GRS  - 6 copies, 2 regions, 16 nines
GZRS - 6 copies, zones + regions, 16 nines

NSG RULE PRIORITY:
100-4096 (lower = higher priority)
Default rules start at 65000

AVAILABILITY:
Single VM (Premium SSD): 99.9%
Availability Set: 99.95%
Availability Zone: 99.99%
```

---

#### **🎓 FINAL LAB: Complete Infrastructure Audit**

**Exercise: Audit and Optimize Your Azure Environment**

Create a comprehensive report of your environment:

```bash
#!/bin/bash

echo "=== AZURE ENVIRONMENT AUDIT ==="
echo ""

echo "1. RESOURCE SUMMARY"
az resource list --query "length(@)"
echo "Total resources: $(az resource list --query 'length(@)' -o tsv)"
echo ""

echo "2. COST BY RESOURCE GROUP"
az group list --query "[].{Name:name, Location:location}" -o table
echo ""

echo "3. VMs AND THEIR STATUS"
az vm list -d --query "[].{Name:name, ResourceGroup:resourceGroup, Status:powerState, Size:hardwareProfile.vmSize}" -o table
echo ""

echo "4. UNATTACHED DISKS (potential cost savings)"
az disk list --query "[?managedBy==null].{Name:name, Size:diskSizeGb, SKU:sku.name, ResourceGroup:resourceGroup}" -o table
echo ""

echo "5. PUBLIC IPS NOT IN USE"
az network public-ip list --query "[?ipConfiguration==null].{Name:name, ResourceGroup:resourceGroup, AllocationMethod:publicIpAllocationMethod}" -o table
echo ""

echo "6. STORAGE ACCOUNTS"
az storage account list --query "[].{Name:name, Location:location, SKU:sku.name, AccessTier:accessTier}" -o table
echo ""

echo "7. NSG RULES ALLOWING ANY SOURCE"
for nsg in $(az network nsg list --query "[].name" -o tsv); do
  echo "NSG: $nsg"
  az network nsg rule list --nsg-name $nsg --query "[?sourceAddressPrefix=='*' && access=='Allow'].{Name:name, Priority:priority, Direction:direction, Port:destinationPortRange}" -o table
done
echo ""

echo "8. RESOURCES WITHOUT TAGS"
az resource list --query "[?tags==null].{Name:name, Type:type, ResourceGroup:resourceGroup}" -o table
echo ""

echo "9. BACKUP STATUS"
az backup item list --vault-name rsv-backup-eastus --resource-group rg-backup-lab --query "[].{Name:name, Status:properties.protectionStatus, LastBackup:properties.lastBackupTime}" -o table
echo ""

echo "10. POLICY COMPLIANCE"
az policy state list --query "[?complianceState=='NonCompliant'].{Resource:resourceId, Policy:policyDefinitionName}" -o table
echo ""

echo "=== AUDIT COMPLETE ==="
```

Save this as `audit.sh` and run it before your exam to practice interpreting Azure environments!

---

## **FINAL WORDS OF ENCOURAGEMENT**

You've covered a tremendous amount in these 2 weeks:
- ✅ Identity and access management
- ✅ Virtual machines and compute
- ✅ Networking and security
- ✅ Storage solutions
- ✅ Monitoring and diagnostics
- ✅ Backup and disaster recovery
- ✅ Governance and compliance
- ✅ Automation and IaC

**You're ready for the AZ-104!**

**Remember during the exam:**
1. Read the question TWICE - focus on the requirement
2. Eliminate obviously wrong answers first
3. Look for keywords that indicate the best answer
4. Don't overthink - your first instinct is usually correct
5. Manage your time - don't spend too long on any question

**Post-Exam:**
- Regardless of the result, you've gained valuable Azure skills
- If you pass: Congratulations! Update your LinkedIn and celebrate!
- If you don't pass: Review the score report, study weak areas, and retake in 14 days

**Good luck! You've got this! 🚀**

---

**Need Help?**
- Microsoft Q&A: https://learn.microsoft.com/answers
- Azure Community: https://techcommunity.microsoft.com/azure
- Official Discord/Reddit communities

Remember: Hands-on practice is more valuable than memorization. The time you spent building these labs will serve you well on the exam and in your Azure career!
