# Learnings: AWS Services and EC2 Setup

## 1. **Understanding AWS Region**
- AWS operates across multiple **regions**, each consisting of multiple **availability zones** (AZs) to ensure high availability and redundancy.
- Regions are geographically distinct, and you should select the region closest to your user base to minimize latency.
- Example: **Mumbai** for India.

## 2. **AWS Services Overview**
- AWS offers various services, including:
   - **Compute**: EC2 (virtual machines), Lambda (serverless), ECS (containers).
   - **Storage**: S3 (object storage), EBS (block storage).
   - **Networking**: VPC (Virtual Private Cloud), Route 53 (DNS).
   - **Databases**: RDS (relational databases), DynamoDB (NoSQL).
   - **Security**: IAM (Identity and Access Management), AWS Shield.

## 3. **Understanding EC2 (Elastic Compute Cloud)**
- EC2 provides scalable virtual machines (instances) in the cloud.
- **Key features**:
   - **Instance types**: Choose based on your application's CPU, memory, and storage needs (e.g., t2.micro for free-tier eligible).
   - **Security**: You can configure security groups (firewalls) to control access.
   - **Key Pairs**: Used for SSH access to your instance (with `.pem` files for Linux/Unix or `.ppk` for Windows).

## 4. **Practical Experience with EC2**
- Launching, configuring, and accessing EC2 instances.
- Focus on learning **AMI** (Amazon Machine Images), **Instance types**, and managing **key pairs** for secure access.

## 5. **Web Server Creation and Setup**
- Set up a web server on an EC2 instance using **Windows Server 2022** and configure it to serve static websites.

## 6. **Deploying a Static Website Using Windows Server**

---

### **EC2 Instance Launch Steps**

1. **Region Setup**:
   - Change the AWS region to **Mumbai** to optimize for local traffic.

2. **Launch an EC2 Instance**:
   - Go to **EC2** under AWS services.
   - Click **Launch Instance**.
   - Name the server instance (e.g., "MyWebServer").

3. **Select an AMI (Amazon Machine Image)**:
   - Choose **Microsoft Windows Server 2022 Base** (this provides a base Windows OS environment).

4. **Select Instance Type**:
   - Choose **t2.micro** (1 vCPU, 1GB memory), suitable for low-traffic websites.
   - This instance is free-tier eligible.

5. **Configure Key Pair**:
   - Create a key pair (e.g., `dwijakey.pem`) for secure access to your instance.
   - Download the `.pem` file, which will be needed to decrypt your password later.

6. **Configure Network Settings**:
   - **VPC**: Use the default VPC.
   - **Subnet**: No preference (AWS will automatically assign one).
   - **Auto-assign Public IP**: Make sure this is **enabled** so you can access the instance over the internet.

7. **Security Group Setup**:
   - Create a security group (e.g., `dwijaSecurityGroup`).
   - Configure **inbound rules** to allow:
     - **RDP (Port 3389)** for remote desktop access (Windows).
     - **HTTP (Port 80)** to allow web traffic.
     - **Source**: Anywhere (for global access) or specific IP range for added security.

8. **Storage Configuration**:
   - Assign 30GB SSD storage (sufficient for a basic web server).

9. **Launch the Instance**:
   - Set **Number of Instances** to 1.
   - Launch the instance and wait for the status check to show **2/2 passed**.

---

### **Retrieving EC2 Machine Credentials**
1. **Find Public IP**:
   - Once the instance is running, select it and locate the **Public IP** and **DNS** in the instance details.
   - Example:
     - Public IP: `13.126.122.178`.
     - DNS: `ec2-13-126-122-178.ap-south-1.compute.amazonaws.com`.

2. **Get Username and Password**:
   - Default username: `Administrator`.
   - To get the password:
     - Go to **Actions** > **Connect** > **RDP Client** > **Get Password**.
     - Upload your `.pem` file to decrypt the password.
   - Example password: `ZruKnCO?OGB00Li(p1Ir)?62*d1j6UfV`.

3. **Remote Desktop Connection (RDP)**:
   - On your local machine, open **Remote Desktop Connection**.
   - Enter the public IP (`13.126.122.178`), change the username to `Administrator`, and use the decrypted password.

---

### **Web Server Creation and Setup**

1. **Open the EC2 Machine**:
   - Once connected via RDP, go to **Control Panel** > **Turn Windows features on or off**.

2. **Install IIS (Internet Information Services)**:
   - In the **Server Roles** section, select **IIS Web Server**.
   - Complete the setup by clicking **Next** through the wizard until you reach the finish screen.

3. **Verify the Web Server**:
   - Open **Microsoft Edge** and type `localhost`. If a blue IIS default page loads, the server is set up correctly.

4. **Allow Public HTTP Access**:
   - Go to your **EC2 Console**, find your instance, and navigate to the **Security Group** settings.
   - Edit **Inbound Rules** to add:
     - **HTTP (Port 80)**: Source: Anywhere (to allow web traffic).

5. **Access the Website**:
   - Open a browser on any device, and type your **public IP** (e.g., `13.126.122.178`) to see the static website.

6. **Website File Management**:
   - On the EC2 machine, navigate to: `C:\inetpub\wwwroot`.
   - Place your **index.html** or any website files in this folder.
   - Refresh the website to see changes.

---

### **Modifying Content on the Website**
- You can upload HTML files and other web assets directly to `wwwroot`.
- Copy the necessary files from your local machine and paste them in the virtual machine’s **wwwroot** folder.
- Your changes will reflect on the public IP.

### **Note**:
- Frameworks like **Django**, **React**, or other **Python**-based applications may require different deployment processes compared to static sites.

---

### **Closing the EC2 Instance**

1. **Terminate the Instance**:
   - Go to the **EC2 Dashboard** > **Instances**.
   - Select the instance you want to terminate.
   - Go to **Actions** > **Instance State** > **Terminate**.

2. **Enable/Disable Termination Protection**:
   - Before terminating, you can enable **termination protection** to prevent accidental deletion:
     - Go to **Instance Settings** > **Change Termination Protection**.
     - Enable or disable it as needed.