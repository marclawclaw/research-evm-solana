# Research: EVM Compatibility on Solana

> **Status:** Phase 2 in progress — Topics 1 (Neon EVM), 2 (Solang), 3 (Developer Migration Patterns), 4 (Neon Stack), 5 (Limitations & Performance), and 6 (Business Models) complete.
> **Last updated:** 2026-03-19
> **Research owner:** Researcher Agent (Marclaw)

## Purpose
Assess EVM compatibility tools on Solana for relevance to LEZ (Logos Execution Zone).

## Files

| File | Type | Description |
|------|------|-------------|
| `discovery.md` | Summary | Phase 1 landscape map — tools, sources, research topics, cron schedule |
| `run-log.md` | Log | Per-topic research run log |
| `notes/Neon EVM.md` | Concept | Full EVM emulation on Solana — architecture, components, constraints ✅ |
| `notes/Neon EVM Adoption.md` | Analysis | Traction metrics, community health, Neon Stack partnerships, Jan 2025 UX upgrade ✅ |
| `notes/Neon EVM Performance.md` | Analysis | TPS benchmarks (778 live), 97% CU optimization, parallel exec model, constraints ✅ |
| `notes/Solang.md` | Concept | Solidity compiler for SBF — architecture, LLVM pipeline, incompatibilities vs EVM ✅ |
| `notes/Solang Adoption.md` | Analysis | Usage stats, community health, vs Neon EVM adoption comparison ✅ |
| `notes/Neon Stack.md` | Concept | B2B EVM compat suite for SVM networks — components, licensing, all 3 integration partners ✅ |
| `notes/Eclipse EVM Integration.md` | Use-case | Eclipse's adoption of Neon Stack — architecture, dev UX, outcomes, LEZ reference case ✅ |
| `notes/Developer Migration Patterns.md` | Analysis | EVM→native Solana migration: do developers stay or move? ✅ |
| `notes/EVM vs Native SVM Tradeoffs.md` | Analysis | Dev experience, tooling, performance, composability tradeoffs across Neon EVM / Solang / native Rust ✅ |
| `notes/Neon EVM Limitations.md` | Analysis | Compute overhead, UX issues (pre/post Jan 2025), feature gaps, congestion exposure, operator centralisation ✅ |
| `notes/Solang Limitations.md` | Analysis | ERC-20 incompatibility, msg.sender absence, value transfer gaps, tooling shallowness, account model burden ✅ |
| `notes/Neon Labs Business Model.md` | Competitor | $45M raised, NEON token (~$0.03 vs $0.10 ICO), gas fees + Neon Stack B2B licensing, no follow-on VC ✅ |
| `notes/Solang Business Model.md` | Competitor | No commercial model — Apache 2.0 OSS under Hyperledger/LF, backed by Solana Labs contributor time + foundation grants ✅ |
| `notes/LEZ EVM Relevance.md` | Analysis | Should Logos build EVM compat for LEZ? |

> Notes marked `(pending)` will be created by Phase 2 cron jobs.

## Quick Summary

**2 primary tools** for EVM compat on Solana:
- **Neon EVM** — full EVM emulator as a native Solana program. $65M+ funded. Active. B2B via Neon Stack.
- **Solang** — Solidity-to-SBF compiler. Open source / Hyperledger. Underused.

**Key question for LEZ:** Neon Stack is the licensable B2B version — could Logos adopt or build something similar?
