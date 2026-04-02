# 🌐 Practical: Set Up a Virtual Private Cloud (VPC)

**Course:** Cloud Computing Fundamentals
**Practical No.:** 03
**Topic:** Creating and Configuring a Custom VPC with Public and Private Subnets

---

## 📋 Objective

To create and configure a custom **Amazon Virtual Private Cloud (VPC)** with a public subnet and a private subnet, launch EC2 instances in each subnet, attach an **Internet Gateway** for public internet access, and configure a **NAT Gateway** to allow private subnet instances to reach the internet without being publicly accessible.

---

## 🗺️ Architecture Overview

```
                        INTERNET
                            |
                    [Internet Gateway]
                            |
         ┌──────────────────────────────────────┐
         │           Custom VPC                  │
         │         10.0.0.0/16                   │
         │                                        │
         │  ┌─────────────────────────────────┐  │
         │  │     Public Subnet               │  │
         │  │     10.0.1.0/24                 │  │
         │  │                                  │  │
         │  │  [EC2 - Public Instance]         │  │
         │  │  [NAT Gateway]                   │  │
         │  └─────────────────────────────────┘  │
         │                 |                      │
         │  ┌─────────────────────────────────┐  │
         │  │     Private Subnet              │  │
         │  │     10.0.2.0/24                 │  │
         │  │                                  │  │
         │  │  [EC2 - Private Instance]        │  │
         │  └─────────────────────────────────┘  │
         └──────────────────────────────────────┘
```

---

## 🛠️ Prerequisites

- An active AWS free-tier account
- AWS Management Console access at [https://console.aws.amazon.com](https://console.aws.amazon.com)
- Completion of Practical 02 (EC2 basics and SSH connection)
- A key pair already created (or create a new one — refer to Practical 02, Part B)

---

## 🔑 Key Concepts

| Term | Definition |
|------|-----------|
| VPC | Virtual Private Cloud — an isolated virtual network within AWS |
| CIDR Block | Classless Inter-Domain Routing — defines the IP address range of a network (e.g., `10.0.0.0/16`) |
| Subnet | A subdivision of a VPC — can be public (internet-accessible) or private |
| Public Subnet | A subnet whose route table directs traffic to an Internet Gateway |
| Private Subnet | A subnet with no direct route to the internet |
| Internet Gateway | A horizontally scaled AWS component that allows VPC resources to communicate with the internet |
| NAT Gateway | Network Address Translation — allows private subnet instances to initiate outbound internet connections without exposing them publicly |
| Route Table | A set of rules that determines where network traffic is directed |
| Elastic IP | A static public IP address required to attach to a NAT Gateway |

---

## Part A — Signing In and Setting the Region

### Step 1: Sign In

1. Go to [https://console.aws.amazon.com](https://console.aws.amazon.com) and sign in.
2. Set your region to **Asia Pacific (Mumbai) — ap-south-1** using the region selector (top-right).

---

#### 📸 Screenshot 1 — AWS Console with Mumbai Region

> _Insert screenshot of the AWS Console with ap-south-1 selected._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS Console — Region set to Asia Pacific (Mumbai) ap-south-1
```

---

## Part B — Creating a Custom VPC

### Step 1: Navigate to VPC Service

1. In the search bar, type `VPC` and click **VPC** under Services.
2. You are on the **VPC Dashboard**. Note the existing default VPC — you will create a new one separately.

---

### Step 2: Create the VPC

1. In the left sidebar, click **Your VPCs**.
2. Click **Create VPC** (top-right).
3. Under **Resources to create**, select **VPC only** (you will create subnets manually in the next steps).
4. Fill in the details:
   - **Name tag:** `lab-vpc`
   - **IPv4 CIDR block:** `10.0.0.0/16`
   - **IPv6 CIDR block:** No IPv6 CIDR block
   - **Tenancy:** Default
5. Click **Create VPC**.

> **What does 10.0.0.0/16 mean?** The `/16` prefix gives you 65,536 IP addresses (from `10.0.0.0` to `10.0.255.255`) within this VPC. You will subdivide this range into smaller subnets.

---

#### 📸 Screenshot 2 — VPC Creation Form

> _Insert screenshot of the Create VPC form filled in with lab-vpc and 10.0.0.0/16._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Create VPC Form (lab-vpc, CIDR 10.0.0.0/16)
```

---

#### 📸 Screenshot 3 — VPC Created Successfully

> _Insert screenshot of the Your VPCs page showing lab-vpc with state "Available"._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — lab-vpc Created with State Available
```

---

### Step 3: Enable DNS Hostnames for the VPC

1. Select **lab-vpc** from the list.
2. Click **Actions** → **Edit VPC settings**.
3. Check **Enable DNS hostnames**.
4. Click **Save**.

> **Why?** Enabling DNS hostnames allows EC2 instances in this VPC to receive public DNS names (e.g., `ec2-13-235-xx-xx.ap-south-1.compute.amazonaws.com`), which is required for proper SSH and web access.

---

## Part C — Creating Subnets

You will create two subnets — one public and one private — in different Availability Zones for resilience.

### Step 1: Create the Public Subnet

1. In the left sidebar, click **Subnets**.
2. Click **Create subnet** (top-right).
3. Under **VPC ID**, select **lab-vpc** from the dropdown.
4. Under **Subnet settings**, fill in:
   - **Subnet name:** `lab-public-subnet`
   - **Availability Zone:** `ap-south-1a`
   - **IPv4 subnet CIDR block:** `10.0.1.0/24`
5. Click **Create subnet**.

> **10.0.1.0/24** gives you 256 IP addresses (10.0.1.0 – 10.0.1.255), with 251 usable (AWS reserves 5 per subnet).

---

#### 📸 Screenshot 4 — Public Subnet Creation Form

> _Insert screenshot of the Create Subnet form for lab-public-subnet._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Create Public Subnet (lab-public-subnet, 10.0.1.0/24)
```

---

### Step 2: Enable Auto-assign Public IP on the Public Subnet

1. Select **lab-public-subnet** from the Subnets list.
2. Click **Actions** → **Edit subnet settings**.
3. Check **Enable auto-assign public IPv4 address**.
4. Click **Save**.

> **Why?** Any EC2 instance launched in this subnet will automatically receive a public IP address, making it reachable from the internet.

---

### Step 3: Create the Private Subnet

1. Click **Create subnet** again.
2. Under **VPC ID**, select **lab-vpc**.
3. Fill in:
   - **Subnet name:** `lab-private-subnet`
   - **Availability Zone:** `ap-south-1b`
   - **IPv4 subnet CIDR block:** `10.0.2.0/24`
4. Click **Create subnet**.

> **Note:** Do NOT enable auto-assign public IP for the private subnet. Instances here should not be directly reachable from the internet.

---

#### 📸 Screenshot 5 — Both Subnets Created

> _Insert screenshot of the Subnets list showing both lab-public-subnet and lab-private-subnet under lab-vpc._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Subnets List Showing Public and Private Subnets
```

---

## Part D — Creating and Attaching an Internet Gateway

An Internet Gateway (IGW) allows instances in the public subnet to communicate with the internet.

### Step 1: Create the Internet Gateway

1. In the left sidebar, click **Internet Gateways**.
2. Click **Create internet gateway** (top-right).
3. Fill in:
   - **Name tag:** `lab-igw`
4. Click **Create internet gateway**.

---

#### 📸 Screenshot 6 — Internet Gateway Created

> _Insert screenshot of the Internet Gateway page showing lab-igw with state "Detached"._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Internet Gateway lab-igw Created (State: Detached)
```

---

### Step 2: Attach the Internet Gateway to the VPC

1. Select **lab-igw** from the list.
2. Click **Actions** → **Attach to VPC**.
3. From the **Available VPCs** dropdown, select **lab-vpc**.
4. Click **Attach internet gateway**.
5. The state will change from **Detached** to **Attached**.

---

#### 📸 Screenshot 7 — Internet Gateway Attached to VPC

> _Insert screenshot of lab-igw showing state "Attached" and VPC ID of lab-vpc._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — lab-igw Attached to lab-vpc (State: Attached)
```

---

## Part E — Configuring Route Tables

Route tables control where traffic is directed within and outside the VPC. You will create a dedicated public route table and leave the private subnet using the default (main) route table.

### Step 1: Create a Public Route Table

1. In the left sidebar, click **Route Tables**.
2. Click **Create route table** (top-right).
3. Fill in:
   - **Name:** `lab-public-rt`
   - **VPC:** `lab-vpc`
4. Click **Create route table**.

---

### Step 2: Add a Route to the Internet Gateway

1. Select **lab-public-rt** from the list.
2. Click the **Routes** tab → **Edit routes**.
3. Click **Add route**:
   - **Destination:** `0.0.0.0/0` (all internet traffic)
   - **Target:** Select **Internet Gateway** → choose **lab-igw**
4. Click **Save changes**.

> **What does 0.0.0.0/0 mean?** This is a catch-all route — any traffic destined for an IP address not within the VPC (`10.0.0.0/16`) will be sent to the Internet Gateway, giving instances internet access.

---

#### 📸 Screenshot 8 — Public Route Table Routes

> _Insert screenshot of lab-public-rt Routes tab showing the 0.0.0.0/0 route pointing to lab-igw._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Public Route Table with Internet Gateway Route (0.0.0.0/0 → lab-igw)
```

---

### Step 3: Associate the Public Route Table with the Public Subnet

1. Still on **lab-public-rt**, click the **Subnet associations** tab.
2. Click **Edit subnet associations**.
3. Check the box next to **lab-public-subnet**.
4. Click **Save associations**.

---

#### 📸 Screenshot 9 — Public Subnet Associated with Public Route Table

> _Insert screenshot of the Subnet associations tab showing lab-public-subnet linked to lab-public-rt._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — lab-public-subnet Associated with lab-public-rt
```

---

### Step 4: Verify the Private Route Table

1. In the Route Tables list, find the route table associated with **lab-vpc** that has no name (the main/default route table).
2. Rename it: click the pencil icon in the Name column → type `lab-private-rt` → confirm.
3. Click it → **Subnet associations** tab → **Edit subnet associations** → check **lab-private-subnet** → **Save associations**.
4. Click the **Routes** tab — confirm it only has a local route (`10.0.0.0/16 → local`) and no internet route.

> **This is intentional.** The private subnet has no route to the internet at this stage. Instances here cannot be accessed from or reach the internet directly.

---

#### 📸 Screenshot 10 — Private Route Table (Local Route Only)

> _Insert screenshot of lab-private-rt Routes tab showing only the local 10.0.0.0/16 route._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Private Route Table with Local Route Only (No Internet Access)
```

---

## Part F — Creating Security Groups

You will create two security groups — one for the public instance and one for the private instance.

### Step 1: Create Security Group for the Public Instance

1. In the left sidebar, click **Security Groups**.
2. Click **Create security group**.
3. Fill in:
   - **Name:** `lab-public-sg`
   - **Description:** `Allow SSH from my IP for public instance`
   - **VPC:** `lab-vpc`
4. Under **Inbound rules**, click **Add rule**:
   - **Type:** SSH | **Protocol:** TCP | **Port:** 22 | **Source:** My IP
5. Click **Create security group**.

---

### Step 2: Create Security Group for the Private Instance

1. Click **Create security group** again.
2. Fill in:
   - **Name:** `lab-private-sg`
   - **Description:** `Allow SSH from public subnet only`
   - **VPC:** `lab-vpc`
3. Under **Inbound rules**, click **Add rule**:
   - **Type:** SSH | **Protocol:** TCP | **Port:** 22 | **Source:** Custom → enter `10.0.1.0/24` (the public subnet CIDR)
4. Click **Create security group**.

> **Why restrict SSH to 10.0.1.0/24?** The private instance should only accept SSH connections from within the public subnet (i.e., via the public EC2 instance acting as a bastion/jump host). It should never be directly accessible from the internet.

---

#### 📸 Screenshot 11 — Security Groups Created

> _Insert screenshot of the Security Groups list showing both lab-public-sg and lab-private-sg._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Security Groups lab-public-sg and lab-private-sg Created
```

---

## Part G — Launching EC2 Instances

### Step 1: Launch the Public EC2 Instance

1. In the search bar, type `EC2` and open the EC2 service.
2. Click **Instances** → **Launch instances**.
3. Configure:
   - **Name:** `lab-public-ec2`
   - **AMI:** Amazon Linux 2 (Free tier eligible)
   - **Instance type:** t2.micro (Free tier eligible)
   - **Key pair:** Select your existing key pair (or create a new one)
4. Under **Network settings** → click **Edit**:
   - **VPC:** `lab-vpc`
   - **Subnet:** `lab-public-subnet`
   - **Auto-assign public IP:** Enable
   - **Security group:** Select existing → `lab-public-sg`
5. Keep storage as default (8 GiB gp3).
6. Click **Launch instance**.

---

#### 📸 Screenshot 12 — Public EC2 Network Settings

> _Insert screenshot of the Network Settings section for lab-public-ec2 showing lab-public-subnet and lab-public-sg selected._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Public Instance Network Settings (lab-public-subnet, lab-public-sg)
```

---

### Step 2: Launch the Private EC2 Instance

1. Click **Launch instances** again.
2. Configure:
   - **Name:** `lab-private-ec2`
   - **AMI:** Amazon Linux 2 (Free tier eligible)
   - **Instance type:** t2.micro (Free tier eligible)
   - **Key pair:** Select the same key pair
3. Under **Network settings** → click **Edit**:
   - **VPC:** `lab-vpc`
   - **Subnet:** `lab-private-subnet`
   - **Auto-assign public IP:** Disable
   - **Security group:** Select existing → `lab-private-sg`
4. Keep storage as default.
5. Click **Launch instance**.

---

#### 📸 Screenshot 13 — Private EC2 Network Settings

> _Insert screenshot of the Network Settings section for lab-private-ec2 showing lab-private-subnet and lab-private-sg, with public IP disabled._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Private Instance Network Settings (lab-private-subnet, lab-private-sg, No Public IP)
```

---

### Step 3: Verify Both Instances Are Running

1. Go to EC2 → **Instances**.
2. Confirm both **lab-public-ec2** and **lab-private-ec2** are in **Running** state with **2/2 checks passed**.
3. Note down:
   - **lab-public-ec2** — Public IPv4 address (e.g., `13.235.xx.xx`) and Private IPv4 (e.g., `10.0.1.xx`)
   - **lab-private-ec2** — Private IPv4 only (e.g., `10.0.2.xx`) — no public IP

---

#### 📸 Screenshot 14 — Both EC2 Instances Running

> _Insert screenshot of the Instances list showing both lab-public-ec2 and lab-private-ec2 in Running state._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Both Instances Running (Public with Public IP, Private with No Public IP)
```

---

## Part H — Connecting to the Public Instance via SSH

### Step 1: SSH into the Public Instance

On your local terminal, run:

```bash
ssh -i your-keypair.pem ec2-user@<PUBLIC_IP_OF_lab-public-ec2>
```

**Example:**
```bash
ssh -i ec2-lab-keypair.pem ec2-user@13.235.45.67
```

Type `yes` when prompted about the host fingerprint. You should see the Amazon Linux 2 welcome banner.

---

### Step 2: Verify Internet Access from the Public Instance

Once connected, run:

```bash
# Check public IP
curl ifconfig.me

# Test internet connectivity
ping -c 4 google.com

# Confirm subnet
ip addr show
```

You should get successful ping replies — confirming the public instance has internet access via the Internet Gateway.

---

#### 📸 Screenshot 15 — SSH into Public Instance with Internet Access Confirmed

> _Insert screenshot of the terminal showing successful SSH login and ping google.com working._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: SSH — Connected to lab-public-ec2, Internet Access Confirmed (ping google.com)
```

---

## Part H — Connecting to the Private Instance (via Bastion/Jump Host)

The private instance has no public IP and cannot be reached directly. You will SSH into the public instance first, and then SSH from there into the private instance. This pattern is called a **bastion host** or **jump host**.

### Step 1: Copy the Private Key to the Public Instance

On your **local machine terminal**, run:

```bash
scp -i ec2-lab-keypair.pem ec2-lab-keypair.pem ec2-user@<PUBLIC_IP>:~/.ssh/
```

**Example:**
```bash
scp -i ec2-lab-keypair.pem ec2-lab-keypair.pem ec2-user@13.235.45.67:~/.ssh/
```

> **Note:** Copying your private key to a server is done here for lab/learning purposes only. In production, use SSH agent forwarding (`ssh -A`) instead to avoid storing the key on the bastion host.

---

### Step 2: Set Key Permissions on the Public Instance

SSH into the public instance, then run:

```bash
chmod 400 ~/.ssh/ec2-lab-keypair.pem
```

---

### Step 3: SSH from the Public Instance into the Private Instance

Still inside the public instance SSH session, run:

```bash
ssh -i ~/.ssh/ec2-lab-keypair.pem ec2-user@<PRIVATE_IP_OF_lab-private-ec2>
```

**Example:**
```bash
ssh -i ~/.ssh/ec2-lab-keypair.pem ec2-user@10.0.2.45
```

You should now be inside the **private EC2 instance**.

---

### Step 4: Verify the Private Instance Has No Internet Access (Yet)

Inside the private instance, run:

```bash
ping -c 4 google.com
```

This should **fail** — confirming the private instance cannot reach the internet directly. You will fix this in Part I by adding a NAT Gateway.

---

#### 📸 Screenshot 16 — SSH into Private Instance via Bastion

> _Insert screenshot showing the two-hop SSH: local → public instance → private instance, with ping failing._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: SSH — Connected to lab-private-ec2 via Bastion (Ping to Internet Fails — Expected)
```

---

## Part I — Setting Up a NAT Gateway

A NAT Gateway allows private subnet instances to initiate outbound internet connections (e.g., to download updates) without being reachable from the internet.

### Step 1: Allocate an Elastic IP Address

A NAT Gateway requires a static public IP (Elastic IP).

1. In the EC2 left sidebar, under **Network & Security**, click **Elastic IPs**.
2. Click **Allocate Elastic IP address** (top-right).
3. Keep all defaults (Amazon's pool of IPv4 addresses).
4. Click **Allocate**.
5. Note the allocated IP address (e.g., `15.206.xx.xx`).

---

#### 📸 Screenshot 17 — Elastic IP Allocated

> _Insert screenshot of the Elastic IPs page showing the newly allocated IP._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Elastic IP Allocated for NAT Gateway
```

---

### Step 2: Create the NAT Gateway

1. Go back to the **VPC** service (search bar → VPC).
2. In the left sidebar, click **NAT Gateways**.
3. Click **Create NAT gateway** (top-right).
4. Fill in:
   - **Name:** `lab-nat-gw`
   - **Subnet:** `lab-public-subnet` ← NAT Gateway must be in the PUBLIC subnet
   - **Connectivity type:** Public
   - **Elastic IP allocation ID:** Select the Elastic IP you just allocated
5. Click **Create NAT gateway**.
6. Wait for the NAT Gateway state to change from **Pending** to **Available** (takes 1–2 minutes).

> **Critical:** The NAT Gateway must be placed in the **public subnet**, not the private subnet. It acts as an outbound proxy for private instances — it sits in the public subnet with internet access and forwards requests on behalf of private instances.

---

#### 📸 Screenshot 18 — NAT Gateway Created

> _Insert screenshot of the NAT Gateways page showing lab-nat-gw with state "Available"._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — NAT Gateway lab-nat-gw in Available State (in lab-public-subnet)
```

---

### Step 3: Update the Private Route Table to Use the NAT Gateway

1. In the VPC left sidebar, click **Route Tables**.
2. Select **lab-private-rt**.
3. Click the **Routes** tab → **Edit routes**.
4. Click **Add route**:
   - **Destination:** `0.0.0.0/0`
   - **Target:** Select **NAT Gateway** → choose **lab-nat-gw**
5. Click **Save changes**.

---

#### 📸 Screenshot 19 — Private Route Table Updated with NAT Gateway Route

> _Insert screenshot of lab-private-rt Routes tab showing 0.0.0.0/0 route pointing to lab-nat-gw._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Private Route Table with NAT Gateway Route (0.0.0.0/0 → lab-nat-gw)
```

---

## Part J — Verifying NAT Gateway Internet Access

### Step 1: SSH Back into the Private Instance

From your local terminal:

```bash
# Step 1: SSH into public instance
ssh -i ec2-lab-keypair.pem ec2-user@<PUBLIC_IP_OF_lab-public-ec2>

# Step 2: From inside the public instance, SSH into the private instance
ssh -i ~/.ssh/ec2-lab-keypair.pem ec2-user@<PRIVATE_IP_OF_lab-private-ec2>
```

---

### Step 2: Test Internet Access from the Private Instance

Inside the private instance, run:

```bash
# Test outbound internet access via NAT Gateway
ping -c 4 google.com

# Try downloading a package update
sudo yum update -y

# Confirm the private IP (no public IP visible)
curl ifconfig.me
```

The ping should now **succeed** — the private instance is reaching the internet via the NAT Gateway.

Note that `curl ifconfig.me` will return the **Elastic IP of the NAT Gateway**, not the private instance's IP. This shows that outbound traffic is being translated (NATted) through the NAT Gateway.

---

#### 📸 Screenshot 20 — Private Instance Internet Access via NAT Gateway

> _Insert screenshot of the private instance terminal showing ping google.com succeeding and curl ifconfig.me returning the NAT Gateway's Elastic IP._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: SSH — Private Instance Internet Access via NAT Gateway Confirmed (ping google.com succeeds)
```

---

## Part K — Final VPC Architecture Verification

### Step 1: Review Your VPC Resource Map

1. Go to VPC → **Your VPCs**.
2. Select **lab-vpc**.
3. Click the **Resource map** tab.
4. You will see a visual diagram of your VPC showing both subnets, route tables, the Internet Gateway, and the NAT Gateway all connected.

---

#### 📸 Screenshot 21 — VPC Resource Map

> _Insert screenshot of the VPC Resource Map tab showing the full architecture of lab-vpc._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Resource Map of lab-vpc (Full Architecture View)
```

---

### Step 2: Summary Table — Fill in Your Values

| Resource | Name | Value |
|----------|------|-------|
| VPC | lab-vpc | CIDR: 10.0.0.0/16 |
| Public Subnet | lab-public-subnet | CIDR: 10.0.1.0/24 / AZ: ap-south-1a |
| Private Subnet | lab-private-subnet | CIDR: 10.0.2.0/24 / AZ: ap-south-1b |
| Internet Gateway | lab-igw | Attached to: lab-vpc |
| NAT Gateway | lab-nat-gw | Subnet: lab-public-subnet |
| Elastic IP | — | IP: (fill in your value) |
| Public Route Table | lab-public-rt | Routes: local + 0.0.0.0/0 → lab-igw |
| Private Route Table | lab-private-rt | Routes: local + 0.0.0.0/0 → lab-nat-gw |
| Public EC2 | lab-public-ec2 | Public IP: (fill in) / Private IP: 10.0.1.x |
| Private EC2 | lab-private-ec2 | Private IP only: 10.0.2.x |

---

## Part L — Cleanup (Important)

> **NAT Gateways and Elastic IPs incur charges even on free tier. Always delete them after the practical.**

### Step 1: Terminate EC2 Instances

1. Go to EC2 → **Instances**.
2. Select both **lab-public-ec2** and **lab-private-ec2**.
3. Click **Instance state** → **Terminate instance** → **Terminate**.

---

### Step 2: Delete the NAT Gateway

1. Go to VPC → **NAT Gateways**.
2. Select **lab-nat-gw** → **Actions** → **Delete NAT gateway** → confirm by typing `delete`.
3. Wait for state to change to **Deleted** (takes 1–2 minutes).

---

### Step 3: Release the Elastic IP

1. Go to EC2 → **Elastic IPs**.
2. Select the allocated IP → **Actions** → **Release Elastic IP address** → **Release**.

> **Note:** If you do not release the Elastic IP after disassociating it, AWS charges a small hourly fee for unused Elastic IPs.

---

### Step 4: Detach and Delete the Internet Gateway

1. Go to VPC → **Internet Gateways**.
2. Select **lab-igw** → **Actions** → **Detach from VPC** → confirm.
3. Then select **lab-igw** → **Actions** → **Delete internet gateway** → confirm.

---

### Step 5: Delete the Subnets

1. Go to VPC → **Subnets**.
2. Select **lab-public-subnet** → **Actions** → **Delete subnet** → confirm.
3. Select **lab-private-subnet** → **Actions** → **Delete subnet** → confirm.

---

### Step 6: Delete the VPC

1. Go to VPC → **Your VPCs**.
2. Select **lab-vpc** → **Actions** → **Delete VPC** → type `delete` to confirm.

> Deleting the VPC will also automatically delete associated route tables and security groups.

---

#### 📸 Screenshot 22 — All Resources Deleted

> _Insert screenshot of the Your VPCs page confirming lab-vpc no longer exists._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — lab-vpc Deleted, Cleanup Complete
```

---

## Part M — Observations and Questions

Answer the following in your written report:

1. What is the difference between a **public subnet** and a **private subnet**? What makes a subnet "public" in AWS?
2. Why does an Internet Gateway need to be **attached** to a VPC before it works? What would happen if you forgot this step?
3. What is the role of the **route table** in directing traffic? Explain the difference between the public and private route tables you configured.
4. Why is the **NAT Gateway placed in the public subnet** and not the private subnet?
5. When you ran `curl ifconfig.me` from the private instance, it returned the NAT Gateway's Elastic IP instead of the instance's private IP. Explain why this happens.
6. What is the difference between using a **NAT Gateway** vs an **Internet Gateway** for outbound internet access?
7. What is the **bastion host** pattern and why is it used? What are the security benefits?
8. Why must the **Elastic IP** be released separately after deleting the NAT Gateway? What happens if you forget?

---

## ⚠️ Important Reminders

- **NAT Gateways are NOT free** — they cost approximately $0.045/hour plus data processing charges. Always delete after the practical.
- **Elastic IPs cost money** when not attached to a running instance — always release them.
- **Terminating an instance is permanent** — stopped instances can be restarted, but terminated ones cannot.
- Never store your private key (`.pem`) on a production bastion host — use SSH agent forwarding instead.

---

## ✅ Submission Checklist

- [ ] Custom VPC `lab-vpc` created with CIDR `10.0.0.0/16`
- [ ] Public subnet `lab-public-subnet` created (`10.0.1.0/24`) with auto-assign public IP enabled
- [ ] Private subnet `lab-private-subnet` created (`10.0.2.0/24`) with no public IP
- [ ] Internet Gateway `lab-igw` created and attached to `lab-vpc`
- [ ] Public route table `lab-public-rt` configured with `0.0.0.0/0 → lab-igw`
- [ ] Private route table `lab-private-rt` configured with local route only (initially)
- [ ] Security groups `lab-public-sg` and `lab-private-sg` created
- [ ] `lab-public-ec2` launched in public subnet with public IP
- [ ] `lab-private-ec2` launched in private subnet with no public IP
- [ ] SSH into public instance confirmed with internet access verified
- [ ] SSH into private instance via bastion confirmed; internet access initially failed
- [ ] Elastic IP allocated and NAT Gateway `lab-nat-gw` created in public subnet
- [ ] Private route table updated with `0.0.0.0/0 → lab-nat-gw`
- [ ] Internet access from private instance confirmed via NAT Gateway
- [ ] VPC Resource Map screenshot captured
- [ ] Summary table filled in with actual IP values
- [ ] All resources cleaned up (EC2, NAT GW, Elastic IP, IGW, subnets, VPC)
- [ ] Screenshots 1–22 captured and inserted
- [ ] All 8 observation questions answered in the report

---

## 📚 References

- Amazon VPC Documentation: https://docs.aws.amazon.com/vpc
- VPC Subnets Guide: https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html
- Internet Gateways: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html
- NAT Gateways: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html
- Route Tables: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html
- Security Groups: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html
- Bastion Host Pattern: https://docs.aws.amazon.com/quickstart/latest/linux-bastion/welcome.html

---

*Practical prepared for academic use. Amazon Web Services, AWS, VPC, EC2, and related services are trademarks of Amazon.com, Inc.*
