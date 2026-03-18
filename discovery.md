---
topic: EVM Compatibility on Solana
type: summary
tags: [evm, solana, neon-evm, solang, developer-tools, lez]
confidence: medium
last_updated: 2026-03-18
sources:
  - https://neonevm.org
  - https://solang.readthedocs.io
  - https://neon-labs.org
  - https://github.com/hyperledger-solang/solang
  - https://medium.com/@neon_evm
---

# Discovery: EVM Compatibility Tools on Solana

## Purpose
This discovery document maps the landscape of EVM compatibility tools that run on or alongside the Solana Virtual Machine (SVM). Research is motivated by evaluating whether a similar tool makes sense for the Logos Execution Zone (LEZ).

---

## Landscape Overview

### Primary Tools Identified

| Tool | Type | Provider | Status |
|------|------|----------|--------|
| **Neon EVM** | Full EVM on Solana (native program) | Neon Labs | Active, mainnet |
| **Solang** | Solidity→SBF compiler | Hyperledger (Solana Labs contributor) | Active, underused |
| **Neon Stack** | EVM compat suite for any SVM chain | Neon Labs team | Active, licensed |
| **Eclipse** | SVM-based L2 on Ethereum with EVM compat | Eclipse Labs | Active |
| **Seahorse** | Python→Anchor/Rust transpiler | Community | Stalled/underused |

### Key Distinction
- **Neon EVM**: EVM *emulation layer* — Solidity contracts run unchanged, as if on Ethereum. Full toolchain compatibility (MetaMask, Hardhat, Ethers.js).
- **Solang**: Solidity *compiler* — compiles `.sol` to native Solana programs (BPF). Developer must adapt to Solana's account model.

---

## Research Questions

1. What tools enable EVM compatibility on Solana? (answered above in overview)
2. What is their adoption success? How many projects use them?
3. Do projects start with EVM tools and then migrate to native Solana?
4. Do projects stay on EVM tools forever or migrate?
5. What are the limitations? (computation costs, performance, UX)
6. Who provides these tools? Are they successful businesses?
7. Is it worth creating such a tool for LEZ?

---

## Topics for Deep Research

### Topic 1: Neon EVM — Architecture & Adoption
**Brief:** Full EVM emulation as a native Solana program. Uses iterative transaction model. 4,500 TPS claimed. ~$40M raised in 2021, additional strategic investment by Microsoft M12 in Aug 2024 (~$25.6M).
**Sources:**
- Official docs: https://neonevm.org/docs/evm_compatibility/overview
- Whitepaper (Dec 2024): https://neonevm.org/blog/Enabling-Solana-Users-to-Access-EVM-dApps-Running-on-Solana
- 2024 recap blog: https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af
- Consensys overview: https://consensys.io/blog/neon-an-ethereum-virtual-machine-on-solana
- Solana Compass deep dive: https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana
- GitHub: https://github.com/neonlabsorg/neon-evm
- Reddit sentiment: https://www.reddit.com/r/solana/comments/17tgwfs/how_actively_developed_is_neon_evm/

### Topic 2: Solang — Solidity Compiler for Solana
**Brief:** Compiles Solidity to Solana BPF bytecode. Part of Solana Tools Suite (v1.16.3+). Under Hyperledger umbrella. ERC-20 not directly compatible. Underused per community assessment.
**Sources:**
- Official docs: https://solang.readthedocs.io/en/latest/
- GitHub: https://github.com/hyperledger-solang/solang
- QuickNode comparison: https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon
- Solana Compass podcast: https://solanacompass.com/learn/Solfate/write-solidity-on-solana-with-solang-feat-sean-young-solana-labs-solfate-podcast-31
- Medium dev overview 2025: https://medium.com/@smilewithkhushi/inside-solanas-developer-toolbox-a-2025-deep-dive-7f7e6c4df389

### Topic 3: Developer Migration Patterns (EVM → Native SVM)
**Brief:** Do projects stay on EVM compat tools forever or migrate to Rust? Is Neon EVM a gateway or final home?
**Sources:**
- Solana.com EVM→SVM guide: https://solana.com/developers/evm-to-svm/smart-contracts
- Neon EVM "next chapter" post: https://neonevm.org/blog/neon-evms-next-chapter-becoming-the-fast-track-to-solana-for-evm-developers
- QuickNode guide (Solang vs Neon): https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon
- Reddit developer threads: https://www.reddit.com/r/solana/

### Topic 4: Neon Stack — Expanding to Other SVM Networks
**Brief:** Neon Stack is a licensable technology suite by the Neon EVM core team. Eclipse (SVM L2 on Ethereum) and Sonic (Solana gaming) have partnered. Represents B2B angle.
**Sources:**
- Eclipse partnership: https://www.eclipse.xyz/articles/bringing-evm-compatibility-to-eclipse-with-the-neon-stack
- Sonic partnership: https://thedefiant.io/news/press-releases/sonic-partners-with-neon-stack-to-bring-evm-compatible-dapps-to-solana
- Neon Medium post: https://medium.com/@neon_evm/neon-stack-powers-evm-compatibility-to-eclipse-svm-network-7dec01c24a9d
- Yona (Bitcoin SVM) interest mentioned in 2024 recap

### Topic 5: Limitations & Performance Analysis
**Brief:** Original compute unit overhead was 20M+ per swap; reduced to ~600K through optimization (97% reduction). EIP-1559 adopted. Solana congestion in 2024 was a stress test. Solang has ERC-20 incompatibilities.
**Sources:**
- Parallel EVM blog: https://neonevm.org/blog/Parallel-processing-EVM
- Solana Compass parallelizing EVM: https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana
- EVM compat overview: https://neonevm.org/docs/evm_compatibility/overview
- Reddit criticism threads

### Topic 6: Business Models & Company Health
**Brief:** Neon Labs raised $40M (2021, Jump Capital + Solana Capital), plus ~$25.6M strategic by Microsoft M12 (Aug 2024). NEON token launched. Solang is Hyperledger/open-source. Business model for Neon = token + grants + Neon Stack licensing?
**Sources:**
- CoinDesk $40M raise: https://www.coindesk.com/business/2021/11/09/neon-labs-raises-40m-to-bring-evm-functionality-to-solana
- Sacra profile (mentions M12): https://sacra.com/c/neon/
- Neon Labs site: https://neon-labs.org
- Crunchbase: https://www.crunchbase.com/organization/neon-labs-f4e6

### Topic 7: LEZ (Logos Execution Zone) Relevance
**Brief:** LEZ is Logos ecosystem's execution layer. No public info on LEZ+EVM found yet. Need to assess: is Logos SVM-inspired? What's the VM? What would EVM compat bring?
**Sources:**
- Internal Logos knowledge (Franck's context)
- Logos ecosystem docs: https://logos.co
- ecosystem.logos.co streams
- logos-co/eco-prio GitHub (private)
- Neon EVM as analogy/template

---

## Cron Schedule

| Topic | Session Label | Fire Time | Status |
|-------|--------------|-----------|--------|
| Topic 1: Neon EVM Architecture & Adoption | research-evm-t1 | T+0h (immediate) | scheduled |
| Topic 2: Solang | research-evm-t2 | T+6h | scheduled |
| Topic 3: Developer Migration Patterns | research-evm-t3 | T+12h | scheduled |
| Topic 4: Neon Stack & B2B Expansion | research-evm-t4 | T+18h | scheduled |
| Topic 5: Limitations & Performance | research-evm-t5 | T+24h | scheduled |
| Topic 6: Business Models & Company Health | research-evm-t6 | T+30h | scheduled |
| Topic 7: LEZ Relevance & Recommendation | research-evm-t7 | T+36h | scheduled |

---

## Initial Findings (Phase 1 Summary)

### What We Know
- **2 major tools**: Neon EVM (full EVM emulation) and Solang (compiler). Neon EVM is dominant.
- **Neon EVM is not dead**: $65M+ total funding, active product, 2024 whitepaper, 200K daily txns in points program.
- **Solang is underused**: Community confirms lack of deep tooling integration.
- **Neon Stack is the B2B play**: Licensing the tech to other SVM networks (Eclipse, Sonic) — this is the relevant model for LEZ.
- **Migration patterns unclear**: No strong evidence of mass EVM→native Solana migration. Neon positions itself as a "permanent home" not a stepping stone.
- **Performance improved dramatically**: 97% compute unit reduction achieved for token swaps.
- **Community health mixed**: Reddit threads show low developer activity in Discord (2023), but 2024 shows re-engagement via points program.

### Key Open Questions
- How many production dApps are actively running on Neon EVM mainnet? (not just testnet)
- What % of Neon users are "permanent" EVM devs vs explorers?
- What is LEZ's VM architecture — is it SVM-inspired or entirely custom?
- Would Logos/LEZ benefit more from Solang-style (native compilation) or Neon-style (full emulation)?
- Is Neon Stack a licensable product Logos could adopt directly?

---

## Notes on LEZ Context
LEZ (Logos Execution Zone) is Franck's team's focus at Logos. No public information found about LEZ + EVM compatibility. The relevance assessment will require internal context from Franck or Logos docs. This research provides the *supply side* (what exists); the *demand side* (what LEZ needs) must be validated internally.
