# Legal & Regulatory Framework: Food DAO Protocol

**Last Updated:** December 25, 2025  
**Document Type:** Compliance Reference & Protocol Position  

---

## 1. Executive Summary & Legal Classification

Food DAO is an **open-source, immutable software protocol** designed to facilitate transparent, peer‑to‑peer commerce for food‑related tokens (FDTs). It is **not** a centralized service provider, financial intermediary, or payment processor.

The protocol operates under the following legal classification principles:

### A. Protocol Classification

Food DAO is best understood as **“infrastructure software”** or a **“publisher of code”**:

- **No operator control**  
  - Core smart contracts are deployed immutably.  
  - The code is open‑source (GPL‑style license) and can be freely forked.

- **No custody**  
  - The protocol never takes possession of, or signatory power over, user funds.  
  - Users transact directly from self‑custody wallets (e.g., MetaMask, hardware wallets) into immutable contracts.

- **No discretionary decision‑making**  
  - Economic parameters such as the DEX fee (1%) and listing flows are encoded in contracts or modified only via decentralized governance.  
  - There is no back‑office or operations team deciding transaction outcomes on a case‑by‑case basis.

### B. Not a VASP (Virtual Asset Service Provider)

Under FATF standards and similar national regimes (e.g., India’s PMLA VASP framework), a VASP is typically an entity that **for or on behalf of another person**:

- exchanges virtual assets,  
- transfers virtual assets,  
- safekeeps/administrates virtual assets or instruments to control them, or  
- participates in and provides financial services related to an issuer’s offer or sale of a virtual asset.

Food DAO, as a protocol, does **not** meet these criteria:

- It does **not hold private keys** or operate custodial wallets.  
- It does **not execute transfers on behalf of users** outside of deterministic smart contract logic.  
- It does **not run a custodial exchange or hosted wallet**.  
- It does **not promise or manage financial returns** for users.

Participants who build businesses **on top of** the protocol (e.g., vendors, integrators) are responsible for evaluating their own status under local VASP, payments, and tax rules.

---

## 2. Compliance Architecture & Design Choices

### A. Merchant Verification (KYB), Not User KYC

The protocol architecture explicitly separates:

- **Vendors (merchants)**:  
  - May be optionally onboarded via **Know Your Business (KYB)** procedures by front‑end operators or integrators.  
  - Typical KYB data: legal name, registration number, tax ID, business address, basic ownership details.

- **End‑users (consumers / attendees)**:  
  - Interact from self‑custody wallets.  
  - No personal data is required at the protocol layer.  
  - No on‑chain KYC, no identity tracking encoded in core contracts.

**Rationale:**

- KYB for vendors is standard B2B risk control (fraud, quality, sanctions).  
- **User privacy is preserved**: the protocol itself never stores, processes, or relies on personal data.

Any third‑party interface that chooses to perform KYC on users (e.g., a regulated exchange or compliant front‑end) does so **on its own account** and not as a protocol requirement.

### B. User Privacy & Data Handling

- The protocol does **not** store names, emails, phone numbers, or any other personal data.  
- All activity visible at the protocol layer is standard public blockchain data:
  - wallet addresses,  
  - transaction hashes,  
  - token balances,  
  - and contract events.

Given this:

- The protocol layer does **not act as a data “controller” or “processor”** under GDPR‑style frameworks.  
- Any GDPR or privacy compliance obligations fall on:
  - front‑end operators that collect user data voluntarily,  
  - vendors that maintain customer records off‑chain,  
  - or other intermediaries building on top of the protocol.

### C. Token Classification

The protocol uses three main token concepts:

#### 1. GFD – Governance Token

- **Role:**  
  - GFD is the **governance token** of the protocol.  
  - It is used to vote on:
    - FDT listing proposals submitted by vendors.  
    - Certain protocol configuration parameters.

- **Economic Rights:**  
  - GFD has a **fixed supply** (no inflation).  
  - Holding GFD does **not** by itself entitle holders to automatic profit share, dividends, or guaranteed yield.  
  - Any economic benefit is indirect and depends on active participation and the evolving market value, not a contractual promise.

- **Regulatory View (High‑Level):**  
  - Designed as a **governance / utility token**, not as a share or debt instrument.  
  - No “investment contract” is offered by the protocol; there is no promise that “others’ efforts” will generate returns for passive GFD holders.

#### 2. GLD – Governance Lock Derivative (ERC‑404‑based)

- **Role:**  
  - GLD represents **time‑locked commitment of GFD** using an ERC‑404‑style structure.  
  - Each GLD position encodes a **power score**, typically:  
    `power_score = (GFD staked at issue) × (lock duration in days at issue)`  
  - GLD is used for:
    - validator selection (Proof‑of‑Power model),  
    - weighting of certain protocol rewards and responsibilities.

- **Nature:**  
  - GLD is not a separate free‑floating financial asset in the same sense as GFD; it is an **on‑chain representation of a locked GFD position**.  
  - Its main function is *governance and security weighting*, not fundraising or speculative issuance.

- **Regulatory View:**  
  - Intended as an internal staking / reputation mechanism rather than a tradable investment instrument.

#### 3. FDT – Food Delivery / Food Descriptor Token

- **Role:**  
  - FDTs are **vendor‑issued tokens**, typically representing:
    - rights to purchase or redeem specific food items, or  
    - standardized “units” of product/service within a venue or event.

- **Economic Rights:**  
  - FDTs function as **digital vouchers / coupons**, not as passive financial assets.  
  - Their main use is **consumption** (redeeming for goods), not hoarding or speculation.

- **Regulatory View:**  
  - Analogous to:
    - prepaid event vouchers,  
    - meal coupons,  
    - closed‑loop stored‑value instruments.  
  - In most jurisdictions, such instruments are treated as **consumer/product tokens**, not securities.

Local law in each jurisdiction always prevails; implementers must review FDT usage with local counsel.

---

## 3. Governance, Control, and Responsibility

### A. Governance via GFD (Proportional Voting)

- **Voting Power:**  
  - 1 GFD = 1 vote (proportional model, by default).

- **Default Governance Parameters (at protocol level):**  
  - Minimum voting members: 2 GFD holders.  
  - Minimum support to pass a vote: 0.15 (15% “yes” among cast votes).  
  - Minimum participation (quorum): 0.51 (51% of total GFD supply).  
  - Vote duration: 3 days.  
  - Early enactment:
    - Enabled with higher support and quorum thresholds:
      - Early support: 0.67.  
      - Early quorum: 0.65.

> Note: These values are **protocol defaults** and are subject to adjustment by governance as participation patterns become clear. They demonstrate a bias toward high participation and strong consensus for changes.

- **Scope of Governance:**  
  - Vendor FDT listing approvals / delistings.  
  - Certain configurable parameters (e.g., thresholds, potentially fee destination split).  
  - Governance **cannot**:
    - Seize user funds.  
    - Arbitrarily change balances.  
    - Override the core 1% DEX fee rationale once hardcoded into specific contracts.

### B. No Admin Keys / “God Mode”

- Core contracts are deployed without privileged admin functions that can:
  - Halt the entire protocol,  
  - Override balances,  
  - Or upgrade arbitrary logic without a transparent governance process.

- Where upgradability or emergency hooks are necessary (e.g., for front‑end or off‑chain services), they are:
  - Explicitly scoped,  
  - Separately documented,  
  - And not used to silently compromise protocol guarantees.

### C. Validator Selection (Proof of Power)

- Validators are chosen based on **GLD power score** and protocol rules.  
- The active validator set is capped (e.g., 200 validators maximum).  
- Validators earn a share of protocol‑level rewards (e.g., a fraction of transaction or redemption fees) according to transparent, deterministic logic.

This design spreads operational responsibility across many independent actors and avoids direct, centralized operational control by any developer team.

---

## 4. Forkability & Open‑Source Position

Food DAO is intentionally designed to be **forkable** and **issuer‑agnostic**:

- **Open Codebase:**  
  - The code is hosted in a public repository (e.g., `github.com/ideo-mind/food-dao`).  
  - Anyone can:
    - Inspect,  
    - Fork,  
    - Deploy modified versions.

- **Multiple Deployments:**  
  - There is no “official” or monopoly deployment from a legal perspective.  
  - Communities, events, or regions can run their own instances with their own parameter choices.

- **Implication for Liability:**
  - Original authors publish a toolkit.  
  - Operators who **deploy** and **configure** instances in their jurisdictions are responsible for:
    - Compliance with local laws (payments, consumer protection, tax, licensing).  
    - Terms & conditions of usage.

The protocol itself is **content‑agnostic** and **jurisdiction‑agnostic**; it does not enforce specific pricing, taxation, or consumer rules—it only provides a programmable structure.

---

## 5. Jurisdictional Notes (Non‑Exhaustive, High‑Level)

> This section is **illustrative only** and not a substitute for localized legal advice.

### A. India

- **PMLA / VASP:**  
  - The protocol is non‑custodial and open‑source.  
  - It does not perform exchange or transfer “on behalf of others” outside the deterministic contract logic.  
  - Implementers in India (e.g., a company running a branded front‑end) should assess whether:
    - They are engaging in VDA services,  
    - They must register as a reporting entity,  
    - They must implement KYC/AML for FIAT on‑ and off‑ramps.

- **GST / Income Tax:**  
  - Vendors and integrators must treat FDT redemptions like any other taxable sale of goods/services.  
  - The protocol does not compute or remit taxes; it only records on‑chain movements.

### B. EU (MiCA, GDPR)

- **MiCA:**  
  - FDTs are structurally closer to **utility tokens/digital vouchers** than to e‑money tokens or asset‑referenced tokens, assuming they:
    - Represent consumable rights,  
    - Are not marketed as financial investments.

- **GDPR:**  
  - The protocol layer does not collect personal data.  
  - GDPR compliance, if any, attaches to:
    - Front‑end operators,  
    - Commercial entities issuing invoices or handling customer records off‑chain.

### C. USA

- **SEC / CFTC:**  
  - GFD is intended as a governance utility token, not a share or investment contract.  
  - No capital raise or investment scheme is conducted **by the protocol** itself.  
  - Operators in the US must ensure:
    - They do not run unregistered exchanges, broker‑dealers, or ATSs if they create secondary markets.  
    - They properly handle money transmitter / MSB status when handling fiat/stablecoin conversion.

---

## 6. Responsibilities of Third‑Party Operators

Any third party using the Food DAO protocol to build products or services should:

1. **Obtain Legal Advice**  
   - Assess their own business model (custodial vs non‑custodial, FIAT ramps, etc.).  
   - Determine licensing and registration obligations (payments, money transmission, VASP, etc.).

2. **Publish Terms of Use**  
   - Clearly state to their users:
     - What they control and what they don’t.  
     - What risks users bear.  
     - Refund / dispute policies for off‑chain issues (wrong delivery, poor quality, etc.).

3. **Maintain Compliance with Local Law**  
   - Tax collection and remittance.  
   - Consumer protection rules.  
   - Health & safety regulations for food vendors.

Food DAO, as a protocol, does **not** manage these obligations on behalf of any operator.

---

## 7. Disclaimers

### A. For Developers & Contributors

- This repository and associated smart contracts are provided **“as is”**, without warranty of any kind.  
- By contributing code, you are:
  - Publishing open‑source software,  
  - Not entering a partnership or joint venture with deployers or operators.  
- You are responsible for ensuring your contributions comply with your local laws (e.g., export controls, sanctions, etc.).

### B. For Users

- Interaction with the protocol is **at your own risk**.  
- You are solely responsible for:
  - Safeguarding your wallet keys,  
  - Understanding transaction costs (gas fees),  
  - Complying with tax and reporting requirements in your jurisdiction.

No one can recover lost private keys or reverse finalized blockchain transactions.

### C. For Regulators & Authorities

- Food DAO’s maintainers are open to good‑faith dialogue and clarification requests about:
  - The technical properties of the protocol,  
  - The intended usage patterns,  
  - Possible integration within compliant business architectures.

This document reflects the authors’ **good‑faith understanding** as of the “Last Updated” date and may evolve as regulations and case law develop.

---

**This document is informational only and does not constitute legal advice.  
All implementers and operators should seek independent legal counsel in their jurisdiction.**
