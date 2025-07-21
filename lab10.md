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

**Report:**

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
![GitHub Actions CI output](https://github.com/ahmedsiddig1/Lab10/blob/main/Images/GitHub%20Actions%20CI%20output.png)
**What it shows:**  
This screenshot shows a completed GitHub Actions workflow named `CI` triggered by a push. It includes two jobs: `sast_scan` (for Bandit static analysis) and `image_scan` (for Docker image scanning). Both jobs are completed successfully.

**Why it matters for secure CI/CD:**  
It proves the secure pipeline is working. Each time new code is pushed, security scans and image builds are automatically triggered. This is a key DevSecOps practice — automating security from the start.

**Why it matters for secure CI/CD:**  
It proves the secure pipeline is working. Each time new code is pushed, security scans and image builds are automatically triggered. This is a key DevSecOps practice — automating security from the start.
### **Bandit scan results**
![Bandit scan results](https://github.com/ahmedsiddig1/Lab10/blob/main/Images/Bandit%20scan%20results.png)
**What it shows:**  
This screenshot shows the Bandit static code analysis running inside GitHub Actions under the `sast_scan` job. It captures the CI log output from the step that executes `bandit -ll -ii -r . -f json -o bandit-report.json`. The output confirms that the scan completed and reported issues in the code.

**Why it matters for secure CI/CD:**  
Running Bandit in CI ensures every code commit is automatically checked for security vulnerabilities like insecure imports, hardcoded credentials, or unsafe function usage. This builds a shift-left security approach where developers get security feedback early, before code merges or deployments.

### **GitHub Actions YAML workflow**
![GitHub Actions YAML workflow](https://github.com/ahmedsiddig1/Lab10/blob/main/Images/GitHub%20Actions%20YAML%20workflow.png)
**What it shows:**  
This screenshot displays the contents of `.github/workflows/main.yml`. It defines two jobs (`sast_scan`, `image_scan`) and includes steps to install Bandit, run Docker build/push, and perform image scanning with Docker Scout.

**Why it matters for secure CI/CD:**  
Storing workflows as code ensures your CI/CD process is versioned and auditable. It also enables consistent security checks for every commit, PR, or deployment.

### **App running on cloud (browser view)**
![App running on cloud (browser view)](https://github.com/ahmedsiddig1/Lab10/blob/main/Images/App%20running%20on%20cloud%20(browser%20view).png)
**What it shows:**  
This screenshot shows the PyGoat app running in a browser, accessed via the Azure VM’s public IP address. It confirms that the app is deployed and publicly accessible.

**Why it matters for secure CI/CD:**  
This demonstrates end-to-end CI/CD with security built in. The app was only deployed after passing scans, showing how security gates can block unsafe releases.

---

## **4. Findings and Reflection**

### **What security practices did you implement?**
- Used GitHub Secrets to securely store sensitive information like DockerHub credentials.
- Ran Bandit to perform static analysis on Python code and generated a SARIF report.
- Implemented Docker Scout to scan container images for known vulnerabilities (CVEs).
- Configured Docker Scout to ignore low-severity vulnerabilities and only report critical/high issues.
- Used GitHub Marketplace verified actions (like `docker/scout-action`) to avoid running untrusted code.
- Uploaded vulnerability reports securely using `actions/upload-artifact`.

### **What were your biggest challenges?**
- Initial setup of Docker Scout CLI inside GitHub Actions runner was tricky — had to install and configure it manually before switching to the Marketplace action.
- Managing DockerHub credentials securely required understanding the right environment variable names and GitHub Secrets syntax.
- Filtering low-severity issues in Docker Scout required custom YAML configuration and testing to get the output right.
- Understanding how SARIF reports work and where to locate the results for review was also a learning curve.
- Adjusting the pipeline to continue even if scan jobs fail (e.g., to ensure artifacts are uploaded) required careful use of `if: always()`.

### How does GitHub Actions + Docker Scout compare to other CI/CD security tools?

- **Integration Simplicity:** GitHub Actions and Docker Scout offer native integration with GitHub, making setup easier than tools like Jenkins or GitLab CI.
- **Security Focus:** Docker Scout provides specific vulnerability scanning tied to containers, while tools like Snyk language/package scanning.
- **Customization:** GitHub Actions is very flexible, but more manual configuration is needed .
- **Report Integration:** GitHub allows uploading SARIF reports, enabling integration with Security tab insights—this is a powerful feature compared to basic email reports in other CI tools.
- **Cost:** GitHub Actions and Docker Scout are free for public repos and limited use on private ones, making them great for open-source or academic projects.

Overall, GitHub Actions + Docker Scout provided a powerful, secure, and cost-effective way to automate security scans in a CI/CD pipeline. It helped enforce secure coding and image practices while keeping the pipeline flexible and auditable.



