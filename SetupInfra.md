# Infrastructure Setup – Baseline Environment

This repository uses an **insecure-by-design baseline environment** based on the **IAAS2019 demo** created by **Rob Foulkrod**. The environment is intentionally deployed with minimal security so it can be redesigned and hardened during the security hackathon.

**Original sources:**
- Rob Foulkrod – IAAS2019 demo: https://github.com/rob-foulkrod/IAAS2019
- MTT Demo Deploy reference: https://aka.ms/mttdemodeployand

All credit for the original demo goes to the original author. This repository adapts the demo **for Microsoft Technical Training and security education purposes only**.

---

## 1. Purpose of This Setup

The goal of this setup is to provide attendees with a **realistic IaaS workload** that reflects common customer misconfigurations:

- Publicly exposed virtual machines
- Limited network segmentation
- Basic NSG rules
- No enforced Zero Trust principles
- Microsoft Defender for Cloud not yet configured

This environment serves as the **starting point** for the hackathon challenges.

---

## 2. Prerequisites

Before deploying, make sure you have:

- An Azure subscription with **Contributor** or **Owner** permissions
- Azure Cloud Shell access (Bash)
- Azure Developer CLI (`azd`) available

---

## 3. Deploying the Baseline Environment

> ⚠️ Do **not** harden or secure the environment before the workshop starts.

### Step 1 – Open Azure Cloud Shell

Open **Azure Cloud Shell** from the Azure Portal and select **Bash**.

---

### Step 2 – Clone the Demo Repository

```bash
git clone https://github.com/rob-foulkrod/IAAS2019
cd IAAS2019
```

---

### Step 3 – Authenticate Azure Developer CLI

```bash
azd auth login
```

Complete the authentication flow in your browser if prompted.

---

### Step 4 – Deploy the Environment

```bash
azd up
```

You will be prompted to select:
- Subscription
- Azure region
- Environment name

Deployment may take several minutes to complete.

---

## 4. Post‑Deployment Manual Configuration

After deployment, perform the following **manual steps**. These are required to prepare the environment for the workshop.

---

### 4.1 Reset the WebVM Password

For workshop connectivity:

1. Open the **WebVM** in the Azure Portal
2. Select **Reset password**
3. Set a new username and password
4. Store the credentials securely during the workshop

> Do **not** commit credentials to source control or scripts.

---

### 4.2 Create Basic Network Security Groups

Create **two Network Security Groups manually** in the Azure Portal:

- One NSG for the **frontend subnet (WebVM)**
- One NSG for the **backend subnet (SQLVM)**

Attach each NSG to its corresponding subnet. Keep the rules minimal at this stage.

---

### 4.3 Add an Inbound NSG Rule for the WebVM

Add **one manual inbound rule** to the WebVM NSG:

- Purpose: allow basic web connectivity
- Direction: inbound
- Scope: limited to required web access

This rule intentionally leaves room for improvement and will be reviewed during the hackathon.

---

## 5. Microsoft Defender for Cloud – Initial State & Expectations

At the start of the workshop, **nothing is configured in Microsoft Defender for Cloud**. This is intentional.

### What Needs to Be Done First

Participants (or instructors) must at minimum:

- Open **Microsoft Defender for Cloud** in the Azure Portal
- Select the workshop subscription
- Enable **Log Analytics auto-provisioning** (default workspace is sufficient)

> Defender for Cloud does **not** automatically inventory or assess resources without this step.

### Free Assessment – What to Expect

Without enabling paid Defender plans, the **free assessment** provides:

- Resource inventory (VMs, VNets, SQL VMs)
- Initial Secure Score (limited control set)
- Configuration-based security recommendations

It does **not** include:

- Vulnerability scanning
- Threat detection or alerts
- Endpoint or SQL workload protection

### Typical Timing (Realistic)

Security insights are **not immediate**. Expected timelines after enabling Defender for Cloud:

- **5–15 minutes:** Subscription recognized
- **15–60 minutes:** Resources appear in inventory
- **30–90 minutes:** Initial Secure Score is calculated
- **1–6 hours:** First recommendations are visible
- **Up to 24 hours:** Stable and complete posture data

This timing variance is normal and expected.

---

## 6. Workshop Guidance

While Defender for Cloud is discovering resources, participants should:

- Review and redesign the architecture
- Identify obvious risks and trust boundaries
- Plan Zero Trust improvements
- Prepare justification for expected findings

Later Defender insights should **validate assumptions**, not drive design from scratch.

---

## 7. Cleanup

After the workshop:

- Remove deployed resources or delete the resource group
- Confirm no public endpoints remain unintentionally exposed
- Ensure credentials are no longer valid

---

## 8. Attribution

This workshop environment is based on:

- Rob Foulkrod – IAAS2019 demo
  - https://github.com/rob-foulkrod/IAAS2019
- MTT Demo Deploy reference
  - https://aka.ms/mttdemodeployand

This repository adapts the demo for **Azure security workshops and hackathons**.
