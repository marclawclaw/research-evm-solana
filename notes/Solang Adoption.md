---
topic: Solang Adoption
type: analysis
tags: [solang, adoption, community, developer-tooling, neon-evm, solana]
confidence: medium
last_updated: 2026-03-18
sources:
  - https://github.com/hyperledger-solang/solang
  - https://medium.com/@smilewithkhushi/inside-solanas-developer-toolbox-a-2025-deep-dive-7f7e6c4df389
  - https://www.theblock.co/post/240527/solana-labs-rolls-out-solang-compiler-to-enhance-ethereum-compatibility
  - https://bitcoinist.com/solana-labs-debuts-solang-compiler/
---

# Solang Adoption — Usage, Community, and Competitive Position

## Summary
Solang has low adoption compared to native Rust/Anchor development on Solana and significantly less traction than [[Neon EVM]]. As of 2025, it is functional and bundled in Solana's tools, but described by community observers as "underused" due to lacking deep tooling integration and requiring developers to adapt to Solana's account model anyway. No major production dApps are known to run exclusively on Solang.

---

## Key Facts

### GitHub Metrics
- **Repo:** https://github.com/hyperledger-solang/solang
- **Stars:** ~1,300 (as of mid-2025) — significantly less than Anchor (~4,400 stars)
- **License:** Apache 2.0
- **Latest release:** v0.3.4 (June 2024) — added Stellar/Soroban target support, LLVM 16 upgrade
- **Active development:** Yes — regular commits, multiple contributors (seanyoung, LucasSte, xermicus, salaheldinsoliman, chioni16)
- **Maintainer:** Sean Young (creator, Solana Labs contributor) + Hyperledger community
- **Notable**: Solana-specific breaking change in v0.3.4: removed `balance`, `transfer`, `send` builtins from Solana target

> [!fact] GitHub repo and release history verified directly

### Community Activity
- **Discord:** Hyperledger Discord has a `#solang` channel — activity level appears moderate/low vs Anchor community
- **Forum presence:** Limited — most Solana developer discussion is Anchor/Rust-centric; Solidity on Solana is niche
- **Hackathon usage:** Mentioned alongside Seahorse as an onboarding tool for non-Rust developers; used in some educational/hackathon contexts
- **Educational resources:** QuickNode guides, Solana official getting-started guide for Solang, Solfate podcast episode

### Adoption Assessment (2025)

> [!analysis] Analyst inference — not verified against on-chain data

| Dimension | Rating | Notes |
|---|---|---|
| Production dApp usage | Very Low | No known major production apps on Solang-only stack |
| Developer community size | Small | Niche; Anchor/Rust dominates |
| Tooling integration | Limited | Works with Anchor, but not Hardhat, ethers.js, MetaMask |
| Tutorial/guide coverage | Moderate | QuickNode, Solana docs, podcast |
| GitHub activity | Active | Regular releases and commits, small contributor set |
| Ecosystem endorsement | Official | Bundled in Solana CLI tools suite (v1.16.3+) |

> [!fact] Confirmed from Medium 2025 deep dive (Khushi, July 2025)
> "Solang (Solidity) exists but is still underused due to lack of deep tooling integration."

### Why Solang Is Underused

1. **Partial compatibility** — ERC-20 not supported, `msg.sender` missing, `try-catch` broken. Devs can't just port EVM contracts.
2. **Toolchain switch** — Must use Solana-Web3.js + Anchor/Phantom, not Hardhat/ethers.js/MetaMask. EVM devs lose their full stack.
3. **Neon EVM is easier for EVM devs** — Neon preserves the entire EVM toolchain. Solang only preserves the language.
4. **Solana account model still required** — Writing in Solidity doesn't abstract away Solana's account/CPI model. Sean Young confirmed this directly: "Writing in the Solidity language doesn't isolate you from the fact that Solana is [different]."
5. **Native Rust/Anchor is the gold standard** — Solana's ecosystem is deeply Rust-first. Solang competes uphill.
6. **Limited production visibility** — No DeFi protocols or consumer apps with Solang branding.

### Official Launch & Positioning (July 2023)

- Announced July 2023 alongside Neon EVM's mainnet launch — coordinated messaging from Solana Labs
- Austin Federa (Solana Head of Strategy) framed Neon EVM vs Solang as complementary: "like running a Windows program on Mac" — different layers of compatibility
- Positioned as an **onboarding bridge**, not a production-grade runtime alternative
- Solana Labs integrated Solang into the official CLI tool suite — institutional endorsement without heavy marketing push

### Solang vs Neon EVM — Adoption Comparison

| Dimension | Solang | Neon EVM |
|---|---|---|
| Approach | Compiler (Solidity → SBF) | EVM emulator on Solana |
| EVM toolchain compat | ❌ No (uses Solana tools) | ✅ Full (Hardhat, ethers.js, MetaMask) |
| ERC-20 support | ❌ Not compatible | ✅ Native ERC-20 |
| Production usage | Very low | Moderate (200K daily txns peak 2024) |
| Funding | Open source / Hyperledger | $65M+ (Jump Capital, Microsoft M12) |
| Token | None | NEON token |
| Developer friction | High (account model adaptation required) | Low (EVM stack preserved) |
| License | Apache 2.0 | Proprietary (source-available, no fork) |
| Community size | Small | Moderate (re-engaged 2024) |

> [!analysis] Analyst inference
> For an EVM developer migrating to Solana, Neon EVM is the path of least resistance. Solang appeals to developers who want to use Solidity syntax but are willing to invest in understanding Solana's architecture — a relatively narrow audience.

### Roadmap Items (from GitHub)
- `Declare accounts for a Solidity function on Solana` — In progress (improves ergonomics of CPI account management)
- `Support OpenZeppelin on Polkadot target` — In progress (not Solana, but signals OpenZeppelin compatibility ambitions)
- `Adopt single static assignment for code generation` — In progress (compiler quality improvement)
- `Tooling for calls between ink! <> Solidity` — In progress
- `Provide CLI for node interactions` — Done (solang-aqd released)
- No public ERC-20 compatibility timeline for Solana target

### Multi-Chain Scope — A Dilution Risk?
Solang now supports Solana, Polkadot, and Stellar (Soroban). This multi-chain ambition may dilute focus on making Solang a compelling Solana-first tool. Community feedback and roadmap items are split across three chains.

---

## How It Relates to Logos

> [!analysis] Analyst inference — not verified

- Solang's adoption story is a cautionary tale: **syntax compatibility alone is insufficient**. Developers want full toolchain preservation (MetaMask, Hardhat, ERC-20) — which only a Neon-style emulator provides.
- If LEZ built a Solang-style compiler (Solidity → native LEZ bytecode), it would face the same adoption headwinds: EVM devs need their full stack, not just the language.
- However, Solang's open-source Apache 2.0 license makes it potentially forkable as a base for a LEZ compiler target.
- The small contributor set (Sean Young + Hyperledger community) suggests LEZ could engage the project as a collaborator or fork point rather than competing.
- LEZ relevance question: **native compilation** (Solang model) vs **full emulation** (Neon model) — Solang data suggests emulation wins on adoption but compilation wins on performance and openness.

---

## Open Questions

- Are there any on-chain programs in production that were compiled with Solang (verifiable via program metadata)?
- Does Solana Labs continue to invest in Solang or has it become purely Hyperledger/community-maintained?
- Could OpenZeppelin contract compatibility (in progress for Polkadot) extend to Solana target?
- Is there a path to `msg.sender` support via account annotations / PDA signing?

---

## Sources

- GitHub repo + releases: https://github.com/hyperledger-solang/solang/releases
- Medium 2025 deep dive: https://medium.com/@smilewithkhushi/inside-solanas-developer-toolbox-a-2025-deep-dive-7f7e6c4df389
- The Block launch coverage (July 2023): https://www.theblock.co/post/240527/solana-labs-rolls-out-solang-compiler-to-enhance-ethereum-compatibility
- Bitcoinist launch coverage: https://bitcoinist.com/solana-labs-debuts-solang-compiler/
- QuickNode Solang vs Neon: https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon
- Solfate podcast with Sean Young: https://solanacompass.com/learn/Solfate/write-solidity-on-solana-with-solang-feat-sean-young-solana-labs-solfate-podcast-31
