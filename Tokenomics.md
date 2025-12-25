# Food DAO Tokenomics

**Version:** 1.1
**Date:** December 25, 2025  
**Authors:** Food DAO Community  
**License:** GPL v3 (Fork Freely)  
**Repository:** [github.com/ideo-mind/food-dao](https://github.com/ideo-mind/food-dao)  
**Contact:** Hiro &lt;hiro@ideomind.org&gt;

---

## Table of Contents

1. [Overview](#overview)
2. [Core Tokens](#core-tokens)
   - [GFD: Governance Food DAO Token](#gfd-governance-food-dao-token)
   - [GLD: Governance Locked Derivatives](#gld-governance-locked-derivatives)
   - [FDT: Food Tokens](#fdt-food-tokens)
3. [Supply & Distribution](#supply--distribution)
4. [Fees & Revenue Model](#fees--revenue-model)
5. [Incentives & Profit Sharing](#incentives--profit-sharing)
6. [Governance Economics](#governance-economics)
7. [Anti-Inflation & Sustainability](#anti-inflation--sustainability)
8. [Projections](#projections)
9. [Risks](#risks)
10. [References](#references)

---

## Overview

Food DAO's tokenomics emphasize scarcity, alignment, and sustainability. With a fixed-supply governance token (GFD) and dynamic derivatives (GLD), the model avoids inflation while rewarding commitment through veToken-style power mechanics (stake × time). Utility tokens (FDT) drive commerce without diluting governance.

**Principles**:

- **Fixed Supply**: No minting; deflation via burns.
- **Time-Weighted Commitment**: GLD locks amplify influence without yield farming speculation.
- **Collaborative Value Capture**: Fees fund validators/treasury; vendors bootstrap via proposals.
- **Forkable Neutrality**: Economics unchanged across forks—protocol profits accrue to participants.

This design supports IRL scalability: From Cannes pilots (€50K volume) to airport chains (€10M+). Full protocol details in [Whitepaper.md](Whitepaper.md); contracts in [Contracts.md](Contracts.md).

---

## Core Tokens

### GFD: Governance Food DAO Token

**Role**: Base layer for voting and staking. ERC-20 standard.

- **Utility**:
  - Proportional voting on proposals (e.g., FDT listings).
  - Staking to mint GLD for enhanced power.
  - Fallback governance if no GLD.

- **Key Features**:
  - Non-transferable during votes (snapshot-style).
  - No direct yields; value accrues via protocol growth.

### GLD: Governance Locked Derivatives

**Role**: ERC-404 semi-fungible positions for time-weighted power. Derived from GFD staking.

- **Utility**:
  - Validator staking (Proof of Power selection).
  - Profit sharing (veToken-weighted: higher power = larger shares).
  - Short-term incentives (e.g., DEX airdrops).

- **Key Features**:
  - **Mint**: Stake GFD × days → GLD with immutable `power_score = stake × days`.
  - **Recompose**: Adjust stake/days along invariant (e.g., trade capital for shorter locks).
  - **Unlock**: Natural expiry or recompose to minimal (e.g., max stake × 1 day).
  - **Short GLD**: 20-day variants for participation (paused on expiry; original owner reclaims GFD).
  - **veToken Play**: Power determines pro-rata profits—no arbitrary duration gates; sustained locks amplify allocation.

GLD blends ERC-20 liquidity (fractions) with ERC-721 uniqueness (full positions as NFTs).

### FDT: Food Tokens

**Role**: ERC-404 utility for fractional food commerce. Per-food-type contracts (e.g., ShawarmaFDT).

- **Utility**:
  - P2P purchases/redeems (1 unit = full NFT meal; <1 = fungible portion).
  - DEX liquidity (FDT/USDC pairs).

- **Key Features**:
  - Dynamic supply (event-capped; burn on redeem).
  - Metadata-embedded (dietary/allergens via IPFS).
  - Anti-speculation: Restricted sells; no GFD pairing.

FDT is isolated—commerce-focused, non-governance.

---

## Supply & Distribution

All tokens prioritize community/merchants over speculation.

### GFD Supply & Distribution

| Allocation       | Percentage | Amount (GFD) | Vesting/Cliff | Description                                       |
| ---------------- | ---------- | ------------ | ------------- | ------------------------------------------------- |
| **Merchants**    | 40%        | 40M          | None          | Airdropped/earned via FDT sales volume.           |
| **Community**    | 20%        | 20M          | None          | Early adopters/events (e.g., Cannes registrants). |
| **Contributors** | 20%        | 20M          | 3-year linear | Devs, auditors (non-transferable until vested).   |
| **Treasury**     | 15%        | 15M          | Governed      | Upgrades, partnerships (GFD votes).               |
| **Liquidity**    | 5%         | 5M           | None          | Initial DEX pools (GFD/USDC).                     |
| **Total**        | **100%**   | **100M**     | -             | Fixed; no inflation/minting.                      |

- **Release Schedule**: 50% community/merchants at TGE; rest phased over 12 months.
- **Burns**: 20% of fees; unclaimed treasury after 2 years.

### GLD Supply

- **Dynamic**: 1:1 derived from staked GFD (no new supply).
- **Cap**: Indirectly limited by GFD total.
- **Distribution**: User-minted; short GLD via DEX rewards (e.g., 1% of buy volume as 20-day GLD).

### FDT Supply

- **Dynamic**: Per-FDT (e.g., 1K units for event shawarmas).
- **Distribution**: Vendor-issued post-approval; burned on redeem (deflationary per item).
- **No Global Cap**: Event/fork-specific; aggregated protocol volume drives value.

---

## Fees & Revenue Model

**Transfer Fees** (FDT P2P): 1% per transfer (immutable).

- **Split**: 50% to validators (random select), 30% treasury, 20% burn (GFD deflation).

**DEX Fees** (FDT/USDC): 1% per swap (LP-priority).

- **Split**: 50% LPs, 30% validators, 20% burn.
- **Gated Sells**: Additional listing incentives (suboptimal rates for delegates).

**Listing Fees**: Vendors pay fixed GFD/USDC to propose FDTs → Direct to profit pool.

**Revenue Streams**:

- **Primary**: Transaction/DEX volume (projected 70% from events).
- **Secondary**: Listing fees (10%); short GLD airdrops (20%).
- **Tertiary**: Treasury yields (governed, e.g., stablecoin staking).

All fees autonomous—smart contracts enforce splits.

---

## Incentives & Profit Sharing

### Validator Incentives

- **Random Selection**: 0.2% tx fees to picked validator (equal chance from 200 actives).
- **Power Challenges**: Maintain set via GLD stake; winners earn entry priority.

### Profit Sharing (veToken Mechanics)

- **Pool**: Aggregated fees + listing revenue (USDC/NFTs).
- **Distribution**: Epochly (weekly); pro-rata to GLD power_scores.
  - Formula: Share = (Your power / Total power) × Pool.
  - Short GLD: Temporary perks (e.g., vote boosts); no full shares.
- **Alignment**: Longer/higher stakes amplify power—rewards commitment without inflation.

### DEX Incentives

- **Buyer Rewards**: 1% of volume as short GLD (20-day; boosts engagement).
- **LP Boosts**: Extra yields for FDT pools (governed via GFD votes).

No inflationary emissions: Incentives from real revenue.

---

## Governance Economics

**Voting**: Proportional to GFD holdings (1:1).

- **Proposals**: Vendor FDT listings (pay fee); community params.
- **Thresholds**:
  - Min voters: 2 members.
  - Pass: ≥0.15 support, ≥0.51 quorum (participation).
  - Duration: 3 days.
  - Early Enact: Enabled; ≥0.67 support, ≥0.65 quorum.
- **GLD Role**: Amplifies via power (e.g., for validators); fallback to raw GFD.

**Economics**: Low thresholds encourage participation; fees bootstrap treasury for grants.

---

## Anti-Inflation & Sustainability

- **Deflation**: 20% fee burns (GFD/FDT); unclaimed GLD reverts to GFD.
- **No Yield Farming**: veToken power favors locks over churn.
- **Sustainability**: Waste reduction (40%) + low fees (0.1-1%) drive adoption; treasury funds audits/upgrades.
- **Fork Resilience**: Economics identical across instances—network effects compound.

---

## Projections

Conservative estimates based on pilots (Cannes: €50K volume).

| Year          | Volume (€M) | Fees (€K) | Burned (%) | Validators Rewarded | Key Driver                    |
| ------------- | ----------- | --------- | ---------- | ------------------- | ----------------------------- |
| **1 (2026)**  | 0.6         | 8         | 20%        | 200 active          | Events (Cannes/Lisbon/Mumbai) |
| **2 (2027)**  | 15          | 200       | 25%        | 500 queued          | Airports (Mumbai pilots)      |
| **3 (2028+)** | 50+         | 450+      | 30%        | Scaled              | Chains (Starbucks/McD)        |

- **Assumptions**: 10% adoption in events; 1% protocol capture.
- **Upside**: Multi-chain → 2x volume; DEX TVL >€1M.

(See [Whitepaper.md](Whitepaper.md) for GTM details.)

---

## Risks

| Risk                  | Impact | Mitigation                                      |
| --------------------- | ------ | ----------------------------------------------- |
| **GFD Concentration** | High   | Vesting + proportional votes; quadratic future. |
| **GLD Gaming**        | Medium | Immutable power; recompose gas costs.           |
| **Low Liquidity**     | Medium | Initial 5% pools; listing incentives.           |
| **Adoption Lag**      | High   | Event forks; short GLD airdrops.                |

Detailed in [Whitepaper.md](Whitepaper.md#risks--mitigations).

---

## References

- veToken Model: [Curve Finance Docs](https://resources.curve.finance/ru/pdf/curve-dao.pdf)
- ERC-404: [Pandora Labs Repo](https://github.com/Pandora-Labs-Org/erc404)
- Projections Basis: FAO Waste Data; ETHGlobal Metrics.

**Discuss:** Hiro &lt;hiro@ideomind.org&gt;

---

_Open-source tokenomics. Propose changes via GFD vote. Iterate with us._
