
# Secure Refactoring Hackathon – Web & SQL on Azure

![IAAS Unsecure Diagram](https://raw.githubusercontent.com/koenraadhaedens/IAAS-azure-secure-refactor-hackathon/main/media/iaasunsecure.png)

Welcome to the **Secure Refactoring Hackathon**. You will receive a deliberately **insecure** deployment and your challenge is to **redesign and harden** it using Zero Trust principles and **Microsoft Defender for Cloud (Plan 2)**. This is a hands-on, time‑boxed exercise. You may refactor architecture, configurations, and controls, provided the application remains functional.

> **Important:** This README intentionally describes the scenario and challenges **without exposing solutions**. Your team must research, design, implement, and justify your choices.

---

## 1. Scenario Overview

You are given an Azure environment with:

- A single **virtual network** (`AZTrainingVNet`) with two subnets:
  - `FrontEndSubnet` hosting a **WebVM** serving HTTP traffic
  - `BackEndSubnet` hosting a **SQLVM** with database services
- The **WebVM** is directly exposed to the internet via a **Public IP** (no TLS/WAF)
- The **WebVM** communicates directly with the **SQLVM** across the VNet
- Minimal network segmentation, permissive NSGs, and limited monitoring

This starting point is **insecure by design** and represents common misconfigurations found in the wild. Your job is to harden it while keeping the app running.

---

## 2. Goals & Constraints

### Must‑Have Goals
- **Perimeter protection:** Introduce a secure, inspected entry point for web traffic and remove direct public exposure from compute instances.
- **Workload protection:** Use **Defender for Cloud (Plan 2)** to improve the environment’s **Secure Score** and reduce high‑severity recommendations.
- **HTTPS exposure:** Expose the website externally via **HTTPS** (frontend). The **backend** may remain HTTP for this workshop’s time constraints.
- **Zero Trust alignment:** Apply least privilege, verify explicitly, and assume breach principles across identity, network, and data.

### Constraints
- Keep the application **functional** end‑to‑end.
- Time‑boxed: two sessions of ~3 hours (Day 1 & Day 2). Prioritize high‑impact controls.
- Do not introduce services that fundamentally change the app’s hosting model (e.g., replatforming to PaaS) unless agreed as a stretch goal.
- Avoid hardcoded secrets or credentials in scripts or code.

---

## 3. Challenge Tracks (Pick All; Sequence Is Up to You)

Each track contains **challenge objectives** to guide implementation and evidence collection. You are expected to explain your design decisions. Do **not** include confidential secrets or internal keys in your submissions.

### Track A – Perimeter & Network Protection
**Objectives:**
- Remove public IP exposure from VM workloads.
- Route inbound traffic through a **layer‑7 protected** entry point.
- Segment the network with clear trust boundaries and least‑privilege NSG rules.
- Apply **egress control** to prevent unnecessary outbound internet access.
**Evidence to collect:** Updated architecture diagram; NSG rule summaries; public entry point details; before/after exposure notes.

### Track B – Defender for Cloud & Secure Score
**Objectives:**
- Enable relevant **Defender plans** for workloads (Plan 2).
- Improve **Secure Score** measurably without breaking the app.
- Address **high severity** recommendations where feasible.
**Evidence to collect:** Secure Score before/after; list of high‑severity items tackled; rationale for deferred items.

### Track C – Identity & Access (Zero Trust)
**Objectives:**
- Reduce reliance on local admin and broad RBAC assignments; enforce **least privilege**.
- Use identity‑based access for automation and workloads where applicable (e.g., Managed Identity).
- Document access paths and privilege boundaries.
**Evidence to collect:** RBAC assignments (roles, scopes); identity flow diagram; justification for access decisions.

### Track D – Data Protection & SQL Security
**Objectives:**
- Review database security posture and auditing/threat detection capabilities.
- Protect data at rest and in transit where feasible within time constraints.
- Remove unnecessary inbound/outbound exposure to the database subnet.
**Evidence to collect:** SQL security posture summary; logging/auditing configuration notes; network exposure changes.

### Track E – Monitoring, Detection & Response
**Objectives:**
- Centralize workload and security logs (e.g., Log Analytics/Azure Monitor).
- Ensure alerting exists for meaningful security events (e.g., suspicious sign‑ins, brute‑force, malware).
- Provide one example of how a detection would be investigated.
**Evidence to collect:** Data collection map; alert definitions/screenshots; short investigation narrative.

### Track F – Patch, Baseline & Configuration Management
**Objectives:**
- Identify missing updates and configuration drift on VMs.
- Establish secure defaults (e.g., disk encryption checks, boot protections where relevant).
- Document baseline vs. current state and remediation actions.
**Evidence to collect:** Vulnerability/patch findings; baseline comparison; remediation plan notes.

### Track G – Automation & Policy (Stretch)
**Objectives:**
- Use **Automation** (e.g., start/stop runbooks) with **Managed Identity** and least privilege.
- Apply **Azure Policy** to enforce guardrails (e.g., prevent public IP on VMs, enforce WAF usage for exposed web apps).
- Demonstrate how controls scale to future deployments.
**Evidence to collect:** Automation design; policy definitions/assignments; example remediation.

---

## 4. Scoring Model

Your team’s score is based on:
- **Secure Score delta** (before vs. after)
- **Zero Trust alignment** (design justification)
- **Defender recommendations** reduced (focus on high severity)
- **Architecture clarity** (diagram and narrative)
- **Operational safety** (no hardcoded secrets, least privilege, change tracking)

**Disqualifiers:**
- Security through obscurity (e.g., relying on non‑discoverable ports only)
- Opening broad ports "temporarily" without documented compensating controls
- Breaking application functionality without rollback or justification

---

## 5. Deliverables

Submit the following at the end of Day 2:
1. **Updated architecture diagram** (PNG/SVG/Visio or Markdown‑compatible ASCII)
2. **Secure Score before/after** with a brief explanation of impactful changes
3. **Top 3 risks eliminated** and **Top 2 risks accepted** (with rationale & next steps)
4. **Configuration evidence** (screenshots or exported summaries) for WAF/perimeter, logging/alerts, identity/RBAC, and key Defender settings
5. **Operational notes**: What you automated and what policies you enforced

> Keep submissions concise. Focus on decisions, evidence, and rationale.

---

## 6. Suggested Timeline

**Day 1 (Architecture & Controls):**
- Perimeter & network redesign
- Enable Defender plans & data collection
- Identity/RBAC tightening
- Initial logging & alerting

**Day 2 (Hardening & Evidence):**
- Patch/drift remediation and SQL security posture updates
- Automation & policy guardrails (stretch)
- Finalize HTTPS exposure
- Compile evidence and prepare the final brief

---

## 7. Provided Resources

- Insecure baseline environment (WebVM + SQLVM in `AZTrainingVNet`)
- Access to **Microsoft Defender for Cloud (Plan 2)**
- Guidance on Zero Trust principles (assume breach, verify explicitly, least privilege)
- A rubric for scoring (see Section 4)

---

## 8. Ground Rules

- Do not include secrets (passwords, keys, connection strings) in any artifact.
- Prefer identity‑based access and least privilege RBAC assignments.
- Document assumptions, constraints, and trade‑offs.
- If an app change is required, propose it and justify — prioritize security without breaking functionality.

---

## 9. Submission & Review

- Submit deliverables to the workshop portal or designated Teams channel.
- Be prepared to **defend your architecture** and **trade‑off decisions** in a 5‑minute readout.
- Judges will assess both **technical rigor** and **clarity of reasoning**.

---

## 10. Appendix – Notes for Teams

- You are encouraged to challenge defaults and think adversarially.
- Favor controls that reduce risk broadly (e.g., policy, identity, perimeter).
- If you defer a recommendation, document **why** and **what compensating control** you applied.

Good luck — and have fun hardening the environment! 🔐
