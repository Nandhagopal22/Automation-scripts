<div align="center">

# 🔧 Jenkins Build Automation
### PowerShell · REST API · CI/CD Integration

[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?style=for-the-badge&logo=powershell&logoColor=white)](https://microsoft.com/powershell)
[![Jenkins](https://img.shields.io/badge/Jenkins-REST%20API-D24939?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

*Trigger, monitor, and report Jenkins build results — fully automated via PowerShell.*

</div>

---

## 📌 Overview

This project automates Jenkins CI/CD pipelines programmatically using **PowerShell** and the **Jenkins REST API** — no browser, no manual clicks.

- 🔐 Authenticates with Jenkins using an API Token
- 🚀 Triggers parameterized builds via HTTP POST
- 🔄 Polls the Jenkins queue until the build starts
- 📡 Monitors the running build until completion
- ✅ Reports the final result — `SUCCESS` or `FAILURE`

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| PowerShell | Core scripting & HTTP requests |
| Jenkins REST API | Build triggering & status polling |
| HTTP Basic Auth | Secure API authentication |
| JSON | Parsing Jenkins API responses |

---

## 📂 Project Structure

```
jenkins-powershell-automation/
├── Invoke-JenkinsBuild.ps1   # Main automation script
└── README.md                 # Documentation
```

---

## ⚙️ Configuration

Open `Invoke-JenkinsBuild.ps1` and update the variables at the top:

```powershell
$jenkinsUrl = "http://your-jenkins-url"   # Jenkins server base URL
$jobName    = "YourJobName"               # Job to trigger
$user       = "your-jenkins-username"     # Jenkins username
$apiToken   = "your-api-token"           # Jenkins API token

$parameters = @{
    "param1" = "value1"
    "param2" = "value2"
}
```

> 💡 Generate your API token from **Jenkins → User Profile → Configure → Add New Token**

---

## 🚀 Usage

```powershell
.\Invoke-JenkinsBuild.ps1
```

**Example output:**

```
Build triggered for job 'deploy-backend-prod'. Waiting for queue...
Build URL: http://jenkins.example.com/job/deploy-backend-prod/42/
Build completed with status: SUCCESS
```

---

## 🔒 Security Notes

Never hardcode credentials. Use environment variables instead:

```powershell
$user     = $env:JENKINS_USER
$apiToken = $env:JENKINS_API_TOKEN
```

- Use **HTTPS** to protect credentials in transit
- Scope API tokens to the **minimum required permissions**

---

## 📋 Prerequisites

- PowerShell 5.1+ or PowerShell Core 7+
- Jenkins instance accessible over HTTP/HTTPS
- Jenkins user account with **Build** permissions
- Jenkins API Token

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
Made with ❤️ for DevOps automation
</div># Automation-scripts