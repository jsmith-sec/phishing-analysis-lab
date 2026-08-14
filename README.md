<div align="center">

# 🦠 Malware Analysis Lab
### Static & Dynamic Analysis of RATs & Ransomware

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3500&pause=800&color=2F81F7&center=true&vCenter=true&width=690&lines=Static+%26+Dynamic+Malware+Analysis;RustyStealer+%7C+AsyncRAT+%7C+Babuk+Ransomware;IOC+Extraction+%26+Capability+Mapping" alt="typing summary" />

<p>
  <img src="https://img.shields.io/badge/Type-Malware%20Analysis%20%2F+Blue%20Team-0A2A66?style=for-the-badge" alt="type" />
  <img src="https://img.shields.io/badge/Samples-3%20Analyzed-2F81F7?style=for-the-badge" alt="samples" />
  <a href="malware_analysis_lab_report.pdf"><img src="https://img.shields.io/badge/Full%20Report-PDF-0A2A66?style=for-the-badge&logo=adobeacrobatreader&logoColor=white" alt="full report" /></a>
</p>

<p>
  <img src="https://img.shields.io/badge/PEStudio-2F81F7?style=flat-square" alt="pestudio" />
  <img src="https://img.shields.io/badge/Sysinternals%20ProcMon-2F81F7?style=flat-square" alt="procmon" />
  <img src="https://img.shields.io/badge/Windows%2011%20ARM64-0078D6?style=flat-square&logo=windows&logoColor=white" alt="windows" />
  <img src="https://img.shields.io/badge/UTM%20%2F%20QEMU-2F81F7?style=flat-square" alt="utm" />
</p>

</div>

Static and dynamic analysis of three malware samples in an isolated Windows 11 ARM64 lab environment. This is Lab 5 in a series of home SOC labs built on Apple Silicon.

## Lab Environment

| Component | Details |
|---|---|
| Host | Apple Mac Mini M4, 32GB RAM, macOS |
| Virtualization | UTM (QEMU) |
| Analysis VM | Windows 11 ARM64 Home, 8GB RAM |
| Network | Host-Only (fully isolated) |
| Static Analysis | PEStudio 9.61 |
| Process Monitoring | Process Monitor (Sysinternals) |

Defender was disabled via registry with Tamper Protection off. The Host-Only adapter prevents any outbound connections, ensuring no live C2 communication during testing.

---

## Samples

### Sample 1: RustyStealer

| Field | Value |
|---|---|
| SHA256 | `7de487e28c9dee3ceb40fec5aca690e2128514ce26929e46fa5442f76ab2439a` |
| MD5 | `ab1f8537f6160dbcda6a5d30edb09885` |
| Original Filename | `2026.04.30板员名单及补偿方案WPS.exe` (Chinese HR document lure) |
| Embedded Name | `StoreInstaller.exe` |
| Size | 707,584 bytes |
| Entropy | 7.118 |
| Architecture | x64 |
| Compiled | 2026-04-30 |
| Tags | RustyStealer, SilverFox, ValleyRAT |
| Detections | 10/70+ |

**Capabilities confirmed via static analysis:**
- Credential theft
- Process injection (`WriteProcessMemory`, `VirtualAllocEx`)
- Service-based persistence (`OpenSCManagerW`, `StartServiceW`)
- Encrypted exfiltration (`BCryptGenRandom`)
- Multi-layer sandbox and debugger evasion (`NtQueryInformationProcess`, `SleepEx`, PDH queries)

**Dropped payload:** `SSMaHtDqksB.exe`  
**Mutex/C2 identifier:** `ZILkwgfk`

**Dynamic analysis:** The sample detected the analysis environment and self-terminated before performing any observable actions. This is consistent with published behavior for the SilverFox/RustyStealer family and is itself a documented finding confirming the evasion capability identified in static analysis.

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
