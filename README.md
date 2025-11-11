# 🚀 **Creorez — AI Resume Builder**  
Smart resume creation with AI, LaTeX → PDF generation, and ATS scoring.

Built with **Next.js**, deployed on **Vercel**, and powered by a **Node.js backend** running on **AWS EC2** with **Nginx + PM2** for production stability.

---

## ✅ **Live Demo**
Frontend (Vercel):  
➡️ *https://cerores.vercel.app/*  

Backend (AWS EC2):  
➡️ *http://13.208.244.123/*

---

# ⭐ **Overview**

Creorez is an AI-driven resume builder that helps users:

✅ Generate professional resumes  
✅ Convert form inputs → LaTeX → PDF  
✅ Analyze resumes using ATS scoring  
✅ Export downloadable PDFs  
✅ Perform fast cloud-based PDF rendering

The system is built as a distributed architecture:

- **Frontend** → Next.js (Vercel)  
- **Backend** → Node.js API (AWS EC2)  
- **Process Manager** → PM2  
- **Reverse Proxy** → Nginx  
- **Documentation** → Inside `/devops` folder  

---

# 🏗️ **Architecture Diagram**


---

# 🔧 **Tech Stack**

### **Frontend**
- Next.js (App Router)
- React.js
- TailwindCSS
- Vercel Deployment

### **Backend**
- Node.js
- Express
- PDF Generation (LaTeX)
- PM2
- Nginx

### **Cloud / DevOps**
- AWS EC2
- Ubuntu Linux
- Firewall (UFW)
- Systemd
- Reverse Proxy Routing
- GitHub / Git

---

# 🛠️ **My DevOps Contribution**

✅ Provisioned AWS EC2 instance  
✅ Installed & configured Node.js runtime  
✅ Deployed backend with PM2 (auto-restart + logs)  
✅ Implemented Nginx reverse proxy  
✅ Set up security groups & firewall  
✅ Connected Vercel frontend ↔ EC2 backend  
✅ Designed the complete documentation suite  
✅ Production deployment troubleshooting  
✅ Monitoring, logs, and restarts  

This is **real DevOps work** — and i documented it perfectly in `/devops`.

---

# 📂 **Repository Structure**

doityourez/
│
├── devops/ # ✅ Full DevOps documentation suite
│ ├── docs/
│ │ ├── architecture.md
│ │ ├── deployment-steps.md
│ │ ├── aws-setup.md
│ │ └── server-setup.md
│ │
│ ├── configs/
│ │ ├── nginx.conf
│ │ └── pm2-ecosystem.config.js
│ │
│ └── screenshots/ # (Add later)
│
├── src/ # Next.js frontend
├── prisma/ # Database schema (if applicable)
├── public/
├── package.json
├── next.config.js
└── README.md # ✅ you are here 



---

# 📘 **Detailed Documentation**

All DevOps documentation is available inside:

✅ [`devops/docs/architecture.md`](devops/docs/architecture.md)  
✅ [`devops/docs/deployment-steps.md`](devops/docs/deployment-steps.md)  
✅ [`devops/docs/aws-setup.md`](devops/docs/aws-setup.md)  
✅ [`devops/docs/server-setup.md`](devops/docs/server-setup.md)

Configs:

✅ [`devops/configs/nginx.conf`](devops/configs/nginx.conf)  
✅ [`devops/configs/pm2-ecosystem.config.js`](devops/configs/pm2-ecosystem.config.js)  

Screenshots (add later):

✅ `devops/screenshots/` folder

---

# 🧪 **How to Run Locally (Frontend)**
``` npm install ```
``` npm run dev ```

App will run at:  
➡️ http://localhost:3000/

---

# 🧪 **How to Run Backend Locally (Optional)**  
*(Only if you cloned backend separately)*
``` npm install ```
``` npm server.js ```


---

# 🔐 **Environment Variables**

Frontend:  
Set inside `.env.local`

Backend:  PORT=8080


---

# 🚀 **Deployment**

### ✅ Frontend
Deployed via GitHub → Vercel integration

### ✅ Backend
Deployed manually using:

- SSH  
- Node.js
- PM2  
- Nginx   

See detailed steps:  
👉 `devops/docs/deployment.md`

---

# 📸 **Screenshots**  
(Add these later into `devops/screenshots`)

Examples 

- EC2 Dashboard  
- PM2 status  
- Nginx status  
- Curl test  
- Terminal window  

---

# 👤 **Author — Adapala Sriharsha Reddy**

**Role:** Cloud + DevOps Engineer  
Built full cloud infrastructure + deployment & documentation.

If you want to collaborate, connect with me on LinkedIn.

https://www.linkedin.com/in/sriharshareddy-adapala-781a76299/

---

# ⭐ **Conclusion**

This project demonstrates:

✅ Real-world DevOps experience  
✅ AWS server deployment  
✅ Reverse proxy + PM2 setup  
✅ Modern frontend using Next.js  
✅ Clean documentation  
✅ Cloud-ready production environment  



---



