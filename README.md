# parsemony
A secure, FastAPI-backed engine that converts web DOM structures into persistent component registries for offline Playwright script generation.

---

## 🚀 Overview

Brittle test automation often fails because scripts are tightly coupled to live web scraping. **Parsemony** solves this by introducing a **database-first architecture**: 
1. **Ingest Once:** A headless browser sweeps a target URL *once* to parse and serialize interactive elements into a local registry (`parsemony.db`).
2. **Offline Compile:** Test scripts are compiled directly from stored database records rather than live web scraping.
3. **Non-Repudiable Audit Logging:** Every transaction is permanently tracked in an immutable audit ledger (`audit.db`) protected by database triggers.

---

## 🛠️ Architecture & Tech Stack

* **Backend:** FastAPI (Python), Pydantic
* **Automation Engine:** Playwright (Python)
* **Persistence Layer:** SQLAlchemy ORM with Dual SQLite Databases
  * `parsemony.db`: Stores URLs, discovered elements, and compiled test scripts.
  * `audit.db`: Append-only immutable transaction log (enforced via SQLite `BEFORE UPDATE OR DELETE` triggers).
* **Frontend:** HTML5, Tailwind CSS, Vanilla JavaScript (Single-Page Dashboard with High-Contrast Controls & Toast Notifications)

---

## ✨ Key Features

* **Deterministic DOM Inspector:** Selects a preset URL or safely inputs an enforced **HTTPS-only** custom target, parsing inputs, buttons, and textareas deterministically.
* **Smart Caching & Force Refresh:** Instant cache-hits pull historical element footprints without redundant browser sweeps, toggleable via a force-refresh option.
* **Immutable Audit Pipeline:** Every `DOM_SCAN` and `SCRIPT_COMPILATION` generates a unique transaction UUID (`transaction_id`), a local UTC+8 timestamp (`Asia/Manila`), and a direct relational foreign key (`page_id`) linking back to the element registry.
* **Context-Aware Script Compiler:** Dynamically maps stored selectors into clean, executable Python Playwright scripts (complete with randomized user credential pooling for test sites like SauceDemo).

---

## 📂 Project Structure

parsemony/
│
├── main-mvp1-25.py      # FastAPI application core & automation engine
├── database_4.py        # SQLAlchemy models, dual DB configs, & immutable triggers
├── parsemony_ui_5.py    # Responsive Tailwind dashboard template
├── parsemony.db         # Persistent element registry (Auto-generated)
└── audit.db             # Append-only immutable transaction ledger (Auto-generated)

⚙️ Getting Started
Prerequisites:
	Python 3.10+
	Playwright for Python

Installation & Setup

1. Clone the repository:

Bash
git clone [https://github.com/your-username/parsemony.git](https://github.com/your-username/parsemony.git)
cd parsemony

2. Install dependencies:

Bash
pip install fastapi uvicorn playwright sqlalchemy pydantic httpx
playwright install chromium

3. Run the application:

Bash
uvicorn main-mvp1-25:app --reload

4. Access the Dashboard:
Open your browser and navigate to:

http://127.0.0.1:8000

🔍 How to Verify the Audit Trail
You can inspect the tamper-proof audit ledger directly using Python to verify that transactions are successfully recorded in audit.db:

Python
---
import sqlite3

conn = sqlite3.connect("audit.db")
cursor = conn.cursor()

cursor.execute("SELECT timestamp, transaction_id, page_id, action_type, target_url FROM audit_trail;")
for row in cursor.fetchall():
    print(f"[{row[0]}] Tx: {row[1][:6]}... | PageID: #{row[2]} | {row[3]} -> {row[4]}")

conn.close()
---

🛡️ License
Distributed under the MIT License. See LICENSE for more information.
