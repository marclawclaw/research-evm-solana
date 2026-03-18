---
topic: Neon Stack
type: concept
tags: [neon-evm, evm, svm, b2b, licensing, interoperability, lez]
confidence: high
last_updated: 2026-03-18
sources:
  - https://medium.com/@neon_evm/neon-stack-powers-evm-compatibility-to-eclipse-svm-network-7dec01c24a9d
  - https://www.eclipse.xyz/articles/bringing-evm-compatibility-to-eclipse-with-the-neon-stack
  - https://thedefiant.io/news/press-releases/sonic-partners-with-neon-stack-to-bring-evm-compatible-dapps-to-solana
  - https://thedefiant.io/news/blockchains/yona-network-integrates-neon-for-evm-and-svm-compatibility-on-bitcoin
  - https://medium.com/@neon_evm/neon-stack-for-yona-network-potential-to-open-new-frontiers-in-bitcoin-defi-84be4c1674a6
---

# Neon Stack

## Summary
Neon Stack is a licensable B2B technology suite developed by the core [[Neon EVM]] team, designed to enable EVM compatibility on any SVM-based blockchain network. It is the first and only production-ready EVM-on-SVM stack, live on Solana mainnet since July 2023, and has been licensed to Eclipse, Sonic, and Yona Network as of mid-2024. For LEZ, this is the primary analogous B2B product to evaluate.

## Key Facts

- **Type:** Standardized, licensable technology suite — B2B product
- **Provider:** Neon EVM core team / Neon Foundation
- **CCO / BD contact:** Davide Menegaldo (`vishvendra.dhayal@neonfoundation.io`)
- **First production use:** Solana mainnet — live since **July 2023**
- **First B2B deployment:** Eclipse, announced **May 3, 2024**

### Components

| Component | Role |
|-----------|------|
| **Neon EVM smart contracts** | EVM execution engine deployed as Solana programs |
| **Neon Proxy** | Wraps EVM transactions into Solana transactions; provides Web3/JSON-RPC API |

Neon Proxy is the critical adapter layer: it accepts standard Ethereum RPC calls and translates them into valid Solana transaction bundles, hiding all SVM-specific mechanics from the EVM developer.

### What It Enables (for the partner network)

- Solidity and Vyper smart contracts deploy from **existing EVM codebase with minimal reconfiguration**
- Full Ethereum RPC API compatibility (MetaMask, WalletConnect, Ledger, Hardhat, Remix, Truffle)
- Ethereum account model, signatures, and ERC token standards
- Native SVM performance: parallel execution, low fees, high throughput
- Access to Ethereum developer ecosystem (oracles: Chainlink, Pyth; indexers: Covalent, The Graph; SDKs: web3.js, OpenZeppelin)

### Market Sizing Rationale

> [!fact] Confirmed from official announcements
> Over 13,000 dApps in the Ethereum ecosystem (DappRadar); only 0.4% have migrated to SVM chains. The addressable market for Neon Stack is the remaining 99.6%.

EVM DeFi TVL is approximately $70B at time of partnership announcements — Neon Stack's pitch to network operators is: "unlock this capital for your chain."

### Differentiation vs Other EVM Stacks

| Stack | Target | EVM → SVM Bridge? |
|-------|--------|-------------------|
| OP Stack | EVM L2s | ❌ EVM-only |
| Polygon CDK | EVM L2s | ❌ EVM-only |
| **Neon Stack** | **SVM-based networks** | **✅ First and only** |

> [!fact] Confirmed from official Medium post (May 2024)
> "Neon Stack is the first EVM stack on SVM and the only mainnet live solution to offer this compatibility between the two L1s — Solana and Ethereum."

### Known Integration Partners (as of 2024)

| Partner | Network Type | Announced | Status |
|---------|-------------|-----------|--------|
| [[Eclipse EVM Integration\|Eclipse]] | SVM L2 on Ethereum (Celestia DA) | May 2024 | First partner; integration live |
| Sonic | Gaming SVM L2 on Solana (HyperGrid) | July 2024 | Production-ready deployment |
| Yona Network | Bitcoin SVM L2 | July 2024 | Integration announced; Bitcoin DeFi focus |

### Licensing Model

> [!analysis] Analyst inference — not verified
> Neon Stack is described as a "licensed" and "standardized" suite across all partner announcements. No public pricing is disclosed. Given the B2B CCO-led BD process, this is likely a negotiated commercial license (not open source). [[Neon EVM]] itself is source-available but non-commercial.

## How It Relates to Logos

> [!analysis] Analyst inference — not verified
> Neon Stack is the closest existing analogue to what LEZ might need: a licensable EVM compatibility layer for a custom VM network. Key questions for LEZ:
> 1. Is LEZ's VM architecture SVM-compatible? Neon Stack requires the underlying network to run the SVM.
> 2. Could Logos license Neon Stack directly, or would it need to build its own equivalent?
> 3. Is the Neon EVM source code (non-commercial license) forkable for research/prototyping purposes?
> 4. The Neon Stack B2B model validates the market: SVM-based network operators will pay for EVM compatibility infrastructure.

If LEZ is SVM-based, Neon Stack is a ready-made solution. If LEZ uses a custom VM, the Neon EVM architecture (iterative transactions, proxy adapter, EVM bytecode interpreter) is the template to study for building an equivalent.

## Open Questions

- Is there a published license agreement or pricing page for Neon Stack?
- What is the integration timeline and effort level for a new network? (weeks? months?)
- Does the partner retain the stack as a hosted service, or does Neon Foundation operate the Proxy?
- Are there performance/TPS ceilings specific to Neon Stack deployments vs mainline Neon EVM?
- Has Eclipse's Neon Stack integration been successfully adopted by EVM dApp teams in practice?
- Are there any additional known integrations beyond Eclipse, Sonic, and Yona?

## Sources

- Neon EVM Medium (May 2024): https://medium.com/@neon_evm/neon-stack-powers-evm-compatibility-to-eclipse-svm-network-7dec01c24a9d
- Eclipse official announcement (May 2024): https://www.eclipse.xyz/articles/bringing-evm-compatibility-to-eclipse-with-the-neon-stack
- The Defiant — Sonic partnership (July 2024): https://thedefiant.io/news/press-releases/sonic-partners-with-neon-stack-to-bring-evm-compatible-dapps-to-solana
- The Defiant — Yona Network (July 2024): https://thedefiant.io/news/blockchains/yona-network-integrates-neon-for-evm-and-svm-compatibility-on-bitcoin
- Neon EVM Medium — Yona deep dive (July 2024): https://medium.com/@neon_evm/neon-stack-for-yona-network-potential-to-open-new-frontiers-in-bitcoin-defi-84be4c1674a6
