# Nikto Web Vulnerability Scanner — Complete Guide (GitHub/Lab Report)

## 1. Introduction
Nikto is an open-source web vulnerability scanner widely used by cybersecurity professionals, penetration testers, and students to identify security issues in web servers and web applications. It performs comprehensive tests against web servers to detect known vulnerabilities, insecure configurations, outdated server software, default files, exposed directories, and missing security headers.

Nikto is designed to be fast, flexible, and easy to use. It operates via a command-line interface, making it lightweight and suitable for automation. Although Nikto does not exploit vulnerabilities, it plays a critical role in the reconnaissance and vulnerability identification phase of penetration testing. Because Nikto can generate a large number of requests, it must only be used on systems where explicit authorization has been granted.

---

## 2. Key Features
- Scans for thousands of known web server vulnerabilities
- Detects outdated and vulnerable server software/components
- Identifies default files, scripts, and directories
- Finds common server misconfigurations
- Checks for missing or insecure HTTP security headers
- Supports HTTP and HTTPS scanning
- Enables targeted scanning using tuning options
- Supports proxying through tools like Burp Suite
- Exports results in multiple formats (TXT/HTML/XML)
- Supports rate limiting to reduce server impact

---

## 3. Getting Started
To use Nikto effectively, install it, verify the installation, and then perform scans using appropriate options depending on your target and testing scope.

---

## 4. Installation

### 4.1 Install Nikto on Linux (Package Manager)
**Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install nikto
```

---

### 4.2 Install Nikto from Source (Any Linux)
Installing from source is useful if you need the latest version or your package repository is outdated.

```bash
git clone https://github.com/sullo/nikto.git
cd nikto
sudo chmod +x nikto.pl
```

## 5. Checking Nikto Version and Database
Use the following command to confirm Nikto and its database/plugins are available:

```bash
nikto -Version
```

This output typically confirms:
- Nikto version
- Plugin version
- Database information

<img width="1067" height="155" alt="image" src="https://github.com/user-attachments/assets/43ec00c3-b84f-43dd-9153-d8d34d26dbad" />

---

## 6. Core Functionalities
Nikto identifies common and critical weaknesses such as:

### 6.1 Outdated Software
Detects older versions of web server software and components that may contain known vulnerabilities.

### 6.2 Default Files and Directories
Finds default or leftover files/directories (e.g., `phpinfo.php`, `/admin/`, `/backup/`) that may expose sensitive information.

### 6.3 Configuration Issues
Detects risky or unnecessary configurations, including insecure HTTP methods like `OPTIONS` and `TRACE`.

### 6.4 Security Headers
Checks for missing security headers (e.g., `X-Frame-Options`, `X-Content-Type-Options`, and others) that help protect against common attacks.

---

## 7. Running a Basic Scan
**Syntax:**
```bash
nikto -h <URL or IP address>
```

**Example:**
```bash
nikto -h http://example.com
```

A basic scan commonly checks:
- Known vulnerabilities
- Misconfigurations
- Outdated software/components
- Missing security headers
- Exposed files and directories

<img width="1074" height="540" alt="image" src="https://github.com/user-attachments/assets/8c446001-b70e-447e-866b-19ffdd6a9bba" />


---

## 8. Useful Nikto Parameters

| Parameter | Purpose |
|---|---|
| `-h` | Target host (URL/IP) |
| `-p` | Scan a specific port (e.g., 443, 8080) |
| `-ssl` | Force SSL/TLS scanning |
| `-o` | Save output to a file |
| `-timeout` | Set request timeout |
| `-C all` | Run all HTTP-related checks |
| `-Tuning` | Focus scan on specific test categories |
| `-Pause` | Delay between requests |

---

## 9. Example Advanced Scan Command
```bash
nikto -h http://example.com -o scan.txt -timeout 10 -C all
```

**What it does:**
- Scans the target host
- Saves output to `scan.txt`
- Uses a 10-second request timeout
- Runs broader HTTP checks

<img width="1068" height="526" alt="image" src="https://github.com/user-attachments/assets/f1a332f3-6677-4824-9e07-0172f7b2872e" />


---

## 10. Scanning for Specific Vulnerabilities (Tuning)

Nikto can narrow scans to specific vulnerability categories using `-Tuning`.

### 10.1 File Upload–Related Checks (Tuning Code 4)
```bash
nikto -h <target-domain> -Tuning 4
```
<img width="1063" height="529" alt="image" src="https://github.com/user-attachments/assets/c7378768-fe6c-45a1-8a8d-6a9f4663ac85" />


### 10.2 Injection-Related Checks (Tuning Code 5)
```bash
nikto -h <target-domain> -Tuning 5
```
<img width="1066" height="528" alt="image" src="https://github.com/user-attachments/assets/a57eb952-0f43-424d-b069-d0bad4019016" />

---

## 11. Nikto Tuning Codes Reference

| Code | Description |
|---:|---|
| 0 | File upload vulnerabilities |
| 1 | Interesting files / files seen in logs |
| 2 | Misconfigurations / default files |
| 3 | Information disclosure |
| 4 | Injection vulnerabilities (XSS/script/HTML) |
| 5 | Remote file execution / shell-related checks |

---

## 12. Exporting Scan Results to a Report
Save results to a file using `-o`:

```bash
nikto -h <target-domain> -o output.txt
```

Verify the file exists:
```bash
ls
```

<img width="1086" height="546" alt="image" src="https://github.com/user-attachments/assets/abca1164-f4cb-4dcd-8cc8-dbb233e9811c" />


---

## 13. Advanced Scanning Options

### 13.1 Port-Specific Scanning
```bash
nikto -h <target-domain> -p 443
```
<img width="1068" height="526" alt="image" src="https://github.com/user-attachments/assets/d8530b3e-8e5a-4cb4-9064-542cfe6257b4" />


### 13.2 Custom User-Agent
Customizing the User-Agent can help mimic browser traffic.

```bash
nikto -h <target-domain> -useragent "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
```

<img width="1079" height="511" alt="image" src="https://github.com/user-attachments/assets/fadc9195-6313-4c2f-85a7-9679d946fca7" />


---

## 14. Using Nikto with Burp Suite (Proxy)
Nikto can be routed through Burp Suite to intercept and analyze scan requests.

```bash
nikto -h <target-domain> -useproxy http://127.0.0.1:8080
```

**Benefits:**
- Inspect requests and responses
- Analyze headers and server behavior
- Correlate automated findings with manual testing

<img width="1069" height="521" alt="image" src="https://github.com/user-attachments/assets/1d707c97-4824-4cbd-bd55-675c1bb5b9f0" />


---

## 15. Rate Limiting (Pause)
Use `-Pause` to slow requests and reduce server impact:

```bash
nikto -h <target-ip> -Pause 3
```

<img width="1075" height="302" alt="image" src="https://github.com/user-attachments/assets/a7db0395-1721-4613-b62f-42b13a23f8a2" />


---

## 16. Best Practices
- Always obtain explicit authorization before scanning
- Avoid scanning production systems without approval
- Schedule scans regularly for continuous monitoring
- Patch/upgrade vulnerable components based on findings
- Remove or secure exposed files and directories
- Implement recommended security headers
- Combine Nikto with other security tools for deeper coverage
- Document results and remediation actions

---

## 17. Conclusion
Nikto is a powerful and effective tool for identifying common vulnerabilities and misconfigurations in web applications and web servers. By understanding its features, scan options, and limitations, security professionals and students can integrate Nikto into security assessment workflows. When used responsibly with proper authorization, Nikto helps improve the overall security posture of web-based systems.

---

## 18. Disclaimer
Use Nikto only on systems you own or where you have explicit permission to test. Unauthorized scanning may be illegal and unethical.
