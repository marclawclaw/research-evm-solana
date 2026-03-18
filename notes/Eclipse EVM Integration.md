---
topic: Eclipse EVM Integration
type: use-case
tags: [eclipse, neon-stack, evm, svm, l2, ethereum, case-study, lez]
confidence: high
last_updated: 2026-03-18
sources:
  - https://www.eclipse.xyz/articles/bringing-evm-compatibility-to-eclipse-with-the-neon-stack
  - https://medium.com/@neon_evm/neon-stack-powers-evm-compatibility-to-eclipse-svm-network-7dec01c24a9d
---

# Eclipse EVM Integration

## Summary
Eclipse is an Ethereum L2 that uses the Solana Virtual Machine for execution. In May 2024, it became the first network to integrate [[Neon Stack]], adding EVM compatibility to its SVM-native chain. This is the canonical reference case for how a non-Solana SVM network can deploy EVM support via a licensable technology suite — directly relevant to evaluating LEZ options.

## Key Facts

### About Eclipse

- **Architecture:** Modular L2 — Ethereum for settlement, SVM for execution, Celestia for data availability, RISC Zero for proving
- **Positioning:** "Ethereum's fastest L2, powered by the Solana Virtual Machine"
- **Native VM:** SVM — natively supports SVM dApps only; cannot run EVM dApps without an adapter
- **Founder:** Neel Somani

### The EVM Gap Problem

Before Neon Stack integration:
- Eclipse's SVM-only architecture could not run any of Ethereum's 13,000+ dApps
- Developers would have to rewrite contracts in Rust/Anchor to deploy on Eclipse
- Only 0.4% of EVM dApps had ever migrated to any SVM chain

> [!fact] Confirmed from Eclipse Labs announcement (May 2024)
> "The Eclipse network is built as an SVM L2, meaning it natively cannot support EVM-based dApps and can only support SVM-based dApps."

### The Neon Stack Solution

Neon Stack integration adds:
- **Solidity/Vyper contract deployment** from existing EVM codebase — minimal reconfiguration required
- **Full Ethereum RPC API** — MetaMask, Hardhat, Truffle, Remix work unchanged
- **EVM account model + ERC token standards** preserved
- **EVM signatures** (ECDSA) retained
- Underlying execution still uses **SVM parallelism** — developers get Ethereum familiarity + Solana throughput

### Announcement & Timeline

| Milestone | Date |
|-----------|------|
| Partnership announced | May 3, 2024 |
| "Fully launching in coming months following test runs and optimizations" (at announcement) | ~Q3 2024 expected |

> [!analysis] Analyst inference — not verified
> The May 2024 announcement stated full launch would follow test runs. No subsequent public announcement of a specific launch date was found in sources crawled. Assumed live by end of 2024 based on typical integration timelines.

### Quotes

> "Our partnership with Neon Stack will allow developers to seamlessly deploy their dApps from EVM chains to Eclipse, further strengthening the harmonization between Solana and Ethereum. Solidity developers that want to build on a high-performance L2 that uniquely utilizes the strengths of the SVM can finally do so."
> — Neel Somani, Founder of Eclipse Labs

> "This partnership isn't just about merging technologies; it is about harmonizing two powerful ecosystems to create a seamless experience for developers."
> — Davide Menegaldo, CCO Neon Foundation

### Developer Experience After Integration

| Aspect | EVM Developer Experience on Eclipse |
|--------|--------------------------------------|
| Smart contract languages | Solidity, Vyper — unchanged |
| Dev tooling | Hardhat, Remix, Truffle — all supported |
| Wallet | MetaMask, WalletConnect, Ledger |
| RPC API | Standard Ethereum JSON-RPC |
| Token standards | ERC-20, ERC-721 etc. preserved |
| Underlying execution | SVM parallelism, Solana-tier throughput |

### User Experience After Integration

- Users interact using familiar EVM tools (MetaMask, WalletConnect)
- Benefit from faster + cheaper transactions vs Ethereum mainnet
- No change to UX layer

## How It Relates to Logos

> [!analysis] Analyst inference — not verified
> Eclipse is a near-perfect reference case for what LEZ might want to offer EVM developers:
> - **SVM-based network** that wants to attract the massive EVM developer pool
> - **Plugged in Neon Stack** as a B2B integration rather than building in-house
> - **Outcome:** EVM developers can deploy Solidity dApps to a non-Ethereum chain with zero rewriting
>
> If LEZ is SVM-based, Eclipse's integration path (license Neon Stack → deploy Neon Proxy → expose Ethereum RPC endpoint) is likely the fastest route to EVM compatibility. Build time: months, not years.
>
> Key structural difference to note: Eclipse settles on Ethereum and targets Ethereum L2 liquidity. LEZ would be settling within the Logos ecosystem. The tooling integration (Neon Stack) is the same; the economic positioning would differ.

## Open Questions

- Has the Eclipse–Neon Stack integration attracted measurable EVM developer adoption? (TVL, dApp count)
- What was the actual engineering lift to deploy Neon Stack on Eclipse's network?
- Is the Neon Proxy operated by Neon Foundation, Eclipse Labs, or a third party?
- Has Eclipse published performance benchmarks for EVM execution vs native SVM on the same network?
- Are there any reported issues (composability gaps, gas price model inconsistencies) post-launch?

## Sources

- Eclipse Labs announcement (May 2024): https://www.eclipse.xyz/articles/bringing-evm-compatibility-to-eclipse-with-the-neon-stack
- Neon EVM Medium (May 2024): https://medium.com/@neon_evm/neon-stack-powers-evm-compatibility-to-eclipse-svm-network-7dec01c24a9d
