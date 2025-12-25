# Food DAO Smart Contracts Specification

**Version:** 1.0  
**Date:** December 25, 2025  
**Authors:** Food DAO Community  
**License:** GPL v3 (Fork Freely)  
**Repository:** [github.com/ideo-mind/food-dao](https://github.com/ideo-mind/food-dao)  
**Contact:** Hiro &lt;hiro@ideomind.org&gt;  

---

## Table of Contents

1. [Overview](#overview)
2. [Tech Stack & Standards](#tech-stack--standards)
3. [Core Contracts](#core-contracts)
   - [GFD: Governance Token (ERC-20)](#gfd-governance-token-erc-20)
   - [FDT: Food Tokens (ERC-404)](#fdt-food-tokens-erc-404)
   - [GLD: Locked Derivatives (ERC-404)](#gld-locked-derivatives-erc-404)
   - [Governance](#governance)
   - [FoodDEX: Restricted Liquidity](#fooddex-restricted-liquidity)
   - [ValidatorPoP: Proof of Power](#validatorpop-proof-of-power)
4. [Deployment Considerations](#deployment-considerations)
5. [Security Principles](#security-principles)
6. [Testing Guidelines](#testing-guidelines)
7. [Immutability](#immutability)
8. [References](#references)

---

## Overview

This specification outlines the raw smart contract architecture for Food DAO. Contracts are designed as conceptual blueprints—immutable where critical (e.g., fees, invariants)—to enable trustless, forkable deployments. Built conceptually on Solidity 0.8.24, they integrate OpenZeppelin for foundational security and Pandora's ERC-404 for semi-fungibility.

**Key Design**:
- **Modularity**: Factories enable forking (e.g., event-specific FDTs).
- **Gas Efficiency**: Hooks combine actions (e.g., transfer + fee in one tx).
- **Economics Integration**: Fees auto-split to validators/treasury/burns.
- **Governance**: GFD-weighted; thresholds for proposals (min 2 voters, 0.15 support, 0.51 quorum, 3 days; early at 0.67/0.65).

Full tokenomics in [Tokenomics.md](Tokenomics.md); protocol vision in [Whitepaper.md](Whitepaper.md). Code snippets here are illustrative; 

---

## Tech Stack & Standards

- **Solidity**: 0.8.24 (SafeMath implicit).
- **Standards**:
  - ERC-20: GFD (governance).
  - ERC-404: FDT/GLD (semi-fungible; [Pandora Labs impl](https://github.com/Pandora-Labs-Org/erc404/blob/main/contracts/ERC404.sol)).
  - ERC-721: Auto-mint for full FDT/GLD units.
- **Libs**:
  - OpenZeppelin: AccessControl, Pausable, Ownable (minimal).
  - Uniswap V3: Pool hooks for DEX.
  - Chainlink: Timestamps for GLD expiry (validators).
- **Conceptual Tools**: Slither for analysis; Foundry for fuzzing ideas.

Deployments envisioned as raw Solidity factories on Polygon.

---

## Core Contracts

### GFD: Governance Token (ERC-20)

**Purpose**: Fixed-supply base for voting/staking.  

**Key Features**:
- Mint initial supply to treasury (100M).
- Pause for emergencies (governed).
- No transfers during votes (internal snapshot).

**Snippet**:
```solidity
contract GFD is ERC20, Ownable {
    uint256 public constant TOTAL_SUPPLY = 100_000_000 * 10**18;
    
    constructor() ERC20("Governance Food DAO", "GFD") {
        _mint(msg.sender, TOTAL_SUPPLY); // To deployer (treasury)
    }
    
    // Pause during votes (governed call)
    function pause() external onlyOwner { _pause(); }
}
```

**Inheritance**: ERC20, Pausable, Ownable.

### FDT: Food Tokens (ERC-404)

**Purpose**: Semi-fungible food stamps; vendor-issued post-approval.

**Key Features**:
- 1 unit = full NFT (redeemable); <1 = fractions.
- 0.1% transfer fee (split: 50% validators, 30% treasury, 20% burn).
- Vendor redeem: Sig-burn for IRL.
- Anti-hoarding: Governed per-wallet caps.

**Snippet**:
```solidity
contract FoodERC404 is ERC404 { // Pandora base
    uint256 public constant TRANSFER_FEE_BPS = 10; // 0.1%
    mapping(address => uint256) public maxPerWallet;
    
    function transfer(address to, uint256 amount) public override returns (bool) {
        uint256 fee = (amount * TRANSFER_FEE_BPS) / 10000;
        uint256 net = amount - fee;
        super.transfer(to, net);
        super.transfer(treasury, fee); // Auto-split via hook
        _handleERC404Conversion(); // Mint/burn NFT if threshold
        return true;
    }
    
    // Vendor redeem
    function redeem(uint256 amount, bytes calldata sig) external {
        require(verifyVendorSig(sig), "Invalid sig");
        _burn(msg.sender, amount);
    }
}
```

**Inheritance**: ERC404 (Pandora), IERC20Metadata. Factory for per-food deploys.

### GLD: Locked Derivatives (ERC-404)

**Purpose**: veToken positions for power (stake × time).

**Key Features**:
- Immutable `power_score = gfd × days`.
- Recompose: Adjust along invariant.
- Unlock: Expiry/recompose.
- Short GLD: Airdrop via DEX.

**Snippet**:
```solidity
contract GLDERC404 is ERC404 {
    IERC20 public gfd;
    
    struct Position {
        uint256 powerScore; // Immutable
        uint256 currentGFD;
        uint256 lockDays;
        uint256 expiry;
    }
    
    mapping(uint256 => Position) public positions;
    
    function mintPosition(uint256 gfdAmount, uint256 lockDays) external returns (uint256 id) {
        gfd.transferFrom(msg.sender, address(this), gfdAmount);
        id = nextId++;
        uint256 power = gfdAmount * lockDays;
        positions[id] = Position(power, gfdAmount, lockDays, block.timestamp + lockDays * 1 days);
        _mint(msg.sender, id, 1e18); // Full position NFT
    }
    
    function recompose(uint256 id, uint256 newGFD, uint256 newDays) external {
        Position storage pos = positions[id];
        require(newGFD * newDays == pos.powerScore, "Invariant broken");
        // Adjust GFD lock (transfer/refund)
        pos.currentGFD = newGFD;
        pos.lockDays = newDays;
        pos.expiry = block.timestamp + newDays * 1 days;
        _handleERC404Conversion(id);
    }
    
    function unlock(uint256 id) external {
        Position storage pos = positions[id];
        require(block.timestamp >= pos.expiry, "Locked");
        gfd.transfer(pos.owner, pos.currentGFD); // Original owner
        _burn(msg.sender, id, 1e18);
    }
}
```

**Inheritance**: ERC404, AccessControl (for treasury airdrops).

### Governance

**Purpose**: GFD-weighted proposals (FDT listings, params).

**Key Features**:
- Proportional votes (1 GFD = 1 vote).
- Thresholds: Min 2 voters; 0.15 support, 0.51 quorum, 3 days.
- Early enact: 0.67 support/0.65 quorum.
- Vendor proposals: Pay listing fee to profit pool.

**Snippet**:
```solidity
contract Governance is IGovernor {
    IERC20 public gfd;
    uint256 public constant MIN_VOTERS = 2;
    uint256 public constant SUPPORT_THRESHOLD = 1500; // 0.15 (basis pts)
    uint256 public constant QUORUM = 5100; // 0.51
    uint256 public constant VOTE_PERIOD = 3 days;
    uint256 public constant EARLY_SUPPORT = 6700; // 0.67
    uint256 public constant EARLY_QUORUM = 6500; // 0.65
    
    function propose(address target, uint256 listingFee, string calldata desc) external {
        require(gfd.balanceOf(msg.sender) >= listingFee, "Fee required");
        gfd.transferFrom(msg.sender, treasury, listingFee); // To profit pool
        // Emit proposal; start vote
    }
    
    function vote(uint256 proposalId, bool support) external {
        uint256 weight = gfd.balanceOf(msg.sender);
        // Accumulate for/against; check thresholds
    }
    
    function execute(uint256 proposalId) external {
        // If passed (support > threshold, quorum met, period over) or early
        _execute(proposalId);
    }
}
```

**Inheritance**: OpenZeppelin Governor (timelock optional).

### FoodDEX: Restricted Liquidity

**Purpose**: FDT/USDC pools; gated sells.

**Key Features**:
- Open buys; gated sells (stake/thresholds or delegate: worse rates).
- 1% fees (50% LPs, 30% validators, 20% burn).
- Issuer stake post-vote; slash on low volume.

**Snippet**:
```solidity
contract FoodDEX is IUniswapV3Pool {
    uint256 public constant FEE_BPS = 100; // 1%
    
    function swapExactInputSingle(...) external returns (uint256 amountOut) {
        // Uniswap logic; apply fee split
        // Gated: Check stake for sells
        require(isStakedSeller[msg.sender] || isDelegate[msg.sender], "Gated sell");
        if (isDelegate[msg.sender]) { /* Apply penalty rate */ }
    }
    
    // Issuer add post-vote
    function addPool(address fdt, uint256 stake) external {
        // Verify governance approval; lock stake
    }
}
```

**Inheritance**: Uniswap V3 interfaces (simplified hooks).

### ValidatorPoP: Proof of Power

**Purpose**: Contestable validators (GLD power).

**Key Features**:
- 200 cap; random select (equal chance).
- Power-weighted challenges (outpower/invalidate expiry).
- Rewards: 0.2% fees to selected.

**Snippet**:
```solidity
contract ValidatorPoP {
    GLDERC404 public gld;
    uint256 public constant MAX_ACTIVE = 200;
    
    function selectValidator() external view returns (address) {
        uint256 rand = uint256(keccak256(abi.encode(block.timestamp, msg.sender)));
        return actives[rand % actives.length];
    }
    
    function challenge(address target) external {
        uint256 challengerPower = _getPower(msg.sender);
        uint256 targetPower = _getPower(target);
        if (challengerPower > targetPower || _hasExpiredGLD(target)) {
            _replace(target, msg.sender);
        }
    }
    
    function _getPower(address who) internal view returns (uint256) {
        // Sum staked GLD power_scores
    }
}
```

**Inheritance**: AccessControl (for reward distributor).

---

## Deployment Considerations

**Conceptual Flow**: Deploy GFD first → GLD factory → Governance → DEX → ValidatorPoP. Use factories for FDT forks (e.g., event shawarmas).

**Gas Targets**: ~500K per tx (Polygon); optimize hooks.

**Verification**: Etherscan for transparency.

---

## Security Principles

- **Immutables**: Fees (0.1%/1%), power invariant, max validators.
- **Access**: Minimal ownable; treasury multisig (conceptual).
- **Guards**: Re-entrancy (OpenZeppelin); Chainlink expiry.
- **Best Practices**: Checks-Effects-Interactions; pausables.

Report vulnerabilities: [security@fooddao.io](mailto:security@fooddao.io).

---

## Testing Guidelines

- **Unit**: Focus on invariants (GLD power, fee splits).
- **Fuzz**: Edge cases (recompose extremes, challenge spam).
- **Integration**: Simulate full flows (propose-vote-execute, redeem-list).
- **Coverage Goal**: 95% (FDT fractions, governance thresholds).

---

## Immutability

- **Core**: Fees, thresholds, ERC-404 logic—no proxies.
- **Flexible**: Governance/DEX (conceptual UUPS; GFD-voted).
- **Forks**: Full immutability per instance.

---

## References

- ERC-404: [Pandora Labs](https://github.com/Pandora-Labs-Org/erc404)
- OpenZeppelin: [Governor](https://docs.openzeppelin.com/contracts/4.x/governance)
- Uniswap V3: [Hooks](https://docs.uniswap.org/protocol/V3/reference/deployments)

**Discuss Code**: Hiro &lt;hiro@ideomind.org&gt;

---

*Raw spec for Food DAO contracts. Implement & iterate via community.*
