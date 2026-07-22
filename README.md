
# LinkedIn App — DevSecOps Pipeline | GitHub Actions | Trivy | AWS ECR

A DevSecOps CI/CD pipeline built on top of a MERN LinkedIn clone, demonstrating automated Docker builds, security vulnerability scanning, and container image delivery to AWS ECR using GitHub Actions.

---

## Pipeline Overview

```
Push to main → Build Docker Image → Trivy Security Scan → Push to Amazon ECR
```

---

## Tech Stack

| Category | Tool |
|----------|------|
| Application | React.js + Node.js (Express) + MongoDB |
| Containerisation | Docker (Multi-stage build) |
| CI/CD | GitHub Actions |
| Security Scanning | Trivy |
| Image Registry | Amazon ECR |
| Cloud | AWS (us-east-1) |

---

## Pipeline Jobs

### 1. Build
- Checks out code from the repository
- Builds a Docker image tagged with `github.run_number`
- Fails fast with a notification if the build breaks

### 2. Security Scan
- Runs **Trivy** against the built Docker image
- Scans OS packages and Node.js libraries
- Flags **HIGH** and **CRITICAL** vulnerabilities with available fixes
- Exports scan results as a **JSON artifact** downloadable from the Actions tab

![Trivy Scan Artifact](images/trivy-artifact.png)


### 3. Build and Publish
- Runs only on pushes to `main`
- Authenticates to AWS using GitHub Secrets
- Logs in to Amazon ECR
- Tags image with both `github.sha` and `latest`
- Pushes both tags to ECR

![ECR Image Pushed](images/ecr-image-pushed.png)


## GitHub Actions Successful Run

![GitHub Actions Success](images/github-actions-success.png)


---

## Project Structure

```
linkedin-devsecops-githubactions-ecr/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── frontend/
├── backend/
├── Dockerfile
├── .gitignore
└── README.md
```

---

## GitHub Secrets Required

| Secret | Description |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | IAM user access key |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret key |
| `AWS_REGION` | AWS region (e.g. us-east-1) |
| `AWS_ACCOUNT_ID` | 12-digit AWS account ID |

---

## Security Scanning with Trivy

Trivy scans the Docker image for vulnerabilities after every push to main. The scan report is exported as a JSON artifact and available for download directly from the GitHub Actions run summary.

- Scan type: Docker image
- Severity: HIGH, CRITICAL
- Only flags vulnerabilities with a fix available
- Report format: JSON artifact

---

## Future improvements

- Deploy to AWS ECS Fargate
- Add Slack/email notification on pipeline failure

---

## Credits

Original MERN LinkedIn clone by [Gustavo Noronha](https://github.com/gusttavonl/LinkedInMernClone)

CI/CD pipeline, Dockerfile, and GitHub Actions workflow built by [AnushaJoseph-00](https://github.com/AnushaJoseph-00)
