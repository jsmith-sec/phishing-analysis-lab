# Phishing Analysis Lab

A hands-on phishing URL analysis lab using three open-source intelligence tools — PhishTank, VirusTotal, and MXToolbox — to investigate verified phishing URLs, identify indicators of compromise, and compare detection capabilities across tools.

## Overview

Three verified phishing URLs were sourced from PhishTank and analyzed to identify attacker infrastructure, hosting details, email authentication failures, and campaign patterns. The lab demonstrates two distinct phishing techniques and highlights why no single tool provides complete coverage.

**Tools:** PhishTank, VirusTotal, MXToolbox  
**Samples:** 3 verified phishing URLs  
**Targets Impersonated:** T-Mobile, Ledger Hardware Wallet (x2)  
**Host:** Apple Mac Mini M4, macOS

---

## Samples Analyzed

| Sample | URL | Target | Technique |
|--------|-----|--------|-----------|
| 1 | https://t-mobile.ygbhd.top/ | T-Mobile | Subdomain spoofing via throwaway domain |
| 2 | https://studio-desktop.wixstudio.com/live | Ledger Wallet | Legitimate platform abuse (Wix) |
| 3 | https://desktop-platforms.wixstudio.com/desktop | Ledger Wallet | Legitimate platform abuse (Wix) |

---

## Key Findings

### Sample 1 — T-Mobile Subdomain Spoofing
- Domain `ygbhd.top` registered 4 days before detection, hosted on Alibaba Cloud (China)
- Only 4/92 VirusTotal vendors detected it — new infrastructure evades most signature-based tools
- Zero email infrastructure (no MX, SPF, or DMARC records) — throwaway domain confirmed
- WHOIS fully redacted, registered through Gname.com — commonly abused registrar

### Samples 2 & 3 — Coordinated Ledger Wallet Campaign
- Both pages impersonate Ledger Live desktop application targeting cryptocurrency users
- Hosted on Wix's legitimate platform — domain reputation checks show clean results
- Both served from the same IP (34.144.206.118) — confirmed coordinated campaign
- 16-18/92 vendor detection rate — content analysis required, domain analysis insufficient
- Sample 3 CNAME revealed attacker's Wix account identifier (`username.wix.com`) — actionable for takedown

---

## Tool Comparison

| Tool | Best For | Limitation |
|------|----------|------------|
| PhishTank | WHOIS, hosting, network data, community verification | Limited data on platform-hosted phishing |
| VirusTotal | Multi-vendor detection, HTML metadata, hosting IP | Low detection rate on new infrastructure |
| MXToolbox | DNS, SPF, DMARC, CNAME analysis | Cannot detect content-level phishing on legitimate platforms |

**Key takeaway:** No single tool caught everything. Each tool provided unique intelligence not available in the others — multi-tool analysis is required for comprehensive phishing investigation.

---

## Detection Comparison

| Sample | VirusTotal Score | Domain Age | Detectable via Domain Analysis? |
|--------|-----------------|------------|--------------------------------|
| Sample 1 | 4/92 (4%) | 4 days | Yes — throwaway domain with no infrastructure |
| Sample 2 | 18/92 (20%) | N/A (Wix) | No — requires content inspection |
| Sample 3 | 16/92 (17%) | N/A (Wix) | No — requires content inspection |

---

## Screenshots

### Sample 1 — T-Mobile

![Sample 1 PhishTank](sample1_phishtank.png)
![Sample 1 VirusTotal Detection](sample1_virustotal_detection.png)
![Sample 1 VirusTotal Details](sample1_virustotal_details.png)
![Sample 1 MXToolbox](sample1_mxtoolbox.png)

### Sample 2 — Ledger Wallet (Wix)

![Sample 2 PhishTank](sample2_phishtank.png)
![Sample 2 VirusTotal Detection](sample2_virustotal_detection.png)
![Sample 2 VirusTotal Details](sample2_virustotal_details_1.png)
![Sample 2 VirusTotal Details](sample2_virustotal_details_2.png)
![Sample 2 MXToolbox](sample2_mxtoolbox.png)

### Sample 3 — Ledger Wallet (Wix Campaign Confirmation)

![Sample 3 PhishTank](sample3_phishtank.png)
![Sample 3 VirusTotal Detection](sample3_virustotal_detection.png)
![Sample 3 VirusTotal Details](sample3_virustotal_details_1.png)
![Sample 3 VirusTotal Details](sample3_virustotal_details_2.png)
![Sample 3 MXToolbox](sample3_mxtoolbox.png)

---

## Full Report

See [phishing_analysis_lab_report.pdf](phishing_analysis_lab_report.pdf) for the complete write-up including IOC tables, tool comparison analysis, campaign correlation findings, and detection recommendations.

---

## Tools Used

- PhishTank (Cisco Talos) — phishing URL database and verification
- VirusTotal — multi-vendor URL threat intelligence
- MXToolbox SuperTool — DNS, SPF, DMARC, and CNAME analysis

---

## Other Labs in This Series

| Lab | Topic | Repo |
|---|---|---|
| Lab 1 | SOC/SIEM Detection | [soc-home-lab](https://github.com/jsmith-sec/soc-home-lab) |
| Lab 2 | Incident Response Simulation | [incident-response-lab](https://github.com/jsmith-sec/incident-response-lab) |
| Lab 3 | Web Application Attack | [web-app-attack-lab](https://github.com/jsmith-sec/web-app-attack-lab) |
| Lab 4 | Vulnerability Assessment | [vulnerability-assessment-lab](https://github.com/jsmith-sec/vulnerability-assessment-lab) |
| Lab 5 | Malware Analysis | [malware-analysis-lab](https://github.com/jsmith-sec/malware-analysis-lab) |
| Lab 6 | Phishing Analysis | This repo |
