# Cloud Server Security Assessment & Hardening: SoftCorp

> A comprehensive vulnerability assessment and security hardening report for a cloud server infrastructure, conducted in preparation for DevOps deployment.

---

## 📋 Overview

This project documents the end-to-end security evaluation and hardening of SoftCorp's cloud server (Ubuntu Linux, IP: `30.30.0.100`). The assessment follows a structured methodology: scanning for vulnerabilities, analysing findings, implementing remediations, and validating results, to bring the server into alignment with industry best practices before production deployment in a DevOps environment.

The server operates under an **Infrastructure as a Service (IaaS)** model, giving SoftCorp direct control over the OS, storage, and deployed applications.

---

## 🎯 Objectives

- Identify vulnerabilities, misconfigurations, and security weaknesses in the cloud server
- Analyse the risk and potential impact of each finding
- Implement prioritised, technical security remediations
- Validate the effectiveness of all implemented controls through follow-up scanning
- Provide a roadmap for ongoing DevOps deployment security

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Nessus** | Comprehensive vulnerability scanning |
| **Greenbone / OpenVAS** | Open-source vulnerability assessment |
| **Nexpose (Rapid7)** | Vulnerability detection and risk scoring |
| **OWASP ZAP** | Web application security testing |
| **Nikto** | Web server misconfiguration scanning |
| **VMware** | Virtualisation environment |
| **Ubuntu Linux** | Target server operating system |

---

## 🔍 Vulnerability Summary (Initial Scan)

A total of **35 vulnerabilities** were identified across all scanners:

| Severity | Count |
|----------|-------|
| 🔴 Critical | 3 |
| 🟠 High | 1 |
| 🟡 Medium | 19 |
| 🔵 Low | 10 |
| ⚪ Informational | 29+ |

### Key Findings

- **Default Credentials** — Admin account accessible with `admin:password`
- **Apache HTTP Server 2.4.52 Vulnerabilities** — Multiple CVEs including HTTP request smuggling (CVE-2022-22720), buffer overflow (CVE-2022-22721), and mod_sed out-of-bounds write (CVE-2022-23943)
- **Weak SSH MAC Algorithms** — Server supported weak algorithms including `umac-64`
- **Cleartext Data Transmission** — Sensitive data sent over HTTP rather than HTTPS
- **Unprotected ownCloud/Nextcloud Data Directory** — Unauthenticated access to user files
- **Directory Browsing Enabled** — File and folder enumeration possible
- **Missing Security Headers** — No CSP, X-Frame-Options, or X-Content-Type-Options
- **Outdated jQuery Library** — Version < 3.5.0 vulnerable to multiple XSS attacks
- **phpinfo() File Exposed** — Sensitive PHP/server configuration disclosed publicly

---

## 🔧 Security Remediations Implemented

### 1. System Updates & Upgrades
- Applied all OS and package updates via `apt update && apt upgrade`
- Configured `unattended-upgrades` for automatic security patching

### 2. Apache Web Server Hardening
- Upgraded Apache to the latest stable version (2.4.58+)
- Installed and configured **ModSecurity** WAF with OWASP Core Rule Set
- Disabled the HTTP OPTIONS method
- Suppressed server version disclosure via `ServerTokens Prod` and `ServerSignature Off`

### 3. Security Headers
- Implemented **Content Security Policy (CSP)**: removed wildcard directives, `unsafe-eval`, and `unsafe-inline`
- Added **X-Frame-Options: SAMEORIGIN** to prevent clickjacking
- Added **X-Content-Type-Options: nosniff** to prevent MIME sniffing

### 4. PHP Hardening
- Disabled `expose_php`, `display_errors`, `file_uploads`
- Blocked dangerous functions: `phpinfo`, `system`, `shell_exec`, `exec`, and others
- Removed the exposed `phpinfo.php` file

### 5. Directory Browsing & Indexing
- Disabled directory listing in Apache configuration (`Options -Indexes`)
- Secured the ownCloud/Nextcloud data directory with strict access controls

### 6. jQuery Library Update
- Replaced outdated jQuery with version **3.6.0**

### 7. Encrypted Data Transmission (HTTPS)
- Installed **Certbot** with Apache plugin
- Configured SSL/TLS certificate
- Enforced HTTPS redirects for all HTTP traffic

### 8. Password Management
- Changed default admin credentials to a strong, unique password
- Updated ownCloud web interface credentials

### 9. SSH Hardening
- Disabled weak MAC algorithms; enforced strong algorithms (`hmac-sha2-512`, `hmac-sha2-256`)
- Generated and configured **SSH key pairs** for authentication

### 10. Network Security
- Configured **UFW firewall**: only SSH, HTTP, and HTTPS traffic permitted
- Blocked ICMP timestamp requests
- Installed and enabled **Fail2Ban** to prevent brute-force attacks
- Configured **AppArmor** for application-level access control
- Disabled TCP timestamps (`net.ipv4.tcp_timestamps=0`)

### 11. Systemctl / Kernel Hardening
- Hardened kernel network parameters via `/etc/sysctl.conf` against SYN floods and malformed packets

### 12. Additional Security
- Installed **OpenVPN** for secure remote access
- Installed **ClamAV** antivirus for malware detection

---

## ✅ Final Scan Results

Post-remediation scans confirmed:

- ✅ **No Critical or High severity vulnerabilities detected**
- ✅ Default credentials resolved
- ✅ Apache CVEs patched
- ✅ Weak SSH MAC algorithms disabled
- ✅ phpinfo file removed
- ✅ jQuery updated
- ✅ Server version information no longer leaked
- ⚠️ **Anti-CSRF token protection**: remains an open item for future development

---

## 🚀 DevOps Deployment Recommendations

Beyond the server itself, the report also outlines a security strategy for the wider DevOps environment:

- **Network Segmentation & DMZ** — isolate the server from direct public internet access
- **Secrets Management** — use tools like HashiCorp Vault or AWS Secrets Manager
- **CI/CD Pipeline Security** — enforce least privilege, signed images, and audit logging
- **Security Monitoring & SIEM** — centralised logging and incident response procedures
- **Infrastructure as Code (IaC)** — version-controlled Terraform/Ansible configurations
- **Container Security** — trusted images, runtime security policies, network policies
- **Security Awareness Training** — regular training for all staff
- **Compliance** — alignment with GDPR, NIST SP 500-299, and CSA Cloud Controls Matrix (CCM)

---

## 📁 Repository Contents

| File | Description |
|------|-------------|
| `SoftCorp Cloud Server Security Assessment Report.docx` | Full security assessment report including scan screenshots, analysis tables, implementation steps, and references |

---

## 📚 Standards & Frameworks Referenced

- NIST Cloud Computing Security Reference Architecture (SP 500-299)
- Cloud Security Alliance: Cloud Controls Matrix (CCM)
- OWASP Top 10 & ModSecurity Core Rule Set
- NIST SP 800-115: Technical Guide to Information Security Testing

---

## ⚠️ Disclaimer

This assessment was conducted in a controlled, virtualised lab environment on infrastructure owned by SoftCorp. All scanning and exploitation techniques were performed with full authorisation. This report is intended for educational and professional security improvement purposes only.

---

## 👤 Author

**ATEJI** | [SoftCorp Cloud Server Security Assessment Report](https://github.com/AtejiEmmanuel/Cloud-Server-Security-Assessment/blob/main/SoftCorp%20Cloud%20Server%20Security%20Assessment%20Report.docx)
