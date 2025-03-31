# 🐳 Project 2: Image and Runtime Application Security

## 1. Overview 🚀
This project demonstrates how to **securely build, scan, and deploy** a containerized application using **Docker** and **Amazon ECS**—with an additional focus on dynamic application security testing (DAST). The CI/CD pipeline employs **Trivy** for container image vulnerability scanning and **OWASP ZAP** for DAST scanning, ensuring that both the static image and the live application are secure before updates are pushed to production.

---

## 2. Key Technologies 🛠
- **Docker** ⚙️  
  - Packages the application (including static content like `index.html`) into a container image.
- **Amazon ECS** ☁️  
  - A fully managed container orchestration service that handles rolling updates, ensuring minimal downtime.
- **Trivy** 🔎  
  - Scans Docker images for **HIGH** and **CRITICAL** vulnerabilities, preventing insecure images from being deployed.
- **OWASP ZAP** 🛡  
  - Performs dynamic application security testing (DAST) on the running ECS service to identify vulnerabilities in the live application.
- **GitHub Actions** 🤖  
  - Automates the entire pipeline—from image build, vulnerability scanning, ECS deployment, to DAST scanning—enforcing shift-left security.

---

## 3. Security Highlights 🔒
- **Trusted Base Images**  
  - The Dockerfile starts with an official, verified base image to reduce inherent vulnerabilities.
- **Automated Vulnerability Scanning**  
  - **Trivy** checks each built image for critical vulnerabilities, halting deployment if any issues are found.
- **Dynamic Application Security Testing (DAST)**  
  - **OWASP ZAP** scans the live application running on ECS, identifying vulnerabilities such as XSS, SQL Injection, and misconfigurations.
- **Least Privilege IAM Roles**  
  - ECS tasks and services are granted only the permissions they require, minimizing the attack surface.
- **Continuous Delivery & Feedback**  
  - Every code push triggers a pipeline that builds and scans the image, deploys it to ECS, and dynamically tests the live application, ensuring a secure and consistent release process.

---

## 4. CI/CD Workflow 🔄
### Job 1: Build, Scan, and Push
1. **Checkout:**  
   - The pipeline fetches the repository source from the `Project-2-Container-Security` directory.
2. **Build Docker Image:**  
   - The image is built using a provided `Dockerfile` and tagged as `latest`.
3. **Trivy Vulnerability Scan:**  
   - The newly built image is scanned with Trivy for **HIGH** and **CRITICAL** vulnerabilities. If issues are found, deployment stops.
4. **Push to Docker Hub:**  
   - Using GitHub secrets for authentication, the image is tagged and pushed to Docker Hub.

### Job 2: Update ECS
- **AWS Credentials:**  
  - The pipeline configures AWS credentials from secrets to interact with ECS.
- **ECS Deployment:**  
  - The ECS service is updated (using a force-new-deployment command) to pull the latest secure image, ensuring zero downtime through rolling updates.

### Job 3: DAST Scan
- **Stabilization Period:**  
  - The pipeline waits for the ECS deployment to stabilize.
- **OWASP ZAP Full Scan:**  
  - Using the stable OWASP ZAP image, a full scan is run against the public endpoint of the ECS service (pointed to by a load balancer).
  - The scan generates a report (`zap_report.html`), which warns of vulnerabilities (without failing the build for demo purposes).

---

## 5. Value for Organizations 💼
- **Early Vulnerability Detection:**  
  - Both the image and the live application are continuously scanned, ensuring that vulnerabilities are caught early.
- **Automated Secure Release Cycle:**  
  - The pipeline automates the build, scan, and deployment process, reducing manual intervention and speeding up go-to-market.
- **Proactive Security Posture:**  
  - Integrating static (Trivy) and dynamic (OWASP ZAP) scans into the CI/CD pipeline helps maintain a robust security posture and reduces risk.
- **Scalability & Reliability:**  
  - ECS handles rolling updates and scaling, ensuring continuous availability even as new images are deployed.

---

## 6. Conclusion ✅
By integrating **Docker**, **Trivy**, **OWASP ZAP**, **ECS**, and **GitHub Actions** into one cohesive pipeline, this project demonstrates a modern approach to securing both the container image and the live application. Each code push triggers a build-and-scan job, followed by an ECS update and a dynamic scan against the deployed service. This flow ensures a **consistent, repeatable** release process that aligns with best practices in DevSecOps, reducing risk while maintaining agility.