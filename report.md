# Assignment 5 — Automation Scanning
**Student:** <Your Name>  
**Target tested:** ${TARGET}  
**Date:** $(date +"%Y-%m-%d")

---

## 1. Objective
Perform safe, authorized automated vulnerability scanning of a web application and produce remediation advice.

## 2. Scope & Authorization
Target used: ${TARGET}  
Only tested allowed target(s). No exploitation performed. Tests were passive/light by default; active scans were run only with time bounds.

## 3. Tools & commands
- Nmap: `nmap -Pn -sV ${TARGET} -oN scans/nmap.txt` (run separately / see scans/)
- Gobuster: `gobuster dir -u http://${TARGET} -w /usr/share/wordlists/dirb/common.txt -o scans/gobuster.txt`
- Nikto: `nikto -h http://${TARGET} -timeout 10 -output scans/nikto.txt`
- Nuclei (bounded): `nuclei -u http://${TARGET} -severity critical,high -c 5 -timeout 10 -o scans/nuclei.txt`
- OWASP ZAP baseline: `zap-baseline.py -t http://${TARGET} -r scans/zap_baseline.html`

## 4. Recon summary (Nmap / WhatWeb)
- _Paste Nmap summary here (open ports, services)._  
(Place full `scans/nmap.txt` in Appendix.)

## 5. Directory discovery (Gobuster) — notable results
The directory enumeration discovered the following interesting paths (redirects and statuses preserved):

/admin                (Status: 302) → /login.jsp  
/aux                  (Status: 200)  
/bank                 (Status: 302) → /login.jsp  
/com3                 (Status: 200)  
/com1                 (Status: 200)  
/com2                 (Status: 200)  
/con                  (Status: 200)  
/images               (Status: 302) → /images/  
/lpt1                 (Status: 200)  
/lpt2                 (Status: 200)  
/nul                  (Status: 200)  
/pr                   (Status: 302) → /pr/  
/prn                  (Status: 200)  
/static               (Status: 302) → /static/  
/util                 (Status: 302) → /util/

Evidence files: `scans/gobuster.txt`, `scans/login.html`, `scans/admin.html`.

## 6. Automated scan results (summary)
- **Nikto**: (insert key findings or "No high risk issues found" after review) — see `scans/nikto.txt`.  
- **Nuclei (critical/high, 5m cap)**: **No critical/high results** found (see `scans/nuclei.txt`).  
- **ZAP baseline**: see `scans/zap_baseline.html`.

> Note: Scans were time-capped for assignment constraints — full template scans may reveal additional low/medium issues.

## 7. Key findings & recommendations (example format)
1. **Login page reachable at `/login.jsp`** — Medium — Evidence: `scans/login.html` / headers.  
   - Recommendation: Enforce HTTPS, implement rate-limiting, protect against brute-force.

2. **Administrative area `/admin` redirects to login** — Medium — Evidence: `scans/admin_headers.txt`.  
   - Recommendation: Ensure admin URLs are protected by IP allowlist or stronger MFA and restrict enumeration.

3. **Static content directories (`/images`, `/static`) exposed** — Low — Evidence: gobuster output, headers.  
   - Recommendation: Verify no sensitive files are present; set correct MIME types and caching policies.

(Replace/add findings after reviewing `nikto.txt`, `nuclei.txt`, and `nmap.txt`.)

## 8. False positives
List any findings you investigated and marked as false positives.

## 9. Appendix — full scan outputs
- `scans/nmap.txt`  
- `scans/gobuster.txt`  
- `scans/nikto.txt`  
- `scans/nuclei.txt`  
- `scans/zap_baseline.html`  
- `scans/login.html`, `scans/admin.html`, `scans/login_headers.txt`, `scans/admin_headers.txt`, `scans/robots.txt`

