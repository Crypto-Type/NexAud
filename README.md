# NexAud — Next Gen Network Security Auditor

**AI-Driven Multi-Vendor Network Security Compliance Auditor**

| Item | Detail |
|---|---|
| Event | Smart India Hackathon 2026 |
| Problem Statement ID | 26155 |
| Organization | National Technical Research Organisation (NTRO) |
| Category | Software |
| Theme | Blockchain & Cybersecurity |
| Status | Working prototype (see [Current Prototype Capabilities](#22-current-prototype-capabilities)) |
| Stack | PHP 8.x, Apache, MariaDB/MySQL (PDO), HTML/CSS/JavaScript |

---

## Table of Contents

1. [Overview](#1-overview)
2. [Problem Statement](#2-problem-statement)
3. [Solution](#3-solution)
4. [Key Capabilities](#4-key-capabilities)
5. [Architecture](#5-architecture)
6. [Technology Stack](#6-technology-stack)
7. [Processing Workflow](#7-processing-workflow)
8. [Supported Vendors / Configuration Paths](#8-supported-vendors--configuration-paths)
9. [Compliance Frameworks](#9-compliance-frameworks)
10. [AI-Assisted Adaptive Training](#10-ai-assisted-adaptive-training)
11. [Evidence Integrity & Blockchain Theme](#11-evidence-integrity--blockchain-theme)
12. [Security Architecture](#12-security-architecture)
13. [Repository Structure](#13-repository-structure)
14. [Prerequisites](#14-prerequisites)
15. [Installation & Setup](#15-installation--setup)
16. [Database Setup](#16-database-setup)
17. [Application Configuration](#17-application-configuration)
18. [Running NexAud](#18-running-nexaud)
19. [Demonstration Workflow](#19-demonstration-workflow)
20. [Testing](#20-testing)
21. [Troubleshooting](#21-troubleshooting)
22. [Current Prototype Capabilities](#22-current-prototype-capabilities)
23. [Future Roadmap](#23-future-roadmap)
24. [Project Context](#24-project-context)
25. [License / Usage Note](#25-license--usage-note)

---

## 1. Overview

NexAud is a web-based, multi-vendor network security compliance auditing platform. It ingests network device configuration files, interprets vendor-specific syntax, normalizes security-relevant settings into a common model, evaluates that model against security controls drawn from recognised frameworks, and produces findings, remediation guidance and traceable evidence.

The platform keeps each stage separate:

```
Configuration → Parsing → Normalization → Control Evaluation → Findings → Evidence → Reporting
```

AI is an assisting component. Compliance decisions (PASS/FAIL) are made by a deterministic rule engine and are repeatable for the same input and the same control set.

## 2. Problem Statement

**Problem Statement 26155 — AI-Driven Multi-Vendor Network Security Compliance Auditor (NTRO).**

Networks are rarely built from a single vendor. The same security requirement (for example, "disable insecure management protocols" or "enforce logging to a central server") is expressed through different syntax, hierarchy and defaults on each vendor and operating system. Manual review of these configurations against frameworks such as CIS, NIST or DISA STIG is slow, inconsistent between auditors, and difficult to reproduce or defend later.

## 3. Solution

NexAud addresses this by isolating vendor-specific syntax from compliance logic:

- **Vendor parsers** read raw configuration text for a given vendor.
- **A normalization layer** converts parsed data into a vendor-neutral representation of security-relevant settings.
- **A deterministic policy/compliance engine** evaluates the normalized representation against controls from the selected framework.
- **Findings, remediation and evidence** are stored for each result, with evidence records linked by SHA-256 hashes so tampering can be detected.
- **Unknown configuration structures** enter an adaptive training path in which AI proposes an interpretation, a human reviews it, and only approved mappings are stored for reuse.

Because framework evaluation never sees vendor syntax, adding a vendor or configuration format does not require rewriting the compliance engine.

## 4. Key Capabilities

| Area | Capability |
|---|---|
| Ingestion | Upload/import of device configuration files linked to a device record |
| Parsing | Vendor-specific parser layer (Cisco, Fortinet/FortiGate, Palo Alto, adaptive path for unknown formats) |
| Normalization | Vendor-neutral model of security-relevant settings |
| Evaluation | Deterministic control evaluation with PASS/FAIL results |
| Frameworks | CIS, NIST SP 800-53, DISA STIG, ISO/IEC 27001:2022, Custom Policies |
| Findings | Findings management with remediation guidance |
| Evidence | Structured evidence per result, SHA-256 hash-linked chain, verification |
| Adaptive training | AI-assisted interpretation of unknown syntax with mandatory human approval |
| AI assistance | Explanations, remediation wording, copilot interaction (provider-abstracted) |
| Access control | Session-based authentication, RBAC, CSRF protection, audit logging |
| Reporting | Report generation from analysis results |

## 5. Architecture

```
                       ┌──────────────────────────────┐
                       │   Browser (HTML / CSS / JS)  │
                       └───────────────┬──────────────┘
                                       │ HTTP
                       ┌───────────────▼──────────────┐
                       │  Apache + PHP 8.x            │
                       │  Session auth · RBAC · CSRF  │
                       └───────────────┬──────────────┘
                                       │
   ┌───────────────┬───────────────┬───┴───────────┬────────────────┬──────────────┐
   │ Configuration │ Parser layer  │ Normalization │ Compliance /   │ Findings /   │
   │ ingestion     │ (per vendor)  │ layer         │ policy engine  │ Remediation  │
   └───────────────┴───────────────┴───────────────┴────────┬───────┴──────┬───────┘
                                                            │              │
                              ┌─────────────────────────────▼──┐   ┌───────▼────────┐
                              │ Evidence ledger (SHA-256 chain)│   │ Reports        │
                              └─────────────────────────────┬──┘   └────────────────┘
                                                            │
                       ┌────────────────────────────────────▼───┐
                       │  MariaDB / MySQL (PDO, prepared stmts) │
                       └────────────────────────────────────────┘

   Assistive (non-authoritative) path:
   Unknown syntax / explanations / copilot → AI provider abstraction → Prompt guard → AI logging
                                           → Human review → Approved mapping stored
```

**Design principles**

- **Separation of concerns.** Parsing, normalization, evaluation, findings, evidence and reporting are separate modules.
- **Deterministic decisions.** Control evaluation does not call an AI model to decide PASS/FAIL.
- **Human-in-the-loop learning.** New mappings are stored only after human approval.
- **Traceability.** Results are tied to evidence, and evidence records are hash-linked.

## 6. Technology Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | PHP 8.x, Apache |
| Database | MariaDB / MySQL, accessed through PDO |
| Application design | Modular PHP application; session-based authentication; RBAC; CSRF protection; audit logging |
| Core modules | Parser layer, normalization layer, compliance/policy engine, findings management, evidence ledger, training/mapping module, reporting module |
| AI architecture | Provider abstraction; Groq provider; enterprise AI provider abstraction; local LLM provider abstraction; prompt guard; AI logging |
| Frameworks | CIS, NIST SP 800-53, DISA STIG, ISO/IEC 27001:2022, Custom Policies |
| Development environment | Windows, XAMPP (Apache, PHP, MariaDB/MySQL, phpMyAdmin), modern web browser |

## 7. Processing Workflow

Normal user workflow:

1. Sign in.
2. Create or select a device.
3. Upload/import a configuration.
4. Select the relevant compliance framework/control set.
5. Parse the configuration.
6. Normalize the vendor-specific configuration.
7. Run deterministic compliance evaluation.
8. Review PASS/FAIL findings.
9. Inspect evidence.
10. Review remediation guidance.
11. Verify evidence integrity.
12. Generate/report results.

### Functional modules

| # | Module | Purpose |
|---|---|---|
| 1 | Authentication & RBAC | Sign-in, session handling, role-based access to features |
| 2 | Dashboard | Summary view of devices, analyses and findings |
| 3 | Device Inventory | Create, list and select network devices to audit |
| 4 | Configuration Ingestion | Upload/import device configuration files and associate them with a device |
| 5 | Vendor Parsing | Read vendor-specific configuration syntax into structured data |
| 6 | Normalization | Map parsed vendor data to a vendor-neutral security model |
| 7 | Compliance Analysis | Run deterministic evaluation of normalized data against selected controls |
| 8 | Controls / Frameworks | Maintain the control sets (CIS, NIST SP 800-53, DISA STIG, ISO/IEC 27001:2022, Custom Policies) |
| 9 | Findings | Record and review PASS/FAIL results per control |
| 10 | Remediation | Provide corrective guidance for failed controls |
| 11 | Evidence Ledger | Store structured evidence per result, linked by SHA-256 hashes |
| 12 | Evidence Verification | Re-compute and check the hash chain to detect modification |
| 13 | Adaptive Training | Review and approve mappings for configuration structures not yet understood |
| 14 | AI Copilot / AI Assistance | Explanations, remediation wording and interactive assistance via a configurable provider |
| 15 | Reports | Generate reports from analysis results |
| 16 | Audit Logs / security administration | Record security-relevant actions and support administrative review |

## 8. Supported Vendors / Configuration Paths

| Path | Description |
|---|---|
| Cisco | Vendor parser and normalization path demonstrated |
| Fortinet / FortiGate | Vendor parser and normalization path demonstrated |
| Palo Alto | Vendor parser and normalization path demonstrated |
| Unknown / adaptive | For configuration structures without a dedicated parser; routed to the adaptive training workflow |

### Vendor-neutral processing

```
Raw configuration
      ↓
Vendor parser
      ↓
Normalized representation
      ↓
Control mapping
      ↓
Deterministic evaluation
```

Vendor-specific syntax is confined to the parser (and, where required, the mapping) layer. Framework controls are written against the normalized representation. New vendors or formats are added by supplying a parser and mappings, not by modifying the evaluation logic.

## 9. Compliance Frameworks

| Framework | Notes |
|---|---|
| CIS | Built-in control set |
| NIST SP 800-53 | Built-in control set |
| DISA STIG | Built-in control set |
| ISO/IEC 27001:2022 | Built-in control set |
| Custom Policies | Organisation-defined controls |

### Compliance engine

The compliance engine evaluates normalized configuration data against deterministic controls. The same configuration and the same control set produce the same result, which is what makes results auditable and repeatable.

AI does **not** replace the compliance rules. AI may assist with:

- interpretation of unknown syntax
- explanations of findings
- remediation wording
- copilot interaction
- adaptive training proposals

## 10. AI-Assisted Adaptive Training

When a configuration contains structure that existing parsers and mappings do not cover:

```
Unknown configuration structure
        ↓
AI-assisted interpretation (proposal only)
        ↓
Human review
        ↓
Approved mapping
        ↓
Stored mapping
        ↓
Future reuse
```

**Human approval is a required step.** An AI-proposed interpretation is not used for compliance evaluation until a reviewer approves it. Once approved and stored, the mapping is reused for later configurations containing the same structure.

### AI provider configuration

| Component | Status |
|---|---|
| Provider abstraction | Implemented; application code is written against a provider interface |
| Groq provider | Implemented; requires a Groq API key (**OPTIONAL**, see [Section 17](#17-application-configuration)) |
| Enterprise AI provider abstraction | Abstraction layer present; endpoint details are deployment-specific |
| Local LLM provider abstraction | Abstraction layer present; endpoint details are deployment-specific |
| Prompt guard | Screens input passed to AI functions |
| AI logging | Records AI interactions |

AI-backed functions are optional. Deterministic parsing, normalization, evaluation, findings, evidence and reporting do not depend on an AI provider.

## 11. Evidence Integrity & Blockchain Theme

### Current prototype: tamper-evident cryptographic evidence chain

For each relevant compliance result, the system can associate structured evidence. Integrity is supported by cryptographic hashing:

- **SHA-256** hashing of configuration and evidence content
- **Linked evidence records**, where each record carries the hash of its predecessor
- **Chain verification**, which recomputes hashes and checks the links
- **Tamper detection**, since modifying a stored record breaks verification from that point onward

> **Scope note:** NexAud does **not** currently implement a decentralized blockchain. The current mechanism is a SHA-256 hash-linked evidence chain stored in the application database. It makes modification detectable; it does not by itself prevent a database administrator from rewriting the entire chain, which is why a distributed ledger is listed as future work.

### Future (not implemented)

**FUTURE:** Production deployments in which multiple organisational parties need independently verifiable evidence may use a permissioned ledger such as Hyperledger Fabric. This is a roadmap item and is not required for, or present in, the current prototype.

## 12. Security Architecture

| Control | Description |
|---|---|
| Authentication | Session-based authentication |
| RBAC | Role-based access control restricting modules and actions by role |
| CSRF protection | Anti-CSRF protection for state-changing requests |
| Secure session handling | Server-side sessions; session lifecycle managed in the application |
| Password protection / change flow | Passwords are stored as protected hashes (not plaintext); a password change flow is provided |
| MFA / TOTP-compatible architecture | The authentication design is compatible with TOTP-based MFA. Verify in the running build whether MFA enrolment is enabled for your deployment |
| Prepared statements | Database access through PDO with prepared SQL statements |
| Audit logging | Security-relevant actions are logged |
| Login protection | Rate limiting / login protection **where implemented**; verify the behaviour in the running build |
| Evidence hashing | SHA-256 hash-linked evidence chain (see Section 11) |
| AI safeguards | Prompt guard and AI interaction logging |

### Repository hygiene

**Never commit API keys, passwords or production secrets to GitHub.**

Never commit:

- `.env` files
- API keys
- passwords
- production database dumps
- private certificates
- private SSH keys
- sensitive customer or production network configurations

## 13. Repository Structure

Top-level layout (concise; verify against your checkout):

```
NexAud/
├── ai/               AI provider abstraction, prompt guard, AI logging
├── analysis/         Analysis execution
├── api/              Application API handlers
├── assets/           Static frontend assets (CSS, JavaScript, images)
├── compliance/       Compliance / policy engine
├── config/           Application, database, security and AI configuration
├── configurations/   Device configuration handling
├── controls/         Controls and frameworks management
├── database/         Database SQL files
├── evidence/         Evidence ledger and verification
├── findings/         Findings and remediation
├── includes/         Shared application code (common includes)
├── normalization/    Vendor-neutral normalization layer
├── parsers/          Vendor-specific parsers
├── reports/          Reporting
├── sample_configs/   Example configurations: Cisco, Fortinet, Palo Alto, Unknown
├── samples/          Additional sample material
├── sql/              Additional SQL files (including AI-related schema)
├── storage/          Runtime storage (must be writable by the web server)
├── tests/            Test and pipeline scripts
├── training/         Adaptive training / mapping module
├── uploads/          Uploaded configuration files (must be writable)
├── index.php
├── login.php
├── dashboard.php
├── logout.php
└── README.md
```

Directory descriptions above reflect the intended role of each module. Where the purpose of an individual file is unclear, treat it as prototype code rather than production-ready.

## 14. Prerequisites

| Requirement | Notes |
|---|---|
| Operating system | Windows (primary development environment) |
| Web server | Apache (XAMPP is the reference environment) |
| PHP | PHP 8.x |
| PHP extensions | PDO with the MySQL driver (`pdo_mysql`) at minimum; any further extensions the code requires are reported in the PHP/Apache error log |
| Database | MariaDB or MySQL |
| Database admin tool | phpMyAdmin (bundled with XAMPP) |
| Browser | Modern web browser |
| Network | Internet access is needed **only** if the OPTIONAL external AI provider is used |

Your XAMPP installation may differ from the reference environment (version, install path, ports). Adjust paths and ports accordingly.

## 15. Installation & Setup

1. **Obtain the project.**

   ```bash
   git clone <REPOSITORY_URL> NexAud
   ```

   Alternatively, copy the project folder into place.

2. **Place it under the Apache web root.**

   ```
   C:\xampp\htdocs\NexAud
   ```

   or, for a non-default installation:

   ```
   <APACHE_WEB_ROOT>\NexAud
   ```

3. **Start services.** In the XAMPP Control Panel (or your own Apache/MySQL setup), start **Apache** and **MySQL/MariaDB**.

4. **Check the PHP version** (the PHP executable path depends on your installation):

   ```
   <XAMPP_DIR>\php\php.exe -v
   ```

   It should report PHP 8.x. To check that the PDO MySQL driver is loaded:

   ```
   <XAMPP_DIR>\php\php.exe -m
   ```

   and confirm `pdo_mysql` appears in the list.

5. **Set up the database** — see [Section 16](#16-database-setup).

6. **Configure the application** — see [Section 17](#17-application-configuration).

7. **Confirm `storage/` and `uploads/` are writable** by the account Apache runs as.

## 16. Database Setup

NexAud requires a MariaDB/MySQL database named **`nexaud`**.

1. Start **Apache** and **MySQL/MariaDB**.
2. Open **phpMyAdmin** (typically `http://localhost/phpmyadmin/`; include the port if your Apache uses a non-default one).
3. Create a new database named `nexaud`. Use a UTF-8 collation (for example `utf8mb4_general_ci`, or the collation your server provides).
4. Select the `nexaud` database and use the **Import** tab to import the project's **schema SQL**.
5. Apply the **seed / control / framework data** where the repository provides it.
6. Set the database credentials in the application configuration ([Section 17](#17-application-configuration)).
7. Open the application and confirm the login page loads without a database error.

### SQL files in the repository

The repository may contain several SQL files. Their intended roles are:

| File | Intended purpose |
|---|---|
| `database/schema.sql` | Core application schema |
| `database/seed.sql` | Seed data (for example initial roles/users/base data) |
| `database/seed_builtin_frameworks.sql` | Built-in framework and control data |
| `sql/schema.sql` | Schema file in the `sql/` directory |
| `sql/ai_schema.sql` | Schema for AI-related tables (provider settings / AI logging) |

> **Do not blindly import every schema file.** `database/schema.sql` and `sql/schema.sql` may overlap. Open the files first, compare the `CREATE TABLE` statements, and import the applicable schema once. Importing overlapping schemas into the same database can fail with "table already exists" errors or produce conflicting definitions. `sql/ai_schema.sql` is intended as a supplement for AI-related tables; check that it does not duplicate tables already created by the main schema.

A suggested order, once you have confirmed which files apply:

1. Main schema (one file only)
2. AI schema, if its tables are not already present
3. Seed data
4. Built-in framework / control data

If you receive an import error, correct the cause (for example, a duplicate table) rather than re-importing over a partially populated database. Dropping and recreating the empty `nexaud` database is a clean way to retry in a test environment.

## 17. Application Configuration

Configuration is located under `config/`. Verify the exact file names in your checkout.

| Area | What to set |
|---|---|
| Database | Host, database name (`nexaud`), username, password |
| Application | Base URL / path (including the Apache port if it is not the default) and general application settings |
| Security | Session, CSRF and authentication-related settings |
| AI provider | Provider selection and credentials (**OPTIONAL**) |

### Database (placeholders)

```
DB_HOST     = <DB_HOST>        (commonly localhost for a local XAMPP install)
DB_NAME     = nexaud
DB_USER     = <DB_USER>
DB_PASSWORD = <DB_PASSWORD>
```

Use the credentials of the MySQL/MariaDB account that has access to the `nexaud` database. A fresh XAMPP install may use its default administrative account; for anything beyond a local demo, create a dedicated account with privileges limited to `nexaud`.

### AI provider (OPTIONAL)

An API key is required **only** for AI-backed functions that use an external provider (for example, the Groq provider). Without one, parsing, normalization, deterministic evaluation, findings, evidence and reporting remain usable; AI-assisted features (explanations, copilot, AI-assisted interpretation of unknown syntax) will not be available through an external provider.

```
GROQ_API_KEY=<your-key>
```

Provide this value through whichever configuration mechanism the repository uses for AI settings (see `config/` and `ai/`). Enterprise and local LLM providers are available through the provider abstraction and need endpoint details specific to your deployment.

**Never commit API keys, passwords or production secrets to GitHub.** Keep real values in local configuration that is excluded from version control.

### Initial access

Use the administrator credentials defined during database seeding/initial setup. If the seed data provides a demonstration account, **change the default password immediately after first login.**

## 18. Running NexAud

1. Clone/copy the repository.
2. Place the project under the Apache web root.
3. Start Apache.
4. Start MySQL/MariaDB.
5. Create/import the database.
6. Configure the database connection.
7. Configure the OPTIONAL AI provider.
8. Open the application in a browser:

   ```
   http://localhost/<NEXAUD_DIRECTORY>/
   ```

   If Apache listens on a port other than 80, include it, for example `http://localhost:<PORT>/<NEXAUD_DIRECTORY>/`.
9. Log in.
10. Create or import a test device configuration (use files from `sample_configs/`).
11. Run analysis.
12. Review findings and evidence.
13. Verify evidence.
14. Generate a report.

### Sample configurations

`sample_configs/` contains example configuration files for demonstration and testing:

| Vendor | Purpose |
|---|---|
| Cisco | Cisco configuration path |
| Fortinet | Fortinet / FortiGate configuration path |
| Palo Alto | Palo Alto configuration path |
| Unknown | Exercises the adaptive training path |

These are **sample data for demonstration/testing only**. They are not production network configurations and must not be treated as representative of any real environment. Do not upload sensitive production configurations to a demonstration instance.

## 19. Demonstration Workflow

A short evaluator walkthrough:

1. **Login** with the administrator account from initial setup.
2. **Device inventory:** show the device list; create a device if none exists.
3. **Upload a vendor configuration** from `sample_configs/` (for example Cisco, Fortinet or Palo Alto) and associate it with the device.
4. **Run compliance analysis** with a chosen framework.
5. **Show normalized/control results:** observe how different vendor syntax maps to the same normalized settings and controls.
6. **Show PASS/FAIL findings** and the associated remediation guidance.
7. **Open evidence** for a result.
8. **Verify evidence integrity** using the chain verification function.
9. **Adaptive training:** upload the Unknown sample, observe the AI-assisted interpretation proposal, and approve the mapping through human review (requires the OPTIONAL AI provider to be configured).
10. **Generate a report.**

## 20. Testing

The `tests/` directory contains functional and pipeline test scripts that can be used to validate processing behaviour (parsing, normalization, evaluation).

The repository does **not** provide a single standardised test runner, and no PHPUnit setup is documented here. Run the individual PHP scripts in `tests/` directly with the PHP CLI, using the script's actual file name:

```
<XAMPP_DIR>\php\php.exe tests\<SCRIPT_NAME>.php
```

Review each script before running it, since some may require the database to be configured and populated, or may write to the database.

## 21. Troubleshooting

| Problem | What to check |
|---|---|
| Apache does not start | Another program (for example IIS or Skype) may be using the port. Check the XAMPP Control Panel log, then either stop the conflicting program or change Apache's port in its configuration |
| MySQL/MariaDB does not start | Check whether another MySQL instance or process is using the port; review the XAMPP MySQL log |
| Database connection failure | Confirm MySQL/MariaDB is running, the host/name/user/password in `config/` are correct, and the `nexaud` database exists |
| Incorrect PHP version | Run `php -v` using the PHP bundled with your Apache. Multiple PHP installations on one machine are common; make sure Apache uses PHP 8.x |
| Incorrect database credentials | Test the same username/password in phpMyAdmin; update `config/` to match |
| Missing PHP extensions | Run `php -m` and confirm `pdo_mysql` is present. Enable required extensions in the `php.ini` used by Apache, then restart Apache. The Apache/PHP error log names any other missing extension |
| Cannot write to `storage/` or `uploads/` | Ensure the directories exist and are writable by the account Apache runs as |
| AI Agent / LLM Models, the provider is selected, and the server has outbound internet access. Core non-AI features work without a key | NLP Integration.
| Configuration parsing errors | Confirm the file is a complete, text-format configuration for the selected vendor. If the structure is not recognised, use the Unknown/adaptive path |
| Empty dashboard / no devices | The dashboard reflects stored data. Create a device and upload a configuration, then run an analysis |
| Evidence verification problems | Verification failing indicates a record's content or link does not match its stored hash. Check whether records were edited directly in the database or whether data was partially imported; analyse a fresh test device to confirm normal operation |

Check the Apache error log (and PHP error output) first for most startup and runtime errors.

## 22. Current Prototype Capabilities

- Multi-vendor configuration ingestion
- Vendor parser layer
- Vendor-neutral normalization
- Multi-framework controls
- Deterministic compliance evaluation
- Findings and remediation
- Evidence generation
- SHA-256 evidence integrity
- Tamper-evident evidence chain
- Adaptive training/mapping
- AI-assisted interpretation/explanation
- RBAC
- Security controls
- Reporting

Not included in the current prototype: a decentralized blockchain, live device collection, continuous monitoring, SIEM/SOAR integration and enterprise identity integration (see roadmap).

## 23. Future Roadmap

> **FUTURE WORK.** None of the items below are implemented in the current prototype or required to run it.

- Additional vendors
- More network OS versions
- Secure live device collection
- Cloud security controls
- Continuous compliance monitoring
- SIEM/SOAR integrations
- Enterprise identity integration
- Expanded control libraries
- Distributed deployment
- Permissioned blockchain/evidence federation (for example Hyperledger Fabric)
- Production-grade multi-tenant architecture

## 24. Project Context

| Item | Detail |
|---|---|
| Event | Smart India Hackathon 2026 |
| Problem Statement ID | 26155 |
| Problem Statement | AI-Driven Multi-Vendor Network Security Compliance Auditor |
| Organization | National Technical Research Organisation (NTRO) |
| Category | Software |
| Theme | Blockchain & Cybersecurity |

This repository is a prototype built for the hackathon submission and demonstration.

## 25. License / Usage Note

This repository is a hackathon prototype submitted for evaluation. No open-source license has been specified; unless a `LICENSE` file is added, all rights are reserved by the project team. Sample configurations are provided for demonstration only. Do not use the prototype with sensitive production data without a security review.
