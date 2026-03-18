---
topic: Neon EVM Performance
type: analysis
tags: [neon-evm, performance, tps, compute-units, parallel-execution, solana, benchmarks]
confidence: high
last_updated: 2026-03-18
sources:
  - https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana
  - https://consensys.io/blog/neon-an-ethereum-virtual-machine-on-solana
  - https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af
  - https://docs.neonevm.org/docs/evm_compatibility/overview
---

# Neon EVM Performance

## Summary
Neon EVM achieves 778 TPS on live Solana mainnet (tested), approaching the theoretical maximum of all Ethereum L2s/rollups combined (~880 TPS). The critical optimization breakthrough was reducing EVM compute unit consumption per swap from 20M+ to ~600K (97% reduction). A 2024 architectural shift to optimistic execution further enables full parallel processing of EVM transactions on Solana.

## Key Facts

### Throughput Benchmarks

| Metric | Value | Source |
|--------|-------|--------|
| Tested TPS (live mainnet) | 778 TPS | Solana Compass / Neon CTO |
| Claimed max TPS | 4,500 TPS | Consensys overview |
| Theoretical all-L2s combined limit | ~880 TPS | Neon CTO comparison |
| Ethereum mainnet TPS | ~1,500 TPS | Consensys |
| Solana theoretical max | 50,000 TPS | Consensys |
| Gas fees per transaction | 0.000015 SOL | Consensys |

> [!fact] 778 TPS tested on live Solana mainnet with other programs running concurrently — this is a real-world benchmark, not theoretical.
> [!analysis] 4,500 TPS claim from Consensys appears to be a theoretical ceiling, not a measured benchmark. Use 778 TPS as the more reliable figure.

### Compute Unit Optimization (Critical Milestone)

- **Original cost (pre-optimization):** Uniswap v2 token swap = **20 million+ compute units** per transaction
  - This consumed ~50% of all compute units in a single Solana block
  - Completely impractical for mainnet use
- **Post-optimization cost:** Same swap = **~600,000 compute units**
  - **97% reduction** in compute unit consumption
  - Complex Ethereum transactions now fit within a single Solana transaction

> [!fact] Confirmed by Neon CTO Andrey Falaleyev in Solana Compass interview.

### Parallel Execution Architecture

**2024 breakthrough: Pessimistic → Optimistic Execution Model**

- **Old (pessimistic):** Pre-declare all accounts accessed in a transaction. Required knowing all state dependencies upfront — difficult for EVM transactions where dependencies emerge during execution.
- **New (optimistic):** Assume transactions don't conflict; execute in parallel; roll back if conflicts detected. Mirrors how Solana's Sealevel works natively.

This transition was crucial during Solana's **mid-2024 network congestion crisis**:
- Distributed computational units among iterative transactions
- Overcame transaction failures and delays that affected many Solana programs
- EVM transactions competed more fairly for block space

**Mechanism for parallel EVM on Solana:**
- Ethereum state split into separate Solana accounts (contracts, user accounts, contract storage)
- Each account can be processed independently when no conflicts
- Solana's Sealevel determines which transactions touch the same accounts → routes non-conflicting ones to parallel execution

### Controlled Transaction Tree
- **Atomic execution:** Groups of related EVM sub-calls execute atomically
- **Parallel execution:** Independent transactions execute simultaneously
- **Intent-based execution:** Bridges EVM contract intents to Solana program calls

### Known Technical Constraints

| Constraint | Limit | Impact |
|-----------|-------|--------|
| Max accounts per transaction | 64 (Solana V0) | Complex DeFi contracts may hit this |
| Heap memory | 256 KB | Large in-memory data structures fail |
| Historical data | Off-chain (Geyser plugin) | No on-chain history; cheaper but requires indexer |
| Reentrancy | `transfer()`/`send()` not safe | Different gas model breaks hardcoded 2300 gwei assumption |

> [!fact] 64-account limit: each Ethereum account involved in a transaction requires a corresponding Solana account to be pre-declared.
> [!analysis] The 64-account limit is likely the most binding constraint for complex DeFi protocols (AMMs with many pools, lending protocols, etc.). Requires careful contract design to stay within limits.

### State Management Design Tradeoffs

Ethereum stores all state in Merkle Patricia Trie. Neon EVM uses Solana accounts instead:
- **Pro:** Leverages Solana's native parallelism; cheaper than single MPT
- **Pro:** Historical data offloaded to Geyser plugin (keeps on-chain costs low)
- **Con:** Historical data requires external indexer infrastructure (not in Solana state)
- **Con:** Contract design must account for account number limits

### Security & Audits (2024)
Full security assessments conducted by:
- **Halborn** — EVM Program + Proxy
- **Neodyme** — Infrastructure audit
- **Ackee Blockchain** — Neon DAO + governance

### Fee Model
- Gas fees paid in NEON token (ERC-20 on Neon EVM), or other ERC-20 tokens accepted by operator
- Operators (proxies) can choose which tokens to accept for fee payment
- EIP-1559 support (Jan 2025): users can set priority fees for Solana block inclusion
- Fees dramatically lower than Ethereum due to Solana as settlement layer

## How it relates to Logos

> [!analysis] Analyst inference — not verified
> The 97% compute unit optimization story is a key data point for LEZ: raw EVM compatibility is not enough — extensive optimization is required to make it practical. Neon Labs spent significant engineering time reducing CU consumption. Any LEZ EVM compat effort should budget for similar optimization work. The 778 TPS real-world benchmark is strong; if LEZ's native throughput is significantly higher, EVM compat becomes the throughput bottleneck.

See also: [[Neon EVM]], [[Neon EVM Adoption]], [[LEZ EVM Relevance]]

## Open Questions

1. Has the optimistic execution model been stress-tested under adversarial conditions (MEV, spam)?
2. What is the per-transaction cost in USD at current SOL prices? Is it competitive with Solana native?
3. Does the 64-account limit block any major DeFi protocols from deploying on Neon EVM?
4. How does Neon EVM's 778 TPS benchmark compare to Arbitrum/Optimism/Base in equivalent real-world conditions?
5. What is the latency profile? Does wrapping EVM txns into Solana txns add meaningful confirmation delay?

## Sources
- [Parallelizing the EVM on Solana — Solana Compass](https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana)
- [Neon: An EVM on Solana — Consensys](https://consensys.io/blog/neon-an-ethereum-virtual-machine-on-solana)
- [Neon EVM Recap 2024 — Medium](https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af)
- [EVM Compatibility Overview — Neon Docs](https://docs.neonevm.org/docs/evm_compatibility/overview)
