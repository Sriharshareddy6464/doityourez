# **Server Setup & Operational Guide – Creorez Backend**

This document describes all server-level commands, maintenance workflows, monitoring steps, and operational procedures required for running the Creorez backend on AWS EC2.  
It includes PM2 operations, Nginx management, Git deployments, logs, debugging, and overall server lifecycle actions.

---

# ✅ **1. Directory Structure on EC2**

Once the backend repository is cloned, the server structure typically looks like:

/home/ubuntu/
├── resume-backend/
│ ├── server.js
│ ├── package.json
│ ├── node_modules/
│ ├── .env
│ └── README.md
│
├── pm2/
│ └── ecosystem.config.js
│
└── nginx/
└── default.conf


---

# ✅ **2. Environment Variables**

Environment file used: PORT = 8080


Load the `.env` file at server start using PM2 or through Node directly.

---

# ✅ **3. Running the Server (PM2)**

Start server:

```bash
pm2 start server.js --name resume-backend

pm2 save

pm2 startup
```

# ✅ **4. Running the Server (PM2)**

pm2 list // to list all pm2 apps 

pm2 logs resume-backend // to logs of pm2 app 

pm2 restart resume-backend // restart 

pm2 stop resume-backend // stop 

pm2 delete reseme-backend // delete 

pm2 monit   // monitor 


