<p align="center"><img src="assets/banner.jpg" alt="Zero Trust Architecture Implementation — SecureBank Digital" width="100%"></p>

[Overview](README.md) · [Asset Inventory](Asset-Inventory.md) · [Risk Register](Risk-Register.md) · **Access Risks** · [Zero Trust Controls](Zero-Trust-Controls.md) · [Zero Trust Policies](Zero-Trust-Policies.md) · [Traceability Matrix](Traceability-Matrix.md) · [Maturity Metrics](Zero-Trust-Maturity-Metrics.md)

---

# Access Risks — Identity & Access Deep Dive
**SecureBank Digital** · Document ID: SBD-ZT-RSK-002 · Version 1.0 · Classification: Internal

## Purpose
A focused assessment of five access-control risks, the identity-centric core of NIST SP 800-207. These risks drive the design of the identity, privileged-access and segmentation controls.

## Methodology
Each risk is written as **[Threat] exploits [Vulnerability] causing [Impact] to [Asset]** and scored **Likelihood (1–5) × Impact (1–5)**.

| Likelihood | | Impact | |
|---|---|---|---|
| 1 | Rare | 1 | Negligible |
| 2 | Unlikely | 2 | Minor |
| 3 | Possible | 3 | Moderate |
| 4 | Likely | 4 | Major |
| 5 | Almost Certain | 5 | Severe |

**Rating bands (applied consistently to every risk in this repository):** 🟢 Low 1–5 · 🟡 Medium 6–11 · 🟠 High 12–19 · 🔴 Critical 20–25

## Register Summary

| ID | Risk | Category | Status | L | I | Score | Rating | Owner | Asset |
|---|---|---|---|---|---|---|---|---|---|
| [AR-01](#ar-01) | External attacker exploits SMS/push-based MFA (vulnerable to SIM-swap and MFA-fatigue) causing account takeover to Mobile Banking API Gateway | Access Control | Open | 4 | 5 | **20** | 🔴 Critical | Head of Digital Channels Security | [A-03](Asset-Inventory.md#a-03) |
| [AR-02](#ar-02) | Malicious or compromised insider exploits standing privileged admin roles causing unauthorised configuration changes to Identity Provider tenant | Access Control | Open | 3 | 5 | **15** | 🟠 High | IAM Manager | [A-04](Asset-Inventory.md#a-04) |
| [AR-03](#ar-03) | Attacker with stolen service-account credentials exploits shared, over-privileged database accounts causing bulk exfiltration of customer PII from Customer Accounts Database | Access Control | In Progress | 3 | 5 | **15** | 🟠 High | Data Platform Lead | [A-02](Asset-Inventory.md#a-02) |
| [AR-04](#ar-04) | Attacker on any compromised internal host exploits a flat network with no east-west controls causing lateral movement to Core Banking Platform | Access Control | Open | 4 | 5 | **20** | 🔴 Critical | Network Security Architect | [A-01](Asset-Inventory.md#a-01) |
| [AR-05](#ar-05) | Departed employee or contractor exploits delayed deprovisioning and VPN-based access causing unauthorised remote access from Employee Laptop Fleet | Access Control | Open | 3 | 3 | **9** | 🟡 Medium | IT Operations Manager | [A-06](Asset-Inventory.md#a-06) |

## Heat Map

| Likelihood ↓ / Impact → | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| **5** |  |  |  |  |  |
| **4** |  |  |  |  | AR-01, AR-04 |
| **3** |  |  | AR-05 |  | AR-02, AR-03 |
| **2** |  |  |  |  |  |
| **1** |  |  |  |  |  |

| Rating | Count |
|---|---|
| 🔴 Critical | 2 |
| 🟠 High | 2 |
| 🟡 Medium | 1 |
| 🟢 Low | 0 |

## Risk Details

### AR-01
**External attacker exploits SMS/push-based MFA (vulnerable to SIM-swap and MFA-fatigue) causing account takeover to Mobile Banking API Gateway**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | Open |
| Likelihood × Impact | 4 × 5 = **20** (🔴 Critical) |
| Owner | Head of Digital Channels Security |
| Linked asset | Mobile Banking API Gateway ([A-03](Asset-Inventory.md#a-03)) |
| Related controls | [ZT-03](Zero-Trust-Controls.md#zt-03) |

**Description.** Customer login relies on SMS OTP with push fallback. No device binding or behavioural risk scoring, so a stolen password plus intercepted OTP gives full account access, including payee creation and transfers.

**Decision rationale.** Likelihood 4: SIM-swap and MFA-fatigue attacks on US banking customers are frequent and cheap, and competitor breaches used this path. Impact 5: direct financial loss across a 500K customer base, Reg E reimbursement, and a clear gap against FFIEC authentication guidance calling for layered, risk-based controls.

### AR-02
**Malicious or compromised insider exploits standing privileged admin roles causing unauthorised configuration changes to Identity Provider tenant**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | Open |
| Likelihood × Impact | 3 × 5 = **15** (🟠 High) |
| Owner | IAM Manager |
| Linked asset | Identity Provider tenant ([A-04](Asset-Inventory.md#a-04)) |
| Related controls | [ZT-01](Zero-Trust-Controls.md#zt-01), [ZT-02](Zero-Trust-Controls.md#zt-02), [ZT-04](Zero-Trust-Controls.md#zt-04), [ZT-12](Zero-Trust-Controls.md#zt-12) |

**Description.** 11 IdP super-admin accounts hold permanent privileges with no just-in-time elevation, no approval workflow and no separation between admin and daily-use identities.

**Decision rationale.** Likelihood 3: admin accounts are high-value phishing targets and standing access widens exposure, though accounts do use phishing-resistant MFA, which keeps likelihood below Likely. Impact 5: the IdP is the Zero Trust policy decision point, so its compromise lets an attacker grant access to every downstream system.

### AR-03
**Attacker with stolen service-account credentials exploits shared, over-privileged database accounts causing bulk exfiltration of customer PII from Customer Accounts Database**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | In Progress |
| Likelihood × Impact | 3 × 5 = **15** (🟠 High) |
| Owner | Data Platform Lead |
| Linked asset | Customer Accounts Database ([A-02](Asset-Inventory.md#a-02)) |
| Related controls | [ZT-07](Zero-Trust-Controls.md#zt-07), [ZT-11](Zero-Trust-Controls.md#zt-11) |

**Description.** Core banking and mobile API connect to the database through two shared service accounts with read/write on all schemas. Credentials are stored in config files and have not been rotated in 14 months.

**Decision rationale.** Likelihood 3: credentials in config files are commonly harvested after an initial foothold, but database access requires network adjacency, which limits it to Possible. Impact 5: mass PII exposure triggers GLBA notification, state breach laws and regulatory scrutiny. Status In Progress because the secrets-vault migration has started.

### AR-04
**Attacker on any compromised internal host exploits a flat network with no east-west controls causing lateral movement to Core Banking Platform**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | Open |
| Likelihood × Impact | 4 × 5 = **20** (🔴 Critical) |
| Owner | Network Security Architect |
| Linked asset | Core Banking Platform ([A-01](Asset-Inventory.md#a-01)) |
| Related controls | [ZT-05](Zero-Trust-Controls.md#zt-05), [ZT-06](Zero-Trust-Controls.md#zt-06) |

**Description.** Core banking servers share a VLAN with 40+ internal application servers. Any host in the segment can reach CBP admin ports. No microsegmentation or workload-identity enforcement exists.

**Decision rationale.** Likelihood 4: perimeter-based design means a single phished endpoint or vulnerable internal app gives a route to CBP, the pattern in most recent bank intrusions. Impact 5: loss of integrity or availability of the ledger halts payments for all customers. This is the primary driver for the microsegmentation workstream.

### AR-05
**Departed employee or contractor exploits delayed deprovisioning and VPN-based access causing unauthorised remote access from Employee Laptop Fleet**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | Open |
| Likelihood × Impact | 3 × 3 = **9** (🟡 Medium) |
| Owner | IT Operations Manager |
| Linked asset | Employee Laptop Fleet ([A-06](Asset-Inventory.md#a-06)) |
| Related controls | [ZT-08](Zero-Trust-Controls.md#zt-08), [ZT-09](Zero-Trust-Controls.md#zt-09), [ZT-10](Zero-Trust-Controls.md#zt-10) |

**Description.** HR-to-IT leaver process is manual. Access review found 23 active VPN accounts belonging to leavers (average 19 days after exit). VPN grants broad network access once connected, regardless of device posture.

**Decision rationale.** Likelihood 3: orphaned accounts exist today, though most leavers had laptops returned, which reduces real-world exploitability. Impact 3: VPN gives network-level access, but core systems still require separate credentials, so impact is moderate rather than major. Remediation: HRIS-driven automated deprovisioning and ZTNA replacing VPN.


---

← Previous: [Risk Register](Risk-Register.md) · [↑ Back to Overview](README.md) · Next: [Zero Trust Controls](Zero-Trust-Controls.md) →
