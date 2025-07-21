# **Lab 10 Practice Report – DevSecOps with GitHub Actions and Docker Scout**

---

## **1. Introduction**
In order to guarantee that security is incorporated into the software development lifecycle from the beginning, DevSecOps combines development, security, and operations into a single process.  It places a strong emphasis on teamwork, automation, and ongoing security testing.

 In this lab, I used Docker Scout to check Docker images for vulnerabilities and GitHub Actions to automate CI/CD pipelines with integrated security scanning phases.  These solutions improve overall security posture and speed up safe software delivery by assisting in the early detection, reporting, and remediation of security vulnerabilities..

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
Copied existing workflow YAML template `.github/workflows/main.yml` to automate the build, test, and scan steps.

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
- Created and edited the workflow YAML file in the GitHub repository under `.github/workflows/main.yml`.  
- Used GitHub Actions  to set up Python.  
- Set up Python environment via GitHub Actions
- Ran Bandit scan against the Python app source

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
- Signed up and logged into https://app.snyk.io
- Connected GitHub repo to Snyk for automated vulnerability scanning
  
### **Step 12: Ensure failed steps don’t block critical job (e.g., report upload).**
#### **What was done:**
Used `--exit-zero` in the  `run: bandit -ll -ii -r . -f json -o bandit-report.json --exit-zero`

### **Step 13: Add Docker image scanning using Docker Scout:**
#### **What was done:**
In this step, Docker Scout was integrated into the CI/CD pipeline to perform security scanning on the Docker image built from the PyGoat application. The goal was to identify any known vulnerabilities within the containerized environment. The following actions were completed:
- Docker Scout CLI was installed using the official script.
- The Docker image was built using the provided Dockerfile.
- Two scanning commands were executed:
  - `docker scout quickview` to get a high-level security summary.
  - `docker scout cves` to view detailed information on CVEs (Common Vulnerabilities and Exposures).
#### **Compare both and explain the use-case for each.**

**Use-case Comparison:**
 - **docker scout quickview:**
   Provides a quick, high-level summary of the container image’s security   posture. It highlights the number of critical, high, medium, and low vulnerabilities, giving a risk overview that’s useful for triage.

 - **docker scout cves:**
   Offers a comprehensive listing of vulnerabilities by CVE ID, including severity, CVSS score, affected packages, and available fixes. This is particularly helpful for making informed remediation decisions.
    
### **Step 14: Handle secrets/variables securely in GitHub Actions.**
#### **What was done:**
Added sensitive credentials like DockerHub username/password to GitHub Secrets `(REPO_USER, REPO_PWD)`. Used them in workflow with `${{ secrets.REPO_USER }}` to avoid exposure.

### **Step 15: Review Docker Scout output:**
#### Description of What Was Done

I reviewed the security output of our Docker image using Docker Scout. The goal was to:

- Determine if Docker Scout scans dependencies beyond the base image.
- Check whether fixes are available for discovered vulnerabilities.

Docker Scout was used within a GitHub Actions workflow. The Docker image was built and scanned using both the `quickview` and `cves` commands to provide a summary and detailed list of vulnerabilities.

---

#### **Changes Made**
- Built Docker image myapp:latest using the local Dockerfile.
- Installed and executed Docker Scout CLI in the GitHub Actions pipeline.
- The image scan indexed 299 packages, including both base and application-level packages.
- Detected vulnerabilities and recommended upgrading from python:3.11-slim to python:3.13-slim.

  - #### **Are dependencies beyond base image scanned?**
    Yes. Docker Scout scanned 299 packages, which include both base image libraries and application-installed dependencies.
  - #### **Can you find fixed versions for all vulnerabilities?**
    Partially. Some vulnerabilities have known fixes (e.g., upgrading to a newer base image), but others do not yet have available patches. Manual package upgrades may be required in some cases.

#### **Issues Encountered**
Received a warning about Docker credentials being stored unencrypted in GitHub Actions

### **Step 16: Adjust Docker Scout scan to ignore low severity results.**
#### **What Was Done**
To reduce noise and improve clarity in the vulnerability scan reports, the Docker Scout GitHub Action was configured to ignore **low** and **unknown** severity vulnerabilities. This adjustment ensures that only vulnerabilities of **medium**, **high**, or **critical** severity are reported, allowing developers to focus on the most impactful security issues.

#### **Commands used:**
The following configuration was added to the GitHub Actions workflow file located at `.github/workflows/ci.yml`:

```yaml
    only-severities: critical,high,medium
```

### **Changes Made**
- Switched from using manual CLI steps (curl, sh install.sh) to the official Docker Scout GitHub Action for improved integration and maintainability.
- Generated and uploaded a SARIF-formatted report for use with GitHub Advanced Security or other static analysis tools.

### **Step 17: Use the GitHub Marketplace version of docker-scout-action and explain its benefits.**
#### **What Was Done**
To standardize and improve security scanning, the GitHub Marketplace version of `docker-scout-action` was used instead of installing and running Docker Scout manually via shell commands.

#### **Commands / Configuration**

The following snippet was added to the GitHub Actions workflow (`.github/workflows/ci.yml`):

```yaml
  uses: docker/scout-action@v1.0.9
```
#### **Benefits of Using the Marketplace Version**
- Security: Managed and maintained by Docker with verified updates.
- Reliability: Integrates cleanly with GitHub Actions without needing manual installation or shell scripts.
- Simplified CI/CD Integration: Offers first-class support for uploading SARIF reports and controlling scan filters.
- Customization: Easily customize scan behavior using inputs like only-severities, command, and artifact output.
- Audit-Friendly: Built-in support for GitHub Advanced Security and centralized reporting.
  
### **Step 18: Configure Docker Scout to only report critical and high vulnerabilities.**
#### **What Was Done**
The Docker Scout GitHub Action was updated to filter out all vulnerabilities except those with **critical** and **high** severity.

#### **Key Line Changed in Workflow YAML**
```yaml
only-severities: critical,high
```
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



