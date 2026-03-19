---
topic: Neon EVM Limitations
type: analysis
tags: [neon-evm, limitations, performance, ux, compute-units, congestion, incompatibilities, developer-experience]
confidence: high
last_updated: 2026-03-19
sources:
  - https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana
  - https://medium.com/@neon_evm/neon-evms-mainnet-upgrade-paving-a-solana-native-future-for-ethereum-dapps-39e9badc142b
  - https://www.reddit.com/r/solana/comments/17tgwfs/how_actively_developed_is_neon_evm/
  - https://blockworks.co/news/lightspeed-newsletter-solana-network-extension
  - https://news.bit2me.com/en/neon-evm-adopts-network-extension-category-in-solana
---

# Neon EVM Limitations

## Summary
Despite 97% compute unit reduction and 778 TPS benchmarks, Neon EVM faces structural limitations: compute overhead versus native Solana programs, deep UX friction (multi-wallet management, token bridging), specific EVM feature gaps (no `block.timestamp` consistency, `receive()` issues), and severe exposure to Solana network congestion. The 2025 mainnet upgrade addressed many UX issues but some are fundamental to the architecture.

## Key Facts

### 1. Compute Overhead — Structural Cost vs Native Solana

- EVM state execution requires **iterative transaction processing** — a single complex Ethereum call may span multiple Solana transactions
- Before 2024 optimization: Uniswap v2 swap = **20M+ compute units** (~50% of an entire Solana block)
- After optimization: same swap = **~600K compute units** — still **3× more** than an equivalent native Solana program (~200K CUs)
- Complex EVM contracts (DeFi protocols, multi-hop swaps, large contract state) can still hit Solana's limits
- Each Ethereum account requires a corresponding Solana account — max **64 accounts per transaction** (Solana V0 limit)
- Heap memory capped at **256 KB** — EVM contracts relying on large in-memory data structures will fail

> [!fact] CU data confirmed by Neon CTO Andrey Falaleyev in Solana Compass interview.
> [!analysis] The 3× CU overhead vs native is the irreducible cost of EVM emulation. It won't go to zero — there is a floor set by the impedance mismatch between EVM and SVM execution models.

### 2. UX Issues — Pre-January 2025 (Historical, Largely Fixed)

Before the January 2025 mainnet upgrade, using Neon EVM as an end user required:

1. **Manually configure wallet**: add Neon RPC endpoint (from ChainList) to MetaMask
2. **Transfer liquidity**: use NeonPass or deBridge to bridge assets from Solana or other EVM chains
3. **Manage two wallets**: MetaMask (for Neon EVM, secp256k1 signatures) + Phantom/Solflare (for native Solana assets)
4. **Interact**: finally usable after all above steps

> [!fact] Described as the "Before" state in official Neon EVM Jan 2025 upgrade post. The process "deterred less tech-savvy users and hindered widespread adoption" — Neon EVM's own characterisation.

**What the Jan 2025 upgrade fixed:**
- Solana Signature SDK: Phantom, Backpack, Solflare can now sign EVM transactions natively (ed25519 → secp256k1 bridging)
- EIP-1559 support: removes fixed gas price model; users can set priority fees for Solana block inclusion
- Users no longer need MetaMask to interact with Neon EVM dApps

> [!analysis] The multi-wallet UX issue was a product of the secp256k1/ed25519 signature algorithm incompatibility. The fix (Solana Signature SDK) is technically complex but production-deployed as of Jan 2025.

### 3. EVM Feature Gaps and Incompatibilities

Features either absent or broken in Neon EVM (as of 2025):

| Feature | Status | Notes |
|---------|--------|-------|
| `block.timestamp` | ✅ Fixed (Jan 2025) | Previously inconsistent due to Solana block timing variability |
| `block.number` | ✅ Fixed (Jan 2025) | Now in parity with Ethereum native behaviour |
| EIP-1559 fee model | ✅ Added (Jan 2025) | Not present prior to Jan 2025 mainnet upgrade |
| CPI to native Solana programs | ⚠️ Limited | Interop exists but complex; can't natively call Jupiter/Raydium directly from EVM contract |
| Solana-native wallets | ✅ Fixed (Jan 2025) | Now supported via Solana Signature SDK |
| `SELFDESTRUCT` opcode | ❓ Unknown status | Not confirmed functional; common EVM security concern |
| EVM historical data | ⚠️ Off-chain | Uses Geyser plugin; not in Solana state — requires external indexer |
| Hardcoded 2300 gas assumptions | ❌ Broken | `transfer()` / `send()` gas assumptions from Ethereum do not hold on Solana |

> [!fact] `block.timestamp` inconsistency and fix confirmed in official Neon EVM mainnet upgrade post (Jan 2025).
> [!analysis] The hardcoded 2300 gas limit in `transfer()` / `send()` is a subtle reentrancy guard assumption that breaks on Neon EVM's different gas model. Any Ethereum contract relying on this pattern (common in older contracts) requires audit and modification before deploying on Neon EVM.

### 4. Gas Fee UX Confusion

- Neon EVM uses the NEON token for gas payment — not SOL, not ETH
- Users must acquire NEON tokens before transacting (onboarding friction)
- Ethereum wallets (MetaMask) misinterpret Solana priority fees: "wallet heuristics misfire — when we represent Solana's priority as a higher gasPrice, Ethereum wallets can over/under-shoot"
- Gas estimators become "noisy or conservative, yielding confusing UX" — Neon EVM's own description
- NEON is also an ERC-20 traded on exchanges — users unfamiliar with the separation between gas token and the broader token ecosystem are often confused

> [!fact] Wallet gas estimator issue described in Neon EVM's own "Route Gas Model the Solana Way" blog post.
> [!analysis] EIP-1559 (Jan 2025) partially addresses this but the root cause — mapping Solana's priority fee model onto Ethereum's gas abstraction — creates ongoing UX friction for wallets that haven't integrated Neon-specific heuristics.

### 5. Solana Network Congestion Exposure

- During **early and mid-2024 Solana congestion**, Neon EVM transactions were severely impacted
- Solana's fee market and priority fee mechanism was not well-integrated with Neon EVM at the time — priority fees "not actively used" pre-congestion
- This exposed a fundamental dependency: Neon EVM's reliability is 100% coupled to Solana's network health
- Congestion on Solana = congestion on Neon EVM, with no independent mitigation path for Neon users
- The 2024 shift to EIP-1559 and optimistic execution was a direct response to the congestion crisis

> [!fact] "Prior to the peak of the Solana congestion/workload in early and mid-2024, the priority fee feature was not actively used" — Neon EVM mainnet upgrade post.

### 6. Community / Developer Support Gap

- November 2023 Reddit (r/solana): "The Discord feels dead. The few developers asking questions get no response."
- December 2024 Reddit: questions raised about Neon EVM's web presence and visibility despite being listed on Coinbase
- 2024 re-engagement via points program (200K daily txns peak) masked low developer activity
- Product-focused activity (points farming) ≠ developer adoption

> [!fact] "Discord feels dead" — Reddit r/solana thread, November 2023, 10 votes / 13 comments.
> [!analysis] Developer support quality is a real adoption barrier. Projects with complex infrastructure (like Neon EVM) require responsive devrel. The gap was notable in 2023; whether 2024–2025 team scaling addressed it is unclear.

### 7. Identity / Categorisation Problem

- Neon EVM struggled with how to describe itself in the broader ecosystem: not a bridge, not an L2
- In October 2024, Neon EVM officially adopted the label **"network extension"** — explicitly rejecting "Layer 2" framing
- CCO Davide Menegaldo acknowledged difficulty fitting Neon EVM into existing blockchain mental models
- "Difficulties in ranking within the blockchain ecosystem" — Bit2me coverage, Oct 2024

> [!analysis] This identity ambiguity has downstream effects: ecosystem funding programs, grant allocations, and developer mindshare all flow through established categories. Being "none of the above" makes it harder to be discovered and funded.

### 8. Centralisation / Operator Model Risk

- Neon EVM uses a **whitelist of trusted operators** (proxies) to execute transactions
- As of 2024, this whitelist was still in place — full decentralisation had not shipped
- Single operator failure or censorship would block transactions
- Plans existed to remove the whitelist (originally targeting May–June of an unspecified year per CTO interview), but timeline unclear

> [!fact] "We will remove our whitelist, and our platform will become really decentralized" — Neon CTO, Solana Compass interview (date of interview unclear; whitelist still active as of 2024).
> [!analysis] The operator whitelist is a meaningfully centralised component. For production DeFi use cases, this is a risk disclosure item. Neon EVM is not trustless in the same way Ethereum mainnet is.

## How it relates to Logos

> [!analysis] Analyst inference — not verified
> For LEZ design: every limitation above is a solved or in-progress problem for Neon EVM, but each required non-trivial engineering. The key implication is that **EVM compat on a non-EVM chain is expensive to do well**: initial compatibility is achievable (months of work), production-quality compatibility with no UX degradation takes years. The congestion dependency is especially relevant — any LEZ EVM compat layer would inherit LEZ network risk. The operator whitelist model is also a trust consideration: deploying a trusted operator set works for a closed/known validator set (as in Logos) but is incompatible with Logos' trust-minimisation principles if applied broadly.

See also: [[Neon EVM]], [[Neon EVM Performance]], [[Neon EVM Adoption]], [[Neon Stack]], [[LEZ EVM Relevance]]

## Open Questions

1. Has the operator whitelist been removed as of early 2026? Is Neon EVM now fully permissionless?
2. Are there documented cases of production dApps failing due to the 64-account limit on Neon EVM?
3. What is the current latency overhead of iterative EVM transaction execution vs native Solana?
4. How many EVM opcodes/precompiles remain unimplemented or inconsistent in Neon EVM as of v2?
5. Has the gas estimator misfiring (Ethereum wallets + Solana priority fees) been fully resolved by EIP-1559 integration?

## Sources
- [Parallelizing the EVM on Solana — Solana Compass](https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana)
- [Neon EVM Mainnet Upgrade (Jan 2025) — Medium](https://medium.com/@neon_evm/neon-evms-mainnet-upgrade-paving-a-solana-native-future-for-ethereum-dapps-39e9badc142b)
- [How actively developed is NEON EVM — Reddit r/solana](https://www.reddit.com/r/solana/comments/17tgwfs/how_actively_developed_is_neon_evm/)
- [Neon EVM adopts 'network extension' title — Blockworks](https://blockworks.co/news/lightspeed-newsletter-solana-network-extension)
- [Neon EVM adopts network extension — Bit2me](https://news.bit2me.com/en/neon-evm-adopts-network-extension-category-in-solana)
- [Neon EVM FAQ — Gas Fees](https://docs.neonevm.org/docs/tokens/gas_fees/)
