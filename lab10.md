# **Lab 10 Practice Report – DevSecOps with GitHub Actions and Docker Scout**

---

## **1. Introduction**
In order to guarantee that security is incorporated into the software development lifecycle from the beginning, DevSecOps combines development, security, and operations into a single process.  It places a strong emphasis on teamwork, automation, and ongoing security testing.

 In this lab, we use Docker Scout to check Docker images for vulnerabilities and GitHub Actions to automate CI/CD pipelines with integrated security scanning phases.  These solutions improve overall security posture and speed up safe software delivery by assisting in the early detection, reporting, and remediation of security vulnerabilities..

---

## **2. Steps Performed**

### **Step 1: Clone the DevSecOps Crash Course Repository.**
#### **What was done:**
Cloned the forked PyGoat vulnerable application repository from GitHub to local machine for further analysis and CI pipeline setup.
#### **Commands:**
```bash
git clone https://github.com/nanuchi/devsecops-crash-course-pygoat.git
cd devsecops-crash-course-pygoat
```
### **Step 2: Understand the repo structure (review Anna’s GitHub Actions tutorial if needed).**
#### **What was done:**
After cloning the GitLab CI/CD tutorial project, I analyzed the file structure to understand how the project is organized.

**Details:**
-Dockerfile for building the image
-requirements.txt for Python dependencies
-Workflow YAML files defining CI jobs
-Application source code under /pygoat
#### **Commands:**
`tree -L 8`

### **Step 3: Use GitHub Actions templates to set up workflow.**
#### **What Was Done:**
Copied existing workflow YAML template or created new `.github/workflows/ci.yml` to automate the build, test, and scan steps.

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
-Set up Python environment via GitHub Actions
-Ran Bandit scan against the Python app source

### **Step 6: Review Bandit scan results:**
- #### **What did the scan detect?**
The Bandit scan detected potential security issues in the Python application code, including common vulnerabilities.

- #### **What is the meaning of job failure?**
A job failure in the Bandit scan typically means that the scan found one or more security issues that meet or exceed the defined severity threshold, causing the CI job to fail to prevent unsafe code from progressing further in the pipeline.

- #### **Discuss severity vs. confidence levels**
- **Severity** indicates the potential impact of the vulnerability if exploited, classified as low, medium, or high.
- **Confidence** measures how certain the tool is that the identified issue is a true positive, also classified as low, medium, or high.
Understanding both helps prioritize which issues to address first — high severity with high confidence should be fixed urgently, while low severity or low confidence findings may require further investigation or can be deprioritized.

### **Step 7: Modify the workflow to ignore low-severity, low-confidence issues.**
#### **What was done**
Added Bandit flags to ignore low severity and confidence issues:
`run: bandit -ll -ii -r . -f json -o bandit-report.json --exit-zero`
#### **Changes made:**
Updated workflow YAML with these flags

### **Step 8: Re-run the pipeline and review updated results.**
#### **What Was Done**
- Re-executed the GitHub Actions CI pipeline after modifying the bandit scan step to allow the job to continue even if vulnerabilities were found.
- Ensured that the Bandit report (bandit-report.json) was generated and uploaded as an artifact.

### **Step 9: Generate and include a Bandit report (supported formats).**
A Bandit security scan was run on the Python project to generate a detailed security report. The report helps identify potential vulnerabilities and insecure coding practices in the codebase.

#### **Commands used:**
The following command was used to run Bandit and generate the report in this format:
`run: bandit -ll -ii -r . -f json -o bandit-report.json --exit-zero`
**Report**
[Report in JSON file](https://github.com/ahmedsiddig1/Lab10/blob/main/bandit-report.json)
### **Step 10: Discuss Human vs. Automated Review of Reports**

#### **Automated Review:**
Automated tools like Bandit quickly scan code for known security issues using predefined rules and patterns. They are consistent, fast, and effective at catching common vulnerabilities such as use of insecure functions or missing input validation.

- **Advantages:**
  - Fast and repeatable
  - Integrated into CI/CD pipelines
  - Can enforce security gates automatically
  - Detects well-known issues early in development

- **Limitations:**
  - May produce false positives or false negatives
  - Cannot fully understand code logic or intent
  - Misses context-specific vulnerabilities or design flaws

#### **Human Review:**
Human code review involves manual inspection of code by developers or security professionals. Humans can understand business logic, context, and the broader architecture, which automated tools cannot.

- **Advantages:**
  - Identifies logic flaws, insecure architecture, and design risks
  - Provides contextual understanding and intent
  - Can validate whether flagged issues are actual risks

- **Limitations:**
  - Time-consuming and requires expertise
  - Prone to human error or inconsistency
  - Not scalable for large codebases alone

#### **Conclusion:**
A **combined approach** is most effective: automated tools catch obvious and known issues quickly, while human reviewers focus on logic, context, and complex vulnerabilities. Together, they create a more robust and secure software development process.

### **Step 11: Upload Bandit report to a vulnerability management tool (e.g., Snyk).**
#### **What was done:**
-Signed up and logged into https://app.snyk.io
-Connected GitHub repo to Snyk for automated vulnerability scanning
### **Step 12: Ensure failed steps don’t block critical job (e.g., report upload).**
#### **What was done:**
Used `--exit-zero` in the  `run: bandit -ll -ii -r . -f json -o bandit-report.json --exit-zero`

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



