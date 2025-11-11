# **AWS Deployment Steps – Creorez Backend (Node.js + PM2 + Nginx)**

This document provides the full sequence of actions used to deploy the Creorez backend on an AWS EC2 instance.It includes instance setup, environment configuration, Node server deployment, process management, and reverse proxy configuration.

---

# ✅ **1. Launch EC2 Instance**

**Configuration used:**

| Setting       | Value            |
|---------------|------------------|
| AMI           | Ubuntu 22.04 LTS |
| Instance Type |  t3.micro        |
| Storage       | 20GB gp3         |
| Inbound Rules | 22 (SSH), 80 (HTTP), 443 (optional HTTPS) |

**Steps:**
1. Go to AWS → EC2 → Launch Instance  
2. Give a name to the instance (i named latex, as im doing latex/pdf generation)
3. Select `Ubuntu 22.04 LTS`  
4. Choose `t3.micro` (free-tier eligible)  
4. Add storage (8GB enough with gp2)  
5. Security Groups:
   - `22` TCP (SSH)
   - `80` TCP (HTTP)
   - `443` TCP (HTTPS)
   allow the ticks for SSH , HTTP , HTTPS

---

# ✅ **2. Connect to Instance via direct connect from AWS or SSH**

if its for terminal 
```bash
ssh -i "your-key.pem" ubuntu@<EC2-Public-IP>
```
.pem file & IP address will autogenerates when creating an Instance 
mine latex.pem & IP address is not ethical to put on public 

# ✅ **3. CLI Command sequence to setup server ready and deploy**

1. let the ubuntu Operating System update 
bash
```sudo apt update``` 
Why:
- Ensures your server has the latest packages
- Prevents missing dependency errors
- Makes your environment stable and predictable

2. Now install Node.js 

```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -```
```sudo apt install -y nodejs```
Why:
- Node.js is the backend engine that executes JavaScript on the server
- Your PDF microservice (Express app) runs on Node.js
- Using NodeSource ensures the official, latest, stable version
- You need npm for adding dependencies (express, cors, etc.)

3. check the versions 

```node -v```
```npm -v```

4. create a directory 

```mkdir ~/resume-backend```
cd ~/resume-backend

5. initialise the directory 

```npm init -y```

6. Install express and cors via npm 

```npm install express cors```
Why these packages:
- Express → lightweight web server to handle routes like /generate
- CORS → allows your Vercel frontend to call this backend
- Without these, your API cannot receive requests or send PDFs

7. Install tectonic 

```curl --proto '=https' --tlsv1.2 -fsSL https://drop-sh.fullyjustified.net | sh```
```sudo mv tectonic /usr/local/bin/tectonic```
```sudo chmod +x /usr/local/bin/tectonic```
Why Tectonic instead of TeXLive:
- TeXLive is 3GB+ — not suitable for micro EC2
- Tectonic is only 70–100 MB
- Compiles LaTeX to PDF quickly
- Perfect for microservices and serverless systems
- No PATH or library conflicts after installation

8. check tectonic version 

```tectonic --version```

9. now create server.js backend funtionality 

```nano server.js```

10. backend code of server.js 

const express = require("express");
const cors = require("cors");
const fs = require("fs");
const os = require("os");
const path = require("path");
const { spawn } = require("child_process");

const app = express();
app.use(express.json({ limit: "4mb" }));
app.use(cors());

app.get("/", (req, res) => {
  res.send("✅ Node.js PDF Server Working");
});

app.post("/generate", async (req, res) => {
  try {
    const { code } = req.body;
    if (!code) {
      return res.status(400).json({ error: "No LaTeX content provided" });
    }

    const fileName = `resume-${Date.now()}`;
    const texPath = path.join(os.tmpdir(), `${fileName}.tex`);
    const pdfPath = path.join(os.tmpdir(), `${fileName}.pdf`);

    // ✅ Write LaTeX content safely
    try {
      fs.writeFileSync(texPath, code);
    } catch (err) {
      console.error("❌ Failed to write .tex:", err);
      return res.status(500).json({ error: "Write failure" });
    }

    // ✅ Run Tectonic safely
    await new Promise((resolve, reject) => {
      const cmd = spawn("/usr/local/bin/tectonic", [
        "--outdir",
        "/tmp",
        texPath
      ]);

      cmd.stdout.on("data", data => console.log(data.toString()));
      cmd.stderr.on("data", data => console.error(data.toString()));

      cmd.on("error", err => {
        console.error("❌ Spawn failed:", err);
        reject(err);
      });

      cmd.on("close", exit => {
        if (exit === 0) resolve();
        else reject(new Error("Tectonic failed with exit code " + exit));
      });
    });

    // ✅ Read PDF
    let pdfBuffer;
    try {
      pdfBuffer = fs.readFileSync(pdfPath);
    } catch (err) {
      console.error("❌ Failed to read PDF:", err);
      return res.status(500).json({ error: "PDF read error" });
    }

    res.setHeader("Content-Type", "application/pdf");
    res.send(pdfBuffer);

    // ✅ Cleanup safely
    try { fs.unlinkSync(texPath); } catch {}
    try { fs.unlinkSync(pdfPath); } catch {}

  } catch (err) {
    console.error("❌ SERVER ERROR:", err);
    res.status(500).json({ error: "Internal server error" });
  }
});

// ✅ Global protection — prevents shutdown on unexpected errors
process.on("uncaughtException", err => {
  console.error("❌ UNCAUGHT EXCEPTION:", err);
});

process.on("unhandledRejection", err => {
  console.error("❌ UNHANDLED PROMISE:", err);
});

app.listen(3001, "0.0.0.0", () => {
  console.log("✅ Node PDF server running on 3001 (PUBLIC) – Stable Mode");
});


Key concept:
- Express receives JSON input (code)
- Writes to a .tex file
- Runs Tectonic to compile .tex → .pdf
- Reads final PDF
- Sends PDF back to user
- Deletes temp files

Why this architecture works:
- Clean separation: input → compile → output
- No user data stored → safe & GDPR-friendly
- Fast PDF generation
- Easy debugging and monitoring

Why:
- 0.0.0.0 makes the API accessible from outside the EC2 machine
- Without it → your server is only reachable from localhost
- Critical for Vercel → EC2 communication

Inbound rule:
- Add Custom TCP → Port 3001 → 0.0.0.0/0
Why:
- AWS firewall must allow traffic to your backend port
- Without this, users cannot connect to your API
- Essential for public API exposure

11. installing PM2 

```sudo npm install -g pm2```

Why:
- PM2 keeps your server alive even if it crashes
- Restarts automatically if EC2 reboots
- Allows logging & monitoring
- Production-grade process manager

12. starting server 

```pm2 start server.js --name pdf-server``

13. reboot server

```pm2 startup```
```pm2 save```

14. check the pm2 server status 

```pm2 status```

# ✅ **4. Summary of Deployment 

1. Create server → give it a safe home
2. Prepare environment → Node.js + dependencies
3. Install PDF engine → Tectonic
4. Write backend logic → compile LaTeX
5. Expose port → AWS Security Group
6. Keep server alive → PM2
7. Connect frontend → via API endpoint
8. Generate PDFs → clean, fast, automated
9. Serve users globally → Vercel frontend + EC2 backend


