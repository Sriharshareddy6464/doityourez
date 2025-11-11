# **System Architecture – Creorez (AI Resume Builder)**

## ✅ Overview

Creorez is a full-stack AI-powered resume builder consisting of:

- **Frontend** — Next.js (App Router) deployed on **Vercel**
- **Backend** — Node.js microservice deployed on **AWS EC2**
- **Core Engine** — LaTeX → PDF resume generation + ATS scoring APIs

The two systems communicate over HTTPS using REST APIs exposed by the AWS backend.

---

## ✅ High-Level Architecture Diagram

                 ┌────────────────────────────────┐
                 │            User Browser         │
                 └───────────────┬────────────────┘
                                 │
                                 ▼
                     (Vercel Frontend - Next.js)
                 ┌────────────────────────────────┐
                 │  UI Pages                      │
                 │--------------------------------│
                 │  Resume Builder                │
                 │  ATS Score Page                │
                 │  latex/pdf                     │
                 │  Fill the Form                 │   // here by filling form this will
                 │  Enhance the Resume            │      take to latex backend server     
                 └───────────────┬────────────────┘         
                                 │  HTTPS API Call
                                 ▼
                 ┌────────────────────────────────┐
                 │      AWS EC2 (Ubuntu 22.04)    │
                 │--------------------------------│  // the whole process done in AWS EC2
                 │  Node.js Runtime               │    within  seconds     
                 │  PM2 Process Manager           │    without user interaction
                 │  Nginx Reverse Proxy           │ 
                 │  LaTeX to PDF Engine           │
                 └───────────────┬────────────────┘
                                 │
                                 ▼
                         PDF / JSON Response
                    (Vercel Frontend - Next.js)
                 ┌────────────────────────────────┐
                 │  UI Pages                      │
                 │--------------------------------│   // after filling form comes to 
                 │  Resume Builder                │      laTex to pdf convertion 
                 │  ATS Score Page                │     action takes place to 
                 │  latex/pdf                     │     aws backend server  
                 │  Fill the Form                 │   
                 │  Enhance the Resume            │          
                 └───────────────┬────────────────┘ 

---

## ✅ Components Breakdown

### **1. Frontend (Vercel + Next.js)**
- Interactive UI
- Resume creation and editing
- Inputs sent to backend for PDF conversion
- Receives PDF / ATS score response

---

### **2. Backend (Node.js on AWS EC2)**

| Component                   | Purpose                                   |
|-----------------------------|-------------------------------------------|
| **Node.js**                 | Serves API endpoints                      |
| **PM2**                     | Keeps server running 24/7                 |
| **Nginx**                   | Reverse proxy, routing to Node            |
| **LaTeX Engine**            | Converts templates into professional PDFs |
| **Resume Processing Logic** | Generates AI-enhanced version             |

---

### **3. API Communication**
All communication uses HTTPS:

Frontend (Vercel) → sends data → Backend (EC2 API) → returns PDF / ATS Score → Frontend 

---

## ✅ Deployment Architecture

### **Frontend Deployment**
- Hosted on **Vercel**
- Uses `app/` directory routing
- Auto-deploys from GitHub on push

### **Backend Deployment**
- Hosted on **AWS EC2**
- Manual provisioning (SSH)
- Node.js installed via apt
- PM2 used for process management
- Nginx reverse proxy handles port 80 → port 8080

---

## ✅ DevOps Workflow

1. **Clone backend repo** onto EC2  
2. Install Node.js & dependencies  
3. Run service using PM2  
4. Configure Nginx reverse proxy  
5. Open HTTP/SSH ports in security group  
6. Test using curl/Postman  
7. Integrate with frontend  
8. Deploy frontend on Vercel  

---

## ✅ Security Considerations

- SSH limited to selected IPs  
- Only ports **22 / 80 / 443** open  
- PM2 restarts service on system reboot  
- Nginx prevents direct Node exposure  
- No environment variables leaked  

---

## ✅ Architecture Files
This document is part of the `devops/` documentation suite.

Additional files:
- `deployment-steps.md`
- `aws-setup.md`
- `server-setup.md`
- `configs/nginx.conf`
- `configs/pm2-ecosystem.config.js`

---

## ✅ Author 

This architectural layout and cloud deployment were implemented by:

**Sri Harsha — DevOps & Cloud Engineer**  
Responsibilities:
- Designed AWS deployment architecture  
- Provisioned EC2 and configured Node.js runtime  
- Implemented PM2 + Nginx setup  
- Ensured production stability  
- Connected backend with Vercel frontend  

---

