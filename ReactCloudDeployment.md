# React.js Deployment in the Cloud with AWS

## 1. **AWS Amplify for React, Android, iOS, Vue, JavaScript**

### **Steps to Deploy React App using AWS Amplify**
1. Go to **[AWS Amplify](https://aws.amazon.com/amplify/)**.
2. Create a React app locally:
   ```bash
   npx create-react-app dwicloud
   ```
3. Build the project:
   ```bash
   npm run build
   ```
4. Serve the build:
   ```bash
   serve build
   ```
5. Navigate to **Amplify Hosting**.
6. Deploy without Git:
   - Compress only the contents inside the `build` folder (not the entire `build` folder).
   - Upload the zipped folder in Amplify Hosting.
7. Set:
   - App Name: Choose any name.
   - Environment Name: Choose any name.
   - Drag the zipped folder (should be named `static`).
8. Save and Deploy.
9. Once deployed, open the provided **DNS domain link** to access the app.
10. To delete the deployment:
   - Go to **Actions** >> **Delete**.

---

## 2. **Deploying on EC2 Ubuntu**

### **Steps to Set Up EC2 Instance**
1. Launch an EC2 instance:
   - Choose **Ubuntu** as the server.
   - Create a **Key Pair** (e.g., `key.pem`).
2. Configure Networking:
   - **Subnet**: No preference.
   - **SSH Port**: 22 (default).
   - **Source Type**: Anywhere (for open SSH access).
3. **Summary**:
   - Number of Instances: 1.
4. Launch the instance and find your public IP address.
   - Example: `Public IP = 65.0.133.103`.
   - Default user: `ubuntu`.
   - Password: Not required (will use key-based authentication via SSH).

### **Connecting to EC2 Ubuntu Instance**
1. Open Git Bash and navigate to the location where the `.pem` file is stored.
   - List the `.pem` file:
     ```bash
     ls *.pem
     ```
2. Set correct permissions for the key:
   ```bash
   chmod 400 key.pem
   ```
3. Connect to the EC2 instance:
   ```bash
   ssh -i "key.pem" ubuntu@65.0.133.103
   ```
   - When prompted, type **yes** and press Enter.

### **Basic Ubuntu Commands**
- Update package list:
   ```bash
   sudo apt update
   ```
- Switch to root (to avoid typing `sudo` repeatedly):
   ```bash
   sudo su
   ```
- List all users:
   ```bash
   cat /etc/passwd
   ```

### **Add a New User**
1. Create a new user (e.g., `dwija`):
   ```bash
   adduser dwija
   ```

### **Connect as a New User**
- In CMD:
   ```bash
   ssh dwija@65.0.133.103
   ```

### **SSH Configuration**
- Open the SSH configuration file in Git Bash:
   ```bash
   cd /etc/ssh
   vi sshd_config
   ```