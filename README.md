<div align="center">

# 🔧 Jenkins Build Automation
### PowerShell · Declarative Pipeline · REST API · CI/CD Integration

[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-blue?style=for-the-badge&logo=powershell&logoColor=white)](https://microsoft.com/powershell)
[![Jenkins](https://img.shields.io/badge/Jenkins-Pipeline-D24939?style=for-the-badge&logo=jenkins&logoColor=white)](https://www.jenkins.io/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

*A complete Jenkins CI/CD setup — declarative pipeline with PowerShell build steps and automated remote job triggering.*

</div>

---

## 📌 Overview

This project provides two complementary automation components that work together:

- **`Jenkinsfile`** — A declarative Jenkins pipeline that checks out code, prepares a timestamped build folder, runs PowerShell build steps, triggers a remote Jenkins job, and sends HTML email notifications on success or failure.
- **`Invoke-JenkinsBuild.ps1`** — A standalone PowerShell script that authenticates with Jenkins via REST API, triggers a parameterized build, polls the queue, monitors the run, and reports the final result.

Together they form a full CI/CD workflow: the pipeline orchestrates your build process, and the PowerShell script handles cross-job triggering without any manual browser interaction.

---

## 📂 Project Structure

```
jenkins-powershell-automation/
├── Jenkinsfile                    # Declarative Jenkins pipeline
├── Invoke-JenkinsBuild.ps1        # Standalone PowerShell build trigger
├── scripts/
│   └── trigger-build.ps1          # Called by the pipeline to trigger remote jobs
└── README.md                      # Documentation
```

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| Jenkins Declarative Pipeline | Orchestrates the full CI/CD workflow |
| PowerShell | Build steps, folder creation, remote triggering |
| Jenkins REST API | Programmatic build triggering & status polling |
| HTTP Basic Auth | Secure API authentication |
| Email Extension Plugin | HTML success/failure notifications |

---

## 🔩 Component 1 — Jenkinsfile

### What It Does

| Stage | Description |
|---|---|
| `Clean Workspace` | Wipes the workspace before each run |
| `Checkout` | Pulls source code via `checkout scm` |
| `Prepare Build Folder` | Creates a timestamped folder: `artifacts/YYYYMMDD_<build#>` |
| `Build` | Writes a `build-info.txt` with build number, date, and environment |
| `Trigger Remote Jenkins Job` | Calls `scripts/trigger-build.ps1` to kick off a downstream job |

### Post Actions

On **success** or **failure**, an HTML email is sent via the Email Extension Plugin containing the project name, build number, environment, date, and a direct link to the build.

### Pipeline Parameters

| Parameter | Default | Description |
|---|---|---|
| `TARGET_ENV` | `dev` | Target deployment environment (`dev`, `staging`, `prod`) |

### Usage

1. Add the `Jenkinsfile` to your repository root.
2. In Jenkins, create a new **Pipeline** job and set the definition to **Pipeline script from SCM**.
3. Trigger the job and pass `TARGET_ENV` as needed.

---

## 🔩 Component 2 — Invoke-JenkinsBuild.ps1

### What It Does

- Authenticates with Jenkins using a Base64-encoded API token
- Triggers a parameterized build via HTTP POST
- Polls the job until the build starts
- Monitors the running build until completion
- Prints the final result — `SUCCESS` or `FAILURE`

### Configuration

Open `Invoke-JenkinsBuild.ps1` and update the variables at the top:

```powershell
$jenkinsUrl = "http://your-jenkins-url"
$jobName    = "YourJobName"
$user       = $env:JENKINS_USER        # Use environment variables
$apiToken   = $env:JENKINS_API_TOKEN   # Never hardcode credentials

$parameters = @{
    "param1" = "value1"
    "param2" = "value2"
}
```

> 💡 Generate your API token from **Jenkins → User Profile → Configure → Add New Token**

### Usage

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

- **Never hardcode credentials** — always use environment variables:

```powershell
$user     = $env:JENKINS_USER
$apiToken = $env:JENKINS_API_TOKEN
```

- Use **HTTPS** to protect credentials in transit
- Scope API tokens to the **minimum required permissions**
- For pipeline email, use a shared team inbox rather than a personal address

---

## 📋 Prerequisites

- Jenkins with the **Email Extension Plugin** installed
- PowerShell 5.1+ or PowerShell Core 7+ on the Jenkins agent
- Jenkins user account with **Build** and **Read** permissions
- Jenkins API Token
- SMTP configured in **Jenkins → Manage Jenkins → Configure System**

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
Made with ❤️ for DevOps automation
</div>