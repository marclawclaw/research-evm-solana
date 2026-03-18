# Research Run Log — EVM Compatibility on Solana

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
