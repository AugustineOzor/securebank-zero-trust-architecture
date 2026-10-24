<p align="center"><img src="assets/banner.jpg" alt="Zero Trust Architecture Implementation — SecureBank Digital" width="100%"></p>

[Overview](README.md) · **Asset Inventory** · [Risk Register](Risk-Register.md) · [Access Risks](Access-Risks.md) · [Zero Trust Controls](Zero-Trust-Controls.md) · [Zero Trust Policies](Zero-Trust-Policies.md) · [Traceability Matrix](Traceability-Matrix.md) · [Maturity Metrics](Zero-Trust-Maturity-Metrics.md)

---

# Asset Inventory
**SecureBank Digital** · Document ID: SBD-ZT-AST-001 · Version 1.0 · Classification: Internal

## Purpose
The protect-surface inventory for the Zero Trust programme. Under NIST SP 800-207, Zero Trust starts by identifying the resources to be protected — every access decision, segmentation boundary and metric in this repository traces back to an asset listed here.

## Summary

| ID | Name | Type | Classification | Owner | Environment |
|---|---|---|---|---|---|
| [A-01](#a-01) | Core Banking Platform (CBP) | Application | 🔴 Critical | Head of Core Banking Engineering | Production |
| [A-02](#a-02) | Customer Accounts Database | Database | 🔴 Critical | Data Platform Lead | Production |
| [A-03](#a-03) | Mobile Banking API Gateway | Application | 🔴 Critical | Digital Channels Engineering Manager | Production |
| [A-04](#a-04) | Identity Provider (Okta/Entra ID tenant) | Cloud Account | 🔴 Critical | IAM Manager | Production |
| [A-05](#a-05) | Payments Hub Server Cluster | Server | 🟠 High | Payments Operations Lead | DR |
| [A-06](#a-06) | Employee Laptop Fleet | Endpoint | 🟠 High | IT Operations Manager | Production |

**Totals:** 6 assets · Critical 4 · High 2

## Asset Details

### A-01
**Core Banking Platform (CBP)**

| Field | Value |
|---|---|
| Type | Application |
| Classification | 🔴 Critical |
| Owner | Head of Core Banking Engineering |
| Environment | Production |
| Linked risks | [R-02](Risk-Register.md#r-02), [AR-04](Access-Risks.md#ar-04) |
| Linked controls | [ZT-05](Zero-Trust-Controls.md#zt-05), [ZT-06](Zero-Trust-Controls.md#zt-06) |

Ledger, account and payments processing for 500K+ customers. Top-priority protect surface for microsegmentation; east-west traffic currently unrestricted inside the data-centre VLAN.

### A-02
**Customer Accounts Database**

| Field | Value |
|---|---|
| Type | Database |
| Classification | 🔴 Critical |
| Owner | Data Platform Lead |
| Environment | Production |
| Linked risks | [R-02](Risk-Register.md#r-02), [AR-03](Access-Risks.md#ar-03) |
| Linked controls | [ZT-07](Zero-Trust-Controls.md#zt-07), [ZT-11](Zero-Trust-Controls.md#zt-11) |

PostgreSQL cluster holding customer PII, balances and transaction history. Accessed by CBP and the mobile API through shared service accounts; target is per-workload identity with mTLS and least-privilege DB roles.

### A-03
**Mobile Banking API Gateway**

| Field | Value |
|---|---|
| Type | Application |
| Classification | 🔴 Critical |
| Owner | Digital Channels Engineering Manager |
| Environment | Production |
| Linked risks | [R-01](Risk-Register.md#r-01), [AR-01](Access-Risks.md#ar-01) |
| Linked controls | [ZT-03](Zero-Trust-Controls.md#zt-03) |

Internet-facing gateway serving the iOS and Android apps. Enforcement point for continuous verification (device posture, session risk scoring, step-up MFA aligned to FFIEC authentication guidance).

### A-04
**Identity Provider (Okta/Entra ID tenant)**

| Field | Value |
|---|---|
| Type | Cloud Account |
| Classification | 🔴 Critical |
| Owner | IAM Manager |
| Environment | Production |
| Linked risks | [R-03](Risk-Register.md#r-03), [AR-02](Access-Risks.md#ar-02) |
| Linked controls | [ZT-01](Zero-Trust-Controls.md#zt-01), [ZT-02](Zero-Trust-Controls.md#zt-02), [ZT-04](Zero-Trust-Controls.md#zt-04), [ZT-12](Zero-Trust-Controls.md#zt-12) |

Central policy decision point (PDP) for workforce identity: SSO, phishing-resistant MFA, conditional access. Admin roles not yet under just-in-time privileged access.

### A-05
**Payments Hub Server Cluster**

| Field | Value |
|---|---|
| Type | Server |
| Classification | 🟠 High |
| Owner | Payments Operations Lead |
| Environment | DR |
| Linked risks | [R-04](Risk-Register.md#r-04) |
| Linked controls | — |

Warm-standby payment-processing nodes for ACH/RTP. Must mirror production segmentation and access policies so Zero Trust controls are not bypassed during failover.

### A-06
**Employee Laptop Fleet**

| Field | Value |
|---|---|
| Type | Endpoint |
| Classification | 🟠 High |
| Owner | IT Operations Manager |
| Environment | Production |
| Linked risks | [R-05](Risk-Register.md#r-05), [AR-05](Access-Risks.md#ar-05) |
| Linked controls | [ZT-08](Zero-Trust-Controls.md#zt-08), [ZT-09](Zero-Trust-Controls.md#zt-09), [ZT-10](Zero-Trust-Controls.md#zt-10) |

~750 managed Windows/macOS devices. Device trust signals (EDR health, patch level, disk encryption) feed the PDP; legacy VPN access to be replaced by ZTNA.

## Allowed Values
| Field | Values |
|---|---|
| Type | Application · Database · Server · Cloud Account · Network Device · Data Store · Endpoint |
| Classification | Critical · High · Medium · Low |
| Environment | Production · Staging · Development · DR |

> Owners are role titles in a fictional organisation.

---

← Previous: [Overview](README.md) · [↑ Back to Overview](README.md) · Next: [Risk Register](Risk-Register.md) →
