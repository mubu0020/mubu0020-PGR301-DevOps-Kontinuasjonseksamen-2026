# 🚀 PGR301 DevOps – Continuation Exam 2026

![CI](https://img.shields.io/badge/CI-GitHub_Actions-blue)
![Build](https://img.shields.io/badge/build-passing-brightgreen)
![Security](https://img.shields.io/badge/security-scanned-success)
![Docker](https://img.shields.io/badge/container-Docker-blue)

This repository contains my submission for the continuation exam in **PGR301 DevOps**.

The solution demonstrates a modern DevOps pipeline with a focus on:

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

   * Maven build of the application

2. **Test**

   * Runs unit tests
   * Stops pipeline on failure (fail fast)

3. **File-level security scanning**

   * Scans source code and dependencies
   * Detects known vulnerabilities early

4. **Docker build**

   * Builds container image

5. **Image scanning (OSC)**

   * Scans container image for vulnerabilities
   * Covers OS packages and layers

6. **SARIF reporting**

   * Results are uploaded to GitHub Security tab

---

## ⚡ Workflow Optimization

The pipeline has been actively improved for performance and quality:

* **Fail fast** → stops early on errors
* **Reduced redundancy** → no unnecessary steps
* **Clear separation of concerns**
* **Efficient job ordering**
* **Improved readability and maintainability**

This results in:

* Faster feedback
* Lower resource usage
* Easier debugging

---

## 🔐 Security (Core Focus)

Security is implemented across multiple layers:

### 📄 File-level scanning

* Scans code and dependencies
* Detects vulnerabilities before build completion
* Implements *shift-left security*

### 📦 Dependency scanning

* Identifies known CVEs
* Promotes safer dependency management

### 🐳 Image scanning (OSC)

* Scans Docker image after build
* Detects vulnerabilities in OS and libraries
* Critical for production environments

### 📊 SARIF (GitHub Security)

* Results are displayed directly in GitHub
* Provides:

  * Visibility
  * Traceability
  * Easy follow-up

---

## 🐳 Docker

The application is containerized for consistent execution.

### Build and run

```bash
docker build -t quiz-app .
docker run -p 8080:8080 quiz-app
```

### Why Docker?

* Same environment everywhere
* Easier deployment
* Fewer "it works on my machine" issues

---

## 🔌 API

### Endpoint

```
POST /quiz
```

### Request

```json
{
  "answers": ["A", "B", "C"]
}
```

### Response

```json
{
  "score": 2
}
```

---

## 🧪 Testing

* Unit tests run automatically in CI
* Pipeline fails if tests fail
* Provides fast feedback and higher quality

Run locally:

```bash
mvn test
```

---

## 🔒 GitHub Governance

The repository follows modern GitHub practices:

* Branch protection
* Required status checks
* Pull request-based development

This ensures:

* A stable main branch
* Quality control
* Traceable history

---

## 📁 Structure

```
.
├── src/
├── .github/workflows/
├── Dockerfile
├── pom.xml
└── docs/
```

---

## 🛠️ Run Locally

```bash
mvn clean install
mvn spring-boot:run
```

Application runs on:

```
http://localhost:8080
```

---

## 🧠 DevOps Reflection

This project focuses on how software is delivered, not just what it does.

Key principles demonstrated:

* 🔁 Continuous Integration
* ⚡ Full pipeline automation
* 🔐 Shift-left security
* 📦 Reproducibility with Docker
* 👀 Visibility through GitHub Security

The pipeline acts as a quality gate that automatically verifies:

* Functionality
* Security
* Build integrity

---

## 🕒 Important Note on Security and Time

This project was originally submitted on **February 20, 2026**, where the entire pipeline passed successfully with a fully green build.

After submission, the pipeline began failing due to newly discovered vulnerabilities in dependencies during file-level scanning.

This happened because:

* New CVEs were published after the exam submission
* Security databases (used by tools like Trivy) are continuously updated
* The same codebase can become “insecure” over time without any code changes

This reflects an important reality in modern DevOps and software engineering:

> Security is not static — it is continuously evolving.

Even stable and previously approved systems can become vulnerable as new threats are discovered.

---

## 🧠 Industry Perspective

This situation highlights a key industry principle:

* Dependencies must be continuously monitored and updated
* Security scanning tools evolve and improve over time
* CI/CD pipelines are not just build tools, but ongoing security controls

In real-world environments, this leads to:

* Continuous patching and upgrades
* Dependency management strategies
* Automated security gates in pipelines

---

## 🏁 Summary

This project demonstrates a complete DevOps approach with:

* An efficient CI/CD pipeline
* Multi-layered security scanning
* Optimized workflows
* Container-based deployment

The main focus has been building a robust, modern, and secure delivery process — while also illustrating how software security must be continuously maintained over time.
