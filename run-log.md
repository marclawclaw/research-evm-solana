# Research Run Log — EVM Compatibility on Solana

---

## [2026-03-19 05:53 AEDT] Topic 4: Neon Stack — B2B EVM Compat Suite — status: ok

- **Notes written:** 2
  - `notes/Neon Stack.md` — What Neon Stack is, how it works (Neon EVM contracts + Neon Proxy), all 3 integration partners (Eclipse, Sonic, Yona), licensing model analysis, LEZ relevance
  - `notes/Eclipse EVM Integration.md` — Eclipse's use of Neon Stack as first integration partner, developer UX outcomes, architecture overview, canonical reference case for LEZ
- **Sources crawled:**
  - `https://www.eclipse.xyz/articles/bringing-evm-compatibility-to-eclipse-with-the-neon-stack` — ✅ Full Eclipse Labs announcement; architecture overview, developer benefits, Neel Somani quote
  - `https://medium.com/@neon_evm/neon-stack-powers-evm-compatibility-to-eclipse-svm-network-7dec01c24a9d` — ✅ Neon EVM Medium; full technical deep dive, component breakdown, market stats (13K dApps / 0.4% SVM), Davide Menegaldo quote
  - `https://neonevm.org/blog/Neon_Stack_Powers_EVM_Compatibility_to_Eclipse_SVM_Network` — ⚠️ Minimal (JS-rendered content; newsletter signup only served); same content captured via Medium post
  - `https://thedefiant.io/news/press-releases/sonic-partners-with-neon-stack-to-bring-evm-compatible-dapps-to-solana` — ✅ Sonic partnership press release (July 2024); gaming vertical, HyperGrid L2, CEO quotes
  - `https://thedefiant.io/news/blockchains/yona-network-integrates-neon-for-evm-and-svm-compatibility-on-bitcoin` — ✅ via web_search snippet; Yona Network (Bitcoin SVM) integration confirmed July 2024
  - `https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af` — ✅ via web_search snippet; confirmed Neon Stack tested in May 2024, Eclipse deployed, Sonic + Yona interest
- **Notable findings:**
  - Neon Stack = Neon EVM smart contracts + Neon Proxy (wraps EVM txns into Solana txns, provides Web3 JSON-RPC API)
  - **3 confirmed integration partners by mid-2024:** Eclipse (SVM L2 on Ethereum), Sonic (gaming SVM L2 on Solana), Yona Network (Bitcoin SVM L2)
  - Eclipse was first, announced May 3, 2024 — launched following "test runs and optimizations" (no hard launch date found)
  - Market framing: 13,000+ EVM dApps; only 0.4% ever migrated to SVM chains → massive untapped market for any SVM network adopting Neon Stack
  - B2B model: CCO-led commercial licensing (Davide Menegaldo); not open source — contrast with Solang's Apache 2.0
  - Neon Stack is positioned as the **only** mainnet-live EVM-on-SVM stack as of announcements — OP Stack and Polygon CDK are EVM-only
  - Yona's use case opens a new angle: Neon Stack enables EVM + SVM compatibility on a **Bitcoin** L2 — not just Ethereum/Solana networks
  - Full EVM toolchain preserved for developers: Hardhat, Remix, Truffle, MetaMask, WalletConnect, OpenZeppelin, Chainlink
- **LEZ relevance flag:**
  - If LEZ is SVM-based: Neon Stack is a ready-made licensable solution — Eclipse integration is the reference playbook
  - If LEZ uses a custom VM: the Neon Stack architecture (proxy adapter + EVM bytecode interpreter + iterative txn model) is the design template
  - Eclipse use case = exact analogue: non-Ethereum, non-mainnet-Solana SVM network adding EVM as a layer
  - Key open question: can Logos negotiate a Neon Stack license, or does LEZ's VM architecture require a custom build?
- **API budget status:** `openclaw usage cost` unavailable — proceeded normally

---

## [2026-03-18 23:52 AEDT] Topic 3: Developer Migration Patterns (EVM → SVM) — status: ok

- **Notes written:** 2
  - `notes/Developer Migration Patterns.md` — Evidence for/against EVM→native Solana migration; who stays vs who moves; Neon EVM as permanent home vs Solang as partial bridge
  - `notes/EVM vs Native SVM Tradeoffs.md` — Comparative analysis: programming model, tooling, performance, composability, security surface across Neon EVM / Solang / native Rust+Anchor
- **Sources crawled:**
  - `https://solana.com/developers/evm-to-svm/smart-contracts` — ⚠️ Minimal content retrieved (JS-heavy page); key quote captured via web search
  - `https://neonevm.org/blog/neon-evms-next-chapter-becoming-the-fast-track-to-solana-for-evm-developers` — ⚠️ Limited (newsletter signup page only served)
  - `https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon` — ✅ Full Solang vs Neon EVM comparison, Solana account model primer
  - `https://medium.com/@neon_evm/neon-evms-mainnet-upgrade-paving-a-solana-native-future-for-ethereum-dapps-39e9badc142b` — ✅ Jan 2025 mainnet upgrade; "permanent home" positioning confirmed
  - `https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af` — ✅ 2024 strategy recap; no-Rust-needed positioning confirmed
  - `https://medium.com/@neon_evm/bridging-the-gap-between-ethereum-and-solana-neon-evm-and-solang-13202e5eff74` — ✅ Neon EVM vs Solang architectural comparison; Solang's account model exposure confirmed
  - `https://cryptoslate.com/how-neon-evm-blends-ethereum-and-solana-to-boost-blockchain-app-development-interview/` — ✅ CCO interview (Apr 2024); "months or a year" migration cost quote
  - `https://www.reddit.com/r/solana/comments/13d7kbl/from_evm_to_solana/` — ⚠️ Limited (JS blocking); key sentiment captured from snippet: "consider just using polygon" advice
  - Reddit search results: ✅ Multiple threads captured via `web_search site:reddit.com` — consistent: account model is primary barrier, not Rust
  - `https://www.zealynx.io/blogs/evm-to-svm-guide` — ✅ 2026 EVM→SVM study plan (6-week program); confirms active migration demand in security space
  - `https://blog.sigmaprime.io/transitioning-from-evm-to-svm-key-concepts-for-solana-security-assessment.html` — ✅ SigmaPrime Apr 2025; SVM-specific security concepts distinct from EVM
- **Notable findings:**
  - **No documented migration from Neon EVM → native Rust found** — the "stay forever" hypothesis is supported by absence of contrary evidence
  - Neon EVM's 2024–2025 strategy explicitly removes every remaining reason to rewrite in Rust (Solana wallets, EIP-1559, whitepaper)
  - Solang is positioned as a partial bridge — requires understanding Solana account model (PDAs, stateless programs, SPL tokens), which naturally leads to Rust
  - Reddit community consensus: the account model (not Rust syntax) is the real learning barrier
  - Security researchers are the one group forced to learn native SVM regardless of tooling preferences
  - Composability gap is the critical limit of Neon EVM: cannot CPI into native Solana programs (Jupiter, Raydium, etc.) — relevant for DeFi use cases
  - Performance ceiling: ~778 TPS (Neon EVM) vs 65K+ TPS (native Solana) — 97% gap; 600K CUs/swap vs ~200K native
- **LEZ relevance flag:** Full EVM emulation (Neon-style) = permanent residency, no migration. Compiler-style (Solang-style) = partial bridge, some migration possible. Composability with native LEZ programs requires compiler/native approach.
- **API budget status:** `openclaw usage cost` unavailable — proceeded normally

---

## [2026-03-18 17:51 AEDT] Topic 2: Solang — status: ok

- **Notes written:** 2
  - `notes/Solang.md` — architecture, LLVM pipeline, Solana-specific adaptations, incompatibilities vs EVM/ERC-20
  - `notes/Solang Adoption.md` — GitHub metrics, community assessment, adoption vs Neon EVM, roadmap gaps
- **Sources crawled:**
  - `https://solang.readthedocs.io/en/latest/` — ✅ landing page, Solana target page
  - `https://solang.readthedocs.io/en/latest/targets/solana.html` — ✅ full incompatibilities list, compute model, account model
  - `https://github.com/hyperledger-solang/solang` — ✅ README, roadmap table, license
  - `https://github.com/hyperledger-solang/solang/releases` — ✅ v0.3.4 release notes, active contributor set
  - `https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon` — ✅ Solang vs Neon comparison table
  - `https://solanacompass.com/learn/Solfate/write-solidity-on-solana-with-solang-feat-sean-young-solana-labs-solfate-podcast-31` — ✅ Sean Young (Solang creator) on compiler design and Solana tradeoffs
  - `https://medium.com/@smilewithkhushi/inside-solanas-developer-toolbox-a-2025-deep-dive-7f7e6c4df389` — ✅ 2025 ecosystem assessment; confirms Solang "underused due to lack of deep tooling integration"
- **Notable findings:**
  - Solang is a true native compiler (Solidity → LLVM IR → SBF); no EVM overhead at runtime
  - ERC-20 explicitly listed as incompatible with Solana target in official docs
  - `msg.sender` unavailable — major breaking change for any EVM contract port
  - Apache 2.0 license — forkable, commercially usable (contrast: Neon EVM is proprietary)
  - ~1,300 GitHub stars vs Anchor's 4,400 — significant adoption gap
  - "Underused" confirmed by both community observers (2025) and indirect evidence (no known production dApps)
  - Neon EVM wins on adoption because it preserves the full EVM toolchain; Solang only preserves the language
  - Solang's multi-chain scope (Solana + Polkadot + Stellar) may dilute focus
  - Sean Young (creator): Solidity on Solana doesn't isolate devs from Solana's account model complexity
- **LEZ relevance flag:** Solang Apache 2.0 license makes it a viable fork/collaboration point for a LEZ Solidity compiler target if needed. Adoption data strongly suggests full EVM emulation (Neon-style) wins on developer uptake vs compiler-only approach.
- **API budget status:** Not checked (command unavailable); proceeded normally

---

## [2026-03-18 11:52 AEDT] Topic 1: Neon EVM Architecture & Adoption — status: ok

- **Notes written:** 3
  - `notes/Neon EVM.md` — architecture, components, technical constraints, license, funding
  - `notes/Neon EVM Adoption.md` — traction metrics, community health, Neon Stack partnerships, Jan 2025 mainnet upgrade
  - `notes/Neon EVM Performance.md` — TPS benchmarks, 97% CU optimization, optimistic execution model, known limits
- **Sources crawled:**
  - `https://docs.neonevm.org/docs/evm_compatibility/overview` — ✅ (redirected from neonevm.org/docs/)
  - `https://medium.com/@neon_evm/neon-evm-recap-2024-the-year-of-extending-solana-e04ac9da49af` — ✅
  - `https://consensys.io/blog/neon-an-ethereum-virtual-machine-on-solana` — ✅
  - `https://solanacompass.com/learn/Validated/parallelizing-the-evm-on-solana` — ✅
  - `https://neonevm.org/blog/achieving-full-parallel-execution-in-blockchain:-a-neon-evm-technical-breakthrough` — ❌ 404 (URL dead); info sourced from 2024 recap instead
  - `https://github.com/neonlabsorg/neon-evm` — ✅ (license, repo structure confirmed)
  - `https://medium.com/@neon_evm/neon-evms-mainnet-upgrade-paving-a-solana-native-future-for-ethereum-dapps-39e9badc142b` — ✅ (bonus: Jan 2025 upgrade details)
  - `https://www.reddit.com/r/solana/comments/17tgwfs/how_actively_developed_is_neon_evm/` — ⚠️ Limited content retrieved (Reddit JS blocking); captured key sentiment ("Discord feels dead" — Nov 2023)
- **Notable findings:**
  - 97% compute unit reduction (20M+ → ~600K for Uniswap v2 swap) — key engineering milestone
  - 778 TPS on live mainnet — near the theoretical combined cap of all Ethereum L2s (~880 TPS)
  - Neon EVM license is proprietary (non-commercial, no fork) — source-available only
  - Jan 2025 mainnet upgrade adds Solana Signature SDK (Phantom/Backpack sign EVM txns) + EIP-1559
  - Optimistic execution model launched 2024 — full parallel EVM processing on Solana
  - Neon Stack (B2B): Eclipse, Sonic already deployed; Yona (Bitcoin SVM) expressed interest
  - Community health: Discord was "dead" in Nov 2023; re-engaged via 2024 points program (11K DAU, 200K daily txns peak)
- **API budget status:** `openclaw usage cost` unavailable — proceeded normally
