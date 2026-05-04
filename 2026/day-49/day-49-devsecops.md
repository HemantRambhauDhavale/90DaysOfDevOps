# Day 49 – DevSecOps

Today I added security to my CI/CD pipeline.

## DevSecOps

- Security integrated into CI/CD
- Detect issues before deployment

---

## Docker Image Scan

- Used Trivy
- Scans image for vulnerabilities
- Fails pipeline on critical issues

---

## Dependency Check

- Added dependency review action
- Runs on pull requests
- Blocks insecure dependencies

---

## Secret Scanning

- Enabled GitHub secret scanning
- Detects exposed API keys and tokens
- Push protection prevents leaks

---

## Workflow Permissions

- Added permissions block
- Limited access to minimum required

---

## Pipeline Flow

PR → test → dependency check  
Main → test → Docker scan → push → deploy  
Always → secret scanning  

---

## Summary

Today I learned:

- DevSecOps basics
- Automating security checks
- Protecting pipelines from vulnerabilities
- Making CI/CD more secure
