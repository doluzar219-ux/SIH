Samajh gaya bhai! Slide-vlide ki baat bilkul nahi karenge.

Niche **ForensiQ (SIH26149)** project ki **100% complete technical knowledge bank, decisions, implementation details, algorithms, aur deep system internals** ka poora dossier hai. Isme shuru se abhi tak ke saare finalized decisions ek jagah structured tareeke se hain.

---

# 🛡️ FORENSIQ (SIH26149) — COMPLETE MASTER TECHNICAL DOSSIER

---

## 1. Project Overview & Foundational Architecture

### Core Purpose

ForensiQ ek enterprise-grade digital forensics, malware inspection, data sanitization, aur automated legal report generation suite hai. Ye non-recoverable data destruction (sanitization) aur deep forensic recovery ko ek unified zero-trust system me joda gaya hai.

### System Stack & Technology Choices

* **Frontend UI:** React 19 + Vite 6.4 + TailwindCSS (Electron Shell Wrapper for Native Windows Windowing).
* **Backend Gateway:** FastAPI (Python 3.12) + Uvicorn + Pydantic v2.
* **Core Low-Level Execution Engine:** Native C++17 / Compiled Python (`forensiq_core.exe`) with Administrator UAC Elevation.
* **Inter-Process Communication (IPC):** Windows Named Pipes (`\\.\pipe\ForensiQ_Engine`).
* **Audit Ledger:** SQLite Database with SHA-256 Cryptographic Hash Chaining.
* **AI Subsystem:** Google Gemini 2.5 Flash API + Local Heuristic Fallback Engine (Air-Gapped Mode).
* **Malware Inspection:** In-RAM YARA Engine (`yara-python`).

### Architecture Model: Dual-Process Isolation

System ko do isolated processes me baanta gaya hai:

1. **User Mode (UI + Gateway):** React UI aur FastAPI normal user context me chalte hain. Real-time monitoring, state management, aur web-socket event streaming sambhalte hain.
2. **Elevated Native Mode (Core Engine):** `forensiq_core.exe` Administrative privileges (`CreateFileW`, `DeviceIoControl`) ke sath chalta hai. Raw physical disk blocks, kernel registers, aur memory buffers se direct deal karta hai.

---

## 2. Low-Level IPC Engine Mechanics (Windows Named Pipes)

Normal web APIs (HTTP REST) raw disk sectors aur heavy binary payload ke liye slow aur laggy hoti hain. Isliye system-level communication ke liye **Windows Named Pipes** implement kiya gaya hai.

### IPC Specifications

* **Pipe Name:** `\\.\pipe\ForensiQ_Engine`
* **Access Mode:** `PIPE_ACCESS_DUPLEX` (Bi-directional binary stream)
* **Pipe Type:** `PIPE_TYPE_MESSAGE` | `PIPE_READMODE_MESSAGE` | `PIPE_WAIT`
* **Buffer Allocation:** 64 KB (65,536 Bytes) input/output ring buffers.
* **Security Context:** Client validation ke liye `Administrator` privileges aur `SE_MANAGE_VOLUME_NAME` rights required hain.

### Key Architectural Benefits

* **Zero HTTP Overhead:** Data direct RAM-to-RAM binary buffers me transfer hota hai, JSON parsing ka compute waste nahi hota.
* **Process Isolation:** Agar React UI ya FastAPI gateway crash bhi ho jaye, background me `forensiq_core.exe` raw disk operations ko bina corrupt kiye safely execute karta rehta hai.
* **Privilege Separation:** UI ko Admin privileges ki zaroorat nahi hoti; sirf IPC core `.exe` UAC elevation maangta hai.

---

## 3. Storage Architecture & Advanced Data Sanitization Engine

Standard tools (jaise DBAN) modern SSDs par fail ho jate hain kyunki SSDs me Flash Translation Layer (FTL) aur Wear-Leveling algorithms hote hain jo logical writes ko over-provisioned NAND blocks par redirect kar dete hain. ForensiQ hardware-aware sanitization logic use karta hai:

### A. Media Detection & Strategy Switch

* System physical geometry Query karta hai (`SetupDiGetClassDevs`, `DeviceIoControl`).
* Storage media identify hoti hai: **HDD (Magnetic)** vs **SATA/NVMe SSD (Solid State)** vs **USB (Removable)**.

### B. HDD Wiping Standards (Magnetic Media)

* **DoD 5220.22-M:**
* Pass 1: Fixed character overwrite (`0x00`)
* Pass 2: Complement character overwrite (`0xFF`)
* Pass 3: Pseudo-random byte stream + verification pass.


* **NIST SP 800-88 Rev 1 (Clear/Purge):** Single/Multi-pass zero-fill with cryptographic pattern verification.

### C. SSD & NVMe Wear-Leveling Bypass

* **ATA Secure Erase & NVMe Crypto Purge:** FTL overlay ko bypass karne ke liye storage controller ko direct low-level commands bheje jate hain (`IOCTL_ATA_PASS_THROUGH`, `NVME_CLI_SANITIZE`).
* Controller chip ko internal voltage surge ya cryptographic key dump trigger karne ka instruction milta hai, jisse over-provisioned blocks samet saare NAND flash cells 2-3 seconds me permanently zero-out ho jate hain.

### D. The Ghost Protocol (Zero-Trace Metadata & Backup Wiping)

1. **NTFS MFT Slack Wiping:** Sector overwrite karne ke baad **NTFS Master File Table ($MFT)** ke empty record slots aur headers ko scrub kiya jata hai taaki file ka Naam, Creation Date, aur Cluster Pointers forensic tools ko na milein.
2. **Volume Shadow Copy (VSS) Purging:** Windows dwara piche banaye gaye historical restore points ko destroy karne ke liye `vssadmin delete shadows /all /quiet` hooks execute kiye jate hain.

---

## 4. Forensic File Carving & Malware Sandbox Engine

### A. Magic Byte Extraction

Disk ke unallocated clusters ko stream karke raw file signatures match kiye jate hain:

* **JPEG:** `FF D8 FF E0`
* **PDF:** `25 50 44 46`
* **ZIP / DOCX / XLSX:** `50 4B 03 04`
* **PNG:** `89 50 4E 47`

### B. Shannon Entropy Heuristics ($H(X)$)

Data randomness measure karne ke liye Shannon Entropy formula apply kiya jata hai:

$$H(X) = -\sum_{i=1}^{n} P(x_i) \log_2 P(x_i)$$

* **$H(X) < 3.5$:** Plain text, code, ya zero-filled empty sectors.
* **$3.5 \le H(X) \le 6.8$:** Standard executables, uncompressed media, normal documents.
* **$H(X) > 7.5$:** AES Encrypted files, BitLocker blocks, compressed archives, ya obfuscated malware payloads.

### C. Heuristic Fragment Stitching

Agar fragmented disk par file ke tukde alag-alag clusters par bikhre ho:

1. Engine magic bytes se start boundary pakadta hai.
2. Fragment gaps par **Shannon Entropy transition rate** calculate karta hai.
3. Logical structure continuity aur byte distribution curves ko analyze karke non-contiguous clusters ko aapas me stitch (re-assemble) karta hai.

### D. In-RAM YARA Threat Inspection

Carve ki gayi file ko direct investigator ke hard drive par save karne ke bajaye:

1. Extracted bytes ko virtual memory sandbox buffer (`VirtualAlloc`) me hold kiya jata hai.
2. Custom **YARA Rulesets** ke dwara memory me hi Ransomware signatures, WebShells, Reverse Shell payloads, aur Macro Viruses ke liye scan kiya jata hai.
3. Safe hone par hi disk par dump kiya jata hai; flag hone par warning tag ke sath isolate kar diya jata hai.

---

## 5. Cryptographic Chain-of-Custody Audit Ledger

Forensic investigation me court evidence ki validity sabse critical hoti hai. Plain text log files ko court me reject kar diya jata hai kyunki unhe edit kiya ja sakta hai.

### Implementation: SHA-256 Hash Chaining

Har system event (Drive connected, Wipe started, File carved, Malware detected) SQLite database me ek cryptographic block banata hai:

$$\text{Block}_n.\text{hash} = \text{SHA256}(\text{Block}_{n-1}.\text{hash} \parallel \text{Timestamp} \parallel \text{ActionPayload})$$

* **Tamper Evidence:** Agar kisi operator ya insider ne log file me 1 byte ya 1 timestamp bhi badla, toh pure database ki cryptographic validation chain break ho jayegi.
* **Court Validity:** Court me exact mathematical proof diya ja sakta hai ki acquisition se lekar report tak evidence tampering-free raha.

---

## 6. System Artifacts, Registry & Live Telemetry

* **USBSTOR Registry Extractor:** Windows Registry (`HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR` & `MountedDevices`) parse karke past me connect hue USB drives ke Vendor ID (VID), Product ID (PID), Serial Numbers, First Connected Timestamp, aur Mounted Drive Letters extract kiye jate hain.
* **Browser History Extractor:** Chromium-based browsers (Chrome/Edge) ke `%LocalAppData%` path se SQLite history DB parse karke recent URLs aur Download logs nikaale jate hain.
* **Live System & Network Forensic Monitor:** `psutil` ke through open network sockets (`psutil.net_connections`), listening ports, aur running process executable paths (`psutil.process_iter`) ka real-time telemetry feed dashboard par dikhaya jata hai.

---

## 7. AI Automated Reporting & Air-Gapped High-Security Engine

### A. Gemini 2.5 Flash Automated Report

* Investigation ke end me saare audit logs, USB registry history, YARA threat detections, aur drive wipe verification blocks JSON payload banakar **Gemini 2.5 Flash API** ko bheje jate hain.
* AI natural language me ek chronological Incident Executive Summary likhta hai.
* System automatically ReportLab/PyPDF module se ek court-admissible PDF report export karta hai jisme verification signatures aur cryptographic hash blocks embedded hote hain.

### B. Air-Gapped / High-Security Offline Mode Switch

* Defense/Military forensic labs me Internet connectivity prohibited hoti hai.
* Frontend header me ek toggle hai: **`Air-Gapped Mode: ACTIVE`**.
* Toggle **ON** hone par Cloud API calls 100% bypass ho jati hain aur system local rule-based heuristic template engine se offline PDF court report generate kar deta hai.

---

## 8. Standards & Compliance Mapping

System ko international regulatory standards ke exact specifications par design kiya gaya hai:

| Standard | Description / Implementation |
| --- | --- |
| **NIST SP 800-88 Rev 1** | Guidelines for Media Sanitization (Clear vs Purge operational states). |
| **DoD 5220.22-M** | National Industrial Security Program Operating Manual (3-pass magnetic wipe). |
| **ISO/IEC 27037:2012** | Digital evidence identification, collection, acquisition, and preservation guidelines. |
| **NIST SP 800-86** | Guide to Integrating Forensic Techniques into Incident Response. |

---

Ye hai **ForensiQ** project ki complete, end-to-end technical identity aur decisions. Isme project ka har single component, logic, formula, aur architectural decision fully documented hai!
