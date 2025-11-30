# 🛡️ Entra Identity Engineering Portfolio

**Architect:** Jason Wiggins  
**Focus:** Identity Governance, Automation, and Zero Trust Architecture  
**Tech Stack:** Microsoft Entra ID P2, Microsoft Graph API, PowerShell 7

---

## 📂 Project Index

This repository documents the architectural designs and automation scripts used to secure enterprise Microsoft 365 environments.

### 1. [Disaster Recovery & Automation](./Export-CAPolicies.ps1)
* **Objective:** Automated backup of Conditional Access policies for change management.
* **Tech:** PowerShell + Microsoft Graph (`Policy.Read.All`).
* **Artifact:** `Export-CAPolicies.ps1`

### 2. [Zero Trust Architecture](./Zero-Trust-Architecture.md)
* **Objective:** Design of a resilient identity perimeter, including Break-Glass accounts and Phishing-Resistant MFA enforcement.
* **Tech:** Conditional Access, FIDO2, Trusted Locations.

### 3. [Identity Governance (JIT Access)](./Identity-Governance-PIM.md)
* **Objective:** Implementation of Least Privilege using Privileged Identity Management (PIM).
* **Feature:** Just-In-Time (JIT) activation for administrative roles with audit trails.

### 4. [Guest Lifecycle Management (B2B)](./Identity-Governance-B2B.md)
* **Objective:** Automated governance for external vendors to prevent "Guest Sprawl."
* **Tech:** Entitlement Management, Access Packages, Self-Service Approvals.

### 5. [Workload Identity Security](./Enterprise-App-Integration.md)
* **Objective:** Secure integration of enterprise applications using Least Privilege API permissions.
* **Tech:** App Registrations vs. Enterprise Apps (Service Principals).

---

## 🔗 Connect
* [LinkedIn](https://www.linkedin.com/in/wigginsjason)
* [GitHub Profile](https://github.com/wigginsjason)
