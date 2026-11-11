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
