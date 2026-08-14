<div align="center">

# 🎣 Phishing Analysis Lab
### OSINT URL & Email Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3500&pause=800&color=2F81F7&center=true&vCenter=true&width=690&lines=Phishing+URL+%26+Email+Analysis;PhishTank+%7C+VirusTotal+%7C+MXToolbox+%7C+PhishTool;IOC+Identification+%26+Campaign+Correlation" alt="typing summary" />

<p>
  <img src="https://img.shields.io/badge/Type-Phishing%20Analysis%20%2F+Blue%20Team-0A2A66?style=for-the-badge" alt="type" />
  <img src="https://img.shields.io/badge/Samples-3%20URLs%20%2B%202%20Emails-2F81F7?style=for-the-badge" alt="samples" />
  <a href="phishing_analysis_lab_report.pdf"><img src="https://img.shields.io/badge/Full%20Report-PDF-0A2A66?style=for-the-badge&logo=adobeacrobatreader&logoColor=white" alt="full report" /></a>
</p>

<p>
  <img src="https://img.shields.io/badge/PhishTank-2F81F7?style=flat-square" alt="phishtank" />
  <img src="https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white" alt="virustotal" />
  <img src="https://img.shields.io/badge/MXToolbox-2F81F7?style=flat-square" alt="mxtoolbox" />
  <img src="https://img.shields.io/badge/PhishTool-2F81F7?style=flat-square" alt="phishtool" />
</p>

</div>

A hands-on phishing URL analysis lab using three open-source intelligence tools — PhishTank, VirusTotal, and MXToolbox — to investigate verified phishing URLs, identify indicators of compromise, and compare detection capabilities across tools.

## Overview

Three verified phishing URLs were sourced from PhishTank and analyzed to identify attacker infrastructure, hosting details, email authentication failures, and campaign patterns. The lab demonstrates two distinct phishing techniques and highlights why no single tool provides complete coverage.

**Tools:** PhishTank, VirusTotal, MXToolbox, PhishTool  
**Samples:** 3 verified phishing URLs + 2 phishing emails  
**Targets Impersonated:** T-Mobile, Ledger Hardware Wallet (x2), Stellar Foundation, Celsius Network  
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

<img src="sample1_phishtank.png" width="720" alt="Sample 1 PhishTank">

*PhishTank record confirming the T-Mobile spoofing URL and its hosting details.*

<img src="sample1_virustotal_detection.png" width="720" alt="Sample 1 VirusTotal Detection">

*VirusTotal — only 4/92 vendors flag the newly registered domain.*

<img src="sample1_virustotal_details.png" width="720" alt="Sample 1 VirusTotal Details">

*VirusTotal details showing hosting on Alibaba Cloud and domain metadata.*

<img src="sample1_mxtoolbox.png" width="720" alt="Sample 1 MXToolbox">

*MXToolbox confirms no MX/SPF/DMARC records — a throwaway domain.*

### Samples 2 & 3 — Coordinated Ledger Wallet Campaign

- Both pages impersonate Ledger Live desktop application targeting cryptocurrency users
- Hosted on Wix's legitimate platform — domain reputation checks show clean results
- Both served from the same IP (34.144.206.118) — confirmed coordinated campaign
- 16-18/92 vendor detection rate — content analysis required, domain analysis insufficient
- Sample 3 CNAME revealed attacker's Wix account identifier (`username.wix.com`) — actionable for takedown

**Sample 2**

<img src="sample2_phishtank.png" width="720" alt="Sample 2 PhishTank">

*PhishTank record for the first Wix-hosted Ledger phishing page.*

<img src="sample2_virustotal_detection.png" width="720" alt="Sample 2 VirusTotal Detection">

*VirusTotal detection — 18/92 vendors, requiring content-level analysis.*

<img src="sample2_virustotal_details_1.png" width="720" alt="Sample 2 VirusTotal Details">

*VirusTotal details showing the shared hosting IP.*

<img src="sample2_virustotal_details_2.png" width="720" alt="Sample 2 VirusTotal Details">

*Additional VirusTotal metadata for Sample 2.*

<img src="sample2_mxtoolbox.png" width="720" alt="Sample 2 MXToolbox">

*MXToolbox DNS/CNAME analysis for the Wix-hosted domain.*

**Sample 3**

<img src="sample3_phishtank.png" width="720" alt="Sample 3 PhishTank">

*PhishTank record for the second Wix-hosted Ledger phishing page.*

<img src="sample3_virustotal_detection.png" width="720" alt="Sample 3 VirusTotal Detection">

*VirusTotal detection — 16/92 vendors on the same coordinated campaign.*

<img src="sample3_virustotal_details_1.png" width="720" alt="Sample 3 VirusTotal Details">

*VirusTotal details confirming the shared hosting IP (34.144.206.118).*

<img src="sample3_virustotal_details_2.png" width="720" alt="Sample 3 VirusTotal Details">

*Additional VirusTotal metadata for Sample 3.*

<img src="sample3_mxtoolbox.png" width="720" alt="Sample 3 MXToolbox">

*MXToolbox CNAME analysis revealing the attacker's Wix account identifier — actionable for takedown.*

---

## Email Samples Analyzed (PhishTool)

Two phishing emails from the PhishingPot repository were analyzed using PhishTool for header analysis, authentication checks, and URL extraction.

| Email | Subject | Impersonated Brand | Sending Domain | Technique |
|-------|---------|-------------------|----------------|-----------|
| 1 | How to become a staker | Stellar Foundation | ayurmithrawellness.com | Compromised domain, double redirect (Google + bit.ly) |
| 2 | Reminder: Withdraw your funds today | Celsius Network | mshtavr.com | Amazon SES abuse, urgency tactics, legitimate link padding |

### Key Email Findings

- Both emails target cryptocurrency users — consistent with the Ledger wallet phishing URLs
- Both sent via Amazon SES — legitimate cloud email infrastructure abused for deliverability
- Neither email passes SPF, DKIM, or DMARC — strict enforcement would have blocked both
- Both use URL obfuscation and include legitimate domain references to add credibility
- Email 2 exploits the real Celsius bankruptcy to target genuine victims waiting for fund recovery

**Email 1 — Stellar Foundation Staking Scam**

<img src="email1_phishtool_details.png" width="720" alt="Email 1 PhishTool Details">

*PhishTool header analysis of the Stellar Foundation staking scam.*

<img src="email1_phishtool_authentication.png" width="720" alt="Email 1 PhishTool Authentication">

*Authentication results — SPF, DKIM, and DMARC all fail.*

<img src="email1_phishtool_urls.png" width="720" alt="Email 1 PhishTool URLs">

*Extracted URLs revealing the Google + bit.ly double redirect.*

**Email 2 — Celsius Network Claims Scam**

<img src="email2_phishtool_details.png" width="720" alt="Email 2 PhishTool Details">

*PhishTool header analysis of the Celsius Network claims scam.*

<img src="email2_phishtool_authentication.png" width="720" alt="Email 2 PhishTool Authentication">

*Authentication results — sent via Amazon SES, failing SPF/DKIM/DMARC.*

<img src="email2_phishtool_urls.png" width="720" alt="Email 2 PhishTool URLs">

*Extracted URLs showing obfuscation and legitimate-link padding.*

---

## Tool Comparison

| Tool | Best For | Limitation |
|------|----------|------------|
| PhishTank | WHOIS, hosting, network data, community verification | Limited data on platform-hosted phishing |
| VirusTotal | Multi-vendor detection, HTML metadata, hosting IP | Low detection rate on new infrastructure |
| MXToolbox | DNS, SPF, DMARC, CNAME analysis | Cannot detect content-level phishing on legitimate platforms |
| PhishTool | Email header analysis, authentication checks, URL extraction | Designed for .eml files, limited URL-only analysis |

**Key takeaway:** No single tool caught everything. Each tool provided unique intelligence not available in the others — multi-tool analysis is required for comprehensive phishing investigation.

---

## Detection Comparison

| Sample | VirusTotal Score | Domain Age | Detectable via Domain Analysis? |
|--------|-----------------|------------|--------------------------------|
| Sample 1 | 4/92 (4%) | 4 days | Yes — throwaway domain with no infrastructure |
| Sample 2 | 18/92 (20%) | N/A (Wix) | No — requires content inspection |
| Sample 3 | 16/92 (17%) | N/A (Wix) | No — requires content inspection |

---

## Full Report

See [phishing_analysis_lab_report.pdf](phishing_analysis_lab_report.pdf) for the complete write-up including IOC tables, tool comparison analysis, email header analysis, campaign correlation findings, and detection recommendations.

---

## Tools Used

- PhishTank (Cisco Talos) — phishing URL database and verification
- VirusTotal — multi-vendor URL threat intelligence
- MXToolbox SuperTool — DNS, SPF, DMARC, and CNAME analysis
- PhishTool — email header analysis, authentication checks, and URL extraction
- PhishingPot (GitHub) — open-source phishing email samples

---

## Other Labs in This Series

| Lab | Topic | Repo |
|---|---|---|
| Lab 1 | SOC/SIEM Detection | [soc-siem-lab](https://github.com/jsmith-sec/soc-siem-lab) |
| Lab 2 | Incident Response Simulation | [incident-response-lab](https://github.com/jsmith-sec/incident-response-lab) |
| Lab 3 | Web Application Attack | [web-app-attack-lab](https://github.com/jsmith-sec/web-app-attack-lab) |
| Lab 4 | Vulnerability Assessment | [vulnerability-assessment-lab](https://github.com/jsmith-sec/vulnerability-assessment-lab) |
| Lab 5 | Malware Analysis | [malware-analysis-lab](https://github.com/jsmith-sec/malware-analysis-lab) |
| Lab 6 | Phishing Analysis | This repo |
| Lab 7 | Active Directory Attack | [active-directory-lab](https://github.com/jsmith-sec/active-directory-lab) |
<img src="rustystealer_pestudio_overview.png" width="720" alt="RustyStealer Overview">

*PEStudio overview — file metadata, entropy, and detection summary.*

<img src="rustystealer_pestudio_imports.png" width="720" alt="RustyStealer Imports">

*Flagged imports revealing process injection, persistence, and crypto APIs.*

<img src="rustystealer_pestudio_indicators.png" width="720" alt="RustyStealer Indicators">

*PEStudio indicators highlighting suspicious and blacklisted behaviors.*

<img src="rustystealer_pestudio_strings.png" width="720" alt="RustyStealer Strings">

*Extracted strings, including the dropped payload and mutex identifier.*

---

### Sample 2: AsyncRAT

| Field | Value |
|---|---|
| SHA256 | `6d23eb561ad602dd178fd4c0fdc63d145df645ff0dd68d8ce123dc868ff29f65` |
| Original Filename | `Stub.exe` (AsyncRAT builder output) |
| Size | 250,880 bytes |
| Architecture | 32-bit .NET |
| Compiled | 2026-04-30 |
| Assembly Name | `gRGBVPLJAmCHSNT` |
| Tags | AsyncRAT |

**Capabilities confirmed via .NET method analysis:**
- Encrypted C2 communication over TCP/SSL (`get_TcpClient`, `get_SslClient`)
- DNS resolution to locate C2 server (`GetHostAddresses`, `CheckHostName`)
- Registry persistence (`CreateSubKey`, `OpenSubKey`, `DeleteSubKeyTree`)
- Configurable heartbeat beacon (`get_ActivatePong`, `get_Interval`)
- Anti-analysis (`CheckRemoteDebuggerPresent`)
- Marks process as critical to prevent termination (`RtlProcessIsCritical`)

**Dynamic analysis:** The sample executed successfully and persisted as a background process (19.8MB memory). No outbound network activity was observable due to the Host-Only network configuration preventing any C2 connection. Process Monitor kernel driver was blocked by Windows 11 ARM64 HVCI enforcement — a known compatibility limitation documented in the lab report.

<img src="asyncrat_pestudio_overview.png" width="720" alt="AsyncRAT Overview">

*PEStudio overview of the .NET stub binary.*

<img src="asyncrat_pestudio_imports.png" width="720" alt="AsyncRAT Imports">

*.NET method references exposing C2, DNS, and persistence behavior.*

<img src="asyncrat_pestudio_indicators.png" width="720" alt="AsyncRAT Indicators">

*Indicators flagging anti-analysis and process-protection techniques.*

<img src="asyncrat_pestudio_strings.png" width="720" alt="AsyncRAT Strings">

*Extracted strings showing the obfuscated assembly name and config artifacts.*

---

### Sample 3: Babuk Ransomware

| Field | Value |
|---|---|
| SHA256 | `c94a81fdf688d220827320e88cc0b89af8690142abe5c602131b6659297c7d24` |
| MD5 | `75a6690d9a4a89bd0cf6ceebcffd3c41` |
| Size | 315,904 bytes |
| Architecture | 32-bit native (x86) |
| Compiler | Microsoft Visual C++ 6.0 - 8.0 |
| Compiled | 2020-01-06 |
| Export Name | `zasawuheb.exe` |
| Entropy | 6.647 |
| First Seen | 2022-04-10 |
| Detections | 15/70+ |
| Tags | Babuk, Ransomware |

**Capabilities confirmed via static analysis:**
- File encryption workflow (`CopyFileExW`, `WriteFile`, `DeleteFileW`)
- Directory traversal to locate targets (`CreateDirectoryExA`, `SearchPathA`)
- Inter-process communication (`CreateNamedPipeA/W`, `CallNamedPipeA`)
- Active user session targeting (`WTSGetActiveConsoleSessionId`)
- System modification (`SetComputerNameW`, `SetVolumeLabelW`)
- Anti-debugging (`DebugBreak`, `OutputDebugStringA/W`)

**Dynamic analysis:** Execution was deliberately deferred. Babuk is functional ransomware that encrypts files system-wide. Running it without a clean VM snapshot would destroy the analysis environment. A VM snapshot restore workflow is required before executing this sample.

<img src="babuk_pestudio_overview.png" width="720" alt="Babuk Overview">

*PEStudio overview of the native x86 Babuk binary.*

<img src="babuk_pestudio_imports.png" width="720" alt="Babuk Imports">

*Imports revealing the file-encryption and directory-traversal workflow.*

<img src="babuk_pestudio_indicators.png" width="720" alt="Babuk Indicators">

*Indicators flagging anti-debugging and system-modification behavior.*

<img src="babuk_pestudio_strings.png" width="720" alt="Babuk Strings">

*Extracted strings from the ransomware binary.*

---

## Lab Report

Full write-up including import analysis tables, findings summary, IOC table, and lessons learned is in `malware_analysis_lab_report.pdf`.

---

## Key Takeaways

- Native malware and .NET malware require different static analysis approaches. PE import analysis works well for native binaries like RustyStealer and Babuk. For .NET samples like AsyncRAT, .NET method names and string extraction are more informative than the PE import table.
- Evasion behavior is a finding, not a failure. RustyStealer's self-termination in the analysis environment confirmed the sandbox detection capabilities identified in static analysis and is consistent with published threat intelligence on the SilverFox family.
- ARM64 virtualization introduces tooling limitations. Process Monitor's kernel driver is incompatible with Windows 11 ARM64 HVCI enforcement. Future iterations of this lab will evaluate Sysmon as an alternative.

---

## Other Labs in This Series

| Lab | Topic | Repo |
|---|---|---|
| Lab 1 | SOC/SIEM Detection | [soc-siem-lab](https://github.com/jsmith-sec/soc-siem-lab) |
| Lab 2 | Incident Response Simulation | [incident-response-lab](https://github.com/jsmith-sec/incident-response-lab) |
| Lab 3 | Web Application Attack | [web-app-attack-lab](https://github.com/jsmith-sec/web-app-attack-lab) |
| Lab 4 | Vulnerability Assessment | [vulnerability-assessment-lab](https://github.com/jsmith-sec/vulnerability-assessment-lab) |
| Lab 5 | Malware Analysis | This repo |
| Lab 6 | Phishing Analysis | [phishing-analysis-lab](https://github.com/jsmith-sec/phishing-analysis-lab) |
| Lab 7 | Active Directory Attack | [active-directory-lab](https://github.com/jsmith-sec/active-directory-lab) |
