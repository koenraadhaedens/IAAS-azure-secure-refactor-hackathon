# Infrastructure Setup – Baseline Environment

This repository uses an **insecure-by-design baseline environment** based on the **IAAS2019 demo** created by **Rob Foulkrod**. The environment is intentionally deployed with minimal security so it can be redesigned and hardened during the security hackathon.

**Original sources:**
- Rob Foulkrod – IAAS2019 demo: https://github.com/rob-foulkrod/IAAS2019
- MTT Demo Deploy reference: https://aka.ms/mttdemodeploy

All credit for the original demo goes to the original author. This repository adapts the demo **for Microsoft Technical Training and security education purposes only**.

---

## 1. Purpose of This Setup

The goal of this setup is to provide attendees with a **realistic IaaS workload** that reflects common customer misconfigurations:

- Publicly exposed virtual machines
- Limited network segmentation
- Basic NSG rules
- No enforced Zero Trust principles
- Defender for Cloud not yet fully optimized

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

## 5. Expected Baseline Architecture

After setup, the environment typically contains:

- One virtual network with two subnets
- A WebVM reachable from the internet
- A SQLVM reachable from the WebVM
- Basic NSG configuration
- No Zero Trust enforcement

This is an **intentional insecure baseline**.

---

## 6. Configuration Guidance

During the workshop, participants should:

- Avoid hardcoded secrets or shared credentials
- Use identity-based access wherever possible
- Maintain application availability while improving security
- Focus on high-risk findings surfaced by Defender for Cloud
- Document assumptions and trade-offs

The goal is to **design and justify security improvements**, not to follow prescriptive instructions.

---

## 7. Cleanup

After the workshop:

- Remove the deployed resources or delete the resource group
- Ensure no public endpoints remain unintentionally exposed
- Verify credentials are no longer valid

---

## 8. Attribution

This workshop environment is based on the following resources:

- Rob Foulkrod – IAAS2019 demo
  - https://github.com/rob-foulkrod/IAAS2019
- MTT Demo Deploy reference
  - https://aka.ms/mttdemodeployand

This repository adapts the demo for **Azure security workshops and hackathons**.
