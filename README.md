# 🚀 DevOps in the Cloud – Continuation Exam 2026

![CI](https://img.shields.io/badge/CI-GitHub_Actions-blue)
![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Security](https://img.shields.io/badge/security-scanned-success)
![Docker](https://img.shields.io/badge/container-Docker-blue)

This repository contains my submission for the continuation exam in **DevOps in the Cloud**, where the project was awarded the grade **A**.

The solution demonstrates a modern DevOps pipeline with a strong focus on:

* Automation
* Security (multi-layer scanning)
* Efficient workflows
* Reproducible deployment

---

## 🧭 About the Project

The application is a simple **Quiz API** built with Spring Boot.

It is intentionally kept simple to shift focus toward:

* CI/CD
* Security
* Infrastructure and delivery processes

---

## 🏗️ High-Level Architecture

Developer → GitHub → GitHub Actions → Build/Test → Security Scan → Docker Build → Image Scan → Results in GitHub Security

The pipeline acts as a complete quality gate before code can be merged or deployed.

---

## ⚙️ CI/CD Pipeline

The pipeline is implemented using **GitHub Actions** and is triggered on:

* Push
* Pull Request

### 🔁 Pipeline Steps

1. **Build**
2. **Test (fail fast)**
3. **Filesystem security scan (Trivy)**
4. **Docker build**
5. **Container image scan (Trivy)**
6. **SARIF upload to GitHub Security**

---

## 🔐 Security Tooling

This project uses multiple layers of security scanning:

### 🔍 Trivy (Aqua Security)

* Filesystem scanning (dependencies + code)
* Container image scanning
* Detects known CVEs
* Integrated into CI/CD pipeline
* Results uploaded via SARIF

### 🛡️ GitHub CodeQL

* Static code analysis
* Detects security vulnerabilities in source code
* Native GitHub integration
* Runs on every push and pull request

---

## ⚡ Workflow Optimization

* Fail fast strategy
* Reduced redundancy
* Clear separation of responsibilities
* Optimized job ordering
* Improved readability

---

## 🐳 Docker

```bash
docker build -t quiz-app .
docker run -p 8080:8080 quiz-app
```

Why Docker:

* Consistent environments
* Easier deployment
* Eliminates environment drift

---

## 🔌 API

**POST /quiz**

Request:

```json
{
  "answers": ["A", "B", "C"]
}
```

Response:

```json
{
  "score": 2
}
```

---

## 🧪 Testing

* Unit tests run automatically in CI
* Pipeline fails on test errors

Run locally:

```bash
mvn test
```

---

## 🔒 GitHub Governance

* Branch protection
* Required status checks
* Pull request workflow

Ensures:

* Stable main branch
* Quality control
* Traceability

---

## 📁 Structure

```
.
├── src/
├── .github/workflows/
├── Dockerfile
├── pom.xml
```

---

## 🛠️ Run Locally

```bash
mvn clean install
mvn spring-boot:run
```

Application runs on:
http://localhost:8080

---

## 🕒 Security Evolution Note

This project was originally submitted on **February 20, 2026**, where the entire CI/CD pipeline passed successfully with a fully green build.

After submission, new vulnerabilities (CVEs) were discovered in dependencies, causing the pipeline to fail during filesystem scanning.

This occurred because:

* Security databases are continuously updated
* New vulnerabilities are discovered over time
* The same codebase can become insecure without any code changes

---

## 🧠 Industry Perspective

This highlights a key principle in modern DevOps:

> Security is not static — it is continuous.

In real-world environments, this leads to:

* Continuous dependency monitoring and updates
* Automated security scanning in CI/CD pipelines
* Ongoing maintenance even after deployment

---

## 🏁 Summary

This project demonstrates:

* A complete CI/CD pipeline
* Multi-layered security scanning (Trivy + CodeQL)
* Container-based deployment
* Real-world DevOps practices aligned with industry standards

It also illustrates an important lesson: even stable and approved systems must be continuously maintained to remain secure over time.
