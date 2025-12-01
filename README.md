# Entra Identity Engineering Portfolio

**Repository Owner:** Jason Wiggins  
**Role:** Senior Identity & Endpoint Security Engineer  
**Focus:** Zero Trust Architecture, Automated Governance, and Disaster Recovery.

## 🌟 Project Overview
This repository contains production-ready Infrastructure-as-Code (IaC) artifacts for managing Microsoft Entra ID (formerly Azure AD). It demonstrates a "Security-First" approach to identity management, moving away from manual portal clicks to automated, version-controlled configurations.

---

## 📂 Repository Contents & Projects

### 1. Disaster Recovery & Automation
* **Objective:** Automated backup of Conditional Access policies for change management.
* **Artifacts:**
    * [`Export-Policy_CA001-BlockLegacyAuth-AllUsers.ps1`](./Export-Policy_CA001-BlockLegacyAuth-AllUsers.ps1) (New: Targeted Policy Export)
    * [`Export-CAPolicies.ps1`](./Export-CAPolicies.ps1) (Bulk Export Utility)
* **Tech:** PowerShell + Microsoft Graph (`Policy.Read.All`).

### 2. Zero Trust Identity Perimeter
* **Objective:** Complete design and implementation of a resilient identity perimeter.
* **Documentation:** [`Zero-Trust-Architecture.md`](./Zero-Trust-Architecture.md)
* **Status:** Deployed (Report-Only Mode).

### 3. Identity Governance (JIT Access)
* **Objective:** Implementation of Least Privilege using Privileged Identity Management (PIM).
* **Documentation:** [`Identity-Governance-PIM.md`](./Identity-Governance-PIM.md)
* **Feature:** Just-In-Time (JIT) activation for administrative roles with audit trails.

### 4. Guest Lifecycle Management (B2B)
* **Objective:** Automated governance for external vendors to prevent "Guest Sprawl."
* **Documentation:** [`Identity-Governance-B2B.md`](./Identity-Governance-B2B.md)
* **Tech:** Entitlement Management, Access Packages, Self-Service Approvals.

### 5. Workload Identity & API Security
* **Objective:** Secure integration of enterprise applications and custom API connections.
* **Documentation:**
    * [`Enterprise-App-Integration.md`](./Enterprise-App-Integration.md) (SaaS App integration logic)
    * [`Custom Graph API Integration.md`](./Custom%20Graph%20API%20Integration.md) (Custom API permission models)

---

# 🛡️ Deep Dive: Zero Trust Identity Perimeter Implementation

## 1. Executive Summary
This project established a resilient identity perimeter for an M365 tenant, aligning with Microsoft Zero Trust principles ("Verify Explicitly"). The implementation enforces phishing-resistant authentication for high-privilege roles while mitigating the risk of tenant lockout through a redundant emergency access strategy.

## 2. Architecture & Configuration

### 2.1 Emergency Access ("Break Glass") Strategy
*Objective: Ensure tenant recovery capabilities during identity service outages.*
* **Accounts:** 2x Cloud-only accounts (`svc-backup01`...) provisioning via PIM.
* **Role Assignment:** **Global Administrator** (Active/Permanent).
* **Authentication:** FIDO2 Security Keys (Hardware-bound).
* **Exclusion Logic:** Managed via security group `Sec-BreakGlass-Exclusion`.

### 2.2 Network Perimeter (Geo-Blocking)
*Objective: Reduce attack surface by restricting authentication attempts to trusted geographies.*
* **Policy:** `Block-Untrusted-Locations`
* **Conditions:** Block access from high-risk regions; Allow Corporate HQ (`1.2.3.4/32`).
* **Safety Mode:** Report-Only.

### 2.3 Phishing-Resistant Administration
*Objective: Mitigate "MFA Fatigue" and "Adversary-in-the-Middle" (AiTM) attacks.*
* **Policy:** `Require-PhishingResistant-Admins`
* **Grant Control:** Require **Phishing-Resistant MFA** (FIDO2 / CBA).
* **Target Roles:** Global Admin, Security Admin.

## 3. Validation & Auditing

### 3.1 Visual Verification (Artifacts)
*Evidence of secure deployment state (Report-Only) and successful API connection.*

<img width="1894" height="877" alt="Zero_Trust_Policies png" src="https://github.com/user-attachments/assets/e2dae953-6cac-4d61-bdd1-90b461065df5" />

<img width="1869" height="632" alt="Graph_API_Audit png" src="https://github.com/user-attachments/assets/71f0e400-eee8-4089-af70-6dac0a304f2f" />

### 3.2 Policy Logic Audit (Microsoft Graph API)
*Method: Programmatic audit via Graph Explorer to verify policy state and prevent configuration drift.*

**API Endpoint:** `GET https://graph.microsoft.com/v1.0/identity/conditionalAccess/policies`

**Response Evidence:**
```json
{
    "displayName": "Block-Untrusted-Locations",
    "state": "enabledForReportingButNotEnforced",
    "conditions": {
        "locations": {
            "includeLocations": ["All"],
            "excludeLocations": ["[Corporate_HQ_GUID]"]
        }
    }
}
