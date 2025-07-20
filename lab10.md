# **Lab 10 Practice Report – DevSecOps with GitHub Actions and Docker Scout**

---

## **1. Introduction**
Integrating security into each stage of the software development lifecycle is known as DevSecOps.  This lab tested a vulnerable Python application (PyGoat) for both static and container-based security vulnerabilities by implementing a safe CI/CD pipeline utilizing **GitHub Actions** and **Docker Scout**.  Docker Scout checks Docker images for vulnerabilities, while GitHub Actions automates build, test, and deploy processes.

---

## **2. Steps Performed**

### **Step 1: Clone the DevSecOps Crash Course Repository.**
#### **What was done:**
Cloned the vulnerable Python app project from GitHub to use it for DevSecOps practice.
#### **Commands:**
```bash
git clone https://github.com/nanuchi/devsecops-crash-course-pygoat.git
cd devsecops-crash-course-pygoat
```
### **Step 2: Understand the repo structure (review Anna’s GitHub Actions tutorial if needed).**
#### **What was done:**
After cloning the GitLab CI/CD tutorial project, I analyzed the file structure to understand how the project is organized.
#### **Commands:**
`tree -L 8`

### **Step 3: Use GitHub Actions templates to set up workflow.**
#### **What Was Done :**
To automate the deployment and testing process, I used GitHub Actions to set up a CI/CD workflow in my GitHub repository. I utilized the available GitHub Actions templates to simplify the process.

#### **Steps Performed:**
1. Opened my GitHub repository in the browser.
2. Clicked the **"Actions"** tab at the top of the repository.
3. GitHub displayed several recommended workflow templates.
4. I Clicked **"Configure"** on one of the suggested templates
5. This opened a pre-filled `.yml` file in the `.github/workflows/` directory.
6. I edited the workflow file to match my project setup. 

### **Step 4: List the security scan jobs included in the workflow.**
#### **What Was Done**
I navigated to the `.github/workflows/` directory in my GitHub repository to view the YAML workflow file created earlier. I inspected the file contents to identify any security scan jobs included by default or manually added.
The following security scan jobs were included in the workflow:
- **sast_scan** – Performs a static application security test (SAST) using Bandit to identify security issues in Python code.
- **image_scan** – Uses Docker Scout to scan the Docker image for known vulnerabilities, focusing on critical and high severity issues.

### **Step 5: Configure a Bandit SAST scan for Python app:**
#### **What was done:**  
I configured the GitHub Actions workflow directly in GitHub to run a Bandit Static Application Security Testing (SAST) scan on the Python app. This involved setting up the Python environment, installing Bandit, running the scan, and uploading the scan report as an artifact for review.
#### **changes made:**  
- Created and edited the workflow YAML file in the GitHub repository under `.github/workflows/ci.yml`.  
- Used GitHub Actions  to set up Python.  
- Added a step to install Bandit.  
- Added a step to run Bandit scanning the whole repo recursively

### **Step 6: Review Bandit scan results:**
- #### **What did the scan detect?**
The Bandit scan detected potential security issues in the Python application code, including common vulnerabilities such as use of insecure functions, potential injection risks, and misconfigurations that could lead to security breaches.

- #### **What is the meaning of job failure?**
A job failure in the Bandit scan typically means that the scan found one or more security issues that meet or exceed the defined severity threshold, causing the CI job to fail to prevent unsafe code from progressing further in the pipeline.

- #### **Discuss severity vs. confidence levels**
- **Severity** indicates the potential impact of the vulnerability if exploited, classified as low, medium, or high.
- **Confidence** measures how certain the tool is that the identified issue is a true positive, also classified as low, medium, or high.
Understanding both helps prioritize which issues to address first — high severity with high confidence should be fixed urgently, while low severity or low confidence findings may require further investigation or can be deprioritized.

### **Step 7: Modify the workflow to ignore low-severity, low-confidence issues.**
#### **What was done**
To reduce noise in the Bandit SAST scan and focus on more critical security findings, I modified the GitHub Actions workflow to ignore issues with low severity and low confidence levels. This helps prioritize actionable vulnerabilities and avoid failing the pipeline due to minor or uncertain issues.

#### **Commands and changes made**
In the existing workflow YAML file, I updated the Run Bandit Scan step to include the --severity-level medium and --confidence-level medium options. These flags tell Bandit to only report vulnerabilities that have at least medium severity and confidence. Also, to prevent the pipeline from failing when vulnerabilities are detected (which is common for low severity issues), I added continue-on-error: true to allow the workflow to continue running.

#### **Issues encountered**
- Initially, the pipeline failed whenever any vulnerability was found because Bandit returns a non-zero exit code on findings.
-  Adding continue-on-error: true fixed this issue by allowing the workflow to continue.
-  Choosing appropriate severity and confidence thresholds required balancing false positives and missing important findings.
-  Ensuring the Bandit report was still uploaded despite errors required the if: always() condition in the artifact upload step.

### **Step 8: Re-run the pipeline and review updated results.**


### **Step 9: Generate and include a Bandit report (supported formats).**
### **Step 10: Discuss human vs. automated review of reports.**
### **Step 11: Upload Bandit report to a vulnerability management tool (e.g., Snyk).**
### **Step 12: Ensure failed steps don’t block critical job (e.g., report upload).**
### **Step 13: Add Docker image scanning using Docker Scout:**
  - #### **Use docker scout quickview and docker scout cves**
  - #### **Compare both and explain the use-case for each.**
### **Step 14: Handle secrets/variables securely in GitHub Actions.**
### **Step 15: Review Docker Scout output:**
  - #### **Are dependencies beyond base image scanned?**
  - #### **Can you find fixed versions for all vulnerabilities?**
### **Step 16: Adjust Docker Scout scan to ignore low severity results.**
### **Step 17: Use the GitHub Marketplace version of docker-scout-action and explain its benefits.**
### **Step 18: Configure Docker Scout to only report critical and high vulnerabilities.**
### **Step 19: Ensure screenshots, logs, and reports are captured for your final submission.**
### **Step 20: Destroy your cloud instance to avoid charges.**
### **Step 21: Ensure your CI pipeline runs securely without exposing terminal access.**

---

## **3. Screenshots**

### **GitHub Actions CI output**
### **Bandit scan results**
### **Docker Scout scan logs**
### **GitHub Actions YAML workflow**
### **App running on cloud (browser view)**
### **Report attachments**
### **Instance/server dashboard**
### **Your name and date on at least one screenshot**

---

## **4. Findings and Reflection**

### **What security practices did you implement?**
### **What were your biggest challenges?**
### **How does GitHub Actions + Docker Scout compare to other CI/CD security tools?**



