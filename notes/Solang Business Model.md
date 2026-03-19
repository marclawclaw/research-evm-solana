---
topic: Solang Business Model
type: competitor
tags: [solang, hyperledger, open-source, business-model, solana-foundation, linux-foundation]
confidence: high
last_updated: 2026-03-19
sources:
  - https://www.lfdecentralizedtrust.org/blog/2022/09/12/meet-hyperledger-solang-a-portable-solidity-compiler
  - https://lf-hyperledger.atlassian.net/wiki/spaces/SOL/overview
  - https://solana.com/news/solang-solana-solidity-evm
  - https://github.com/hyperledger-solang/solang
---

# Solang Business Model

## Summary
Solang has no commercial business model. It is a free, open-source (Apache 2.0) Solidity compiler maintained under the Hyperledger (Linux Foundation) umbrella as an incubation-stage project. It is sustained through foundation membership fees, contributor time from Solana Labs employees (primarily Sean Young), Solana Foundation ecosystem support, and ad hoc blockchain ecosystem grants. There is no token, no VC funding, no revenue, and no commercial entity.

## Key Facts

### Governance & Ownership

- **License:** Apache 2.0 — fully open, commercial use permitted without restriction
- **Org:** Hyperledger Foundation / LF Decentralized Trust (Linux Foundation umbrella)
- **Status:** Active incubation project (as of 2022, updated 2025)
- **GitHub:** https://github.com/hyperledger-solang/solang
- **Stars:** ~1,000+ (as of 2024/2025)
- **Lead developer:** Sean Young (previously Solana Labs; original creator)

> [!fact] Solang started in Hyperledger Labs (a lighter-weight incubation space), then graduated to a full Hyperledger top-level project in September 2022.

### Funding & Sustainability

| Source | Type | Notes |
|--------|------|-------|
| Solana Labs (now Anza) | Contributor time | Sean Young employed by Solana Labs, contributed Solang as his work |
| Linux Foundation / Hyperledger | Infrastructure | Hosting, CI tooling, governance support from LF membership fees |
| Solana Foundation | Ecosystem grants | Formal announcement of Solang support in July 2023 (solana.com) |
| Cosmos Interchain Foundation | Ad hoc grant | Offered to fund CosmWasm support via Solang (community grant model) |

> [!fact] Solana Labs formally announced Solang support in July 2023 with a beginner's guide, marking it as an officially endorsed Solana developer tool.

> [!analysis] Solang's sustainability depends on continued employer sponsorship (Anza/Solana Labs) and Hyperledger umbrella support. If Sean Young or his employer reduced investment, the project could stall — which aligns with community observations of underuse and slow velocity.

### Business Model: None

- **No token**
- **No VC funding rounds**
- **No SaaS or commercial offering**
- **No licensing fees** (Apache 2.0 = free to use commercially)
- **No revenue**

> [!analysis] Solang is a **public good project**, not a business. Its commercial impact is indirect: by enabling Solidity devs to write native Solana programs, it benefits the Solana ecosystem broadly — which benefits Solana Labs / Anza commercially (more developers = more transactions = more protocol fee revenue).

### Targets Supported

- **Solana** (primary focus) — compiles to SBF (Solana Berkeley Format / BPF)
- **Substrate / Polkadot** — compiles to WebAssembly (Wasm)
- **EVM** — was planned (ewasm abandoned), now under consideration as future target
- CosmWasm interest noted but not implemented

### Why Solana Foundation Backs It

- More programming language options → lower barrier to entry for Ethereum devs
- No EVM emulation overhead → programs run natively, cheaper to operate
- Complements rather than competes with Neon EVM (different approach)
- Aligns with Solana's stated 2023 goal: "expand reach of Solana ecosystem"

### Comparison vs Neon Labs (Business)

| Dimension | Solang | Neon Labs |
|-----------|--------|-----------|
| Entity type | Open-source project under LF | VC/token-backed startup |
| Revenue | None | Token fees + B2B licensing |
| Funding | Foundation + employer sponsorship | $45M raised |
| Token | None | NEON (SPL, 1B supply) |
| IP control | Community / Apache 2.0 | Neon Labs (proprietary stack) |
| Commercial risk | Low (no investors to satisfy) | Medium-high (token down, no follow-on) |
| Longevity risk | Medium (depends on employer) | Medium (depends on B2B traction) |

## How it relates to Logos

Solang is the **low-cost reference model** for what a Logos EVM compat layer could look like if done as a public good / open-source project rather than a commercial startup. Key implications:

- A Solang-style approach for LEZ (open-source compiler to LEZ native format) would have zero licensing cost and maximum developer freedom.
- It would require sustained contributor time from Logos/IFT employees — similar to Solana Labs backing Solang.
- No token or commercial pressure means the project moves at research/ecosystem pace, not startup pace.
- The Apache 2.0 model means any chain could fork and extend it — aligns with Logos' open-source ethos.

See also: [[Solang]], [[Solang Adoption]], [[Solang Limitations]], [[Neon Labs Business Model]]

## Open Questions

- Is Sean Young still the primary active maintainer as of 2025/2026, or has the contributor base diversified?
- Has the Cosmos CosmWasm target been implemented? (would expand reach significantly)
- Does the Solana Foundation provide cash grants to Solang, or only in-kind support?
- Could Logos Foundation sponsor a new Solang target for LEZ's VM — similar to how Cosmos sponsored CosmWasm support?

## Sources
- LF Decentralized Trust blog (Sep 2022): https://www.lfdecentralizedtrust.org/blog/2022/09/12/meet-hyperledger-solang-a-portable-solidity-compiler
- Hyperledger Solang wiki: https://lf-hyperledger.atlassian.net/wiki/spaces/SOL/overview
- Solana Labs Solang announcement (Jul 2023): https://solana.com/news/solang-solana-solidity-evm
- GitHub: https://github.com/hyperledger-solang/solang
- LF Decentralized Trust project page: https://www.lfdecentralizedtrust.org/projects/solang
