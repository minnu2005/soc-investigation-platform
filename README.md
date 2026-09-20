# 🛡️ AI-Assisted SOC Investigation & Security Incident Correlation Platform

> A rule-based Security Operations Center (SOC) platform for detecting, correlating, enriching, and investigating security events with AI-assisted analysis.


 **Overview**

Modern organizations generate large volumes of security events from firewalls, operating systems, authentication systems, web servers, intrusion detection systems, and other infrastructure.

These events are often:

* Generated in different formats
* Distributed across multiple systems
* Difficult to correlate
* High in volume
* Missing contextual information
* Time-consuming to investigate manually

A single attack may generate many seemingly unrelated events.

For example:

```text
Multiple Failed Logins
        ↓
Successful Login
        ↓
Suspicious Command Execution
        ↓
Outbound Network Connection
        ↓
Suspicious IOC
        ↓
Security Incident
```

The **AI-Assisted SOC Investigation & Security Incident Correlation Platform** is designed to connect these fragmented security events and transform them into meaningful security incidents.

The platform follows the pipeline:

```text
Ingest
  ↓
Parse
  ↓
Normalize
  ↓
Store
  ↓
Detect
  ↓
Correlate
  ↓
Create Incident
  ↓
Threat Intelligence
  ↓
MITRE ATT&CK
  ↓
Risk Assessment
  ↓
SOC Investigation
  ↓
AI Assistance
  ↓
Incident Report
```

---

# 🎯 Problem Statement

Security monitoring systems can generate thousands of alerts from different sources.

For example:

```text
Alert 1 → Failed Login
Alert 2 → Failed Login
Alert 3 → Failed Login
Alert 4 → Successful Login
Alert 5 → PowerShell Execution
Alert 6 → Network Connection
Alert 7 → Suspicious IP
```

Looking at these events individually makes it difficult for an analyst to understand the complete attack sequence.

The main problem addressed by this project is:

> **How can heterogeneous security events be automatically processed, correlated, enriched, and presented as meaningful security incidents to support SOC investigation?**

---

# 💡 Proposed Solution

The platform provides a centralized security event processing and investigation workflow.

```text
                 SECURITY DATA SOURCES
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       Linux          Windows        Firewall
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                  LOG INGESTION
                         ↓
                     PARSING
                         ↓
                   NORMALIZATION
                         ↓
                    PostgreSQL
                         ↓
                RULE-BASED DETECTION
                         ↓
                      ALERTS
                         ↓
                 CORRELATION ENGINE
                         ↓
                     INCIDENT
                    /        \
                   ↓          ↓
          THREAT INTEL    MITRE ATT&CK
                   \          /
                    \        /
                     ↓      ↓
                  RISK ANALYSIS
                         ↓
                   SOC DASHBOARD
                         ↓
                AI INVESTIGATION
                    ASSISTANT
                         ↓
                 INCIDENT REPORT
```

---

# ✨ Key Features

## 1. Multi-Source Log Ingestion

The system is designed to accept security events from multiple sources, including:

* Linux logs
* Windows security events
* Firewall logs
* Web server logs
* IDS/IPS alerts
* Synthetic security events
* Public security datasets

---

## 2. Log Parsing

Raw logs are converted into structured information.

Example:

### Raw Log

```text
Failed password for admin from 192.168.1.50
```

### Parsed Event

```text
Event Type : Authentication
Action     : Failed Login
Username   : admin
Source IP  : 192.168.1.50
```

Different log sources can have dedicated parsers.

```text
LinuxParser
WindowsParser
FirewallParser
SuricataParser
```

---

# 3. Event Normalization

Different security systems produce different event formats.

The normalization layer converts them into a common schema.

Example:

```json
{
  "timestamp": "2026-09-20T10:05:00",
  "source": "linux",
  "event_type": "authentication",
  "username": "admin",
  "source_ip": "192.168.1.50",
  "destination_ip": null,
  "hostname": "server01",
  "process": "sshd",
  "action": "failed_login",
  "raw_log": "Failed password for admin...",
  "normalized_data": {}
}
```

This allows the rest of the system to work with a standardized event representation.

---

# 4. Rule-Based Detection Engine

The detection engine identifies suspicious behavior using configurable security rules.

### Example — Brute Force

```text
IF

failed_login_count >= 5

AND

same source IP

AND

within 10 minutes

THEN

generate BRUTE_FORCE alert
```

### Example — Suspicious Authentication

```text
IF

multiple failed logins

AND

successful login

AND

same source IP

AND

within configured time window

THEN

generate SUSPICIOUS_LOGIN alert
```

### Example — Suspicious Command Execution

```text
IF

PowerShell execution

AND

suspicious outbound connection

THEN

generate SUSPICIOUS_EXECUTION alert
```

---

# 5. Event Correlation Engine

The correlation engine is the core component of the platform.

Instead of treating alerts independently, it identifies relationships between events.

Correlation can be based on:

* Source IP
* Destination IP
* Username
* Hostname
* Process
* Event type
* IOC
* Timestamp
* Configurable time windows

Example:

```text
Failed Login
     │
     ├── Same IP
     │
     ├── Same User
     │
     └── Same Time Window
             ↓
      Successful Login
             ↓
       PowerShell
             ↓
      Network Connection
             ↓
       IOC Detection
             ↓
        ONE INCIDENT
```

---

# 6. Incident Management

Related alerts are grouped into security incidents.

Example:

```text
Incident ID:
INC-0001

Title:
Possible Account Compromise

Severity:
HIGH

Confidence:
91%

Status:
OPEN

Source IP:
192.168.1.50

Affected User:
admin

First Seen:
10:01

Last Seen:
10:09
```

Associated evidence:

```text
✓ Multiple failed logins
✓ Successful authentication
✓ PowerShell execution
✓ Network connection
✓ Threat intelligence information
```

---

# 7. Threat Intelligence Enrichment

Suspicious indicators can be enriched using external threat intelligence sources.

Supported indicator types may include:

```text
IP Address
Domain
URL
File Hash
```

Example:

```text
Suspicious IP
     ↓
Threat Intelligence
     ↓
Reputation
ASN
Country
Known Associations
Historical Information
Confidence
```

Threat intelligence is treated as additional investigation context rather than automatically proving malicious activity.

---

# 8. MITRE ATT&CK Mapping

Observed security behavior can be mapped to MITRE ATT&CK techniques.

Example:

```text
Repeated Authentication Attempts
            ↓
T1110 — Brute Force
```

and:

```text
PowerShell
     ↓
T1059.001
Command and Scripting Interpreter:
PowerShell
```

An incident can therefore contain:

```text
MITRE ATT&CK

T1110
Brute Force

T1059.001
PowerShell
```

---

# 9. Risk Assessment

The platform uses a transparent, rule-based risk scoring mechanism.

Example:

```text
Multiple Failed Logins       +20
Successful Login             +20
Suspicious Process            +20
Threat Intelligence Match     +30
ATT&CK Behavior               +10
                              ----
                              100
```

Example output:

```text
Risk Score : 82/100
Severity   : HIGH
Confidence : 91%
```

The scoring weights will be configurable and documented as the implementation develops.

---

# 10. SOC Dashboard

The frontend provides a centralized SOC analyst interface.

Example:

```text
┌─────────────────────────────────────────────┐
│                 SOC DASHBOARD               │
├────────────┬────────────┬───────────────────┤
│ CRITICAL   │ HIGH       │ MEDIUM            │
│     2      │     7      │      15           │
├────────────┴────────────┴───────────────────┤
│ Recent Incidents                            │
│                                             │
│ INC-001  Possible Account Compromise  HIGH  │
│ INC-002  Brute Force                MEDIUM   │
│ INC-003  Suspicious PowerShell        HIGH   │
└─────────────────────────────────────────────┘
```

---

# 11. Incident Investigation

Each incident will have an investigation view containing:

### Incident Information

```text
Incident ID
Title
Severity
Confidence
Risk Score
Status
Affected User
Affected Host
Source IP
```

### Event Timeline

```text
10:01 ─ Failed Login
10:02 ─ Failed Login
10:03 ─ Failed Login
10:04 ─ Failed Login
10:05 ─ Successful Login
10:07 ─ PowerShell
10:08 ─ Network Connection
10:09 ─ IOC Match
```

### Evidence

```text
Detection Rule
Correlated Events
Source IP
Username
Hostname
Threat Intelligence
ATT&CK Techniques
Risk Factors
```

---

# 12. AI Investigation Assistant

The AI component is designed as an **analyst-assistance layer**.

It does not replace the detection engine.

The system first produces evidence:

```text
Events
  ↓
Alerts
  ↓
Correlation
  ↓
Incident
  ↓
Evidence
```

The AI then uses this information to assist the analyst.

Possible capabilities:

* Incident summarization
* Timeline explanation
* Evidence explanation
* Investigation questions
* Risk explanation
* ATT&CK explanation
* Incident report generation

Example:

### Analyst

```text
Why was this incident created?
```

### AI

```text
The incident was created because multiple failed
authentication attempts were followed by a successful
login from the same source IP within the configured
correlation window. PowerShell execution and subsequent
network activity were also associated with the same
investigation.
```

The AI should distinguish between:

```text
FACT
INFERENCE
UNKNOWN
```

This reduces the possibility of presenting unsupported conclusions as established evidence.

---

# 13. Incident Reporting

The platform can generate structured investigation reports containing:

```text
Incident ID
Incident Summary
Severity
Confidence
Risk Score
Affected Assets
Indicators
Event Timeline
Detection Rules
Correlation Evidence
Threat Intelligence
MITRE ATT&CK Techniques
Investigation Summary
Analyst Notes
Response Actions
```

---

# 14. Response Simulation

The platform may provide simulated response actions:

```text
[ Block IOC ]
[ Disable Account ]
[ Isolate Host ]
[ Escalate Incident ]
[ Generate Report ]
```

For the academic/demo environment, these actions are simulated rather than directly modifying production infrastructure.

Example:

```text
BLOCK IOC

IOC:
192.168.1.50

Status:
SIMULATED

No actual firewall modification performed.
```

---

# 🧠 Detection vs Correlation vs AI

A major design principle of this project is keeping these responsibilities separate.

| Component           | Responsibility                  |
| ------------------- | ------------------------------- |
| Parser              | Understand raw log format       |
| Normalizer          | Convert events to common schema |
| Detection Engine    | Identify suspicious patterns    |
| Correlation Engine  | Connect related events          |
| Incident Engine     | Create/manage incidents         |
| Threat Intelligence | Add external IOC context        |
| MITRE ATT&CK        | Classify observed behavior      |
| Risk Engine         | Calculate transparent risk      |
| AI Assistant        | Help analysts investigate       |
| Dashboard           | Present information             |

### Machine Learning

**Not required.**

The core platform is intentionally designed around deterministic security engineering:

```text
Rules
+
Correlation
+
Threat Intelligence
+
ATT&CK
+
Risk Scoring
+
AI Assistance
```

Machine learning can be added as a future enhancement if required.

---

# 🏗️ System Architecture

```text
┌───────────────────────────────────────────────┐
│                DATA SOURCES                   │
│                                               │
│ Linux | Windows | Firewall | IDS | Web Logs  │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│                INGESTION LAYER                │
│                                               │
│ API / File / JSON / Dataset Input            │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│               PROCESSING LAYER                │
│                                               │
│ Parser → Normalizer                           │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│                STORAGE LAYER                  │
│                                               │
│ PostgreSQL                                    │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│               SECURITY ENGINE                 │
│                                               │
│ Detection → Correlation → Incident Creation │
└───────────────────────┬───────────────────────┘
                        │
             ┌──────────┴───────────┐
             ▼                      ▼
   ┌──────────────────┐    ┌──────────────────┐
   │ Threat Intelligence│   │ MITRE ATT&CK    │
   └─────────┬────────┘    └────────┬─────────┘
             └──────────┬───────────┘
                        ▼
               ┌─────────────────┐
               │  Risk Engine    │
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │  SOC Dashboard  │
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ AI Investigation│
               │    Assistant    │
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ Incident Report │
               └─────────────────┘
```

---

# 📂 Project Structure

```text
soc-investigation-platform/
│
├── README.md
├── LICENSE
├── .gitignore
├── docker-compose.yml
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   │
│   │   ├── api/
│   │   │   ├── events.py
│   │   │   ├── alerts.py
│   │   │   ├── incidents.py
│   │   │   └── investigations.py
│   │   │
│   │   ├── models/
│   │   │   ├── event.py
│   │   │   ├── alert.py
│   │   │   └── incident.py
│   │   │
│   │   ├── services/
│   │   │   ├── parser.py
│   │   │   ├── normalizer.py
│   │   │   ├── detector.py
│   │   │   ├── correlator.py
│   │   │   ├── threat_intel.py
│   │   │   └── attack_mapper.py
│   │   │
│   │   └── core/
│   │       ├── config.py
│   │       └── database.py
│   │
│   ├── tests/
│   └── requirements.txt
│
├── frontend/
│   ├── app/
│   │   ├── dashboard/
│   │   ├── alerts/
│   │   ├── incidents/
│   │   ├── investigations/
│   │   └── reports/
│   │
│   ├── components/
│   └── lib/
│
├── detection-rules/
│   ├── authentication.yaml
│   ├── network.yaml
│   └── execution.yaml
│
├── datasets/
│   └── README.md
│
├── scripts/
│
├── docs/
│   ├── architecture.md
│   ├── detection.md
│   └── threat-model.md
│
└── .github/
    └── workflows/
```

---

# 🔄 Complete Data Flow

The complete flow of the platform is:

```text
1. SECURITY EVENT
        ↓
2. INGESTION
        ↓
3. PARSING
        ↓
4. NORMALIZATION
        ↓
5. DATABASE STORAGE
        ↓
6. RULE EVALUATION
        ↓
7. ALERT GENERATION
        ↓
8. EVENT CORRELATION
        ↓
9. INCIDENT CREATION
        ↓
10. THREAT INTELLIGENCE
        ↓
11. MITRE ATT&CK MAPPING
        ↓
12. RISK ASSESSMENT
        ↓
13. SOC DASHBOARD
        ↓
14. ANALYST INVESTIGATION
        ↓
15. AI ASSISTANCE
        ↓
16. INCIDENT REPORT
```

---

# 🧪 Example Investigation Scenario

Consider the following events:

```text
10:01  Failed login
10:02  Failed login
10:03  Failed login
10:04  Failed login
10:05  Successful login
10:07  PowerShell execution
10:08  External connection
10:09  Suspicious IOC
```

The system processes them as follows:

```text
Raw Events
    ↓
Parsed Events
    ↓
Normalized Events
    ↓
Stored in Database
    ↓
Detection Rules Triggered
    ↓
Alerts Generated
    ↓
Events Correlated
    ↓
Incident Created
```

The incident may contain:

```text
INC-0001
Possible Account Compromise

Severity: HIGH
Confidence: HIGH

Evidence:
✓ Multiple failed logins
✓ Successful login
✓ PowerShell execution
✓ External connection
✓ IOC information
```

ATT&CK mapping:

```text
T1110
Brute Force

T1059.001
PowerShell
```

The analyst can then inspect the complete timeline and ask the AI assistant to summarize the available evidence.

---

# 📊 Evaluation Metrics

The system will be evaluated using measurable cybersecurity and software-engineering metrics.

### Detection

* True Positives
* False Positives
* False Negatives
* Precision
* Recall
* F1-score

### Correlation

* Correctly correlated events
* Incorrect correlations
* Incident creation accuracy
* Correlation latency

### System Performance

* Events processed per second
* Average processing latency
* API response time
* Database query performance

### Investigation

* Number of alerts reduced into incidents
* Investigation workflow time
* Evidence completeness
* Report generation time

---

# 🛠️ Development Roadmap

## Phase 1 — Foundation

```text
✓ Repository
✓ Python environment
✓ FastAPI
✓ Database configuration
✓ Project structure
```

---

## Phase 2 — Event Pipeline

```text
Event Model
     ↓
Ingestion API
     ↓
Parser
     ↓
Normalizer
     ↓
Database
```

---

## Phase 3 — Detection

```text
Detection Rules
     ↓
Rule Engine
     ↓
Alert Generation
```

---

## Phase 4 — Correlation

```text
Events
  ↓
Correlation Rules
  ↓
Related Events
  ↓
Incident Creation
```

---

## Phase 5 — Security Enrichment

```text
Threat Intelligence
        +
MITRE ATT&CK
        +
Risk Scoring
```

---

## Phase 6 — SOC Dashboard

```text
Dashboard
Alerts
Incidents
Timeline
Evidence
ATT&CK
Threat Intelligence
```

---

## Phase 7 — AI Investigation

```text
Incident Evidence
       ↓
AI Assistant
       ↓
Summary
Explanation
Investigation Q&A
Report
```

---

## Phase 8 — Testing & Evaluation

```text
Synthetic Scenarios
        ↓
Security Datasets
        ↓
Detection Testing
        ↓
Correlation Testing
        ↓
Performance Evaluation
```

---

# 📚 Data Sources

The project can use a combination of:

### Public security datasets

* Security event datasets
* Network security datasets
* IDS datasets
* Endpoint/security telemetry datasets

### Synthetic scenarios

Controlled security scenarios can be generated for testing:

```text
scenario_001_account_compromise
scenario_002_bruteforce
scenario_003_powershell_activity
scenario_004_port_scan
scenario_005_suspicious_network_activity
```

Datasets should be documented separately with their source, license, and intended use.

---

# 🔐 Security Considerations

This project is intended for:

* Academic research
* Security monitoring
* Defensive security analysis
* Controlled testing
* SOC workflow demonstration

The platform should not perform unauthorized actions against external systems.

Response actions are simulated unless explicitly integrated into an authorized test environment.

### Repository security

Do not commit:

```text
API Keys
Passwords
Database Credentials
Private Tokens
.env files
Private certificates
```

Use environment variables instead.

For a public GitHub repository, GitHub recommends enabling security mechanisms such as secret scanning, push protection, Dependabot alerts, and code scanning where available.

---

# 🚀 Installation

## Requirements

```text
Python 3.x
Node.js
PostgreSQL
Git
Docker (optional)
```

---

## Clone Repository

```bash
git clone https://github.com/<your-username>/soc-investigation-platform.git

cd soc-investigation-platform
```

---

# Backend Setup

```bash
cd backend
```

Create a virtual environment:

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# Start FastAPI

```bash
uvicorn app.main:app --reload
```

Backend:

```text
http://127.0.0.1:8000
```

API documentation:

```text
http://127.0.0.1:8000/docs
```

---

# Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend will be available through the Next.js development server.

---

# ⚙️ Configuration

Sensitive configuration should be stored using environment variables.

Example:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/socdb

THREAT_INTEL_API_KEY=your_key_here

AI_API_KEY=your_key_here
```

Do not commit `.env` files.

Use:

```text
.env.example
```

to document required configuration variables.

---

# 🧪 Testing

Backend tests will use Pytest.

Run:

```bash
pytest
```

Example testing areas:

```text
Parser Tests
Normalizer Tests
Detection Tests
Correlation Tests
Incident Tests
Threat Intelligence Tests
API Tests
```

---

# 📈 Future Enhancements

Potential future extensions include:

* Real-time log streaming
* Additional security log parsers
* Advanced correlation rules
* Automated threat hunting
* More threat intelligence providers
* SOAR integrations
* Case management
* Role-based access control
* Real-time notifications
* SIEM integrations
* Machine-learning-based anomaly detection
* Distributed event processing

**Machine learning is intentionally not required for the current core implementation.**

---

# 🎓 Academic Contribution

The project demonstrates the integration of multiple cybersecurity concepts:

```text
Security Log Analysis
        ↓
Event Normalization
        ↓
Detection Engineering
        ↓
Security Event Correlation
        ↓
Incident Response
        ↓
Threat Intelligence
        ↓
MITRE ATT&CK
        ↓
SOC Operations
        ↓
AI-Assisted Investigation
```

The project can therefore be evaluated as both:

**Software Engineering + Cybersecurity Research**

---

# 👥 Team

### Project Team

| Name          | Role                         |
| ------------- | ---------------------------- |
| Team Member 1 | Backend / Detection          |
| Team Member 2 | Frontend / Dashboard         |
| Team Member 3 | Threat Intelligence / ATT&CK |
| Team Member 4 | AI / Investigation / Testing |

> Update this table with the actual team members and responsibilities.

---

# 📄 Project Status

**Current Status:** 🚧 In Development

### Completed

* [x] Project concept
* [x] System architecture
* [x] Technology selection
* [x] Repository structure
* [x] Backend environment setup

### In Progress

* [ ] Event schema
* [ ] Database models
* [ ] Log ingestion
* [ ] Parser
* [ ] Normalization
* [ ] Detection engine
* [ ] Correlation engine
* [ ] Incident engine

### Planned

* [ ] Threat intelligence
* [ ] MITRE ATT&CK
* [ ] Risk scoring
* [ ] SOC dashboard
* [ ] AI investigation assistant
* [ ] Incident reporting
* [ ] Testing and evaluation

---

# 📜 License

This project is intended for academic and educational purposes.

Add the project's chosen license in the `LICENSE` file.

---

# ⚠️ Disclaimer

This project is designed for authorized defensive security monitoring, academic research, and controlled testing environments.

Do not use the platform or any integrated security functionality against systems without appropriate authorization.

---

## ⭐ Project Philosophy

The core philosophy of this project is:

```text
Don't just detect the alert.

Understand the incident.
```

The platform transforms:

```text
Raw Events
    ↓
Security Signals
    ↓
Related Events
    ↓
Security Incident
    ↓
Evidence
    ↓
Investigation
    ↓
Actionable Report
```

**AI assists the analyst.
Rules detect known patterns.
Correlation connects the evidence.
The analyst makes the final investigation decision.**
