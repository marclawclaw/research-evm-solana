# Research Run Log — EVM Compatibility on Solana

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
