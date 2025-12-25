# Food DAO Protocol

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

**Decentralized QR-Code Inspired Payment Protocol for Food Commerce**

Food DAO is an open-source, forkable protocol for **instant, low-fee food payments** using ERC-404 tokens. Inspired by India's UPI QR codes (street vendors scan-to-pay) and Argentina's Mercado Pago QR system, users scan a vendor's wallet QR, transfer fractional FDTs (Food Tokens), and redeem IRL. Vendors issue FDTs, community votes via GFD, committed holders earn via GLD power. Cut waste with on-chain forecasts, slash fees (1% protocol on payments + 1% DEX), built for events like ETHGlobal Cannes.

- **No KYC/KYB**: Pure DeFi—self-sovereign.
- **veToken-Inspired**: Stake × time = power for profits/validators.
- **Restricted DEX**: Open buys, gated sells to prevent rugs.

**Docs**: [Whitepaper.md](Whitepaper.md) | [Tokenomics.md](Tokenomics.md) | [Contracts.md](Contracts.md)

---

## Features

- **Fractional Payments**: ERC-404 FDTs—full meals as NFTs, portions as fungible (e.g., pay 0.5 for half shawarma).
- **QR-Code Inspired Payments**: User scans vendor wallet QR → instant P2P FDT transfer (1% protocol fee) → vendor redeems (burns for IRL handoff).
- **Governance**: GFD proportional votes (min 2 voters; 0.15 support, 0.51 quorum, 3 days; early enact at 0.67/0.65).
- **DEX Liquidity**: FDT/USDC pools (1% fees); gated sells via stakes/thresholds.
- **Validators**: 200-cap Proof of Power—random selection (fair rewards), power-weighted challenges.
- **Waste Reduction**: On-chain demand forecasts; per-wallet caps.
- **Forkable**: One-click event instances (Polygon deploy).

Powered by [Pandora Labs ERC-404](https://github.com/Pandora-Labs-Org/erc404/blob/main/contracts/ERC404.sol).




















---

## Architecture Overview

```
┌─────────────────────────────┐
│        User Layer           │
│  (App: React/Wagmi, AI Chat)│
│  - Wallet QR Scan/Generate  │
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

- **Core Libs**: OpenZeppelin (access), Pandora ERC-404.
- **Off-Chain**: IPFS for FDT metadata; Chainlink for timestamps (validators).

---

## License

GPL v3—fork freely, but share alike. No warranties; use at own risk.

---

## Community & Support

- **Github**: [@ideomind/food-dao](https://github.com/ideo-mind/food-dao/issues/new)
- **X/Twitter**: [@Ideomind](https://x.com/ideomind)
- **Email**: Hiro &lt;hiro@ideomind.org&gt; (Partnerships).
- **Events**: ETHGlobal Cannes (April 2026)—first pilot!

**Roadmap**: [Whitepaper.md#roadmap](Whitepaper.md#roadmap).  
**Tokenomics**: [Tokenomics.md](Tokenomics.md).  
**Audit Status**: TBD (OpenZeppelin Q1 2026).

---

*Built by the community, for the fork. Scan, pay, eat—DeFi style.* 🚀
