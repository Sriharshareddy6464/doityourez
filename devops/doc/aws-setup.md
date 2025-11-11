# **AWS Setup – Creorez Backend Infrastructure**

This document describes the exact AWS configurations used for deploying the Creorez backend. It includes EC2 provisioning, security groups, network configuration, and essential system setup.

---

# ✅ **1. EC2 Instance Configuration**

### **Instance Details**
| Parameter     | Value                     |
|---------------|---------------------------|
| AMI           | Ubuntu 24.04 LTS (x86_64) |
| Instance Type | t3.micro                  |
| vCPU          | 2                         |
| RAM           | 1GB                       |
| Storage       | 8GB gp2                   |
| Architecture  | 64-bit                    |
| Network       | Default VPC               |

### **Why Ubuntu 24.04?**
- Stable LTS release  
- Excellent support for Node.js  
- Lightweight for microservices  
- Compatible with DevOps tooling (Nginx, PM2, Git)

---
# ✅ **2. Key Pair Setup**

When creating the EC2 instance:

- Generated a `.pem` key  
- Permissions adjusted locally:

---

# ✅ **3. Security Group Rules**

These inbound rules were configured:

| Port | Protocol | Purpose                      |
|------|----------|------------------------------|
| 22   | TCP      | SSH access for deployment    |
| 80   | TCP      | Public HTTP access via Nginx |
| 443  | TCP      | HTTPS (optional for SSL)     |

### **Outbound Rules**
- Allow all (default)

### **Security Best Practices**
- SSH key-pair required  
- Avoid using password login  
- Auto-close SSH after deployment (optional)  

---
# ✅ **4. Storage Setup**

### **Volume**
- 8GB gp2 (General Purpose SSD)

### **Rationale**
- Enough space for Node modules + LaTeX engine + logs  
- gp2 offers balanced cost/performance  

# ✅ **5. launch the EC2 instance terminal **

```bash
chmod 400 your-key.pem
```
```bash
ssh -i "your-key.pem" ubuntu@<EC2-Public-IP> 
```

# ✅ **6. Updating ubuntu server in EC2 instance terminal **

```sudo apt update && sudo apt upgrade -y```

# ✅ **7. Node.js installation & version verification  **

```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -```
```sudo apt install -y nodejs```
```node -v```
```npm -v```

# ✅ **8. PM2 installation & version verification  **
```sudo npm install -g pm2```

```pm2 list```
```pm2 logs```
```pm2 restart resume-backend```
```pm2 save```
```pm2 startup```


# ✅ **9. nginx installation & version verification  **

```sudo apt install nginx -y```
```sudo systemctl status nginx```

```sudo systemctl restart nginx```
```sudo systemctl status nginx```



