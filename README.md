# 🔁 CI/CD Pipeline with Jenkins, GitHub, and WebEx Bot Notifications

---

## 📘 About the Project

This project demonstrates a complete CI/CD pipeline configured on a local machine using:
- **GitHub** for code hosting and version control  
- **Jenkins (in Docker)** for automated builds and testing  
- **Ngrok** to securely expose local Jenkins to GitHub  
- **WebEx Bot** for build notifications in real time  

The entire pipeline is triggered by a **code commit to GitHub**, which kicks off a Jenkins build and ends with a message delivered to a **WebEx space**.

---

## 🔗 Pipeline Flow (Trigger on GitHub Commit)

```bash
git commit -m "Update sample_code.py"
git push origin main
```
➡️ GitHub Webhook fires
➡️ Ngrok exposes local Jenkins instance
➡️ Jenkins build is triggered
➡️ Build/test scripts are executed
➡️ WebEx Bot sends notification to WebEx space

## 🛠️ Tech Stack
GitHub (Code Repo + Webhook)

Python (Sample App + Tests)

Jenkins (via Docker)

Ngrok (Expose localhost)

WebEx Bot (Bot Notifications)

Shell Scripts (build.sh, test.sh)

✅ Tasks & Setup Guide

## 1. 🧑‍💻 Set up a GitHub Repository
Create a new repository and clone it locally

Add simple Python code (sample_code.py)

Include test scripts using pytest

Include build.sh and test.sh for automation

Commit and push the files:

``` bash
Copy
Edit
git add .
git commit -m "Initial commit with Python code"
git push origin main

```

## 2. 🔔 Configure GitHub Webhook
Go to Settings > Webhooks in your GitHub repo

Add a Payload URL using your ngrok link:
http://<ngrok-url>/github-webhook/

Content type: application/json

Events: Just the push event

## 3. 🐳 Install Jenkins in Docker
``` bash
Copy
Edit
docker run -p 8080:8080 -p 50000:50000 --name jenkins \
  -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

```

## 4. 🌐 Expose Jenkins with Ngrok
``` bash
Copy
Edit
ngrok http 8080
Use the https://<your-ngrok-subdomain>.ngrok.io URL for GitHub Webhook

```

## 5. ⚙️ Configure Jenkins
Install Plugins: GitHub Integration, Pipeline, Webhook, etc.

Set up a new job:

Source: GitHub repo URL

Build Triggers: GitHub hook trigger

Build Steps:

``` bash
Copy
Edit
chmod +x build.sh
./build.sh
chmod +x test.sh
./test.sh

```

## 6. 🤖 Set Up WebEx Bot
Go to https://developer.webex.com

Create a bot and note:

Bot Access Token

Room ID where it should send messages

## 7. 📬 Jenkins → WebEx Notification Integration
Install HTTP Request plugin in Jenkins

Add a post-build action using a shell or webhook:

``` bash
Copy
Edit
curl -X POST https://webexapis.com/v1/messages \
  -H "Authorization: Bearer <BOT_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "roomId": "<ROOM_ID>",
    "text": "✅ Build passed for latest GitHub commit!"
  }'

```

## 8. 🧪 Test the Pipeline
Push code changes to GitHub:

``` bash
Copy
Edit
git commit -am "Test commit"
git push origin main

```

Watch:

GitHub Webhook triggers Jenkins (via ngrok)

Jenkins pulls and builds the code

Jenkins runs test.sh

WebEx bot sends a success or failure message to your space 🎉

### 🧪 Sample Output
GitHub shows successful push ✅

Jenkins console output confirms build success ✅

WebEx space message:

✅ Build passed for latest GitHub commit!
