---
topic: LEZ EVM Relevance
type: analysis
tags: [lez, logos-execution-zone, evm, risc-zero, risc-v, zk-vm, neon-evm, solang, neon-stack, recommendation, synthesis]
confidence: high
last_updated: 2026-03-19
sources:
  - https://github.com/logos-blockchain/logos-execution-zone (lssa)
  - https://github.com/logos-co/logos-docs
  - https://github.com/logos-blockchain/lssa (redirects to logos-execution-zone)
  - All prior notes in this research collection
---

# LEZ EVM Relevance — Should Logos Build EVM Compatibility for LEZ?

## Executive Summary

**Recommendation: No EVM compat in the near term. Revisit after mainnet stability. When pursued, build native — not via Neon Stack.**

LEZ is a RISC-V zkVM running on the Logos Blockchain, not an SVM-derivative. Neither Neon EVM nor Neon Stack can be directly applied — they are SVM-specific and built around Solana's BPF execution model. Any EVM layer for LEZ would require building a new EVM interpreter compiled to RISC-V bytecode. That is technically feasible but engineering-expensive, and it would be a distraction from LEZ's genuine differentiation: **protocol-level privacy via ZKPs in a hybrid public/private state model that no other programmable chain offers.**

The Neon/Solang story provides clear lessons on cost and outcome. Those lessons argue against rushing into EVM compat on an early-stage chain.

---

## What LEZ Actually Is (Discovered 2026-03-19)

Based on the public `logos-blockchain/logos-execution-zone` repository and `logos-co/logos-docs` documentation:

| Property | Value |
|---------|-------|
| VM bytecode | **RISC-V** (via RISC Zero zkVM) |
| Execution model | Stateless programs — all state in accounts (like SVM, not EVM) |
| Privacy model | Protocol-level: public + private state; ZKPs for private txns |
| Native language | **Rust** (compiles to RISC-V / RISC Zero guest programs) |
| Parallelism | Parallel execution similar to Solana Sealevel |
| Proof system | RISC Zero (zk-STARKs, RISC-V microarchitecture) |
| Current status | **Testnet v0.1** (as of March 2026) |

> [!fact] From lssa GitHub README: "Both public and private executions use the same Risc0 VM bytecode. Public transactions are executed directly on-chain like any standard RISC-V VM call, without proof generation. Private transactions are executed locally by users, who generate Risc0 proofs that validators verify instead of re-executing the program."

> [!fact] From logos-docs quickstart: "LEZ separates account state into public (visible, on-chain) and private (hidden, off-chain)... the client runs the private part locally using your private keys and local client data, producing a zero-knowledge proof (ZKP)."

**Key implication:** LEZ is neither EVM nor SVM. It is a **custom RISC-V zkVM** with a hybrid privacy model. This is architecturally distinct from both Ethereum (MPT state, EVM bytecode, secp256k1 accounts) and Solana (BPF/SBF bytecode, account-based, ed25519 keys).

---

## Could Logos Use or License Neon Stack?

**Short answer: No — not directly.**

Neon Stack is an EVM compatibility suite built specifically for **SVM (Solana BPF) chains**. Its core components are:

1. **Neon EVM smart contracts** — Solana programs (BPF bytecode) that implement EVM instruction execution iteratively on-chain
2. **Neon Proxy** — an off-chain service that accepts Ethereum JSON-RPC calls, wraps EVM transactions into Solana transactions, and manages iterative execution state

**Why it doesn't map to LEZ:**
- Neon EVM runs as a Solana BPF program. LEZ programs are RISC-V RISC Zero guest programs. These are entirely different instruction sets and runtime models.
- Neon Stack was designed with Solana's account model, CPI, compute units, and BPF loader as host environment. None of these apply to LEZ.
- Adapting Neon Stack to RISC-V would require rewriting the EVM interpreter core from scratch — at which point you haven't licensed anything, you've built a new implementation.
- Neon EVM's license is proprietary (non-commercial, no fork, no redistribution). Any commercial use requires negotiating with Neon Protocol Ltd. Even with a commercial license, the port effort would be substantial.

**Conclusion:** Neon Stack is not a shortcut for LEZ. The architecture mismatch is fundamental.

> [!analysis] Eclipse's integration of Neon Stack works because Eclipse is an SVM L2 on Ethereum — it's running Solana BPF programs. LEZ is not SVM-based. Eclipse = plug-and-play for Neon Stack. LEZ = full rebuild required.

---

## Could Logos Adapt Solang for LEZ?

**Short answer: Possible in principle, but limited value.**

Solang compiles Solidity → LLVM IR → SBF bytecode. LLVM does support RISC-V as a target. In theory:
- Add a RISC-V LEZ target to Solang
- Adapt the Solana-specific account model handling to LEZ's account model
- Solang's Apache 2.0 license allows this fork

**Why it's harder than it sounds:**
- Solang's Solana-specific code (account creation, PDAs, SPL token, CPI) would need to be rewritten for LEZ's account model and token programs
- LEZ's RISC Zero bytecode is RISC-V but with zkVM-specific constraints — standard RISC-V binary output from LLVM is not the same as RISC Zero guest code
- Most critically: Solang's adoption data shows that **compiler-only approaches don't attract EVM developers**. The full EVM toolchain (Hardhat, MetaMask, ethers.js, ERC-20) is what developers actually need. Solang only preserves the Solidity language. This proved insufficient on Solana.

**Conclusion:** Adapting Solang for LEZ would produce a tool that, by Solana adoption data, would be underused. The audience who wants Solidity-the-language without Solidity-the-toolchain is small.

---

## The Build-From-Scratch Path: EVM Interpreter in RISC-V

This is the theoretically sound path if EVM compat is ever pursued:

**Architecture:**
1. Write a RISC-V EVM interpreter (e.g., port [revm](https://github.com/bluealloy/revm) — a Rust-based EVM implementation — to RISC Zero guest programs)
2. Map Ethereum account model to LEZ accounts (similar to how Neon EVM mapped EVM accounts to Solana accounts)
3. Build a JSON-RPC proxy that accepts Ethereum transactions, wraps them into LEZ transactions, and manages EVM state
4. Handle signature scheme translation (secp256k1 → LEZ's signature model)

**Why this is feasible:**
- RISC Zero runs arbitrary Rust programs. `revm` is a high-quality Rust EVM that could in principle be compiled to a RISC Zero guest.
- LEZ's stateless account model is conceptually closer to Solana's than Ethereum's, so the mapping patterns from Neon EVM's design are a useful reference.
- ZK-proof of EVM execution would be a strong technical story — this is a "zkEVM on RISC Zero" and could be a genuine innovation.

**Why this is expensive:**
- Neon Labs spent **years** engineering their EVM compat with 97% compute unit reduction. The initial implementation worked; production-quality compatibility required sustained effort.
- RISC Zero proof generation for EVM execution would be computationally intensive client-side (ZK proofs for EVM traces are expensive).
- The 64-account / heap limits of Solana don't apply to RISC-V, but new LEZ-specific constraints (ZK circuit size, proof generation time) would emerge.
- Full EVM toolchain compat requires proxy infrastructure, wallet bridge, token bridge, explorer support — each a non-trivial workstream.
- Without MEV protection, sequencer design, and fee markets designed for EVM users, the UX will be broken regardless of technical correctness.

---

## Lessons from Neon EVM and Solang Directly Applicable to LEZ

### Lesson 1: Raw EVM compatibility is insufficient
Neon EVM shipped in 2022 and remained functionally incomplete (poor UX, dead Discord, low adoption) until the January 2025 mainnet upgrade. **3 years of engineering** to get to usable. The lesson: first-mover EVM compat is broken EVM compat.

### Lesson 2: Full toolchain preservation is what developers actually need
Solang proved that Solidity-only compatibility without MetaMask, Hardhat, ERC-20, and ethers.js doesn't attract developers. If Logos builds EVM compat for LEZ, it must preserve the **entire EVM developer stack**, not just some of it.

### Lesson 3: Compute overhead is permanent
Even after Neon's 97% CU reduction, EVM execution on Solana is ~3× more compute-intensive than native programs. On LEZ, ZK-proof generation for EVM execution would have similar or worse overhead. This creates a permanent two-tier experience: native LEZ apps are faster and cheaper; EVM apps are slower and more expensive.

### Lesson 4: Congestion coupling is a real risk
Neon EVM's exposure to Solana's mid-2024 congestion crisis revealed that an EVM compat layer inherits the host chain's network risk. Any LEZ EVM layer would inherit LEZ's network risk — particularly the ZK proof submission congestion during high-activity periods.

### Lesson 5: Token economics for EVM compat are hard
Neon's NEON token trades at 70% below its ICO price. The sustainable business model for EVM compat is **B2B licensing** (Neon Stack), not native token economics. If Logos pursues EVM compat for LEZ, the sustainability model matters.

### Lesson 6: Developer migration rarely happens
Post-Neon EVM adoption data: developers who deploy on Neon EVM do not migrate to native Solana Rust. Neon EVM's 2025 strategy explicitly leans into this by removing all remaining reasons to migrate. If Logos adds EVM compat to LEZ, it should expect EVM developers to **stay in the EVM layer permanently** — not use it as a bridge to native LEZ programming.

---

## Tradeoffs Analysis: EVM Compat on LEZ

| Dimension | Pro | Con |
|---------|-----|-----|
| Developer reach | Access to 13K+ EVM dApps, Ethereum developer pool | EVM devs won't be LEZ-native; composability is limited |
| Time-to-market | EVM dApps deploy faster than native rewrites | 2–3+ years to production-quality compat (per Neon EVM precedent) |
| Tooling | MetaMask, Hardhat, OpenZeppelin, ethers.js if done right | All requires proxy infrastructure + ongoing maintenance |
| Privacy story | EVM compat with ZKPs would be genuinely novel | Tension: EVM toolchain is built around transparent state |
| Performance | LEZ's RISC-V execution is fast for native programs | EVM emulation adds overhead; ZK proof generation on client side is expensive |
| Composability | EVM contracts could call native LEZ programs (if designed carefully) | Loses EVM toolchain composability with ERC-20/ERC-721 ecosystem |
| Engineering cost | Feasible using revm on RISC Zero | High: new EVM interpreter, proxy, wallet bridge, token bridge, explorer integration |
| Licensing | No licensing needed if built from scratch | Cannot reuse Neon Stack directly |
| Strategic fit | Broader ecosystem reach | May dilute focus from LEZ's core privacy differentiator |
| Privacy-EVM tension | Could be a first-mover in private EVM execution | EVM's transparent state model is fundamentally at odds with private accounts |

---

## The Privacy-EVM Tension (LEZ-Specific Analysis)

This deserves special attention because it's unique to LEZ.

**LEZ's differentiation** is protocol-level privacy: the same program code runs transparently on public accounts and privately on private accounts. Validators verify ZKPs without seeing private state. This is LEZ's most compelling property.

**EVM's design** assumes transparent state. The EVM opcode set includes `SLOAD`, `SSTORE`, `BALANCE`, `EXTCODECOPY` — operations that read chain state. In an EVM compat layer on LEZ:
- EVM programs could only practically operate on the **public state** side of LEZ — losing the privacy differentiator entirely
- Making EVM programs interact with private LEZ accounts would require extending EVM semantics in ways that break Ethereum compatibility
- The toolchain (MetaMask, Hardhat, ethers.js) has no concept of private accounts — users would need new tooling anyway

**Conclusion:** An EVM layer on LEZ would be a **public-only EVM execution environment** living on top of a privacy-first chain. LEZ's unique value proposition is not accessible to EVM programs. This means EVM compat on LEZ attracts EVM developers but not *because of* what makes LEZ compelling.

> [!analysis] The irony: the only way to let EVM apps access LEZ's privacy model is to build LEZ-native privacy primitives into the EVM layer itself — which requires custom EVM extensions, breaking standard toolchain compat. You either get EVM compat OR LEZ privacy. Not both, without major non-standard extensions.

---

## Recommendation

### Near Term (2026–2027): **Do Not Build EVM Compat**

LEZ is at Testnet v0.1. The priority is:
1. Stabilise the core RISC-V/RISC Zero execution model
2. Build native developer experience in Rust — SDK, documentation, sample apps (AMM, token programs are already live)
3. Establish LEZ's identity as the **privacy-first programmable chain** — not a generic EVM chain
4. Complete the Logos module integration (Blockchain + Storage + Messaging)

Building EVM compat now would be a distraction that attracts developers who will never use LEZ's privacy model and requires 2–3 years to do properly.

### Medium Term (2027–2028): **Evaluate, Then Decide**

If by 2027:
- LEZ mainnet is live and stable
- Developer adoption is constrained by "non-EVM" barrier (not the current constraint — ZK complexity is the current barrier)
- Ecosystem partners (DeFi protocols, gaming, identity) are asking for EVM compat

Then the right path is:

**Option A: Native RISC-V EVM Interpreter (Recommended if pursued)**
- Port `revm` (Rust EVM) to RISC Zero guest execution
- Build LEZ-specific proxy and account model adapter
- Design public-only EVM execution environment with explicit bridge to native LEZ accounts
- Open source under Apache 2.0 (builds ecosystem goodwill; contrast with Neon's proprietary approach)
- Timeline: 12–18 months minimum for production quality

**Option B: Solang RISC-V Target Fork (Lower priority)**
- Fork Solang, add RISC-V/RISC Zero target
- Engage Hyperledger community for upstream contribution
- Lower dev reach (Solidity-language-only compat) but smaller engineering ask
- Only useful if Logos wants to target Solidity developers who accept tool-chain changes
- Timeline: 6–9 months for a basic target; years for full parity

**Option C: Partner / License (Not Recommended)**
- Neon Stack is not licensable for LEZ (SVM-specific)
- No other licensable EVM-on-RISC-V solution exists in 2026
- This option is a dead end until a RISC-V EVM solution matures elsewhere

### What to Watch
- **RISC Zero + EVM research:** Multiple zkEVM projects are exploring RISC Zero as the proof backend. If a production-quality RISC-V EVM emerges from the broader zkEVM ecosystem, Logos could integrate rather than build.
- **Neon Labs financial health:** If Neon Labs' NEON token at $0.03 (70% below ICO) forces a pivot or sale, Neon Stack's licensing terms could change dramatically.
- **EVM developer demand signals from LEZ ecosystem:** Real-world evidence of EVM demand from the LEZ builder community should be the trigger, not general market assumptions.

---

## Summary of Findings

| Question | Answer |
|---------|--------|
| Is LEZ SVM-like? | No. RISC-V zkVM via RISC Zero. Different instruction set, different account model. |
| Is Neon Stack licensable for LEZ? | Effectively no — architecture mismatch is fundamental, not just licensing |
| Can Solang be adapted? | Theoretically yes (Apache 2.0, LLVM backend possible) but low adoption payoff |
| Should Logos build EVM compat now? | No — LEZ is pre-mainnet; wrong time and wrong priority |
| Can LEZ EVM compat access LEZ privacy? | Only with major non-standard extensions; standard EVM compat gets public-only execution |
| What's the right path if needed? | Build native: port `revm` to RISC Zero, open source it |
| When to revisit? | Post-mainnet stability, when developer demand is evidenced |
| Key risk if delayed? | Missing the window when EVM developer adoption is highest (pre-mainnet is not that window) |

---

## Open Questions

1. Does the Logos team have a published or internal roadmap item for EVM compat on LEZ?
2. Are any ecosystem partners (DeFi protocols, bridge providers) already asking for EVM compat?
3. Is there a RISC Zero guest program that implements EVM execution that Logos could leverage or contribute to?
4. What is LEZ's intended gas / fee model — is it compatible with EVM's gas accounting semantics if needed?
5. Does the RISC Zero zkVM have compute cost characteristics that make EVM emulation within ZK proofs prohibitively expensive for private EVM execution?
6. Is there a path to making private LEZ accounts visible to EVM programs through a selective disclosure mechanism?

---

## See Also
- [[Neon EVM]] — Full EVM emulation on Solana reference architecture
- [[Neon Stack]] — B2B EVM compat suite; Eclipse integration reference case
- [[Neon EVM Limitations]] — Costs and tradeoffs of EVM emulation
- [[Solang]] — Compiler-only approach; adoption failure case
- [[Solang Adoption]] — Why syntax compat alone is insufficient
- [[Developer Migration Patterns]] — EVM devs rarely go native; expect permanent residency
- [[EVM vs Native SVM Tradeoffs]] — Dev experience comparison across compat layers
- [[Neon Labs Business Model]] — Sustainability considerations for any EVM compat project

## Sources
- [logos-blockchain/logos-execution-zone (lssa) — GitHub](https://github.com/logos-blockchain/logos-execution-zone)
- [logos-co/logos-docs — GitHub](https://github.com/logos-co/logos-docs)
- [LEZ Quickstart — logos-co/logos-docs](https://github.com/logos-co/logos-docs/blob/main/docs/apps/wallet/journeys/quickstart-for-the-logos-execution-zone-wallet.md)
- [AMM Liquidity Pool on LEZ — logos-co/logos-docs](https://github.com/logos-co/logos-docs/blob/main/docs/apps/sample-apps/journeys/create-and-use-an-amm-liquidity-pool-on-the-logos-execution-zone.md)
- [RISC Zero — GitHub](https://github.com/risc0/risc0)
- [revm — Rust EVM Implementation](https://github.com/bluealloy/revm)
- All notes in this research collection (see index.md)
