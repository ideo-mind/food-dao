# Food DAO Protocol

[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

**Decentralized Semi-Fungible Food Commerce Protocol**

Food DAO is an open-source, forkable protocol for P2P food exchanges using ERC-404 tokens. Vendors issue fractional FDTs (Food Tokens), community votes via GFD (Governance Food DAO), and committed holders earn via GLD (Locked Derivatives) power. Cut waste, slash fees (0.1% transfers + 1% DEX), redeem IRL—built for events like ETHGlobal Cannes.

- **No KYC/KYB**: Pure DeFi—self-sovereign.
- **veToken-Inspired**: Stake × time = power for profits/validators.
- **Restricted DEX**: Open buys, gated sells to prevent rugs.

**Docs**: [Whitepaper.md](Whitepaper.md) | [Tokenomics.md](Tokenomics.md) | [Contracts.md](Contracts.md)

---

## Features

- **Fractional Commerce**: ERC-404 FDTs—full meals as NFTs, portions as fungible (e.g., 0.5 shawarma).
- **P2P & Redemption**: Wallet QR transfers; vendor sig burns for IRL handoff.
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
│        Frontend (React)     │
│  - Wagmi Hooks, AI Chat    │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┐
│     Contracts (Solidity)    │
│  - FDT/GLD (ERC-404)        │
│  - Governance (GFD Votes)   │
│  - DEX (Restricted Pools)   │
│  - ValidatorPoP             │
└──────────────┬──────────────┘
               │
┌──────────────▼──────────────┘
│        Polygon Chain        │
└─────────────────────────────┘
```

- **Core Libs**: OpenZeppelin (upgrades, access), Pandora ERC-404.
- **Off-Chain**: IPFS for FDT metadata; Chainlink for timestamps (validators).

---

## License

GPL v3—fork freely, but share alike. No warranties; use at own risk.

---

## Community & Support

- **Github**: [@ideomind/food-dao](https://github.com/ideo-mind/food-dao/issues/new)
- **X/Twitter**: [@Ideomind](https://x.com/ideomind).
- **Email**: Hiro &lt;hiro@ideomind.org&gt; (Partnerships).
- **Events**: ETHGlobal Cannes (April 2026)—first pilot!

**Roadmap**: [Whitepaper.md#roadmap](Whitepaper.md#roadmap).  
**Tokenomics**: [Tokenomics.md](Tokenomics.md).  
**Audit Status**: TBD (OpenZeppelin Q1 2026).

---

*Built by the community, for the fork. Let's eat DeFi.* 🚀
