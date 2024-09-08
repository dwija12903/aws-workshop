# AWS Auto Scaling, EC2, Load Balancer, and Web Hosting

## Agenda
- What is Auto Scaling?
- Understanding AMI and Templates
- Understanding Auto Scaling Groups
- Creating Notifications (SNS)
- Creating Alarms (CloudWatch)
- Adding Auto Scaling Policies

---

### **What is Auto Scaling?**

- **Auto Scaling** automatically adjusts the number of EC2 instances to handle application load, ensuring performance and availability.
- **Features**:
  - No additional fees beyond EC2 and CloudWatch costs.
  - Manages a group of EC2 instances dynamically based on load.
  - Set minimum and maximum instance counts.

---

### **EC2 Setup and Auto Scaling**

#### 1. **Launching an EC2 Instance**

- **EC2 Dashboard** > **Launch Instance**
  - **Choose AMI**: E.g., Ubuntu, Amazon Linux 2, or Windows Server.
  - **Instance Type**: E.g., `t2.micro`.
  - **Configure Instance**: Set VPC, Subnet, IAM roles if needed.
  - **Add Storage**: E.g., 30GB SSD.
  - **Add Tags**: Optional for organization.
  - **Configure Security Group**: E.g., `myAppSG` for necessary ports.
  - **Review and Launch**.

#### 2. **Configuring SSH Access**

- **Check Password Authentication**:
  ```bash
  grep "PasswordAuthentication" /etc/ssh/sshd_config
  ```

- **Enable Password Authentication**:
  ```bash
  sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
  ```

- **Restart SSH Service**:
  ```bash
  sudo systemctl restart sshd
  ```

#### 3. **Set User Password**

- **Set Password for `ubuntu`**:
  ```bash
  sudo passwd ubuntu
  ```

#### 4. **Install Packages**

- **Switch to Root**:
  ```bash
  sudo su -
  ```

- **Install Apache**:
  ```bash
  apt update
  apt install apache2 -y
  systemctl start apache2
  ```

- **Install MySQL**:
  ```bash
  apt install mysql-server
  mysql_secure_installation
  ```

- **Install Other Packages**:
  ```bash
  apt install unzip
  ```

#### 5. **Transfer Web Application**

- **From Windows Git Bash**:
  ```bash
  cd path_to_project
  scp tnpLab.zip ubuntu@<Public_IP_of_EC2>:/home/ubuntu
  ```

- **On EC2**:
  ```bash
  sudo cp tnpLab.zip /var/www/html/
  sudo rm /var/www/html/index.html
  sudo unzip tnpLab.zip -d /var/www/html/
  ```

#### 6. **Verify Web Application**

- Enter the **Public IP** of the EC2 instance in a browser to check the web application.

---

### **Creating and Managing AMI and Templates**

#### 7. **Create AMI**

- **EC2 Dashboard** > **Actions** > **Image and Templates** > **Create Image**.
  - Name: `tnpLabWebServerImage`.
  - Description: Version 1 of the tnpLab web server.

#### 8. **Create Launch Templates**

- **EC2 Dashboard** > **Launch Templates** > **Create Template from Instance**.
  - Name: `tnpLabWebServerTemplate`.
  - Select AMI, instance type (`t2.micro`), key pair, VPC, and security groups.

#### 9. **Create a Load Balancer**

- **EC2 Dashboard** > **Load Balancers** > **Create Load Balancer**:
  - **Type**: Application Load Balancer (ALB).
  - Name: `myAPPtnpLB`.
  - **Network Mapping**: Select VPC and availability zones.
  - **Security Group**: `myAppSG`.
  - **Listener**: HTTP on port 80.
  - **Target Group**: Create and assign.

#### 10. **Verify Load Balancer**

- Check the **DNS** of the Load Balancer in a browser:
  ```plaintext
  myAPPtnpLB-XXXXXXXX.ap-south-1.elb.amazonaws.com
  ```

---

### **Auto Scaling Configuration**

#### 11. **Create Auto Scaling Group**

- **EC2 Dashboard** > **Auto Scaling Groups** > **Create Auto Scaling Group**:
  - **Name**: `mytnpLabAG`.
  - **Launch Template**: Select `tnpLabWebServerTemplate`.
  - **Subnets**: `ap-south-1a`, `ap-south-1b`.
  - **Load Balancer**: Attach `myAPPtnpLB`.
  - **Group Size**: Min (2), Desired (4), Max (100).

---

### **CloudWatch and SNS for Notifications**

#### 12. **Create SNS Topic**

- **SNS Dashboard** > **Create Topic**:
  - Name: `myWebServer-Notification`.
  - Type: **Standard**.
  - **Add Email Subscription**: tnplabs@gmail.com and confirm.

#### 13. **Create CloudWatch Alarms**

- **CloudWatch Dashboard** > **Alarms** > **Create Alarm**:
  - **High CPU Usage**: Trigger when CPU > 75%.
  - **Low CPU Usage**: Trigger when CPU < 40%.
  - **Actions**: Send notifications to `myWebServer-Notification`.

---

### **Adding Auto Scaling Policies**

#### 14. **Add Scaling Policies**

- **Auto Scaling Groups** > **Automatic Scaling** > **Add Policy**:
  - **Increase Policy**: Add 1 EC2 instance when `myAlarm1` (CPU > 75%) triggers.
  - **Decrease Policy**: Remove 1 EC2 instance when `myAlarm2` (CPU < 40%) triggers.

---

### **Clean-Up Process**

#### 15. **Deleting Resources**

- **Delete Auto Scaling Group**: Terminate associated EC2 instances.
- **Delete Launch Configuration**.
- **Delete Load Balancer**.
- **Delete SNS Topic**.
- **Delete CloudWatch Alarms**.