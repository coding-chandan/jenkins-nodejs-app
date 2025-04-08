# jenkins-nodejs-app

Great! Let's create a Jenkins CI/CD project using a Node.js application, deployed on an EC2 Ubuntu AWS instance. This project will help you practice and also look great on your resume.

✅ Project Overview
We will:

Create a simple Node.js app

Push it to GitHub

Install Jenkins on EC2 Ubuntu

Set up a freestyle Jenkins project

Clone code from GitHub, install dependencies, and run the Node.js app

Automate deployment every time new code is pushed

🧱 Step-by-Step Guide
🔧 Step 1: Launch EC2 Ubuntu Instance
Choose Ubuntu (22.04 LTS)

Allow ports: 22 (SSH), 8080 (Jenkins), and 3000 (Node.js app)

📦 Step 2: Install Node.js and Git on EC2
bash
Copy
Edit
sudo apt update
sudo apt install -y nodejs npm git
node -v
npm -v
🚀 Step 3: Create a Simple Node.js App
On your local system (or directly on EC2):

bash
Copy
Edit
mkdir jenkins-nodejs-app && cd jenkins-nodejs-app
npm init -y
npm install express
Create app.js:

js
Copy
Edit
const express = require("express");
const app = express();
const PORT = 3000;

app.get("/", (req, res) => res.send("Hello from Jenkins Node.js App!"));

app.listen(PORT, () => console.log(`App running on http://localhost:${PORT}`));
Create a start.sh:

bash
Copy
Edit
#!/bin/bash
npm install
node app.js
Make it executable:

bash
Copy
Edit
chmod +x start.sh
📁 Step 4: Push the Code to GitHub
bash
Copy
Edit
git init
git remote add origin https://github.com/YOUR_USERNAME/jenkins-nodejs-app.git
git add .
git commit -m "Initial commit"
git push -u origin master
⚙️ Step 5: Install Jenkins on EC2 Ubuntu
bash
Copy
Edit
# Install Java (required by Jenkins)
sudo apt install -y openjdk-11-jdk

# Add Jenkins key and source
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

# Install Jenkins
sudo apt update
sudo apt install -y jenkins

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins
Open Jenkins in browser:

cpp
Copy
Edit
http://EC2_PUBLIC_IP:8080
Unlock Jenkins:

bash
Copy
Edit
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Install Suggested Plugins → Create admin user.

🔐 Step 6: Allow Jenkins to Use GitHub
Install "Git" and "NodeJS" plugins from Manage Jenkins → Plugins

Go to Manage Jenkins → Global Tool Configuration

Configure Git (set path: /usr/bin/git)

Configure NodeJS (install latest version, check “Install automatically”)

🔧 Step 7: Create Freestyle Jenkins Job
Go to Jenkins Dashboard → New Item → Name: nodejs-ci-cd → Freestyle Project

In Source Code Management:

Select Git

Repo URL: https://github.com/YOUR_USERNAME/jenkins-nodejs-app.git

In Build Environment, check:

Provide Node & npm bin/ folder to PATH

Use the NodeJS version you configured

In Build Steps:

Add Execute shell:

bash
Copy
Edit
chmod +x start.sh
./start.sh
🚨 Step 8: Fix Firewall (Optional)
Allow port 3000 (Node.js):

bash
Copy
Edit
sudo ufw allow 3000
🔁 Step 9: Enable Webhook for CI/CD
Go to your GitHub Repo → Settings → Webhooks → Add Webhook

Payload URL:

arduino
Copy
Edit
http://<EC2_PUBLIC_IP>:8080/github-webhook/
Content type: application/json

Select Just the push event

Save

In Jenkins:

Install GitHub Integration Plugin

In your Job → Build Triggers → check “GitHub hook trigger for GITScm polling”

✅ Final Output
Jenkins pulls your code from GitHub

Builds and runs it using Node.js

Your app is live at: http://EC2_PUBLIC_IP:3000

📸 Bonus: Add Screenshot to Resume
Add a screenshot of:

Jenkins Dashboard with successful build

The Node.js app running in browser
