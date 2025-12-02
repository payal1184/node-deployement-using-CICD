# node-deployment-using-CICD-pipeline

## About This Project

This project shows how to deploy a Node.js application to an AWS EC2 instance using a Jenkins CI/CD pipeline.  
Whenever code is pushed to GitHub, Jenkins automatically pulls the latest changes, installs dependencies, and deploys the updated app on the Node.js server.

---

## Architecture

The overall flow of the CI/CD pipeline is shown below:

![Architecture Diagram](images/architecture.png)

---

## Tools & Services Used

- **AWS EC2** – Hosts Jenkins and the Node.js application
- **GitHub** – Version control for application source code
- **Jenkins** – Automates build and deployment pipeline
- **Node.js & npm** – Runtime for the application
- **PM2** – Process manager to keep the Node app running in background
- **Ubuntu** – Operating system for both servers

---

## Step-by-Step Setup

### Step 1: Launch EC2 Instances

Create **two** EC2 instances:

1. **Jenkins Server**
2. **Node.js Application Server**

For both instances:

- Choose **Ubuntu** as OS
- Configure Security Group to allow:
  - **22** – SSH
  - **8080** – Jenkins UI (Jenkins server)
  - **3000** – Node.js application (Node server)

---

### Step 2: Install Required Packages

#### On Node.js Server

![Architecture Diagram](images/img-2.png)

---

### Step 3: Push Code to GitHub

Step 3: Push Code to GitHub

On local machine, **add**, **commit**, and **push** your changes to GitHub (main or master branch).


---

### Step 4: Configure Jenkins Job

- Install Jenkins plugins: **Git**, **SSH Agent**, **Pipeline**
- Add SSH credentials for the **Node.js EC2 server**
- Create job **`node-app-deployment`** (Pipeline/Freestyle)
- Set **GitHub repo URL** and use the `Jenkinsfile` from this repo

Pipeline (high level): SSH to Node server → pull latest code → npm install → start/restart app with PM2

---

### Step 5: Deployment & Verification

After the Jenkins job runs, it:

- SSHs into the **Node.js EC2** server  
- Pulls the latest code and installs dependencies  
- Starts the app with **PM2**, e.g.:


---

### Step 6 (Optional): Configure GitHub Webhook

Go to GitHub repo → Settings → Webhooks → Add webhook.  
Set Payload URL: http://<JENKINS_EC2_PUBLIC_IP>:8080/github-webhook/  
Content type: application/json  
Trigger: Just the push event → Save

Now, any Git push will automatically trigger the Jenkins pipeline.

---

### Final Application Output (Browser)
![Architecture Diagram](images/Screenshot%20(302).png)


## Benefits of This CI/CD Setup

- **Automated Deployment:** Manual SSH, pull, install यांची गरज नाही.
- **Zero Downtime:** PM2 ensures app stays running even during updates.
- **Error-Free Builds:** Jenkins validates code before deployment.
- **Faster Delivery:** Every push automatically updates the live server.
- **Scalable Setup:** Same pipeline can be used for multiple environments (dev, test, prod).
- **Secure Deployment:** SSH credentials securely managed by Jenkins.

## Conclusion

This setup ensures **automated, seamless, and consistent deployment** of your Node.js application using a CI/CD pipeline with Jenkins and AWS EC2.



