# Week 2 – Project Module 1: Footprinting & Reconnaissance with Kali Linux

**Cybersecurity & Ethical Hacking Internship | Networkwalks**  
**Prepared by:** Ezekiel Arom  
**Project reference:** W2-PM1  
**Target specified by training material:** `networkwalks.com`  
**Environment:** Kali Linux

> **Authorization and scope:** This project is for education and authorized security assessment only. The training PDF names `networkwalks.com` as its example target; this does not independently establish permission for new testing. Perform live requests only within your documented authorization and agreed scope. Do not attempt exploitation, WAF bypass, or intrusive enumeration.

## 1. Project overview

Footprinting and reconnaissance identify publicly exposed information about a domain and its infrastructure. This exercise uses six Kali Linux utilities to examine domain registration, web technology, DNS resolution, HTTP response headers, WAF indicators, and DNS records. The goal is to understand what information may be visible to an external observer and how defenders can reduce unnecessary disclosure.

### Learning objectives

- Interpret public domain registration and name-server information.
- Identify web technologies exposed by a site.
- Resolve a hostname to an IP address.
- Inspect HTTP response headers and redirects.
- Interpret indications of web application firewall protection.
- Inventory DNS records and assess information exposure.
- Capture reproducible evidence without disclosing sensitive information.

## 2. Tools and tasks

| Task | Tool | Purpose | Screenshot |
|---|---|---|---|
| 1 | `whois` | Domain registration and name servers | `screenshots/task1-whois.png` |
| 2 | `whatweb` | Web technology fingerprinting | `screenshots/task2-whatweb.png` |
| 3 | `nslookup` | Domain-to-IP resolution | `screenshots/task3-nslookup.png` |
| 4 | `curl` | HTTP response headers | `screenshots/task4-curl.png` |
| 5 | `wafw00f` | WAF detection indicators | `screenshots/task5-wafw00f.png` |
| 6 | `dnsrecon` | DNS record enumeration | `screenshots/task6-dnsrecon.png` |

## 3. Lab environment and evidence collection

Use a Kali Linux VM and a terminal. Confirm the six utilities are installed, and create folders for screenshots and command output.

```bash
mkdir -p screenshots outputs
command -v whois whatweb nslookup curl wafw00f dnsrecon
```

The commands below reproduce the training PDF's task syntax. **Run them against the named live domain only if you have current authorization.** Replace the target with an authorized domain otherwise. Save a screenshot after each task, and store the terminal output as text. Results can change over time.

## 4. Task 1 – WHOIS: Domain registration

**Objective:** Review publicly available registration details, including registrar, registration/expiration dates (if disclosed), and authoritative name servers.

```bash
whois networkwalks.com | tee outputs/task1-whois.txt
```

**Interpretation:** WHOIS can reveal registrar and name-server information, although privacy protection and registry rules may hide registrant identity. The supplied PDF associates the name servers with HostGator; independently verify this before presenting it as a current observation.

**Security relevance:** Public infrastructure and registration metadata can aid profiling and social engineering. Organizations should protect registrar accounts and minimize unnecessary public contact details.

**Evidence – Task 1**

<img src="screenshots/task1-whois.png" alt="Kali Linux WHOIS terminal output for authorized target" width="850">

*Figure 1. WHOIS query and observed registration details. Replace with your own screenshot.*

## 5. Task 2 – WhatWeb: Technology fingerprinting

**Objective:** Identify visible web-server, CMS, framework, and plugin indicators.

```bash
whatweb networkwalks.com | tee outputs/task2-whatweb.txt
```

**Interpretation:** The training PDF illustrates WordPress 7.0.4 and WP Download Manager 3.3.58 as detected technology versions. These are **PDF example findings, not independently verified current versions**. Confirm your own output before reporting any software version.

**Security relevance:** Exposed version strings may help an adversary research publicly known weaknesses. Defenders should keep components patched, remove unnecessary version disclosure, and verify whether findings are accurate.

**Evidence – Task 2**

<img src="screenshots/task2-whatweb.png" alt="WhatWeb technology fingerprint results" width="850">

*Figure 2. Web technology fingerprinting output. Replace with your own screenshot.*

## 6. Task 3 – Nslookup: DNS resolution

**Objective:** Resolve the domain to its advertised IP address.

```bash
nslookup networkwalks.com | tee outputs/task3-nslookup.txt
```

**Interpretation:** The PDF shows `192.232.216.135` as its example resolved address. DNS answers may change with time, location, CDN, or hosting configuration. Record the IP from your actual run and its date.

**Security relevance:** DNS information helps map publicly reachable infrastructure. Defenders should review public DNS entries and avoid exposing internal-only hostnames or addresses.

**Evidence – Task 3**

<img src="screenshots/task3-nslookup.png" alt="Nslookup domain resolution result" width="850">

*Figure 3. DNS resolution evidence. Replace with your own screenshot.*

## 7. Task 4 – cURL: HTTP response headers

**Objective:** Inspect the HTTP status and response headers without downloading the full response body.

```bash
curl -I https://networkwalks.com | tee outputs/task4-curl.txt
```

**Interpretation:** Response headers can expose redirects, cookies, cache settings, web-server identifiers, and application links. The PDF discusses a WordPress REST API path (`/wp-json/`) as an example. Note that a `HEAD` response may differ from a `GET` response and some sites restrict HEAD requests.

**Security relevance:** Review unnecessary software banners and configure suitable security headers. The mere presence of a REST API or header is not itself proof of a vulnerability.

**Evidence – Task 4**

<img src="screenshots/task4-curl.png" alt="cURL HTTP response header output" width="850">

*Figure 4. HTTP response headers. Replace with your own screenshot.*

## 8. Task 5 – WAFW00F: WAF detection

**Objective:** Check for indications of a Web Application Firewall.

```bash
wafw00f networkwalks.com | tee outputs/task5-wafw00f.txt
```

**Interpretation:** The PDF reports ModSecurity (SpiderLabs) as a detected WAF. WAF fingerprinting is heuristic: detection can be inaccurate, and an undetected WAF may still exist. Report the tool's actual wording and avoid claiming certainty beyond the evidence.

**Security relevance:** A WAF can help filter malicious requests but does not replace secure coding, patch management, and monitoring. Do not attempt to evade or bypass WAF controls.

**Evidence – Task 5**

<img src="screenshots/task5-wafw00f.png" alt="WAFW00F firewall detection results" width="850">

*Figure 5. WAF detection output. Replace with your own screenshot.*

## 9. Task 6 – DNSRecon: DNS record enumeration

**Objective:** Enumerate publicly available DNS information, including NS, MX, TXT/SPF, and available SRV records.

```bash
dnsrecon -d networkwalks.com | tee outputs/task6-dnsrecon.txt
```

**Interpretation:** The training PDF describes mail records, SPF configuration, cPanel-related service records, and a BIND 9.16.23 version indicator. These are examples from the PDF and should not be assumed to reflect the current environment. DNS enumeration results depend on the records exposed and the tool's query behavior.

**Security relevance:** Review unnecessary public service records, confirm SPF/DKIM/DMARC policies, and avoid exposing internal naming conventions. DNS records alone do not prove exploitability.

**Evidence – Task 6**

<img src="screenshots/task6-dnsrecon.png" alt="DNSRecon DNS record enumeration results" width="850">

*Figure 6. DNS record enumeration. Replace with your own screenshot.*

## 10. Findings summary

The following are **illustrative observations reported by the supplied training PDF**, not confirmed results of a new scan.

| Area | PDF example | Validation / defensive action |
|---|---|---|
| Domain registration | Name servers associated with HostGator | Verify current records; secure registrar account |
| Web stack | WordPress 7.0.4; WP Download Manager 3.3.58 | Verify versions; review patches and exposure |
| DNS resolution | `192.232.216.135` | Check current DNS and intended exposure |
| HTTP headers | WordPress REST API path `/wp-json/` mentioned | Review response headers and endpoint access |
| WAF | ModSecurity (SpiderLabs) reported | Confirm WAF coverage and monitoring |
| DNS footprint | BIND 9.16.23 indicator, SPF and cPanel-related records | Review DNS hygiene and mail authentication |

### Your verified observations

Complete this table using **your own** command output before submitting the repository.

| Task | Date/time (timezone) | Observed result | Evidence file | Status |
|---|---|---|---|---|
| WHOIS | TBD | TBD | `outputs/task1-whois.txt` | Not yet documented |
| WhatWeb | TBD | TBD | `outputs/task2-whatweb.txt` | Not yet documented |
| Nslookup | TBD | TBD | `outputs/task3-nslookup.txt` | Not yet documented |
| cURL | TBD | TBD | `outputs/task4-curl.txt` | Not yet documented |
| WAFW00F | TBD | TBD | `outputs/task5-wafw00f.txt` | Not yet documented |
| DNSRecon | TBD | TBD | `outputs/task6-dnsrecon.txt` | Not yet documented |

## 11. Lessons learned

This project demonstrates how six different utilities provide complementary views of a public-facing website. WHOIS and DNS tools reveal registration and naming infrastructure; WhatWeb and cURL expose application-level signals; and WAFW00F provides an indication of protective controls. Combining these observations supports a better understanding of the externally visible attack surface and informs defensive hardening.

**Important distinction:** WHOIS queries are typically registry lookups, while tools such as WhatWeb, cURL, WAFW00F, and DNSRecon may send requests to the target or its infrastructure. Therefore, this exercise should not be described as entirely passive or undetectable.

## 12. Repository layout

```text
networkwalks-week2-recon/
├── README.md
├── screenshots/
│   ├── task1-whois.png
│   ├── task2-whatweb.png
│   ├── task3-nslookup.png
│   ├── task4-curl.png
│   ├── task5-wafw00f.png
│   └── task6-dnsrecon.png
└── outputs/
    ├── task1-whois.txt
    ├── task2-whatweb.txt
    ├── task3-nslookup.txt
    ├── task4-curl.txt
    ├── task5-wafw00f.txt
    └── task6-dnsrecon.txt
```

> Screenshot and output paths are placeholders until you add your own files. Remove credentials, tokens, personal information, and sensitive details before committing evidence.

## 13. Submission checklist

- [ ] Confirm written authorization and target scope.
- [ ] Execute each of the six tasks in the authorized environment.
- [ ] Save original terminal output for each task.
- [ ] Capture six legible screenshots.
- [ ] Name screenshots exactly as shown in the repository layout.
- [ ] Replace all `TBD` findings with verified results and timestamps.
- [ ] Review screenshots and logs for sensitive data before uploading.
- [ ] Commit `README.md`, screenshots, and approved evidence to GitHub.

## 14. Reference and acknowledgment

**Primary source:** Networkwalks, *W2-PM1 – Week 2, Project Module 1: Footprinting & Reconnaissance Attacks with Multiple Kali Tools*, supplied training PDF, pp. 1–10.  
**Training provider:** [Networkwalks](https://www.networkwalks.com/)  

Documentation prepared by **Ezekiel Arom** for educational internship reporting. The descriptions of the PDF's sample findings are attributed to the training material; they are not presented as independently verified live results.
