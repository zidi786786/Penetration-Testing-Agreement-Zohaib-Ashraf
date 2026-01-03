# Web Application Vulnerability Scanning with OWASP ZAP

# 1. Objective

The objective of this lab is to perform a full web application vulnerability scan using OWASP ZAP, identify common web security issues, and understand their potential impact and remediation techniques.

# 2. Lab Environment

Scanner Tool: OWASP ZAP

Target: Test web application (DVWA)

Attacker Machine: Kali Linux

Scan Type: Automated Active Scan

# 3. Tool Used
OWASP ZAP (Zed Attack Proxy)

OWASP ZAP is an open-source web application security scanner used to find vulnerabilities such as:

**Cross-Site Scripting (XSS)

SQL Injection

Security misconfigurations

Missing security headers**

# 4. Workflow (Step-by-Step)

Step 1: Launch OWASP ZAP

Open OWASP ZAP

Select “Automated Scan”

<img width="521" height="490" alt="1  opening zap" src="https://github.com/user-attachments/assets/8717ea3f-bcd1-416f-942f-d74f2939d359" />


Step 2: Open The Website you want to Attack 

<img width="1109" height="547" alt="3  attack dvwa" src="https://github.com/user-attachments/assets/1ee58fbd-dffd-4aff-a08a-a2fd388aac2d" />


Step 3: Define the Target

Enter the target application URL

Select Traditional Spider

Enable Active Scan

<img width="1187" height="490" alt="4  pasting dvwa address in zap" src="https://github.com/user-attachments/assets/2a995efc-7634-4c86-b5dd-0e7c43bedf6a" />


Step 5: Review Alerts

..Navigate to the Alerts tab

..Review vulnerabilities by:

  **Risk level

  **Confidence

  **URL affected

  <img width="992" height="306" alt="5  scan results" src="https://github.com/user-attachments/assets/832a267d-a0a4-4444-a0fa-45b24b7fbbfd" />


Step 6. More research About the Valnerability

<img width="1359" height="642" alt="6  searching more about vuln found" src="https://github.com/user-attachments/assets/c4f973bb-a18b-4520-94a4-23ba63d32658" />


# 6. Observations

Automated scanning quickly reveals common misconfigurations.

Not all findings are exploitable, but each represents a security weakness.

Manual verification is necessary to confirm true positives.


# 7. Limitations of Automated Scanning

False positives may occur

Business logic flaws are often missed

Requires human analysis for validation


# 8. Why This Matters

Web applications are a major attack surface. Vulnerabilities like XSS and misconfigurations can lead to:

Session hijacking

Data theft

Reputational damage

OWASP ZAP helps identify these issues early in development or testing phases.


# 9. Conclusion

This lab demonstrates the importance of continuous security testing. OWASP ZAP is a powerful tool for identifying common vulnerabilities, but it should be combined with manual testing and secure coding practices.
