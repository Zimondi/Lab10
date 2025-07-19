# **Lab 10 Practice Report – DevSecOps with GitHub Actions and Docker Scout**

---

## **1. Introduction**
Integrating security into each stage of the software development lifecycle is known as DevSecOps.  This lab tested a vulnerable Python application (PyGoat) for both static and container-based security vulnerabilities by implementing a safe CI/CD pipeline utilizing **GitHub Actions** and **Docker Scout**.  Docker Scout checks Docker images for vulnerabilities, while GitHub Actions automates build, test, and deploy processes.

---

## **2. Steps Performed**

### **Step 1: Clone the DevSecOps Crash Course Repository.**

### **Step 2: Understand the repo structure (review Anna’s GitHub Actions tutorial if needed).**
### **Step 3: Use GitHub Actions templates to set up workflow.**
### **Step 4: List the security scan jobs included in the workflow.**
### **Step 5: Configure a Bandit SAST scan for Python app:**
  - #### **Install dependencies**
  - #### **Set up Python environment**
  - #### **Run Bandit**
### **Step 6: Review Bandit scan results:**
  - #### **What did the scan detect?**
  - #### **What is the meaning of job failure?**
  - #### **Discuss severity vs. confidence levels.**
### **Step 7: Modify the workflow to ignore low-severity, low-confidence issues.**
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



