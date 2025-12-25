# Food DAO: A Decentralized Protocol for Semi-Fungible Food Commerce

**Version:** 1.2
**Date:** December 25, 2025  
**Authors:** Food DAO Community (Open-Source Contributors)  
**License:** GPL v3 (Fork Freely)  
**Repository:** [github.com/ideo-mind/food-dao](https://github.com/ideo-mind/food-dao)  
**Contact:** Hiro &lt;hiro@ideomind.org&gt;

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [The Problem: Friction in Global Food Commerce](#the-problem)
3. [The Vision: Decentralized Food Commerce](#the-vision)
4. [Protocol Architecture](#protocol-architecture)
5. [Core Components](#core-components)
   - [FDT: Food Tokens (ERC-404)](#fdt-food-tokens-erc-404)
   - [GFD: Governance Token](#gfd-governance-token)
   - [GLD: Locked Derivatives (ERC-404)](#gld-locked-derivatives-erc-404)
   - [DEX & Restricted Liquidity](#dex--restricted-liquidity)
   - [Validators & Proof of Power](#validators--proof-of-power)
6. [User Flows](#user-flows)
7. [Tokenomics](#tokenomics) _(See [Tokenomics.md](Tokenomics.md) for details)_
8. [Smart Contracts](#smart-contracts) _(See [Contracts.md](Contracts.md) for code & audits)_
9. [Go-to-Market Strategy](#go-to-market-strategy)
10. [Risks & Mitigations](#risks--mitigations)
11. [Roadmap](#roadmap)
12. [Conclusion](#conclusion)
13. [References](#references)

---

## Executive Summary

Food DAO is an open, decentralized protocol built on Ethereum-compatible chains (e.g., Polygon) that reimagines food commerce as a semi-fungible, P2P DeFi primitive. Using ERC-404 tokens for fractional ownership of food items, it enables seamless exchanges—from full meals (NFTs) to shared portions (fungible fractions)—while reducing waste through on-chain demand signals and anti-hoarding mechanics.

The protocol facilitates a collaborative ecosystem where:

1. **Vendors** issue FDTs, propose them for approval, and pay a listing fee to the protocol profit pool.
2. **Community & Validators** vote proportionally with GFD on proposals (e.g., FDT listings), using structured thresholds for fairness and efficiency.
3. **Profits** are distributed via veToken-style power play, rewarding committed participants through GLD-weighted mechanisms.

**Key Tokens**:

- **FDT (Food Tokens)**: ERC-404 utility for food purchases (fractional, redeemable IRL).
- **GFD (Governance Food DAO)**: Fixed-supply (100M) base token for voting and staking into GLD.
- **GLD (Governance Locked Derivatives)**: ERC-404 positions for time-weighted power in validators and profit sharing.

**Innovations**:

- **Semi-Fungibility**: Buy/sell portions of items like DeFi liquidity.
- **Restricted DEX**: Open buys, gated sells to prevent rugs/inflation.
- **Proof of Power Validators**: Random selection for fair rewards; power-weighted challenges for security.
- **Forkable Design**: Deploy event-specific instances in minutes (Uniswap-inspired).

Launched for IRL events like ETHGlobal Cannes (April 2026), Food DAO targets $600K Year 1 volume, scaling to airports and chains. It's not a company—it's a protocol. Fork it, govern it, eat with it.

For implementation details, see [Readme.md](Readme.md); token models in [Tokenomics.md](Tokenomics.md); contracts in [Contracts.md](Contracts.md). ERC-404 implementation based on [Pandora Labs' ERC404.sol](https://github.com/Pandora-Labs-Org/erc404/blob/main/contracts/ERC404.sol).

---

## The Problem: Friction in Global Food Commerce

Food commerce, especially at international events, is plagued by inefficiencies:

### For Event Organizers

- **Inventory Blindness**: 30-40% waste from unknown demand (e.g., over-ordered croissants at conferences).
- **Payment Silos**: Forex fees (3-10%) and delays for global attendees.
- **Dietary Gaps**: Allergen/vegan verification buried in PDFs or language barriers.

### For Merchants & Vendors

- **Settlement Drag**: 2-3% card fees + 3-5 day delays; no real-time demand.
- **Scale Barriers**: Onboarding per-venue; cash hoarding in pop-ups.
- **Rug Risks**: Informal trades lead to disputes or inflation in informal "tokens" (e.g., event vouchers).

### For Users (Hackers, Travelers)

- **Checkout Chaos**: Lines, incompatible payments (no UPI in Cannes), trust issues ("Is this really gluten-free?").
- **Waste Contribution**: Overbuying leads to 40% event food landfill.
- **Inclusion Gaps**: 60% of airport traffic is international—yet payments favor locals.

**Market Size**: Global event food = $100B+ annually; waste alone = $1T globally. Blockchain solves transparency, but prior attempts (e.g., IBM Food Trust) ignore P2P fungibility.

Food DAO: Fractional tokens + DeFi liquidity = 40% waste reduction, 1% fees, verifiable metadata.

---

## The Vision: Decentralized Food Commerce

Food DAO empowers a self-sustaining ecosystem of participants:

1. **Vendors** drive issuance by creating and proposing FDTs for specific food items, paying a listing fee that funds the protocol's profit pool.
2. **Community & Validators** ensure quality through proportional GFD voting on proposals, with clear thresholds for passage and early enactment to balance speed and consensus (minimum 2 voting members; 0.15 minimum support to pass; 0.51 minimum participation/quorum; 3-day vote duration; early enactment enabled at 0.67 support/0.65 quorum).
3. **Profits** flow back via veToken-style mechanics, where GLD power (stake × time) determines shares, rewarding sustained participation.

Inspired by Paytm's Mumbai vendor QR revolution and Uniswap's permissionless pools, Food DAO makes food "DeFi-native": Trade fractions, redeem IRL, govern collectively.

---

## Protocol Architecture

Food DAO's stack is modular and forkable:

```
┌─────────────────────────────┐
│        User Layer           │
│  (App: React/Wagmi, AI Chat)│
│  - P2P QR, DEX Swaps        │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│     Blockchain (Polygon)    │
└──────────────┬──────────────┘
               │
    ┌──────────┼──────────┬─────┐
    │          │          │     │
    ▼          ▼          ▼     ▼
┌────┐ ┌──────┐ ┌──────┐ ┌─────┐
│FDT │ │ GFD  │ │ GLD  │ │DEX/ │
│ERC-│ │ERC-20│ │ERC-404│ │Val  │
│404 │ │Fixed │ │Locks │ │idator│
└────┘ └──────┘ └──────┘ └─────┘
```

- **Layer 1**: Immutable core (fees, invariants).
- **Governance**: GFD-weighted votes on FDT listings, params.
- **Security**: Proof of Power validators (200 max, contestable).

(See [Contracts.md](Contracts.md) for Solidity specs.)

---

## Core Components

### FDT: Food Tokens (ERC-404)

Semi-fungible ERC-404 tokens per food type (e.g., ShawarmaFDT.sol):

- **1 Unit = Full Item** (auto-mints NFT for redemption).
- **Fractions <1** (fungible for sharing, e.g., 0.5 shawarma).
- **P2P Transfers**: 1% fee; redeem via vendor sig (burns tokens).
- **Metadata**: JSON with allergens, calories, dietary flags (IPFS-hosted).
- **Anti-Hoarding**: Per-wallet caps (governed).

Issuance: Vendor-proposed and community-approved; listing fee to profit pool.

### GFD: Governance Token

- **Supply**: 100M fixed (no inflation).
- **Utility**: Proportional voting on proposals; stake to mint GLD.
- **Distribution**: 40% merchants, 20% community, 20% contributors (vested), 15% treasury, 5% liquidity.

### GLD: Locked Derivatives (ERC-404)

veToken-inspired positions:

- **Mint**: Stake GFD × days → GLD with immutable power_score = stake × days.
- **Recompose**: Adjust stake/days along invariant (e.g., 1 GFD × 1000 days ↔ 1000 GFD × 1 day).
- **Unlock**: Expiry or recompose to minimal lock.
- **Short-Term**: 20-day GLD airdrops via DEX (paused on expiry; original owner reclaims GFD).
- **Profits**: Power-weighted shares of fees/USDC/NFTs via veToken mechanics (higher power = larger pro-rata allocation).

GLD enables committed influence: Stake × time = power.

### DEX & Restricted Liquidity

Uniswap V3-inspired, FDT/USDC pools only:

- **Open Buys**: Anyone swaps stables → FDT.
- **Gated Sells**: Requires sufficient stake and thresholds to list or join pools (or delegate: suboptimal rates, no stake).
- **Issuer Accountability**: Post-vote stake; low volume → slash/suspend.
- **Fees**: 1% (LP-priority); queue for buybacks.

Prevents rugs: Permissioned pools, validated reserves.

### Validators & Proof of Power

Contestable set for profit distribution:

- **Cap**: 200 actives (ranked by GLD power sum).
- **Selection**: Pure random from actives per tx/epoch (fair, equal chance).
- **Challenges**: Power-weighted—outpower or invalidate expired GLD to replace.
- **Rewards**: 0.2% tx fees to selected; epoch profits to GLD holders (veToken power play).
- **Lifecycle**: Queue entry, self-adjust (recompose/add GLD), partial/full exit.

Ensures decentralization: Random rewards, merit-based security.

---

## User Flows

### Buyer: Acquire & Redeem Half Shawarma

1. Swap USDC → 0.5 FDT (open DEX buy).
2. P2P transfer to vendor (1% fee).
3. Vendor preps partial; redeems (burns FDT).
4. (Optional) List remainder on DEX (if thresholds met).

### Vendor: Issue FDT & Liquidate

1. Issue FDT; propose to governance (pay listing fee to profit pool).
2. Community votes (GFD proportional; min 2 voters; pass if ≥0.15 support, ≥0.51 quorum over 3 days; early enact at ≥0.67 support/≥0.65 quorum).
3. Approved? List on DEX pool.
4. Accept P2P; redeem.
5. Sell: Private buyback or gated list (stake/delegate).

### Community Member: Vote on Proposal

1. Hold GFD; review vendor proposal (e.g., new FDT).
2. Vote for/against (proportional to GFD; min 2 voters total).
3. If passes (0.15 support, 0.51 quorum in 3 days), FDT live.
4. Early? Boost support to 0.67/0.65 quorum for instant enact.

### Validator: Stake for Rewards

1. Stake GFD → Mint GLD (e.g., power=3650).
2. Stake GLD to validators → Enter set/queue.
3. Randomly selected? Earn 0.2% fees.
4. Epoch profits: veToken power-weighted share.

(See full flows in [Readme.md](Readme.md).)

---

## Tokenomics

Food DAO's economics prioritize alignment and scarcity. Full details in [Tokenomics.md](Tokenomics.md), including:

| Token   | Supply              | Utility                           | Incentives                        |
| ------- | ------------------- | --------------------------------- | --------------------------------- |
| **GFD** | 100M fixed          | Proportional voting/staking       | Vested contributors; no inflation |
| **GLD** | Dynamic (positions) | Time-power for validators/profits | Short airdrops; veToken shares    |
| **FDT** | Per-food dynamic    | Food fractions                    | Burn on redeem; DEX liquidity     |

- **Fees**: 1% transfers + 1% DEX → 50% validators, 30% treasury, 20% burn.
- **Projections**: Year 1: €8K fees (Cannes pilot); Year 3: €450K+ (chains/airports).

---

## Smart Contracts

Immutable, audited core (OpenZeppelin base + Pandora ERC-404):

- **FoodERC404.sol**: FDT mint/transfer/redeem.
- **GLDERC404.sol**: GLD positions (invariant enforcement).
- **FoodDEX.sol**: Restricted pools/queues.
- **ValidatorPoP.sol**: Random select, power challenges.
- **Governance.sol**: GFD-weighted proposals (min 2 voters; 0.15 support, 0.51 quorum, 3-day duration; early: 0.67 support/0.65 quorum).

Deployment: Factory for forks; Polygon for low gas. Full code/audits in [Contracts.md](Contracts.md).

---

## Go-to-Market Strategy

**Phase 1: Events (Q1-Q2 2026)**

- Cannes Pilot (April): 500 users, €50K volume, 40% waste cut.
- Lisbon/Devcon Mumbai: Scale to 5K users.

**Phase 2: Airports/Chains (Q3 2026+)**

- Mumbai Terminal: 50 vendors, fractional tourist shares.
- Partnerships: Starbucks India pilots.

Metrics: 95% forecast accuracy; <10% validator churn. (Details in [Readme.md](Readme.md).)

---

## Risks & Mitigations

| Risk                       | Probability | Mitigation                                 |
| -------------------------- | ----------- | ------------------------------------------ |
| **Oracle/Expiry Failures** | Medium      | Chainlink for timestamps; on-chain checks. |
| **Vote Manipulation**      | Low         | Proportional GFD; min voters/quorum.       |
| **Low Adoption**           | Medium      | Event forks + DEX airdrops.                |
| **Regulatory (MiCA/PMLA)** | Low         | Utility focus; no custody.                 |

Full analysis in [Tokenomics.md](Tokenomics.md).

---

## Roadmap

- **Q1 2026**: MVP Deploy (Cannes); GLD/validator audits.
- **Q2 2026**: Lisbon Pilot; DEX v1.
- **Q3 2026**: Multi-chain; Airport Rollout.
- **2027+**: Supply Chain Hooks; Mainstream Chains.

Community-driven: Vote upgrades via GFD.

---

## Conclusion

Food DAO democratizes food commerce: Fractional, fair, forkable. Vendors propose, community votes with GFD, power shares profits—via GLD veToken play. No more waste, no more walls— just P2P plates and decentralized dinners.

Join: Stake GFD, vote proposals, fork the future.

**Next: [Readme.md](Readme.md) for quickstart; [Tokenomics.md](Tokenomics.md) for models; [Contracts.md](Contracts.md) for devs.**

---

## References

- ERC-404 Standard: [Pandora Labs ERC404.sol](https://github.com/Pandora-Labs-Org/erc404/blob/main/contracts/ERC404.sol)
- veToken Patterns: Curve Finance Whitepaper
- PoS Contestability: Ethereum Validator Docs
- Food Waste Stats: FAO Reports (2023)

**Contact:** Hiro &lt;hiro@ideomind.org&gt;

---

_This whitepaper is open-source. Contributions welcome via GitHub. Version 1.2—Iterate with us._
