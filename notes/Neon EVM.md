---
topic: Neon EVM
type: concept
tags: [neon-evm, evm, solana, evm-compatibility, solana-native, neon-labs]
confidence: high
last_updated: 2026-03-18
sources:
  - https://docs.neonevm.org/docs/evm_compatibility/overview
  - https://consensys.io/blog/neon-an-ethereum-virtual-machine-on-solana
  - https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana
  - https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af
  - https://medium.com/@neon_evm/neon-evms-mainnet-upgrade-paving-a-solana-native-future-for-ethereum-dapps-39e9badc142b
  - https://github.com/neonlabsorg/neon-evm
---

# Neon EVM

## Summary
Neon EVM is a full Ethereum Virtual Machine implemented as a native Solana program (written in Rust, compiled to BPF). It wraps Ethereum-like transactions into Solana transactions, enabling EVM dApps to run unmodified on Solana while inheriting its parallel execution and low fees. It is positioned as a "Solana network extension" for Ethereum developers — not an L2, not a bridge.

## Key Facts

- **What it is:** Native Solana program implementing full EVM emulation. Not a sidechain, not an L2, not a bridge.
- **Implementation language:** Rust, compiled to BPF (Berkeley Packet Filter) bytecode — runs as a Solana smart contract.
- **Mechanism:** EVM transactions are wrapped into Solana transactions → sent to Solana network → executed with parallel processing via Sealevel.
- **Developer experience:** Full Ethereum toolchain compatibility — MetaMask, Hardhat, Truffle, Ethers.js, OpenZeppelin, Solidity, Vyper all work with minimal reconfiguration.
- **Architecture components:**
  - **Neon Proxy (Web3 Proxy):** Service layer between EVM clients and Neon EVM; exposes Ethereum-standard JSON RPC API. Permissionless — anyone can run a proxy.
  - **NeonPass:** Native bridge for transferring tokens between Solana SPL and Neon EVM.
  - **ERC-20 SPL-Wrapper:** Wraps SOL → wSOL for EVM compatibility.
  - **On-chain mempool:** Efficient transaction scheduling directly on-chain, no external dependency.
- **State management:** Splits Ethereum state (contract code, user accounts, storage) into separate Solana accounts. Uses Solana Geyser plugin for historical data instead of storing on-chain (cost optimization).
- **Solana Signature SDK (Jan 2025):** Allows Solana wallets (Phantom, Backpack, Solflare) to sign and execute Neon EVM transactions using ed25519. Eliminates need for MetaMask for Solana users.
- **EIP-1559 support:** Added in Jan 2025 mainnet upgrade — flexible priority fees.
- **Composability:** `ICallSolana.sol` interface enables Solidity contracts deployed on Neon EVM to call native Solana programs directly (Composability v1 live on mainnet; v2 for iterative transactions in development).
- **Execution model:** Transitioned from pessimistic to **optimistic execution model** in 2024 — enables full parallel execution of EVM transactions on Solana.
- **Controlled transaction tree:** Atomic + parallel transaction execution model for state management of complex interactions.

### Key Technical Constraints (from official docs)
- **Account limit:** Max 64 accounts per transaction (Solana V0 limit). Ethereum accounts must map to Solana accounts.
- **Heap limit:** 256 KB heap per contract call (BPF constraint).
- **Opcode differences:** Most opcodes supported; a subset handled differently.
- **Gas calculation:** Significantly cheaper than Ethereum (Solana as settlement layer). `transfer()` and `send()` are NOT reentrancy-safe due to variable gas.
- **Storage model:** Mappings create more accounts than arrays; design matters for staying under 64-account limit.
- **Transaction types supported:** Legacy (type 0), type 1, and EIP-1559 (type 2).

### License
Proprietary — Neon Protocol Ltd. Non-commercial use only. No copy, fork, or redistribution without written permission. Source available on GitHub for inspection only.

### Funding
- $40M Series A (2021) — Jump Capital, Solana Capital
- ~$25.6M strategic investment by Microsoft M12 (August 2024)
- Total: $65M+

### Key Repos
- `neonlabsorg/neon-evm` — Core EVM loader (develop branch); includes `evm_loader/`, `audit/`, `ci/` directories and whitepaper PDF
- `neonlabsorg/neon-tutorials` — Example contracts including ICallSolana composability

## How it relates to Logos

> [!analysis] Analyst inference — not verified
> Neon EVM is the most direct architectural analogue for any team considering adding EVM compatibility to a non-EVM VM. The Neon Stack (B2B licensing of this tech) is the most directly relevant product for LEZ: Neon Stack was already adopted by Eclipse (SVM L2) and Sonic. If LEZ has an SVM-like architecture, Neon Stack could be licensed rather than built from scratch. Key evaluation point: does LEZ need *full EVM emulation* (Neon-style) or *Solidity compilation to native* (Solang-style)?

See also: [[Neon Stack]], [[Neon EVM Adoption]], [[Neon EVM Performance]], [[Solang]], [[LEZ EVM Relevance]]

## Open Questions

1. How many production dApps are running on Neon EVM mainnet (not testnet, not points program)?
2. Is Neon Stack available as a licensable product to external teams (e.g. Logos/LEZ)?
3. What is the commercial model for Neon Stack — revenue share, flat fee, or NEON token requirement?
4. Does the optimistic execution model introduce new security assumptions vs pessimistic?
5. Does the 64-account limit create meaningful restrictions for complex DeFi contracts?

## Sources
- [EVM Compatibility Overview — Neon Docs](https://docs.neonevm.org/docs/evm_compatibility/overview)
- [Neon: An Ethereum Virtual Machine on Solana — Consensys](https://consensys.io/blog/neon-an-ethereum-virtual-machine-on-solana)
- [Parallelizing the EVM on Solana — Solana Compass](https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana)
- [Neon EVM Recap 2024 — Medium](https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af)
- [Neon EVM Mainnet Upgrade (Jan 2025) — Medium](https://medium.com/@neon_evm/neon-evms-mainnet-upgrade-paving-a-solana-native-future-for-ethereum-dapps-39e9badc142b)
- [GitHub — neonlabsorg/neon-evm](https://github.com/neonlabsorg/neon-evm)
