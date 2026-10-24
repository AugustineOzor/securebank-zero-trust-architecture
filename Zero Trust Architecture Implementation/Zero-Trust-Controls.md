<p align="center"><img src="assets/banner.jpg" alt="Zero Trust Architecture Implementation — SecureBank Digital" width="100%"></p>

[Overview](README.md) · [Asset Inventory](Asset-Inventory.md) · [Risk Register](Risk-Register.md) · [Access Risks](Access-Risks.md) · **Zero Trust Controls** · [Zero Trust Policies](Zero-Trust-Policies.md) · [Traceability Matrix](Traceability-Matrix.md) · [Maturity Metrics](Zero-Trust-Maturity-Metrics.md)

---

# Zero Trust Controls
**SecureBank Digital** · Document ID: SBD-ZT-CTL-001 · Version 1.0 · Classification: Internal

## Purpose
Twelve Zero Trust controls that implement NIST SP 800-207 tenets across the CISA Zero Trust Maturity Model pillars. Each control is specific, measurable and testable, with an effectiveness rating backed by evidence.

## Effectiveness Scale
| Rating | Meaning |
|---|---|
| 🟢 Effective | Designed correctly, operating, and tested with evidence |
| 🟡 Partially Effective | Operating, but with incomplete coverage or known gaps |
| 🔴 Ineffective | Designed but not operating, or failing tests |
| ⚪ Not Tested | Implemented in pilot or non-production; no production evidence yet |

## Control Summary

| ID | Control | Framework | Ref | Effectiveness | Frequency | Pillar | Owner |
|---|---|---|---|---|---|---|---|
| [ZT-01](#zt-01) | Enforce phishing-resistant MFA (FIDO2 security keys or platform passkeys) for 100% of workforce accounts via Okta; SMS and push factors disabled | NIST CSF | PR.AA-03 | 🟡 Partially Effective | Continuous | Identity | IAM Manager |
| [ZT-02](#zt-02) | Replace standing IdP and infrastructure admin rights with just-in-time elevation via PAM: approval required, 4-hour maximum session, all sessions recorded | NIST CSF | PR.AA-05 | 🔴 Ineffective | Continuous | Identity | IAM Manager |
| [ZT-03](#zt-03) | Apply risk-based authentication to mobile banking: device binding plus behavioural risk score, with step-up verification for new payees and transfers over $2,500 | NIST CSF | PR.AA-03 | 🟡 Partially Effective | Continuous | Identity | Head of Digital Channels Security |
| [ZT-04](#zt-04) | Continuously re-evaluate sessions: revoke tokens within 5 minutes when user risk, device posture or location changes | NIST CSF | DE.CM-03 | 🟡 Partially Effective | Continuous | Identity | IAM Manager |
| [ZT-05](#zt-05) | Microsegment the Core Banking Platform: default-deny east-west traffic, with allow-list policies per workload enforced by the host-based segmentation agent | CIS Controls | 13.4 | ⚪ Not Tested | Continuous | Networks | Network Security Architect |
| [ZT-06](#zt-06) | Issue workload identities and enforce mutual TLS for all service-to-service calls to core banking and the accounts database | NIST CSF | PR.DS-02 | ⚪ Not Tested | Continuous | Applications & Workloads | Platform Engineering Lead |
| [ZT-07](#zt-07) | Store database service-account credentials in a secrets vault with automatic 30-day rotation and per-service least-privilege database roles | PCI DSS | 8.6.3 | 🟡 Partially Effective | Monthly | Data | Data Platform Lead |
| [ZT-08](#zt-08) | Automatically deprovision all access within 4 hours of termination, triggered by the HRIS leaver event via SCIM | CIS Controls | 6.2 | 🔴 Ineffective | Continuous | Identity | IT Operations Manager |
| [ZT-09](#zt-09) | Replace the legacy VPN with zero trust network access (ZTNA): per-application access only, granted after identity and device-posture checks | NIST CSF | PR.IR-01 | ⚪ Not Tested | Continuous | Networks | Network Security Architect |
| [ZT-10](#zt-10) | Require a compliant device for access to sensitive applications: EDR healthy, OS patched within 14 days, full-disk encryption on | NIST CSF | DE.CM-09 | 🟢 Effective | Continuous | Devices | IT Operations Manager |
| [ZT-11](#zt-11) | Recertify privileged and critical-system access quarterly: managers attest each entitlement and unattested access is revoked automatically after 10 days | ISO 27001 | A.5.18 | 🟢 Effective | Quarterly | Identity | GRC Manager |
| [ZT-12](#zt-12) | Stream all authentication, PAM and policy-decision logs to the SIEM, with UEBA alerts on anomalous access triaged by the SOC within 30 minutes | NIST CSF | DE.CM-03 | 🟢 Effective | Continuous | Visibility & Analytics | SOC Manager |

## Effectiveness Summary

| Effectiveness | Count | % |
|---|---|---|
| 🟢 Effective | 3 | 25% |
| 🟡 Partially Effective | 4 | 33% |
| 🔴 Ineffective | 2 | 16% |
| ⚪ Not Tested | 3 | 25% |
| **Total** | **12** | 100% |

## Coverage by Zero Trust Pillar

| Pillar (CISA ZTMM v2.0) | Controls |
|---|---|
| Identity | ZT-01, ZT-02, ZT-03, ZT-04, ZT-08, ZT-11 |
| Networks | ZT-05, ZT-09 |
| Applications & Workloads | ZT-06 |
| Data | ZT-07 |
| Devices | ZT-10 |
| Visibility & Analytics | ZT-12 |

## Control Details

### ZT-01
**Enforce phishing-resistant MFA (FIDO2 security keys or platform passkeys) for 100% of workforce accounts via Okta; SMS and push factors disabled**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — PR.AA-03 |
| Effectiveness | 🟡 Partially Effective |
| Frequency | Continuous |
| Owner | IAM Manager |
| ZT pillar | Identity |
| Mitigates | IdP standing admin; Leaver VPN access |
| Linked asset | Identity Provider tenant ([A-04](Asset-Inventory.md#a-04)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-02](Zero-Trust-Policies.md#pol-zt-02) |

**Description.** All 750 employees and contractors authenticate to SSO with FIDO2 keys or device-bound passkeys. Legacy factors removed from the Okta authentication policy. Measured by % of accounts enrolled and % of logins using phishing-resistant factors.

**Decision rationale.** Partially effective: 71% of accounts (533/750) enrolled; SMS still permitted for 217 users in a transition group. Policy is correct but coverage is incomplete. Becomes Effective at 100% enrolment with legacy factors disabled.

### ZT-02
**Replace standing IdP and infrastructure admin rights with just-in-time elevation via PAM: approval required, 4-hour maximum session, all sessions recorded**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — PR.AA-05 |
| Effectiveness | 🔴 Ineffective |
| Frequency | Continuous |
| Owner | IAM Manager |
| ZT pillar | Identity |
| Mitigates | IdP standing admin |
| Linked asset | Identity Provider tenant ([A-04](Asset-Inventory.md#a-04)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-02](Zero-Trust-Policies.md#pol-zt-02) |

**Description.** Privileged roles are held by zero standing accounts. Admins request elevation in the PAM tool, a second approver authorises, access expires automatically and sessions are recorded for review. Separate admin identities from daily-use accounts.

**Decision rationale.** Ineffective: PAM platform is deployed, but 11 super-admin accounts still hold permanent privileges and only 2 have been migrated. The control is designed but not operating, so the risk is not reduced yet.

### ZT-03
**Apply risk-based authentication to mobile banking: device binding plus behavioural risk score, with step-up verification for new payees and transfers over $2,500**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — PR.AA-03 |
| Effectiveness | 🟡 Partially Effective |
| Frequency | Continuous |
| Owner | Head of Digital Channels Security |
| ZT pillar | Identity |
| Mitigates | Customer account takeover |
| Linked asset | Mobile Banking API Gateway ([A-03](Asset-Inventory.md#a-03)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-04](Zero-Trust-Policies.md#pol-zt-04) |

**Description.** Each session is scored on device fingerprint, geolocation velocity, SIM-change signal and behaviour. High-risk sessions and high-value actions trigger step-up (in-app passkey). Aligns with FFIEC guidance on layered, risk-based authentication.

**Decision rationale.** Partially effective: device binding and step-up are live on iOS; Android release is pending (~46% of customers). SIM-swap signal integration with carriers is not yet in place. Fraud losses from ATO down 38% on iOS since launch.

### ZT-04
**Continuously re-evaluate sessions: revoke tokens within 5 minutes when user risk, device posture or location changes**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — DE.CM-03 |
| Effectiveness | 🟡 Partially Effective |
| Frequency | Continuous |
| Owner | IAM Manager |
| ZT pillar | Identity |
| Mitigates | IdP standing admin; Leaver VPN access |
| Linked asset | Identity Provider tenant ([A-04](Asset-Inventory.md#a-04)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-04](Zero-Trust-Policies.md#pol-zt-04) |

**Description.** Continuous access evaluation is enabled between the IdP and SaaS and internal apps. Session tokens are short-lived (60 minutes) and revoked on risk events (impossible travel, EDR alert, account disable).

**Decision rationale.** Partially effective: works for 14 of 22 critical apps that support CAE. The other 8 rely on 8-hour token lifetimes. Tested revocation time is 3–4 minutes on supported apps, which meets target.

### ZT-05
**Microsegment the Core Banking Platform: default-deny east-west traffic, with allow-list policies per workload enforced by the host-based segmentation agent**

| Field | Value |
|---|---|
| Primary framework | CIS Controls — 13.4 |
| Effectiveness | ⚪ Not Tested |
| Frequency | Continuous |
| Owner | Network Security Architect |
| ZT pillar | Networks |
| Mitigates | Flat network lateral movement |
| Linked asset | Core Banking Platform ([A-01](Asset-Inventory.md#a-01)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-03](Zero-Trust-Policies.md#pol-zt-03) |

**Description.** CBP servers are isolated into their own segment. Only documented application flows (CBP to accounts DB on 5432, API gateway to CBP on 443) are permitted. All other east-west traffic is denied and logged.

**Decision rationale.** Not tested: policies are built and running in monitor-only mode in staging. Enforcement in production is scheduled after a 30-day traffic baseline. Effectiveness cannot be rated until a blocked lateral-movement test passes in production.

### ZT-06
**Issue workload identities and enforce mutual TLS for all service-to-service calls to core banking and the accounts database**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — PR.DS-02 |
| Effectiveness | ⚪ Not Tested |
| Frequency | Continuous |
| Owner | Platform Engineering Lead |
| ZT pillar | Applications & Workloads |
| Mitigates | Service-account PII exfiltration; Flat network lateral movement |
| Linked asset | Core Banking Platform ([A-01](Asset-Inventory.md#a-01)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-03](Zero-Trust-Policies.md#pol-zt-03) |

**Description.** Each service gets a short-lived, automatically rotated cryptographic identity (SPIFFE/SVID). The service mesh rejects any connection without a valid peer identity, so network location alone grants no trust.

**Decision rationale.** Not tested: service mesh is deployed in development only. Design review is complete; production rollout is planned for next quarter.

### ZT-07
**Store database service-account credentials in a secrets vault with automatic 30-day rotation and per-service least-privilege database roles**

| Field | Value |
|---|---|
| Primary framework | PCI DSS — 8.6.3 |
| Effectiveness | 🟡 Partially Effective |
| Frequency | Monthly |
| Owner | Data Platform Lead |
| ZT pillar | Data |
| Mitigates | Service-account PII exfiltration |
| Linked asset | Customer Accounts Database ([A-02](Asset-Inventory.md#a-02)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-02](Zero-Trust-Policies.md#pol-zt-02) |

**Description.** Hard-coded credentials are removed from config files. Each application retrieves dynamic credentials from the vault at runtime. The two shared accounts are replaced with per-service roles scoped to required schemas only.

**Decision rationale.** Partially effective: mobile API is migrated to the vault with dynamic credentials. Core banking still uses one shared static account pending a vendor library update. Rotation is proven on migrated services (last rotation succeeded).

### ZT-08
**Automatically deprovision all access within 4 hours of termination, triggered by the HRIS leaver event via SCIM**

| Field | Value |
|---|---|
| Primary framework | CIS Controls — 6.2 |
| Effectiveness | 🔴 Ineffective |
| Frequency | Continuous |
| Owner | IT Operations Manager |
| ZT pillar | Identity |
| Mitigates | Leaver VPN access |
| Linked asset | Employee Laptop Fleet ([A-06](Asset-Inventory.md#a-06)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-02](Zero-Trust-Policies.md#pol-zt-02) |

**Description.** Termination in the HRIS triggers automatic disablement in the IdP, which cascades through SCIM to connected apps, VPN and PAM. Weekly reconciliation report flags any account with no matching active HR record.

**Decision rationale.** Ineffective: process is still manual through tickets. The last access review found 23 active leaver accounts, with a median 19 days to disable. SCIM integration is approved but not built.

### ZT-09
**Replace the legacy VPN with zero trust network access (ZTNA): per-application access only, granted after identity and device-posture checks**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — PR.IR-01 |
| Effectiveness | ⚪ Not Tested |
| Frequency | Continuous |
| Owner | Network Security Architect |
| ZT pillar | Networks |
| Mitigates | Leaver VPN access; Flat network lateral movement |
| Linked asset | Employee Laptop Fleet ([A-06](Asset-Inventory.md#a-06)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-03](Zero-Trust-Policies.md#pol-zt-03) |

**Description.** Users reach internal applications through the ZTNA broker, which grants access to named applications rather than network segments. The VPN concentrator is decommissioned after migration.

**Decision rationale.** Not tested: pilot runs with 40 IT users. VPN is still the primary access path for the other 710 users. No production evidence yet.

### ZT-10
**Require a compliant device for access to sensitive applications: EDR healthy, OS patched within 14 days, full-disk encryption on**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — DE.CM-09 |
| Effectiveness | 🟢 Effective |
| Frequency | Continuous |
| Owner | IT Operations Manager |
| ZT pillar | Devices |
| Mitigates | Leaver VPN access; Flat network lateral movement |
| Linked asset | Employee Laptop Fleet ([A-06](Asset-Inventory.md#a-06)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-04](Zero-Trust-Policies.md#pol-zt-04) |

**Description.** The MDM/EDR posture signal is passed to the IdP. Non-compliant devices are blocked from tier-1 applications and redirected to remediation. Monitored by daily compliance dashboard.

**Decision rationale.** Effective: 97% of devices are compliant. Testing with a deliberately non-compliant device (EDR stopped) confirmed it was blocked within 2 minutes. Exceptions are documented and time-limited.

### ZT-11
**Recertify privileged and critical-system access quarterly: managers attest each entitlement and unattested access is revoked automatically after 10 days**

| Field | Value |
|---|---|
| Primary framework | ISO 27001 — A.5.18 |
| Effectiveness | 🟢 Effective |
| Frequency | Quarterly |
| Owner | GRC Manager |
| ZT pillar | Identity |
| Mitigates | IdP standing admin; Service-account PII exfiltration |
| Linked asset | Customer Accounts Database ([A-02](Asset-Inventory.md#a-02)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-02](Zero-Trust-Policies.md#pol-zt-02) |

**Description.** The IGA tool generates quarterly campaigns covering IdP admins, PAM roles, core banking and database access. Reviewers approve or revoke each entitlement, and the tool removes anything not attested.

**Decision rationale.** Effective: the last two quarterly cycles completed on time with 100% attestation. 64 excess entitlements were removed in Q3. Evidence is retained for audit. Control operates as designed, though it is detective and periodic, so it does not replace ZT-02.

### ZT-12
**Stream all authentication, PAM and policy-decision logs to the SIEM, with UEBA alerts on anomalous access triaged by the SOC within 30 minutes**

| Field | Value |
|---|---|
| Primary framework | NIST CSF — DE.CM-03 |
| Effectiveness | 🟢 Effective |
| Frequency | Continuous |
| Owner | SOC Manager |
| ZT pillar | Visibility & Analytics |
| Mitigates | All five access risks |
| Linked asset | Identity Provider tenant ([A-04](Asset-Inventory.md#a-04)) |
| Governing policy | [POL-ZT-01](Zero-Trust-Policies.md#pol-zt-01), [POL-ZT-04](Zero-Trust-Policies.md#pol-zt-04) |

**Description.** IdP, PAM, ZTNA, API gateway and database audit logs are centralised. Detection rules and UEBA models cover impossible travel, MFA fatigue, privilege escalation and bulk data queries. Logs are retained for 12 months.

**Decision rationale.** Effective: log-source coverage is 100% for in-scope systems. Mean time to triage over the last 90 days is 18 minutes. A purple-team MFA-fatigue simulation was detected and contained. Rated effective as a detective control.

> References: NIST CSF 2.0 subcategories · CIS Controls v8 safeguards · PCI DSS v4.0 · ISO/IEC 27001:2022 Annex A · pillars per CISA Zero Trust Maturity Model v2.0.

---

← Previous: [Access Risks](Access-Risks.md) · [↑ Back to Overview](README.md) · Next: [Zero Trust Policies](Zero-Trust-Policies.md) →
