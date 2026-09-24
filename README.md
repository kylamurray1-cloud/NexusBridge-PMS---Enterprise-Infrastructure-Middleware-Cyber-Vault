# NexusBridge-PMS---Enterprise-Infrastructure-Middleware-Cyber-Vault
Cloud native FastAPI infrastructure middleware for legacy PMS integration featuring zero trust PCI credit card tokenization, dynamic fraud heuristic checks, and air-gapped cyber vault replication.


/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
## The Problem
Legacy Property Management Systems (PMS) in the hospitality industry rely on outdated, unencrypted data flows to receive bookings from third-party Online Travel Agencies (OTAs) like Expedia, Booking.com, and Agoda. 

When reservations stream in, raw payment card data (PAN) is frequently stored in cleartext across local operational databases. This exposes hotels to severe PCI-DSS compliance violations, payment fraud, and ransomware attacks that can wipe out active guest ledgers and cause operational shutdown.

## The Cause
* **Monolithic Legacy Architecture:** Most PMS platforms were designed decades ago and lack native microservices or modern API security layers.
* **Direct Channel Exposure:** OTA webhooks push booking payloads directly into operational databases without an intervening zero-trust filtering layer.
* **Unprotected Backup Loops:** Traditional night-audit database backups are stored on attached network drives, leaving them vulnerable to encryption during a ransomware event.

## The Solution
**NexusBridge PMS** acts as an isolated, cloud-native infrastructure middleware layer that intercepts incoming reservation payloads before they reach the core database:

* **Zero-Trust Tokenization:** Intercepts raw credit card numbers and converts them into secure cryptographic tokens (`TOK-XXXXXXXXXXXX`) using MD5/SHA-256 seed hashing.
* **Automated Fraud Heuristics:** Evaluates transaction risk in real time, instantly flagging high-value anomalies (e.g., charges over $5,000 flagged as `FLAGGED_HIGH_RISK`).
* **Air-Gapped Cyber Vault:** Provides a dedicated replication endpoint (`/api/v1/vault/replicate`) that creates isolated, immutable operational snapshots with automated CyberSense integrity validation.

## The Goal
To build a resilient, high-throughput integration bridge that secures guest financial data, eliminates raw payment exposure, prevents booking fraud, and guarantees business continuity and zero ransomware blast radius for hotel operations.

/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

## The Model 

## Technical Architecture & Data Flow
[ Third-Party OTAs ] ───> [ FastAPI Gateway Layer ] ───> [ Zero-Trust Token Engine ]
(Expedia, Booking)        (/api/v1/ota/ingest)           (Card Hashing & Fraud Rules)
│
▼
[ Air-Gapped Cyber Vault ] <─── [ Snapshot Replication ] <─── [ Operational Ledger ]
(100% CyberSense Score)          (/api/v1/vault/replicate)      (SQLite Database)

## System Verification & Proof of Concept

### Batch Ingestion & Fraud Pipeline Execution
Demonstration of batch reservation processing running via Python async requests, displaying real-time token generation and dynamic fraud filtering:

<img width="1012" height="991" alt="Screenshot 2026-09-24 041453" src="https://github.com/user-attachments/assets/2d4580a8-2590-499c-84e6-a92daa511ae3" />

DROP_TERMINAL_SCREENSHOT_HERE

### Real Time Operational Dashboard
High-contrast control center (`http://127.0.0.1:8000`) displaying live ingestion metrics, tokenized card ledgers, fraud status badges, and air-gap recovery state:

DROP_DASHBOARD_SCREENSHOT_HERE
<img width="1182" height="832" alt="Screenshot 2026-09-24 041546" src="https://github.com/user-attachments/assets/98457e31-57e5-4ddf-b140-0b26b1879714" />



## 7. Tech Stack

* **Language & Framework:** Python 3.13, FastAPI, Uvicorn
* **Database & Storage:** SQLite3 (Operational Ledger & Air-Gapped Vault Ledger)
* **Security & Cryptography:** `hashlib` (SHA-256, MD5), `secrets`
* **Testing & HTTP Ingestion:** Python `requests`
* **Frontend Control Panel:** HTML5, Modern CSS Grid/Flexbox, JavaScript Fetch API
