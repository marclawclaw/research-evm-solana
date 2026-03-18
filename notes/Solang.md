---
topic: Solang
type: concept
tags: [solang, solidity, compiler, sbf, solana, evm, hyperledger]
confidence: high
last_updated: 2026-03-18
sources:
  - https://solang.readthedocs.io/en/latest/
  - https://github.com/hyperledger-solang/solang
  - https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon
  - https://solanacompass.com/learn/Solfate/write-solidity-on-solana-with-solang-feat-sean-young-solana-labs-solfate-podcast-31
---

# Solang — Solidity Compiler for Solana

## Summary
Solang is an open-source Solidity compiler (written in Rust) that compiles `.sol` source files to Solana BPF/SBF bytecode via LLVM. Unlike [[Neon EVM]], which runs a full EVM emulator on Solana, Solang produces native Solana programs — developers get Solidity syntax but must adapt to Solana's account model. ERC-20 contracts are not directly compatible.

---

## Key Facts

### Architecture
- **Compiler pipeline:** Solidity source → parse tree → semantic validation → control flow graph → LLVM IR → SBF bytecode (`.so`)
- **Backend:** LLVM (Low Level Virtual Machine) — handles optimization, binary production
- **Targets:** Solana (SBF), Polkadot (WASM), Stellar/Soroban (WASM)
- **Written in:** Rust
- **Creator:** Sean Young (formerly at IBM; started Solang after Web3 Foundation grant for Polkadot compiler)
- **Governance:** Under the [Hyperledger](https://www.hyperledger.org/) umbrella (Linux Foundation) since 2023
- **License:** Apache 2.0 (fully open source — contrast with Neon EVM's proprietary license)
- **Distribution:** Bundled in Solana Tools Suite since v1.16.3 — no separate install needed

### How It Compiles Solidity to SBF
1. Source parsed into AST (parse tree)
2. Variables resolved, types validated (Solidity 0.8 compat)
3. Control flow graph generated
4. CFG lowered to LLVM IR
5. LLVM optimizes and produces SBF binary
6. Output: `.so` file deployable as a native Solana program

> [!fact] Confirmed from official docs + podcast with Sean Young
> LLVM is the key enabler — it handles the heavy lifting of low-level optimization and BPF target code generation.

### Solana-Specific Adaptations
Solang compiles to native Solana programs, which means Solidity semantics are adjusted to fit Solana's runtime model:

| EVM Concept | Solana Equivalent in Solang |
|---|---|
| Single contract account (code + data) | Two accounts: program account (code) + data account (state) |
| `msg.sender` | **Not available** — no equivalent |
| 20-byte address (`0x...`) | 32-byte address (`address"Base58..."` syntax required) |
| Gas (per-instruction cost) | Compute units (200K CU per tx, no cost unless priority) |
| `uint256` / `int256` | Works but slow — SVM registers are 64-bit; use `uint64` / `int64` |
| ECDSA / `ecrecover()` | Not available — Solana uses Ed25519; `signatureVerify()` instead |
| ERC-20 interface | **Not compatible** with Solana SPL tokens |
| `try-catch` | Does not work — failures revert entire transaction |
| Function selectors (4 bytes) | Discriminators (8 bytes) |
| Stateful contracts | Stateless programs — state lives in external accounts |
| Contract upgrades via proxy | Native Solana upgrade via `solana program deploy --program-id` |

### Key Incompatibilities with Ethereum Solidity

> [!fact] Confirmed from official docs (solang.readthedocs.io/en/latest/targets/solana.html)

- `msg.sender` is unavailable on Solana — major breaking change for most EVM contracts
- `ecrecover()` unavailable — breaks signature-based auth patterns
- `try-catch` doesn't work
- Error definitions and reverts with messages not yet implemented
- Value transfer with function call doesn't work
- **ERC-20 is not compatible** — SPL tokens use a different standard and model
- Ethereum address literals (`0x...`) not supported
- Most Yul builtins unavailable
- External calls require explicit account declaration (Solana's CPI model)
- 64-bit registers preferred — `uint256` math is split into multiple ops (slower, more CUs)

### Compute Budget
- **Default limit:** 200,000 compute units per transaction
- **Unlike gas:** not charged per unit; users pay priority fees only if they want priority execution
- **Gas builtins** (`gasleft`, `tx.gasprice`, etc.) are absent — Solana has no gas concept
- Operations wider than 64 bits (e.g., `uint256` multiply, divide, modulo) split into multiple ops, consuming more CUs

### Tooling Integration
- Works with **Anchor** framework (de facto Solana framework) — Solang generates Anchor-compatible IDLs
- Clients interact using **Solana-Web3.js** (not ethers.js — contrast with Neon EVM)
- Wallets: Solana wallets (Phantom, Backpack) — EVM wallets (MetaMask) do not work
- Gas token: SOL (not NEON or ETH)
- Explorer: any Solana explorer (Solscan, etc.)
- VS Code extension available; language server supports formatting, go-to-definition, references

### Multi-Chain Support
- **Polkadot:** Compiles to WASM; OpenZeppelin support in progress
- **Stellar/Soroban:** Added in v0.3.4 (June 2024); supports cross-contract calls, SAC, Soroban Authorization
- Solang positioned as a cross-chain Solidity compiler, not just Solana-specific

---

## How It Relates to Logos

> [!analysis] Analyst inference — not verified

- **LEZ parallel:** If LEZ uses a custom VM (not EVM), a Solang-style compiler (Solidity → native LEZ bytecode) is one approach — pure compilation, no emulation overhead, open source model
- **Contrast with Neon model:** Solang = native programs that happen to be written in Solidity; Neon EVM = EVM compatibility layer that wraps Solana
- **LEZ tradeoff:** Solang approach is lighter (no overhead of full EVM emulation) but breaks EVM toolchain compatibility — devs need Solana/LEZ-specific tools, not MetaMask/Hardhat
- **ERC-20 incompatibility** is a significant barrier for any EVM asset migration use case
- Solang shows that a Rust-based, LLVM-backed Solidity compiler is achievable by a small team; open source + Hyperledger foundation provides governance template
- Apache 2.0 license = freely forkable, no commercial restriction

---

## Open Questions

- When will ERC-20 compatibility land on Solana target? (marked "in progress" but no ETA)
- Will `msg.sender` ever be implemented? (fundamental Solana account model conflict)
- Is there a roadmap for LEZ-equivalent targets in Solang, or would a fork be needed?
- How does Solang's output compare in compute unit efficiency vs native Rust Anchor programs?
- Can Solang-compiled contracts be called from Rust Anchor programs seamlessly (CPI)?

---

## Sources

- Official docs: https://solang.readthedocs.io/en/latest/targets/solana.html
- GitHub repo: https://github.com/hyperledger-solang/solang
- QuickNode Solang vs Neon guide: https://www.quicknode.com/guides/solana-development/solidity/getting-started-solang-neon
- Solfate podcast (Sean Young, Solang creator): https://solanacompass.com/learn/Solfate/write-solidity-on-solana-with-solang-feat-sean-young-solana-labs-solfate-podcast-31
