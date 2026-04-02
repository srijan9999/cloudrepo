# 🖥️ Practical: Launch Your First Amazon EC2 Instance

**Course:** Cloud Computing Fundamentals
**Practical No.:** 02
**Topic:** Deploying a Virtual Machine on AWS Using Amazon EC2

---

## 📋 Objective

To deploy a virtual machine on AWS by launching an Amazon EC2 instance from the Management Console, configuring a security group to allow SSH access, and connecting to the instance using an SSH client.

---

## 🛠️ Prerequisites

- An active AWS free-tier account (refer to Practical 01)
- AWS Management Console access at [https://console.aws.amazon.com](https://console.aws.amazon.com)
- A computer with a terminal:
  - **Linux / macOS** — built-in Terminal app
  - **Windows** — PuTTY, MobaXterm, or Windows Subsystem for Linux (WSL)
- Basic understanding of what a virtual machine is

---

## 🔑 Key Concepts

| Term | Definition |
|------|-----------|
| EC2 | Elastic Compute Cloud — AWS's virtual machine service |
| AMI | Amazon Machine Image — a pre-configured OS template used to launch instances |
| Instance | A running virtual machine in the cloud |
| Key Pair | A set of cryptographic keys (public + private) used for secure SSH login |
| Security Group | A virtual firewall that controls inbound and outbound traffic to an instance |
| SSH | Secure Shell — a protocol for securely connecting to a remote server via terminal |
| Elastic IP | A static public IP address that can be attached to an EC2 instance |

---

## Part A — Signing In and Setting the Region

### Step 1: Sign In to AWS Console

1. Go to [https://console.aws.amazon.com](https://console.aws.amazon.com).
2. Select **Root user**, enter your email and password, and click **Sign in**.

---

### Step 2: Set Your Region to Mumbai

1. Click the **region selector** in the top-right corner of the console.
2. Select **Asia Pacific (Mumbai) — ap-south-1**.

> **Why Mumbai?** Choosing a region close to your physical location reduces network latency and keeps data within the country.

---

#### 📸 Screenshot 1 — AWS Console with Mumbai Region Selected

> _Insert screenshot of the AWS Console home with ap-south-1 selected in the region dropdown._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS Console — Region set to Asia Pacific (Mumbai) ap-south-1
```

---

## Part B — Creating a Key Pair

A key pair is required to securely SSH into your EC2 instance. You must download the private key before launching the instance — AWS does not store it.

### Step 1: Navigate to Key Pairs

1. In the search bar, type `EC2` and open the EC2 service.
2. In the left sidebar, scroll down to **Network & Security** → click **Key Pairs**.

---

### Step 2: Create a New Key Pair

1. Click **Create key pair** (top-right).
2. Fill in the details:
   - **Name:** `ec2-lab-keypair`
   - **Key pair type:** RSA
   - **Private key file format:**
     - Choose `.pem` if you are on **Linux or macOS**
     - Choose `.ppk` if you are on **Windows using PuTTY**
3. Click **Create key pair**.
4. The private key file will **automatically download** to your computer. Save it somewhere safe (e.g., `~/Downloads/ec2-lab-keypair.pem`).

> **Warning:** You can only download this file once. If you lose it, you cannot connect to the instance and will need to create a new key pair.

---

### Step 3: Set Correct Permissions on the Key File (Linux/macOS only)

Open your terminal and run:

```bash
chmod 400 ~/Downloads/ec2-lab-keypair.pem
```

This restricts the key file so only your user can read it — SSH will refuse to use the key if permissions are too open.

---

#### 📸 Screenshot 2 — Key Pair Created

> _Insert screenshot of the Key Pairs page showing ec2-lab-keypair listed._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Key Pairs Page with ec2-lab-keypair Created
```

---

## Part C — Configuring a Security Group

A security group acts as a virtual firewall. You will configure it to allow SSH traffic (port 22) so you can connect to the instance.

### Step 1: Navigate to Security Groups

1. In the EC2 left sidebar, under **Network & Security**, click **Security Groups**.
2. You will see the **default** security group. Do not modify it — create a new one instead.

---

### Step 2: Create a New Security Group

1. Click **Create security group** (top-right).
2. Fill in the details:
   - **Security group name:** `ec2-lab-sg`
   - **Description:** `Allow SSH access for EC2 lab`
   - **VPC:** Leave as default VPC

---

### Step 3: Add an Inbound Rule for SSH

1. Under **Inbound rules**, click **Add rule**.
2. Configure the rule:
   - **Type:** SSH
   - **Protocol:** TCP (auto-filled)
   - **Port range:** 22 (auto-filled)
   - **Source:** My IP (AWS will auto-detect and fill in your current public IP address)
3. Leave **Outbound rules** as default (allow all outbound traffic).
4. Click **Create security group**.

> **Why "My IP" and not "Anywhere"?** Setting source to `0.0.0.0/0` (Anywhere) exposes port 22 to the entire internet, making your instance vulnerable to brute-force attacks. Using "My IP" restricts access to your current network only.

---

#### 📸 Screenshot 3 — Security Group Inbound Rules

> _Insert screenshot of the security group ec2-lab-sg showing the SSH inbound rule on port 22._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Security Group ec2-lab-sg with SSH Inbound Rule (Port 22)
```

---

#### 📸 Screenshot 4 — Security Group Created Successfully

> _Insert screenshot of the Security Groups list showing ec2-lab-sg created._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Security Groups List with ec2-lab-sg
```

---

## Part D — Launching the EC2 Instance

### Step 1: Open the Launch Instance Wizard

1. In the EC2 left sidebar, click **Instances**.
2. Click **Launch instances** (top-right, orange button).

---

### Step 2: Name Your Instance

1. Under **Name and tags**, enter the name: `my-first-ec2`.

---

### Step 3: Choose an Amazon Machine Image (AMI)

1. Under **Application and OS Images (Amazon Machine Image)**:
   - Make sure **Quick Start** tab is selected.
   - Click **Amazon Linux**.
   - From the dropdown, select **Amazon Linux 2 AMI (HVM) — Kernel 5.10, SSD Volume Type**.
   - Confirm the label shows **Free tier eligible**.

> **What is an AMI?** An AMI is a pre-configured template that contains the OS, software, and configuration needed to launch an instance. Amazon Linux 2 is a lightweight, AWS-optimized Linux distribution well-suited for cloud workloads.

---

#### 📸 Screenshot 5 — AMI Selection (Amazon Linux 2)

> _Insert screenshot of the AMI selection screen with Amazon Linux 2 chosen and "Free tier eligible" label visible._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Amazon Linux 2 AMI Selected (Free Tier Eligible)
```

---

### Step 4: Choose an Instance Type

1. Under **Instance type**, click the dropdown.
2. Select **t2.micro** — 1 vCPU, 1 GB RAM.
3. Confirm the label shows **Free tier eligible**.

> **Instance type naming convention:** The `t` in `t2.micro` stands for "burstable" — these instances can burst CPU performance briefly when needed, making them ideal for low-traffic workloads and labs.

---

#### 📸 Screenshot 6 — Instance Type Selection (t2.micro)

> _Insert screenshot showing t2.micro selected with "Free tier eligible" label._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — t2.micro Instance Type Selected (Free Tier Eligible)
```

---

### Step 5: Select the Key Pair

1. Under **Key pair (login)**, click the dropdown.
2. Select **ec2-lab-keypair** (the key pair you created in Part B).

---

### Step 6: Configure Network Settings

1. Click **Edit** next to Network settings.
2. Confirm the **VPC** is set to the default VPC.
3. Set **Auto-assign public IP** to **Enable** — this gives your instance a public IP address so you can connect to it from outside AWS.
4. Under **Firewall (security groups)**, select **Select existing security group**.
5. From the dropdown, choose **ec2-lab-sg** (the security group you created in Part C).

---

#### 📸 Screenshot 7 — Network Settings with Security Group Selected

> _Insert screenshot of the Network Settings section showing ec2-lab-sg selected and public IP enabled._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Network Settings with ec2-lab-sg and Public IP Enabled
```

---

### Step 7: Configure Storage

1. Under **Configure storage**, the default is **8 GiB gp3** (General Purpose SSD).
2. Keep this as default. Free tier allows up to **30 GiB** of EBS storage.

> **EBS (Elastic Block Store)** is AWS's block storage service — it acts as the hard drive attached to your EC2 instance.

---

### Step 8: Review and Launch

1. Review the **Summary** panel on the right side:
   - AMI: Amazon Linux 2
   - Instance type: t2.micro
   - Key pair: ec2-lab-keypair
   - Security group: ec2-lab-sg
   - Storage: 8 GiB gp3
2. Click **Launch instance**.
3. You will see a success message: *"Successfully initiated launch of instance (i-xxxxxxxxxxxxxxxxx)"*.
4. Click **View all instances** to go to the Instances page.

---

#### 📸 Screenshot 8 — Launch Instance Summary Panel

> _Insert screenshot of the Summary panel on the right side before clicking Launch._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Launch Instance Summary (Before Launch)
```

---

#### 📸 Screenshot 9 — Instance Launch Success Message

> _Insert screenshot of the success screen after clicking Launch instance._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Instance Launch Success Confirmation
```

---

## Part E — Monitoring the Instance State

### Step 1: View the Instance in the Console

1. On the Instances page, locate **my-first-ec2**.
2. Note the **Instance state** column — it will first show **Pending**, then change to **Running** after 1–2 minutes.
3. Note the **Status check** column — wait until it shows **2/2 checks passed** before connecting.

---

### Step 2: Note the Instance Details

Click on the instance ID to open the **Instance summary** panel. Record the following:

| Detail | Value (fill in your own) |
|--------|--------------------------|
| Instance ID | i-xxxxxxxxxxxxxxxxx |
| Instance state | Running |
| Instance type | t2.micro |
| Public IPv4 address | e.g., 13.235.xx.xx |
| Private IPv4 address | e.g., 172.31.xx.xx |
| AMI name | Amazon Linux 2 |
| Key pair name | ec2-lab-keypair |
| Security group | ec2-lab-sg |
| Availability Zone | ap-south-1a / 1b / 1c |

---

#### 📸 Screenshot 10 — Instance in Running State

> _Insert screenshot of the Instances page showing my-first-ec2 in "Running" state with 2/2 status checks passed._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Instance Running with 2/2 Status Checks Passed
```

---

#### 📸 Screenshot 11 — Instance Summary Details Panel

> _Insert screenshot of the Instance summary panel showing Public IP, Instance ID, and other details._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Instance Summary Panel with Public IP and Details
```

---

## Part F — Connecting to the Instance via SSH

### For Linux and macOS Users

### Step 1: Open the Terminal

Open the **Terminal** application on your computer.

---

### Step 2: Navigate to the Key File Location

```bash
cd ~/Downloads
```

---

### Step 3: Connect via SSH

Replace `<YOUR_PUBLIC_IP>` with the Public IPv4 address from the instance summary panel:

```bash
ssh -i ec2-lab-keypair.pem ec2-user@<YOUR_PUBLIC_IP>
```

**Example:**
```bash
ssh -i ec2-lab-keypair.pem ec2-user@13.235.45.67
```

---

### Step 4: Accept the Host Fingerprint

On first connection, you will see a prompt:

```
The authenticity of host '13.235.45.67 (13.235.45.67)' can't be established.
ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type `yes` and press Enter.

---

### Step 5: Confirm You Are Logged In

You should see the Amazon Linux welcome banner:

```
       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
```

Your terminal prompt will change to:

```
[ec2-user@ip-172-31-xx-xx ~]$
```

You are now inside your cloud virtual machine.

---

### For Windows Users (Using PuTTY)

### Step 1: Install PuTTY

Download and install PuTTY from [https://www.putty.org](https://www.putty.org).

---

### Step 2: Open PuTTYgen to Convert the Key

1. Open **PuTTYgen** (installed with PuTTY).
2. Click **Load** → select your `.pem` file (set file filter to "All Files").
3. Click **Save private key** → save as `ec2-lab-keypair.ppk`.
4. Close PuTTYgen.

> **Note:** If you selected `.ppk` format when creating the key pair in AWS, skip Steps 2–3 and use the `.ppk` file directly.

---

### Step 3: Connect Using PuTTY

1. Open **PuTTY**.
2. In the **Host Name** field, enter: `ec2-user@<YOUR_PUBLIC_IP>`
3. In the left panel, go to **Connection → SSH → Auth → Credentials**.
4. Under **Private key file for authentication**, click **Browse** and select `ec2-lab-keypair.ppk`.
5. Go back to **Session** and click **Open**.
6. Click **Accept** when prompted about the host key.
7. You are now connected to your EC2 instance.

---

#### 📸 Screenshot 12 — SSH Terminal Connected (Linux/macOS)

> _Insert screenshot of your terminal showing the Amazon Linux 2 welcome banner and ec2-user prompt._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: SSH — Successfully Connected to EC2 Instance (Amazon Linux 2 Banner Visible)
```

---

#### 📸 Screenshot 13 — PuTTY Connection (Windows only)

> _Insert screenshot of PuTTY terminal showing successful connection (Windows users only)._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: PuTTY — Successfully Connected to EC2 Instance (Windows)
```

---

## Part G — Exploring the Instance via SSH

Once connected, run the following commands inside the EC2 instance and record your observations:

### Step 1: Check System Information

```bash
# View OS details
cat /etc/os-release

# Check hostname
hostname

# View uptime
uptime
```

---

### Step 2: Check Hardware Resources

```bash
# View CPU information
nproc
lscpu | grep "Model name"

# View RAM
free -h

# View disk space
df -h
```

---

### Step 3: Check Network Configuration

```bash
# View IP address (private IP)
ip addr show

# Check internet connectivity
ping -c 4 google.com

# View routing table
ip route
```

---

### Step 4: Update the System Packages

```bash
# Update all installed packages
sudo yum update -y
```

---

### Step 5: Install a Simple Web Server (Apache)

```bash
# Install Apache HTTP server
sudo yum install httpd -y

# Start the Apache service
sudo systemctl start httpd

# Enable Apache to start on boot
sudo systemctl enable httpd

# Verify it is running
sudo systemctl status httpd
```

---

#### 📸 Screenshot 14 — System Info Commands Output

> _Insert screenshot of the terminal showing output of cat /etc/os-release and free -h._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: EC2 SSH Session — System Info (OS Release and RAM Output)
```

---

#### 📸 Screenshot 15 — Apache Installed and Running

> _Insert screenshot of the terminal showing sudo systemctl status httpd with "active (running)" status._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: EC2 SSH Session — Apache HTTP Server Running (systemctl status httpd)
```

---

## Part H — Connecting to Apache via Browser (Optional)

To access the Apache web server from your browser, you need to open port 80 in the security group.

### Step 1: Add HTTP Rule to Security Group

1. Go back to the AWS Console → EC2 → **Security Groups**.
2. Click on **ec2-lab-sg**.
3. Click **Inbound rules** tab → **Edit inbound rules**.
4. Click **Add rule**:
   - **Type:** HTTP
   - **Protocol:** TCP
   - **Port:** 80
   - **Source:** My IP
5. Click **Save rules**.

---

### Step 2: Access Apache in Your Browser

1. Copy the **Public IPv4 address** of your instance from the Instance summary panel.
2. Open a browser and go to: `http://<YOUR_PUBLIC_IP>`
3. You should see the **Apache HTTP Server Test Page** — a red and white page confirming Apache is running.

---

#### 📸 Screenshot 16 — Apache Test Page in Browser

> _Insert screenshot of the browser showing the Apache HTTP Server Test Page at the instance's public IP._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: Browser — Apache HTTP Server Test Page Accessed via EC2 Public IP
```

---

## Part I — Terminating the Instance (Cleanup)

> **Important:** Always terminate instances after the practical to avoid consuming free-tier hours.

### Step 1: Terminate the EC2 Instance

1. Go to EC2 → **Instances**.
2. Select the checkbox next to **my-first-ec2**.
3. Click **Instance state** (top-right dropdown) → **Terminate instance**.
4. Click **Terminate** in the confirmation dialog.
5. The instance state will change to **Shutting down**, then **Terminated**.

> **Note:** Terminated instances remain visible in the console for a short period before disappearing. You cannot restart a terminated instance — termination is permanent.

---

### Step 2: Verify No Running Instances

1. Refresh the Instances page.
2. Confirm no instances are in **Running** state.
3. Check **Elastic IPs** in the left sidebar — release any unattached Elastic IPs to avoid charges.

---

#### 📸 Screenshot 17 — Instance Terminated

> _Insert screenshot of the Instances page showing my-first-ec2 in "Terminated" state._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Instance Successfully Terminated
```

---

## Part J — Observations and Questions

Answer the following in your written report:

1. What is the difference between an **AMI** and an **EC2 instance**? Use an analogy to explain.
2. What does the **t2.micro** instance type offer in terms of vCPU and RAM? What kind of workloads is it suitable for?
3. Why is it important to restrict SSH access to **My IP** rather than allowing access from anywhere (`0.0.0.0/0`)?
4. What is the difference between the **public IP** and **private IP** of your instance? When would you use each?
5. What happened when you ran `sudo yum update -y`? How many packages were updated?
6. What is the purpose of running `sudo systemctl enable httpd`? What would happen if you skipped this command and rebooted the instance?
7. What is the difference between **stopping** and **terminating** an EC2 instance?

---

## ⚠️ Important Reminders

- **Terminate your instance** immediately after the practical — a running `t2.micro` consumes 750 free-tier hours per month shared across all instances.
- **Delete your S3 bucket** if created during exploration — object storage also has free-tier limits.
- **Do not share your `.pem` private key** file — treat it like a password.
- If your public IP changes (e.g., after stopping and starting the instance), you must update the SSH command with the new IP.

---

## ✅ Submission Checklist

- [ ] Key pair `ec2-lab-keypair` created and `.pem` file downloaded
- [ ] Security group `ec2-lab-sg` created with SSH (port 22) inbound rule
- [ ] EC2 instance `my-first-ec2` launched with Amazon Linux 2 AMI and t2.micro
- [ ] Instance reached **Running** state with **2/2 status checks passed**
- [ ] Successfully connected to instance via SSH
- [ ] Ran system info commands (`cat /etc/os-release`, `free -h`, `df -h`)
- [ ] Ran `sudo yum update -y` and `sudo yum install httpd -y`
- [ ] (Optional) Apache test page accessed in browser via public IP
- [ ] Instance terminated after the practical
- [ ] Screenshots 1–17 captured and inserted
- [ ] All 7 observation questions answered in the report

---

## 📚 References

- Amazon EC2 Documentation: https://docs.aws.amazon.com/ec2
- Amazon Linux 2 AMI: https://aws.amazon.com/amazon-linux-2
- AWS Key Pairs Guide: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html
- AWS Security Groups Guide: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html
- Connecting to EC2 via SSH: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-ssh.html
- PuTTY Download: https://www.putty.org
