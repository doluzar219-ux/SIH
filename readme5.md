Ye le bhai, tera final GitHub-ready `README.md`. Maine content ka ek bhi word change nahi kiya hai, bas jahan backticks (``) toot gaye the, wahan proper Markdown code blocks (`````) laga diye hain taaki diagrams aur code ekdum neat and clean render hon.

Seedha copy kar aur apne repo mein paste maar de:

```markdown
# ⚡ ForensiQ — Digital Forensics & Data Sanitization Platform
### `SIH26149` · Smart India Hackathon · Enterprise Forensics Suite

---

```text
███████╗ ██████╗ ██████╗ ███████╗███╗   ██╗███████╗██╗ ██████╗
██╔════╝██╔═══██╗██╔══██╗██╔════╝████╗  ██║██╔════╝██║██╔═══██╗
█████╗  ██║   ██║██████╔╝█████╗  ██╔██╗ ██║███████╗██║██║   ██║
██╔══╝  ██║   ██║██╔══██╗██╔══╝  ██║╚██╗██║╚════██║██║██║▄▄ ██║
██║     ╚██████╔╝██║  ██║███████╗██║ ╚████║███████║██║╚██████╔╝
╚═╝      ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝╚══════╝╚═╝ ╚══▀▀═╝

```

> **"Don't just delete — destroy. Don't just scan — prove."**
> ForensiQ is a **production-grade, real-time digital forensics and data sanitization platform** built for law enforcement agencies, corporate IT security teams, and government forensic labs. Every byte scanned, every file carved, every wipe executed is cryptographically logged and court-admissible.

---

## 📋 Table of Contents

1. [What is ForensiQ?](https://www.google.com/search?q=%23-what-is-forensiq)
2. [Live Architecture](https://www.google.com/search?q=%23-live-architecture)
3. [All 6 Modules Explained](https://www.google.com/search?q=%23-all-6-modules-explained)
4. [Full Backend API Reference](https://www.google.com/search?q=%23-full-backend-api-reference)
5. [Frontend Component Guide](https://www.google.com/search?q=%23-frontend-component-guide)
6. [Backend Modules Deep Dive](https://www.google.com/search?q=%23-backend-modules-deep-dive)
7. [Database & Audit Chain](https://www.google.com/search?q=%23-database--audit-chain)
8. [Getting Started](https://www.google.com/search?q=%23-getting-started)
9. [Environment Variables](https://www.google.com/search?q=%23-environment-variables)
10. [Tech Stack](https://www.google.com/search?q=%23-tech-stack)
11. [Security Standards Compliance](https://www.google.com/search?q=%23-security-standards-compliance)
12. [Project Structure](https://www.google.com/search?q=%23-project-structure)

---

## 🔍 What is ForensiQ?

ForensiQ is a **full-stack desktop & web forensic investigation platform** that runs a **Python FastAPI backend** (port `8000`) paired with a **React + Vite** cyberpunk-themed frontend (port `3000`).

It is designed to:

* 🔬 **Carve and recover** deleted files from storage media using real binary signature detection
* 🧹 **Securely wipe** drives and files using military-grade sanitization standards (NIST, DoD, Gutmann)
* 🛡️ **Scan carved payloads** for malware — ransomware, reverse shells, webshells, and hidden executables
* 🔌 **Extract USB history** directly from the Windows Registry with real device serial numbers
* 📄 **Generate court-admissible PDF reports** with Gemini AI executive summaries embedded
* 🔗 **Maintain a tamper-evident blockchain-style audit ledger** with SHA-256 hash chain validation

---

## 🏗️ Live Architecture

ForensiQ operates on a **Dual-Process Isolation Model** to ensure that UI crashes never interrupt low-level raw disk operations. Instead of slow HTTP REST calls for binary data, we utilize **Windows Named Pipes** for zero-overhead, RAM-to-RAM binary streaming.

```text
┌──────────────────────────────────────────────────────────┐
│               BROWSER  http://localhost:3000             │
│         React 19 + Vite 6.4 + TailwindCSS 4              │
└──────────────────────┬───────────────────────────────────┘
                       │  axios REST API (Lightweight JSON)
                       ▼
┌──────────────────────────────────────────────────────────┐
│         FastAPI Gateway   http://127.0.0.1:8000          │
│          (Session State, AI Routing, Dashboard)          │
└──────────────────────┬───────────────────────────────────┘
                       │  IPC Named Pipes (\\.\pipe\ForensiQ_Engine)
                       │  Zero HTTP Overhead / Process Isolation
                       ▼
┌──────────────────────────────────────────────────────────┐
│  forensiq_core.exe (Elevated Native Engine / Privileged) │
│                                                          │
│  [ Hardware ATA/NVMe Purge ]  [ MFT/VSS Ghost Protocol ] │
│  [ Shannon Entropy Carver  ]  [ In-RAM YARA Sandbox    ] │
│                                                          │
│  ┌──────────────────────────────────────────────────┐    │
│  │  vibe_shield.db  (SQLite Blockchain Ledger)      │    │
│  │  SHA-256(prev_hash || payload_hash) per block    │    │
│  └──────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────┘

```

---

## 🧩 All 6 Modules Explained

### 🖥️ Module 0 — Target Devices Dashboard

Real-time system drive enumeration and health monitoring.

| What it does | How |
| --- | --- |
| Lists all mounted drives (C:, D:, USB etc.) | `psutil.disk_partitions()` queries OS disk table |
| Shows used/free space and filesystem type | `shutil.disk_usage()` for real byte counts |
| Computes drive SHA-256 cryptographic fingerprint | `hashlib.sha256()` rolling block digest |
| Auto-polls health every 15 seconds | Frontend `setInterval(loadData, 15000)` |
| Launches wipe workflow from drive card | Connects to Module 1 (Secure Eraser) |

---

### ✂️ Copy-Paste Snippet 2: Replace your `🧹 Module 1 — Secure Storage Eraser` Section

*Apne File Carving (Module 3) se theek pehle jo Module 1 ka table hai, usko isse replace kar de. Isme **Wear-Leveling Bypass** aur **Ghost Protocol (MFT/VSS)** aa gaya hai.*

### 🧹 Module 1 — Secure Storage Eraser & Ghost Protocol

Military-grade data sanitization with court-admissible audit logging. Standard software tools fail on modern SSDs due to Wear-Leveling and Flash Translation Layers (FTL). ForensiQ bypasses the OS and sends direct hardware instructions.

| Algorithm | Standard / Target | Execution Methodology |
| --- | --- | --- |
| `NIST_800_88_CLEAR` | NIST SP 800-88 Rev.1 | 1-pass logical overwrite for general storage |
| `DOD_5220_22_M` | DoD 5220.22-M | 3-Pass ECE (0x00 → 0xFF → Random) for HDDs |
| `ATA_SECURE_ERASE` | SSD / NVMe Bypass | Issues direct `IOCTL_ATA_PASS_THROUGH` to storage controllers to flush over-provisioned NAND cells instantly. |

**The Ghost Protocol (Zero-Trace Metadata Wipe):**
Overwriting sectors isn't enough. ForensiQ actively hunts and destroys forensic breadcrumbs:

* 👻 **MFT Slack Wiping:** Scrubs residual file names, timestamps, and cluster pointers from the NTFS Master File Table (`$MFT`).
* 👻 **VSS Purging:** Executes `vssadmin` hooks to destroy Windows Volume Shadow Copies, eliminating historical system restore snapshots.

**Key Functions:**

* `detect_storage_type(path)` — Auto-detects SSD vs HDD for dynamic algorithm switching.
* `execute_hardware_purge()` — Bypasses FTL for solid-state media.
* `wipe_storage_drive(drive_id, algorithm)` — Volume-level sanitization with SHA-256 pre/post verification.

---

### 🔍 Module 3 — File Carving & Evidence Recovery

Recovers deleted/fragmented files from raw storage by scanning binary magic byte signatures.

**How File Carving Works:**

```text
[Raw Disk / File Stream]
       │
       ▼
 Scan byte-by-byte for known file magic headers
 JPEG: 0xFF 0xD8 0xFF  |  PNG: 0x89 0x50 0x4E 0x47
 PDF:  0x25 0x50 0x44  |  ZIP: 0x50 0x4B 0x03 0x04
       │
       ▼
 Found header → read forward until matching footer
       │
       ▼
 Compute SHA-256 fingerprint + Shannon entropy H(X)
       │
       ▼
 Confidence Score = Header(+35%) + Footer(+35%) + Entropy(+15%)
       │
       ▼
 Classify: RECOVERED (≥60%) or FRAGMENTED (<60%)

```

**Supported File Types:** JPEG, PNG, PDF, ZIP, MP4, AVI, DOCX, XLSX, SQLite, EXE/PE, MP3, GIF, BMP, PST, and more.

**Shannon Entropy Formula:**

H(X) = -∑ p(x) · log₂(p(x))

0.0 – 2.0  →  Text / plain data
5.0 – 7.5  →  Normal binary files
7.75 – 8.0 →  Encrypted / suspicious

---

### 🛡️ Module 3B — YARA Malware & Threat Scanner

Scans carved evidence payloads for active malware and threat indicators.

| Threat | Detection | Severity |
| --- | --- | --- |
| **Ransomware** | `vssadmin delete shadows`, `.locked` headers, ransom notes | 🔴 HIGH |
| **Reverse Shell** | `nc -e /bin/sh`, PowerShell `-encodedcommand`, Python sockets | 🔴 HIGH |
| **Webshell** | `c99shell`, `r57shell`, `eval($_POST[`, `system($_GET` | 🔴 HIGH |
| **Hidden Executable** | `MZ` (PE), `\x7FELF` embedded in non-exe files | 🟡 MEDIUM |

`scan_payload(file_bytes)` → returns `{ is_malicious, matched_rules, threat_level, findings, sha256 }`

Uses compiled `yara-python` if available; auto-falls back to Python `re` regex engine.

---

### 🔌 Module 6 — USB Registry Artifact Extractor

Extracts the full history of ALL USB devices ever connected to this Windows machine from the Registry.

**Registry Keys Queried:**

```text
HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR    ← USB Mass Storage tree
HKLM\SYSTEM\CurrentControlSet\Enum\USB        ← All USB device classes

```

**For each device, ForensiQ extracts:**

* ✅ Device Friendly Name (e.g. "SanDisk Cruzer Blade")
* ✅ Hardware Serial Number (e.g. `03003221080323194704&0`)
* ✅ Vendor ID (VID) and Product ID (PID)
* ✅ Last Connected Timestamp (Windows FILETIME → UTC)
* ✅ Assigned Drive Letter (E:, F:, etc.)
* ✅ Current Connection Status (MOUNTED / DISCONNECTED)
* ✅ Full Registry Path for forensic chain of custody

---

### 📄 Module 4+5 — Sensitivity Classifier & Legal PDF Report Generator

**Module 4 — ML Sensitivity Classifier:**

| Level | Score Range | Triggered By |
| --- | --- | --- |
| `PUBLIC` | 0–20 | No sensitive patterns found |
| `INTERNAL` | 20–45 | HR records, salary, audit trail keywords |
| `CONFIDENTIAL` | 45–75 | Financial PAN/SSN, corporate NDA/trade secret |
| `TOP-SECRET` | 75–100 | `CLASSIFIED`, `LAW_ENFORCEMENT`, crypto private keys |

**Module 5 — Cryptographic Audit Chain:**

Every forensic operation is logged as a SHA-256 hash-linked block:

```text
Block[N].chain_hash = SHA-256( Block[N-1].chain_hash || Block[N].sha256_hash )

```

The genesis block is anchored with 64 zero bytes. Any tampering with a past record causes every subsequent chain hash to fail — mathematically proving tampering.

**Gemini AI Executive Summary:**
Calls `google-genai` Gemini 2.5 Flash to synthesize a 4-sentence executive cyber incident summary from audit data, embedded directly into the generated PDF certificate.

---

## 📡 Full Backend API Reference

| Method | Endpoint | Module | Description |
| --- | --- | --- | --- |
| `GET` | `/api/health` | Core | System health check + uptime |
| `GET` | `/api/devices` | M0 | List all real mounted system drives |
| `POST` | `/api/hash` | M0 | Compute SHA-256 hash of any file |
| `POST` | `/api/erase/file` | M1 | Securely wipe a specific file |
| `POST` | `/api/erase/drive` | M1 | Full volume sanitization of a drive |
| `POST` | `/api/carve/start` | M3 | Start file carving job on a path |
| `GET` | `/api/carve/status/{job_id}` | M3 | Poll carving job progress |
| `GET` | `/api/carve/results` | M3 | Get all recovered evidence items |
| `POST` | `/api/carve/export` | M3 | Export recovered file payload to disk |
| `POST` | `/api/scan/malware` | M3B | YARA scan a base64-encoded file payload |
| `POST` | `/api/classify` | M4 | ML sensitivity classification |
| `GET` | `/api/audit/logs` | M5 | Retrieve all audit chain log records |
| `GET` | `/api/audit/verify` | M5 | Verify entire SHA-256 chain integrity |
| `POST` | `/api/reports/generate` | M5 | Generate court-ready PDF report |
| `GET` | `/api/ai/summary` | M5 | Gemini AI executive case summary |
| `GET` | `/api/forensics/usb_history` | M6 | Windows Registry USB artifact history |

---

## 🖥️ Frontend Component Guide

| Component | File | What it Renders |
| --- | --- | --- |
| **App** | `src/App.jsx` | Root component, 5-tab routing, audit log modal state |
| **Sidebar** | `Sidebar.jsx` | Cyberpunk vertical nav with active tab indicators |
| **Dashboard** | `Dashboard.jsx` | Live drive cards, SHA-256 hash verifier, health pulse |
| **SecureEraser** | `SecureEraser.jsx` | Algorithm picker, live terminal log stream, pre/post hash |
| **FileCarving** | `FileCarving.jsx` | Evidence table, YARA `MALWARE DETECTED` badges, export |
| **UsbArtifacts** | `UsbArtifacts.jsx` | USB device table from registry, serial copy, CSV export |
| **AuditReports** | `AuditReports.jsx` | Blockchain ledger, hash visualizer, AI summary, PDF gen |
| **AuditLogsModal** | `AuditLogsModal.jsx` | Slide-in modal showing last 100 audit chain entries |
| **SanitizeModal** | `SanitizeModal.jsx` | Wipe confirmation dialog with danger UX patterns |
| **api.js** | `src/api.js` | All axios HTTP calls to FastAPI backend |

---

## 🐍 Backend Modules Deep Dive

### `forensiq_core.exe` — Native Execution Engine

* Runs as a standalone, UAC-elevated privileged binary.
* Communicates directly with the FastAPI gateway via **Windows Named Pipes** (`\\.\pipe\ForensiQ_Engine`).
* Handles all heavy-lifting: Raw physical disk stream reading, `DeviceIoControl` hardware hooks for ATA wiping, and In-RAM YARA threat scanning.

### `main.py` — FastAPI Application Server

* FastAPI v1.6.0 acting as the communication gateway.
* Manages React UI sessions, handles Named Pipe IPC routing to the Core Engine, and polls system health.

### `main.py` — FastAPI Application Server

* FastAPI v1.6.0 with CORS middleware for React dev server
* Registers all route handlers across 6 modules
* Uses `psutil.disk_partitions()` + `shutil.disk_usage()` to enumerate **real system drives**

### `database.py` — SQLite Audit Ledger

* Creates `vibe_shield.db` on startup
* Table: `audit_logs` with `id`, `timestamp`, `operation_type`, `target_path`, `algorithm`, `sha256_hash`, `chain_hash`, `status`
* `chain_hash` maintains blockchain-like immutability

### `modules/audit.py`

* `log_audit_event()` — Writes a new block to the chain
* `get_audit_history()` — Returns all log records sorted by ID
* `verify_chain_integrity()` — Re-computes every chain hash and detects tampering
* `compute_file_sha256()` — SHA-256 digest of any file

### `modules/carving.py`

* `scan_and_carve_evidence(target_path, selected_file_types)` — Main carving engine
* Walks directory tree or reads raw file stream
* Searches binary buffers for 15+ file type magic byte signatures
* Computes real SHA-256 + Shannon entropy per recovered fragment
* Calculates confidence score (0–100%) from header/footer/entropy matrix


* `get_all_carved_items()` — Returns current evidence index (auto-scans on first call)
* `export_carved_file_to_disk()` — Safely dumps recovered payload bytes to disk

### `modules/eraser.py`

* `detect_storage_type(path)` — Returns `SSD` or `HDD`
* `overwrite_and_verify_file(path, algorithm)` — File-level secure deletion with pre/post SHA-256 comparison
* `wipe_storage_drive(drive_id, algorithm)` — Drive-level block overwriting

### `modules/malware_scanner.py`

* `scan_payload(file_bytes)` — Core threat analysis
* Tries native `yara-python` library
* Falls back to Python `re` byte pattern matching
* Detects Ransomware, Reverse Shells, Webshells, Hidden Executables
* Returns `HIGH` / `MEDIUM` / `CLEAN` threat level



### `modules/classifier.py`

* `classify_evidence_content(payload_bytes, metadata)` — ML heuristic sensitivity classifier
* Shannon entropy calculation
* Executable/database binary signature checks
* Regex pattern scanning (TOP-SECRET → CONFIDENTIAL → INTERNAL → PUBLIC)
* Returns weighted confidence score + sensitivity label + risk flags



### `modules/reporting.py`

* `generate_gemini_case_summary(audit_data)` — Calls Gemini 2.5 Flash to synthesize executive summary
* `generate_forensic_pdf_report(params)` — Builds binary PDF 1.4 certificate with SHA-256 chain attestation
* `verify_chain_integrity()` — Mathematically re-validates the entire audit ledger

### `modules/usb_forensics.py`

* `get_usb_forensic_history()` — Queries Windows Registry via Python `winreg`
* Opens `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR`
* Enumerates all device class keys and serial number subkeys
* Extracts FriendlyName, DeviceDesc, Last Write Time (Windows FILETIME → UTC datetime)



---

## 🗄️ Database & Audit Chain

**File:** `backend/vibe_shield.db` (SQLite 3)

```sql
CREATE TABLE audit_logs (
    id              INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp       TEXT NOT NULL,
    operation_type  TEXT NOT NULL,
    target_path     TEXT,
    algorithm       TEXT,
    sha256_hash     TEXT NOT NULL,
    chain_hash      TEXT NOT NULL,
    status          TEXT
);

```

**Blockchain Chain Integrity Formula:**

```text
genesis_hash   = "0000...0000"  (64 zeros)
block_1.chain  = SHA-256("0000...0000" || block_1.sha256)
block_2.chain  = SHA-256(block_1.chain || block_2.sha256)
block_N.chain  = SHA-256(block_N-1.chain || block_N.sha256)

```

If ANY past record is modified → ALL subsequent chain_hash values fail → tamper proven.

---

## 🚀 Getting Started

### Prerequisites

* Python 3.11+ (tested on 3.14.0)
* Node.js 18+
* Windows 10/11 (USB Registry; other features are cross-platform)

### 1. Clone & Install Frontend

```bash
git clone https://github.com/YourOrg/ForensiQ.git
cd ForensiQ
npm install

```

### 2. Install Backend Dependencies

```bash
pip install fastapi>=0.141.0 uvicorn[standard] pydantic>=2.6.0 python-multipart psutil
# Optional for YARA:
pip install yara-python
# Optional for Gemini AI PDF summaries:
pip install google-genai

```

### 3. Configure Environment

```bash
cp .env.example .env
# Add your GEMINI_API_KEY in .env

```

### 4. Start Backend

```bash
python backend/main.py
# API available at: http://127.0.0.1:8000
# Swagger Docs:     http://127.0.0.1:8000/docs

```

### 5. Start Frontend

```bash
# Windows-safe (handles & in folder name):
node node_modules/vite/bin/vite.js --port=3000 --host=0.0.0.0
# Open: http://localhost:3000

```

---

## 🔑 Environment Variables

| Variable | Required | Description |
| --- | --- | --- |
| `GEMINI_API_KEY` | Optional | Google AI API key for Gemini 2.5 Flash AI summaries in PDF reports |

Without `GEMINI_API_KEY`, the system generates a heuristic executive summary as fallback — all other features work fully offline.

---

## 🛠️ Tech Stack

### Backend

| Package | Version | Role |
| --- | --- | --- |
| `FastAPI` | ≥0.141.1 | REST API framework |
| `Uvicorn` | ≥0.28.0 | ASGI production server |
| `Pydantic` | ≥2.6.0 | Request/response validation |
| `psutil` | latest | Real system drive enumeration |
| `yara-python` | optional | Native YARA rule engine |
| `google-genai` | optional | Gemini AI API SDK |
| `winreg` | stdlib | Windows Registry USB extraction |
| `hashlib` | stdlib | SHA-256 cryptographic hashing |
| `sqlite3` | stdlib | Audit chain database |

### Frontend

| Package | Version | Role |
| --- | --- | --- |
| `React` | 19.0.1 | UI framework |
| `Vite` | 6.4.3 | Ultra-fast dev server + bundler |
| `lucide-react` | 0.546.0 | Icon library |
| `axios` | 1.20.0 | HTTP client |
| `@google/genai` | 2.4.0 | Gemini SDK |
| `TailwindCSS` | 4.1.14 | Utility CSS framework |
| `motion` | 12.23.24 | Animation library |

---

## 🛡️ Security Standards Compliance

| Standard | Module | Coverage |
| --- | --- | --- |
| **NIST SP 800-88 Rev. 1** | Module 1 | Clear + Purge sanitization methods |
| **DoD 5220.22-M** | Module 1 | 3-Pass ECE (0x00 / 0xFF / Random) |
| **Gutmann 35-Pass** | Module 1 | Legacy magnetic media destruction |
| **SHA-256 Cryptographic Hashing** | All | Pre/post-wipe evidence fingerprinting |
| **ISO/IEC 27037:2012** | Modules 3, 5 | Digital evidence identification and preservation |
| **YARA Threat Intelligence** | Module 3B | IOC signature-based malware detection |

---

## 📁 Project Structure

```text
ForensiQ/
├── backend/
│   ├── main.py                  ← FastAPI server, all API routes (v1.6.0)
│   ├── database.py              ← SQLite audit chain initialization
│   ├── vibe_shield.db           ← Live blockchain-linked audit ledger
│   ├── requirements.txt         ← Python dependencies
│   └── modules/
│       ├── audit.py             ← SHA-256 chain logging + verification
│       ├── carving.py           ← Binary signature file carving engine
│       ├── classifier.py        ← ML sensitivity heuristic classifier
│       ├── eraser.py            ← NIST/DoD/Gutmann secure erasure
│       ├── malware_scanner.py   ← YARA + regex threat detection
│       ├── reconstruction.py    ← Fragment reconstruction from partial carves
│       ├── reporting.py         ← Gemini AI PDF report generator
│       └── usb_forensics.py     ← Windows Registry USB history extractor
├── src/
│   ├── App.jsx                  ← Root component, tab routing
│   ├── api.js                   ← All axios API call bindings
│   ├── main.jsx                 ← React entry point
│   ├── index.css                ← Global CSS + cyberpunk theme
│   └── components/
│       ├── Sidebar.jsx          ← Navigation sidebar (6 tabs)
│       ├── Dashboard.jsx        ← Live drive cards + hash verifier
│       ├── SecureEraser.jsx     ← Wipe engine UI + terminal stream
│       ├── FileCarving.jsx      ← Carving table + malware badges
│       ├── UsbArtifacts.jsx     ← USB registry history table
│       ├── AuditReports.jsx     ← Crypto chain + AI summary + PDF gen
│       ├── AuditLogsModal.jsx   ← Slide-in audit log modal
│       └── SanitizeModal.jsx    ← Wipe confirmation dialog
├── public/
│   └── electron.js              ← Electron desktop wrapper entry point
├── index.html                   ← Root HTML shell
├── vite.config.js               ← Vite configuration
├── tailwind.config.js           ← Tailwind theme
├── package.json                 ← Node.js dependencies + scripts
└── .env                         ← GEMINI_API_KEY (not committed to git)

```

---

## 👥 Submission Info

| Field | Value |
| --- | --- |
| **Problem Statement** | SIH26149 — Digital Forensics & Data Sanitization |
| **Edition** | Smart India Hackathon 2026 |
| **Category** | Software · Cybersecurity & Forensics |
| **Backend Version** | v1.6.0 |
| **License** | MIT |

---

> **⚠️ Legal Disclaimer:** ForensiQ is intended for authorized forensic investigations, IT security audits, and certified data destruction workflows only. Unauthorized use of forensics tools without explicit legal authorization is illegal. Always obtain proper written authorization before executing any forensic or erasure operation.

---

*Built with ❤️ and `0xFF` bytes.*

```



```
