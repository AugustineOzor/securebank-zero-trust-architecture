<p align="center"><img src="assets/banner.jpg" alt="Zero Trust Architecture Implementation — SecureBank Digital" width="100%"></p>

[Overview](README.md) · [Asset Inventory](Asset-Inventory.md) · **Risk Register** · [Access Risks](Access-Risks.md) · [Zero Trust Controls](Zero-Trust-Controls.md) · [Zero Trust Policies](Zero-Trust-Policies.md) · [Traceability Matrix](Traceability-Matrix.md) · [Maturity Metrics](Zero-Trust-Maturity-Metrics.md)

---

# Risk Register — Zero Trust Programme
**SecureBank Digital** · Document ID: SBD-ZT-RSK-001 · Version 1.0 · Classification: Internal

## Purpose
Enterprise-level risks arising from SecureBank's perimeter-based security model. These risks justify the Zero Trust transformation mandated by the board and span access control, data protection, availability and human error.

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
| [R-01](#r-01) | External attacker exploits MFA-fatigue and SMS OTP weaknesses causing account takeover of customer accounts in the Mobile Banking API Gateway | Access Control | Open | 4 | 5 | **20** | 🔴 Critical | Head of Digital Channels Security | [A-03](Asset-Inventory.md#a-03) |
| [R-02](#r-02) | Compromised workload exploits flat east-west network causing lateral movement to the Core Banking Platform and Customer Accounts Database | Data Protection | In Progress | 3 | 5 | **15** | 🟠 High | Head of Infrastructure Security | [A-01](Asset-Inventory.md#a-01), [A-02](Asset-Inventory.md#a-02) |
| [R-03](#r-03) | Malicious insider or stolen credential exploits standing IdP admin privileges causing tenant-wide policy changes to the Identity Provider | Access Control | Open | 2 | 5 | **10** | 🟡 Medium | IAM Manager | [A-04](Asset-Inventory.md#a-04) |
| [R-04](#r-04) | Ransomware operator exploits unpatched legacy VPN appliance causing outage of the Payments Hub Server Cluster during DR failover | Availability | Open | 3 | 4 | **12** | 🟠 High | Payments Operations Lead | [A-05](Asset-Inventory.md#a-05) |
| [R-05](#r-05) | Employee exploits unmanaged personal devices and permissive BYOD access causing leakage of customer data from the Employee Laptop Fleet | Human Error | Open | 3 | 3 | **9** | 🟡 Medium | IT Operations Manager | [A-06](Asset-Inventory.md#a-06) |

## Heat Map

| Likelihood ↓ / Impact → | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| **5** |  |  |  |  |  |
| **4** |  |  |  |  | R-01 |
| **3** |  |  | R-05 | R-04 | R-02 |
| **2** |  |  |  |  | R-03 |
| **1** |  |  |  |  |  |

| Rating | Count |
|---|---|
| 🔴 Critical | 1 |
| 🟠 High | 2 |
| 🟡 Medium | 2 |
| 🟢 Low | 0 |

## Risk Details

### R-01
**External attacker exploits MFA-fatigue and SMS OTP weaknesses causing account takeover of customer accounts in the Mobile Banking API Gateway**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | Open |
| Likelihood × Impact | 4 × 5 = **20** (🔴 Critical) |
| Owner | Head of Digital Channels Security |
| Linked asset | Mobile Banking API Gateway ([A-03](Asset-Inventory.md#a-03)) |
| Related controls | [ZT-03](Zero-Trust-Controls.md#zt-03) |

**Description.** Customer authentication relies on SMS OTP and push approvals with no number matching or device binding. SIM-swap and push-bombing campaigns target US challenger banks; a successful takeover enables fraudulent transfers and exposure of PII.

**Decision rationale.** Likelihood 4: SIM-swap/MFA-fatigue attacks on banking apps are active and widely tooled, and our controls (SMS OTP, no device binding) are the ones attackers target. Impact 5: direct customer financial loss, Reg E liability, FFIEC authentication expectations not met, and reputational damage at a 500K-customer scale.

### R-02
**Compromised workload exploits flat east-west network causing lateral movement to the Core Banking Platform and Customer Accounts Database**

| Field | Value |
|---|---|
| Category | Data Protection |
| Status | In Progress |
| Likelihood × Impact | 3 × 5 = **15** (🟠 High) |
| Owner | Head of Infrastructure Security |
| Linked asset | Core Banking Platform; Customer Accounts Database ([A-01](Asset-Inventory.md#a-01), [A-02](Asset-Inventory.md#a-02)) |
| Related controls | [ZT-05](Zero-Trust-Controls.md#zt-05), [ZT-06](Zero-Trust-Controls.md#zt-06) |

**Description.** Core banking servers, the customer database and supporting services share one data-centre VLAN with permissive any-any internal rules. One compromised host can reach the database directly.

**Decision rationale.** Likelihood 3: no known compromise, but a single foothold (phishing, unpatched service) is plausible and nothing stops lateral movement. Impact 5: full ledger and PII exposure or ransomware on the core platform. Status In Progress because the microsegmentation pilot has started.

### R-03
**Malicious insider or stolen credential exploits standing IdP admin privileges causing tenant-wide policy changes to the Identity Provider**

| Field | Value |
|---|---|
| Category | Access Control |
| Status | Open |
| Likelihood × Impact | 2 × 5 = **10** (🟡 Medium) |
| Owner | IAM Manager |
| Linked asset | Identity Provider tenant ([A-04](Asset-Inventory.md#a-04)) |
| Related controls | [ZT-01](Zero-Trust-Controls.md#zt-01), [ZT-02](Zero-Trust-Controls.md#zt-02), [ZT-04](Zero-Trust-Controls.md#zt-04), [ZT-12](Zero-Trust-Controls.md#zt-12) |

**Description.** Seven engineers hold permanent Global/Super Admin roles with no just-in-time elevation or approval workflow. An attacker with one of these accounts could disable MFA or conditional-access policies for all users.

**Decision rationale.** Likelihood 2: the admin group is small and uses phishing-resistant keys, so compromise is unlikely. Impact 5: the IdP is the Zero Trust policy decision point, so its compromise defeats every downstream access control.

### R-04
**Ransomware operator exploits unpatched legacy VPN appliance causing outage of the Payments Hub Server Cluster during DR failover**

| Field | Value |
|---|---|
| Category | Availability |
| Status | Open |
| Likelihood × Impact | 3 × 4 = **12** (🟠 High) |
| Owner | Payments Operations Lead |
| Linked asset | Payments Hub Server Cluster ([A-05](Asset-Inventory.md#a-05)) |
| Related controls | — |

**Description.** The DR site is reached through a legacy VPN appliance two firmware versions behind, and DR segmentation does not mirror production. Failover could expose payment nodes to a compromised network path.

**Decision rationale.** Likelihood 3: edge VPN appliances are among the most exploited initial-access vectors and this one lags on patches. Impact 4 rather than 5: production payments continue unless failover is in progress, but an outage during an incident would disrupt ACH/RTP settlement and breach recovery-time objectives.

### R-05
**Employee exploits unmanaged personal devices and permissive BYOD access causing leakage of customer data from the Employee Laptop Fleet**

| Field | Value |
|---|---|
| Category | Human Error |
| Status | Open |
| Likelihood × Impact | 3 × 3 = **9** (🟡 Medium) |
| Owner | IT Operations Manager |
| Linked asset | Employee Laptop Fleet ([A-06](Asset-Inventory.md#a-06)) |
| Related controls | [ZT-08](Zero-Trust-Controls.md#zt-08), [ZT-09](Zero-Trust-Controls.md#zt-09), [ZT-10](Zero-Trust-Controls.md#zt-10) |

**Description.** About 12% of staff access email and file shares from personal devices with no posture check. Customer data can be downloaded to devices outside EDR and DLP coverage.

**Decision rationale.** Likelihood 3: BYOD use is common and data downloads have already been seen in logs. Impact 3: leakage is likely limited in scope (individual files rather than bulk databases), but it may still trigger GLBA notification and regulatory findings.


---

← Previous: [Asset Inventory](Asset-Inventory.md) · [↑ Back to Overview](README.md) · Next: [Access Risks](Access-Risks.md) →
