# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
**Project:** Secure Banking Application | **Course:** SCMP 401 Software Engineering Studio

| Field | Details |
| :--- | :--- |
| **Team Name:** | Team SIGMA |
| **Date & Version:** | 2026-11-11 \| Version 1.0 (Milestone 1 Submission) |
| **Document Authors:** | Natheer Nihad (Product Owner), Ahmed Adil (ScrumMaster), Ahmed Emad & Honyar Bahir (System Architect & Security Lead), Lawand Bestoon (UI/UX Developer), Bavin Kawa & Ahmed Emad (Database & Back-End Lead) |
| **Course & Instructor:** | SCMP 401 — Software Engineering Studio \| Dr. Bestoon Hussien |
| **Document Standard:** | Adapted from IEEE 830-1998 & Ian Sommerville Chapter 4 |

---

## Document Preface & Revision History

This Software Requirements Specification (SRS) outlines the functional, non-functional, and system requirements for the Secure Banking Application developed as part of SCMP 401. It serves as the primary technical agreement between project stakeholders, system architects, software developers, and quality assurance engineers.

| Version | Date | Description of Changes | Author(s) | Approved By |
| :--- | :--- | :--- | :--- | :--- |
| v0.1 | 2026-10-28 | Initial draft of User Requirements & System Scope | Natheer Nihad, Lawand Bestoon | Natheer Nihad (Product Owner) |
| v0.5 | 2026-11-04 | Added System Requirements, Account Logic & NFR Metrics | Ahmed Emad, Bavin Kawa | Ahmed Adil (ScrumMaster) |
| v1.0 | 2026-11-11 | Complete Phase 1 SRS with System Specs, Use Case Tables & RTM | Honyar Bahir, Team SIGMA | Dr. Bestoon Hussien |

---

## 1. Introduction & System Vision

### 1.1 Purpose of the System
This document defines the functional, non-functional, and detailed system requirements for the Secure Web-Based Banking Application developed by Team SIGMA. The system provides authenticated bank customers with secure digital access to manage financial accounts, perform real-time balance inquiries, and execute atomic monetary deposits and withdrawals. It serves as the foundational technical specification for Milestone 1 of the SCMP 401 Software Engineering Studio project.

### 1.2 Scope & System Boundary
The Banking Application encompasses multi-factor customer identity verification, account management (specifically differentiating Current Accounts with overdraft privileges from Savings Accounts with monthly interest calculation and minimum balance floors), real-time balance inquiries, and atomic transaction processing. Functions outside this software boundary (such as physical ATM hardware maintenance, manual vault management, or credit underwriting) are explicitly excluded.

### 1.3 Definitions, Acronyms, and Abbreviations
* **SRS:** Software Requirements Specification — formal document specifying software capabilities.
* **NFR:** Non-Functional Requirement — whole-system quality attributes and operational constraints.
* **MFA:** Multi-Factor Authentication — security verification requiring two or more authentication factors.
* **Current Account:** A transactional deposit account permitting frequent withdrawals and overdraft privileges.
* **Savings Account:** An interest-bearing deposit account subject to transaction frequency and balance minimums.
* **TDD:** Test-Driven Development — engineering practice where unit tests are written prior to code.
* **ACID:** Atomicity, Consistency, Isolation, Durability — database transaction properties ensuring financial operations complete indivisibly or roll back completely.
* **AES-256 & BCrypt:** Cryptographic algorithms used to encrypt stored financial records at rest (AES-256) and hash user passwords with a work factor of $12$ (BCrypt).

---

## 2. Stakeholders & Reader Perspectives

Different stakeholders read the SRS for distinct purposes across the Software Development Life Cycle (SDLC):

| Reader Category | Primary Stakeholders | Document Purpose & Focus |
| :--- | :--- | :--- |
| **Client Managers** | Course Instructor (Dr. Bestoon Hussien), Evaluators | Verifies that the system meets educational, architectural, and business objectives. |
| **System End-Users** | Bank Customers, Bank Tellers | Reads User Requirements (UR-1 to UR-5) to confirm system capabilities and daily workflow usability. |
| **System Architects & Developers** | Team SIGMA Engineering Team (4th-Year Computer Engineering) | Uses System Requirements (SR-1.1 to SR-3.3) as an unambiguous technical blueprint for TypeScript (Node.js / NestJS / Express) implementation. |
| **System Test Engineers** | Quality Assurance Lead, Automated TypeScript (Jest) Test Suite | Designs automated unit, integration, and acceptance test cases directly from verifiable specifications. |
| **Agile Project Leads** | Product Owner (Natheer Nihad), ScrumMaster (Ahmed Adil) | Uses specifications to prioritize the Product Backlog, plan sprints, and track requirements traceability. |

---

## 3. User Requirements Definition

User requirements describe the functional services and operational constraints provided to end-users in high-level natural language. Mandatory services use 'shall'; optional enhancements use 'should'.

* **UR-1 [Security & Authentication]:** The Banking Application **shall** authenticate customers using multi-factor credentials (User ID, password, and one-time passcode) before granting access to account services, and **shall** temporarily lock accounts after repeated failed login attempts.
  * *Rationale:* Protects customer financial assets against unauthorized access and complies with data privacy legislation.
* **UR-2 [Account Creation & Types]:** The system **shall** enable authorized personnel or authenticated customers to establish both Current Accounts (supporting overdraft privileges) and Savings Accounts (supporting monthly interest accrual and minimum balance rules).
  * *Rationale:* Provides product differentiation tailored to diverse customer transactional and savings needs.
* **UR-3 [Balance & Ledger Inquiry]:** Authenticated customers **shall** be able to view real-time account balances, available overdraft or withdrawal limits, and recent transaction histories across their Current and Savings accounts.
  * *Rationale:* Gives customers immediate visibility into their financial position before initiating transfers or withdrawals.
* **UR-4 [Monetary Deposit]:** Authenticated customers **shall** be able to deposit positive monetary amounts into an active Current or Savings account with immediate balance updates and digital receipt confirmation.
  * *Rationale:* Ensures reliable crediting of customer funds with an auditable ledger record.
* **UR-5 [Monetary Withdrawal]:** Authenticated customers **shall** be able to withdraw funds from an active Current or Savings account, provided the requested amount satisfies account-specific overdraft limits or minimum balance floors.
  * *Rationale:* Prevents unapproved over-drafting while enforcing bank liquidity and account policy constraints.

---

## 4. Detailed System Requirements Specification

System requirements expand user requirements into explicit technical statements detailing exact inputs, outputs, preconditions, postconditions, and exception handling logic.

### 4.1 Authentication & Access Control
* **SR-1.1 [Credential Hashing & MFA Verification]:** Upon user login, the system **shall** prompt for User ID, Password, and a Multi-Factor Authentication (MFA) one-time passcode. Passwords **shall** be salted and hashed using BCrypt (cost factor $12$) prior to verification against the authentication database.
* **SR-1.2 [Brute-Force Lockout Policy]:** If three ($3$) consecutive incorrect password or MFA attempts are detected within $5$ minutes, the system **shall** lock the account for $15$ minutes, block subsequent login attempts during the lockout window, and issue a security audit log.

### 4.2 Account Management Logic
* **SR-2.1 [Current Account Overdraft]:** The system **shall** permit negative balances on Current Accounts up to a pre-configured overdraft limit $L_{\text{max}} = \$500.00$. Any withdrawal that would result in a Current Account balance below $-\$500.00$ **shall** be rejected.
* **SR-2.2 [Savings Account Interest]:** The system **shall** calculate and credit monthly interest accrued on active Savings Accounts using $I = B \times (r / 12)$ on the last calendar day of each month, where $B$ is the minimum daily balance recorded during the month and $r = 2.5\%$.
* **SR-2.3 [Savings Balance Floor]:** The system **shall** reject withdrawal requests on Savings Accounts if the resulting balance falls below the minimum required balance $B_{\text{min}} = \$100.00$. Savings Accounts **shall not** permit overdrafts ($L_{\text{max}} = \$0.00$).

### 4.3 Transaction Processing Logic
* **SR-3.1 [Atomic Withdrawal Processing Specification]:**
  * **Preconditions:** Customer authenticated; Account status = `ACTIVE`; Session token valid.
  * **Input:** Account ID, Withdrawal Amount ($A > \$0.00$, precision to two decimal places).
  * **Processing Action:** Within an atomic database transaction, the system checks if $(\text{Balance}_{\text{old}} + L_{\text{max}}) \ge A$ for a Current Account (where $L_{\text{max}} = \$500.00$) or if $(\text{Balance}_{\text{old}} - A) \ge B_{\text{min}}$ for a Savings Account (where $B_{\text{min}} = \$100.00$). If satisfied, deduct $A$ from $\text{Balance}_{\text{old}}$ and append an immutable debit record to the database ledger.
  * **Postconditions:** Updated $\text{Balance}_{\text{new}} = \text{Balance}_{\text{old}} - A$; immutable audit log committed; system dispatches withdrawal acknowledgment.
  * **Exception Handling:** If balance or floor constraints are violated, the transaction is aborted, database state rolls back completely, and the system returns `ERR_INSUFFICIENT_FUNDS`.

* **SR-3.2 [Atomic Deposit Processing Specification]:**
  * **Preconditions:** Customer authenticated; Account status = `ACTIVE`; Session token valid.
  * **Input:** Account ID, Deposit Amount ($D > \$0.00$, precision to two decimal places).
  * **Processing Action:** Within an atomic database transaction, the system validates $D > \$0.00$, computes $\text{Balance}_{\text{new}} = \text{Balance}_{\text{old}} + D$, updates the account record in the Core Banking Database, and appends an immutable credit record to the transaction ledger.
  * **Postconditions:** Updated $\text{Balance}_{\text{new}} = \text{Balance}_{\text{old}} + D$; immutable audit log entry committed; deposit confirmation receipt returned.
  * **Exception Handling:** If $D \le \$0.00$ or a database write error occurs, the transaction is aborted, uncommitted state rolls back, and the system returns `ERR_INVALID_DEPOSIT_AMOUNT`.

* **SR-3.3 [Real-Time Balance Inquiry Specification]:**
  * **Preconditions:** Customer authenticated; Session token valid; Customer authorized to access Account ID.
  * **Input:** Account ID.
  * **Processing Action:** System executes a read-consistent query against the Core Banking Database to retrieve current balance $B$, account type (`CURRENT` or `SAVINGS`), effective available withdrawal limit, and recent ledger entries.
  * **Postconditions:** Account balance remains unchanged (read-only query); formatted account summary is returned to the client interface within $< 1.5\text{ seconds}$.
  * **Exception Handling:** If the session token has expired or access is unauthorized, the query is rejected with `ERR_UNAUTHORIZED_ACCESS` and the user is redirected to Log In.

---