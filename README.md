# ☁️ Practical: Introduction to AWS Cloud Platform

**Course:** Cloud Computing Fundamentals
**Practical No.:** 01
**Topic:** Familiarizing with Amazon Web Services (AWS) and Its Interface

---

## 📋 Objective

To familiarize students with **Amazon Web Services (AWS)** by creating a free-tier account, exploring the Management Console, identifying key services (compute, storage, networking), and understanding the AWS pricing model.

---

## 🛠️ Prerequisites

- A valid email address
- A credit/debit card (for identity verification only — no charges for free tier)
- A stable internet connection
- A modern web browser (Chrome or Firefox recommended)

---

## Part A — Creating an AWS Free-Tier Account

### Step 1: Visit the AWS Free Tier Page

1. Open your browser and go to [https://aws.amazon.com/free](https://aws.amazon.com/free).
2. Click the **Create a Free Account** button.

---

### Step 2: Enter Your Account Details

1. Enter your **email address** in the field provided.
2. Choose a unique **AWS account name** (e.g., `john-aws-lab`).
3. Click **Verify email address** — AWS will send a verification code to your email.
4. Open your email, copy the OTP, and enter it on the verification screen.
5. Click **Verify**.

---

### Step 3: Set a Root Password

1. Create a strong **root user password** (minimum 8 characters, mix of upper/lower case, numbers, symbols).
2. Confirm the password and click **Continue**.

> **Warning:** The root account has full access to all AWS services. Protect these credentials carefully.

---

### Step 4: Select Account Type and Fill Contact Details

1. Choose **Personal** as the account type.
2. Fill in your full name, phone number, country (India), address, city, state, and postal code.
3. Read and agree to the AWS Customer Agreement.
4. Click **Continue**.

---

### Step 5: Add Payment Information

1. Enter your **credit or debit card** details (Visa, Mastercard, or RuPay accepted).
2. AWS will charge a small verification amount (approximately ₹2) that is **automatically refunded**.
3. Click **Verify and Continue**.

> **Note:** You will not be charged as long as you stay within free tier limits.

---

### Step 6: Phone Verification

1. Select your country code (+91 for India).
2. Enter your mobile number and click **Send SMS** (or choose voice call).
3. Enter the 4-digit OTP received on your phone.
4. Click **Continue**.

---

### Step 7: Choose a Support Plan

1. On the support plan page, select **Basic support — Free**.
2. Click **Complete sign up**.
3. You will see a confirmation screen. AWS will send a confirmation email within a few minutes.

---

#### 📸 Screenshot 1 — AWS Account Created Confirmation

> _Insert screenshot of the account creation success/confirmation screen._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS — Account Creation Confirmation Screen
```

---

## Part B — Exploring the AWS Management Console

### Step 1: Sign In to the Console

1. Go to [https://console.aws.amazon.com](https://console.aws.amazon.com).
2. Select **Root user**, enter your email address, and click **Next**.
3. Enter your password and click **Sign in**.
4. You will land on the **AWS Management Console Home**.

---

### Step 2: Familiarize Yourself with the Console Layout

1. **Top navigation bar** — contains the AWS logo, search bar, services menu, region selector, account menu, and notifications bell.
2. **Recently visited** — shows services you have used recently (empty on first login).
3. **AWS Health** — shows any service disruptions or alerts.
4. **Cost and Usage** — shows your current month's charges (should show $0.00 on free tier).

---

#### 📸 Screenshot 2 — AWS Management Console Home

> _Insert screenshot of the AWS Console home dashboard after signing in._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS Management Console — Home Dashboard
```

---

### Step 3: Set Your Region

1. Click the **region selector** in the top-right corner (it may show a default like US East).
2. Change it to **Asia Pacific (Mumbai) — ap-south-1** for lower latency from India.

> **Note:** AWS organises data centers into **Regions** (geographic areas) and **Availability Zones** (isolated data centers within a region). Always select the region closest to your users.

---

#### 📸 Screenshot 3 — AWS Region Selector

> _Insert screenshot showing the region dropdown with Mumbai selected._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS — Region Selector set to Asia Pacific (Mumbai)
```

---

## Part C — Exploring Key AWS Services

### Step 1: Compute — EC2 (Elastic Compute Cloud)

EC2 is AWS's virtual machine service. It allows you to rent virtual servers in the cloud.

1. In the search bar at the top, type `EC2` and click **EC2** under Services.
2. You are now on the EC2 Dashboard. Note the panels for:
   - **Instances** — running or stopped virtual machines
   - **Instance Types** — available hardware configurations (vCPU, RAM, storage)
   - **AMIs (Amazon Machine Images)** — pre-configured OS templates
   - **Security Groups** — virtual firewalls controlling inbound/outbound traffic
   - **Key Pairs** — SSH keys used to securely connect to instances
   - **Elastic IPs** — static public IP addresses
3. Click **Instances** in the left sidebar — it will be empty since you haven't launched any yet.
4. Click **Instance Types** in the left sidebar and search for `t2.micro` — this is the free-tier eligible instance (1 vCPU, 1 GB RAM).

---

#### 📸 Screenshot 4 — EC2 Dashboard

> _Insert screenshot of the EC2 service dashboard._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Service Dashboard Overview
```

---

#### 📸 Screenshot 5 — EC2 Instance Types

> _Insert screenshot of the Instance Types page with t2.micro visible._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Instance Types (t2.micro highlighted)
```

---

### Step 2: Launch a Free-Tier EC2 Instance (Demo)

1. From the EC2 Dashboard, click **Launch Instance**.
2. Enter a name: `my-first-ec2`.
3. Under **Application and OS Images**, select **Amazon Linux 2023 AMI** (Free tier eligible).
4. Under **Instance type**, select `t2.micro` (Free tier eligible).
5. Under **Key pair**, click **Create new key pair** → name it `my-keypair` → select RSA → `.pem` format → click **Create key pair** (it will download automatically).
6. Under **Network settings**, keep the default VPC and enable **Allow SSH traffic from: My IP**.
7. Under **Configure storage**, keep the default 8 GiB gp3 volume (free tier allows up to 30 GB).
8. Review the **Summary** panel on the right.
9. Click **Launch instance**.

> **Important:** After the practical, go back to Instances, select your instance, click **Instance State → Terminate instance** to avoid charges.

---

#### 📸 Screenshot 6 — EC2 Launch Instance Configuration

> _Insert screenshot of the Launch Instance page with t2.micro and Amazon Linux selected._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Launch Instance Configuration Page
```

---

#### 📸 Screenshot 7 — EC2 Instance Running

> _Insert screenshot of the Instances page showing the instance in "running" state._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS EC2 — Instance in Running State
```

---

### Step 3: Storage — S3 (Simple Storage Service)

S3 is AWS's object storage service. It stores files (called **objects**) inside containers called **buckets**.

1. In the search bar, type `S3` and click **S3** under Services.
2. You are on the S3 console. Note:
   - **Buckets** — globally unique containers for your files
   - **Access Points** — custom access configurations for buckets
   - **Storage Classes** — Standard, Intelligent-Tiering, Glacier (for archiving)
3. Click **Create bucket**.
4. Enter a unique bucket name (e.g., `myname-lab-bucket-2026`).
5. Select Region: **Asia Pacific (Mumbai)**.
6. Leave **Block all public access** enabled (default — recommended).
7. Click **Create bucket**.
8. Click on your new bucket → click **Upload** → drag and drop any small text file → click **Upload**.
9. Once uploaded, click the file name to view its properties (URL, size, storage class, last modified).

---

#### 📸 Screenshot 8 — S3 Buckets Page

> _Insert screenshot of the S3 console showing your created bucket._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS S3 — Buckets Page with Created Bucket
```

---

#### 📸 Screenshot 9 — S3 Object Uploaded

> _Insert screenshot showing the uploaded file inside the bucket._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS S3 — Object Uploaded Inside Bucket
```

---

### Step 4: Networking — VPC (Virtual Private Cloud)

A VPC is your own private network within AWS, where you control IP ranges, subnets, routing, and firewalls.

1. In the search bar, type `VPC` and click **VPC** under Services.
2. You are on the VPC Dashboard. AWS automatically creates a **default VPC** in each region. Note:
   - **VPCs** — your isolated virtual networks
   - **Subnets** — subdivisions of a VPC (public or private)
   - **Route Tables** — rules for routing traffic between subnets and the internet
   - **Internet Gateways** — allow VPC resources to connect to the internet
   - **Security Groups** — instance-level firewalls (stateful)
   - **Network ACLs** — subnet-level firewalls (stateless)
3. Click **Your VPCs** — you will see the default VPC (CIDR: `172.31.0.0/16`).
4. Click **Subnets** — you will see multiple default subnets, one per Availability Zone.
5. Click **Internet Gateways** — the default VPC has one attached already.
6. Click **Security Groups** — explore the default security group inbound/outbound rules.

---

#### 📸 Screenshot 10 — VPC Dashboard

> _Insert screenshot of the VPC dashboard showing the default VPC._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Dashboard with Default VPC
```

---

#### 📸 Screenshot 11 — VPC Subnets View

> _Insert screenshot of the Subnets page showing subnets across Availability Zones._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS VPC — Subnets Across Availability Zones
```

---

### Step 5: IAM — Identity and Access Management

IAM controls who can access your AWS account and what they can do.

1. Search for `IAM` in the search bar and click **IAM**.
2. On the IAM Dashboard, note:
   - **Users** — individual accounts with specific permissions
   - **Groups** — collections of users sharing the same permissions
   - **Roles** — permissions assigned to AWS services (e.g., EC2 accessing S3)
   - **Policies** — JSON documents that define permissions
3. Click **Users** → **Create user**.
4. Enter a username: `lab-user`.
5. Select **Provide user access to the AWS Management Console**.
6. Choose **I want to create an IAM user** and set a custom password.
7. On the next screen, choose **Attach policies directly** → search and attach `AmazonS3ReadOnlyAccess`.
8. Review and click **Create user**.

> **Best practice:** Never use the root account for day-to-day tasks. Always create IAM users with only the permissions they need (principle of least privilege).

---

#### 📸 Screenshot 12 — IAM Dashboard

> _Insert screenshot of the IAM dashboard._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS IAM — Dashboard Overview
```

---

#### 📸 Screenshot 13 — IAM User Created

> _Insert screenshot of the newly created IAM user with attached policy._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS IAM — New User with S3 ReadOnly Policy Attached
```

---

## Part D — AWS Pricing Calculator

### Step 1: Open the Calculator

1. Go to [https://calculator.aws/pricing/2/home](https://calculator.aws/pricing/2/home).
2. Click **Create estimate**.

---

### Step 2: Add EC2 to Your Estimate

1. Click **Add service** → search `EC2` → click **Configure**.
2. Set:
   - Region: **Asia Pacific (Mumbai)**
   - Operating system: **Linux**
   - Instance type: `t2.micro`
   - Usage: **730 hours/month** (on-demand, 24/7)
3. Click **Save and add service**.

---

### Step 3: Add S3 to Your Estimate

1. Click **Add service** → search `S3` → click **Configure**.
2. Set:
   - Region: **Asia Pacific (Mumbai)**
   - Storage: **10 GB Standard**
   - GET requests: **1000**
   - PUT requests: **100**
3. Click **Save and add service**.

---

### Step 4: Add Data Transfer

1. Click **Add service** → search `Data Transfer` → click **Configure**.
2. Set outbound data transfer to internet: **5 GB/month**.
3. Observe that the first 100 GB/month out to internet is charged at a per-GB rate.
4. Click **Save and add service**.

---

### Step 5: Review and Export

1. View the **Total monthly estimate** at the bottom of the page.
2. Try switching the EC2 instance type to `t3.large` — observe how the cost changes.
3. Try switching to **Reserved Instance (1-year)** pricing — observe the discount vs on-demand.
4. Click **Share** to generate a shareable URL for your estimate.
5. Click **Export** → **CSV** to download the estimate.

---

#### 📸 Screenshot 14 — AWS Pricing Calculator Estimate

> _Insert screenshot of the completed estimate showing EC2 + S3 + Data Transfer costs._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS Pricing Calculator — Monthly Estimate (EC2 + S3 + Data Transfer)
```

---

#### 📸 Screenshot 15 — Reserved vs On-Demand Pricing Comparison

> _Insert screenshot showing the cost difference between on-demand and 1-year reserved pricing._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS Pricing Calculator — On-Demand vs Reserved Instance Comparison
```

---

## Part E — AWS CloudShell

AWS CloudShell is a browser-based terminal with the AWS CLI pre-installed — no local setup required.

1. Click the **CloudShell icon** (terminal icon) in the top navigation bar of the AWS Console.
2. Wait for the shell to initialize (takes about 30 seconds on first use).
3. Run the following commands and note the output:

```bash
# Check AWS CLI version
aws --version

# List your S3 buckets
aws s3 ls

# List EC2 instances in Mumbai region
aws ec2 describe-instances --region ap-south-1

# Check your account ID
aws sts get-caller-identity
```

---

#### 📸 Screenshot 16 — AWS CloudShell Terminal

> _Insert screenshot of the CloudShell terminal showing command output._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS CloudShell — CLI Commands and Output
```

---

## Part F — Setting Up Billing Alerts

### Step 1: Enable Billing Alerts

1. Click your account name (top-right) → **Billing and Cost Management**.
2. In the left panel, click **Billing Preferences**.
3. Enable **Receive Free Tier Usage Alerts** and enter your email.
4. Enable **Receive AWS Billing Alerts**.
5. Click **Save preferences**.

---

### Step 2: Create a Cost Alert in CloudWatch

1. Go to the search bar → type `CloudWatch` → open **CloudWatch**.
2. In the left sidebar, click **Alarms** → **All alarms** → **Create alarm**.
3. Click **Select metric** → **Billing** → **Total Estimated Charge** → select **USD** → click **Select metric**.
4. Set the threshold: **Greater than $1** (to alert before any meaningful charge).
5. Click **Next** → under Notification, create a new SNS topic with your email.
6. Confirm the subscription from your email inbox.
7. Name the alarm `FreeTierBillingAlert` → click **Create alarm**.

---

#### 📸 Screenshot 17 — Billing Alert Created

> _Insert screenshot of the CloudWatch alarm showing "OK" status._

```
[ SCREENSHOT PLACEHOLDER ]
Caption: AWS CloudWatch — Billing Alert for Free Tier
```

---

## Part G — Observations and Questions

Answer the following in your written report:

1. What is the difference between an **AWS Region** and an **Availability Zone**? Why does it matter for deploying applications?
2. What is the difference between a **Security Group** and a **Network ACL** in AWS VPC?
3. What are the free-tier limits for EC2, S3, and data transfer per month?
4. Why should you create an **IAM user** instead of using the root account for everyday tasks?
5. What is the difference between **on-demand** and **reserved** instance pricing? When would you choose each?
6. What happened to your monthly cost estimate when you changed the EC2 instance type from `t2.micro` to `t3.large`?

---

## ⚠️ Important Reminders

- **Terminate your EC2 instance** after the practical to avoid free-tier hour exhaustion.
- **Delete your S3 bucket and objects** to avoid storage charges.
- **Do not share your AWS credentials** or commit them to any public repository.
- **Monitor your billing dashboard** at least once a week during the course.
- **Never upgrade** from the Basic Support plan unless required.

---

## ✅ Submission Checklist

- [ ] AWS free-tier account created and email confirmed
- [ ] Signed in to AWS Management Console
- [ ] Region changed to Asia Pacific (Mumbai)
- [ ] EC2 dashboard explored and instance type `t2.micro` identified
- [ ] EC2 instance launched and terminated after demo
- [ ] S3 bucket created and a file uploaded
- [ ] VPC dashboard explored — default VPC, subnets, and security groups noted
- [ ] IAM user created with `AmazonS3ReadOnlyAccess` policy
- [ ] AWS Pricing Calculator estimate created (EC2 + S3 + Data Transfer)
- [ ] Billing alert configured in CloudWatch
- [ ] Screenshots 1–17 captured and inserted
- [ ] All 6 observation questions answered in the report
- [ ] All created resources (EC2 instance, S3 bucket) deleted after the practical

---

## 📚 References

- AWS Free Tier: https://aws.amazon.com/free
- AWS Management Console: https://console.aws.amazon.com
- AWS Pricing Calculator: https://calculator.aws
- AWS EC2 Documentation: https://docs.aws.amazon.com/ec2
- AWS S3 Documentation: https://docs.aws.amazon.com/s3
- AWS VPC Documentation: https://docs.aws.amazon.com/vpc
- AWS IAM Documentation: https://docs.aws.amazon.com/iam
- AWS CloudShell: https://docs.aws.amazon.com/cloudshell

