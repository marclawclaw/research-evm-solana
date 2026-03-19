---
topic: Solang Limitations
type: analysis
tags: [solang, limitations, erc-20, account-model, tooling, msg-sender, incompatibilities, developer-experience]
confidence: high
last_updated: 2026-03-19
sources:
  - https://solang.readthedocs.io/en/v0.3.3/targets/solana.html
  - https://medium.com/@smilewithkhushi/inside-solanas-developer-toolbox-a-2025-deep-dive-7f7e6c4df389
  - https://dev.to/shivamsspirit/minting-fungible-tokens-in-solana-using-solidity-solang-programming-language-50i6
  - https://soliditydeveloper.com/solana
---

# Solang Limitations

## Summary
Solang compiles Solidity to Solana BPF but cannot preserve Ethereum semantics — developers must rewrite key patterns to fit Solana's account model. ERC-20 is explicitly unsupported, `msg.sender` is unavailable, value transfers via CPI don't work, and try-catch statements break at runtime. The tooling ecosystem is shallow compared to both Anchor (native Solana) and Neon EVM (full EVM emulation), making Solang the worst of both worlds for developers seeking either native performance or drop-in compatibility.

## Key Facts

### 1. ERC-20 Incompatibility — Explicit Documentation

- **Official docs (v0.3.0 through v0.3.4, and latest):** `"The ERC-20 interface is not compatible with Solana at the moment."`
- This is not a bug — it's a structural incompatibility: Solana's token standard (SPL Token) works entirely differently from ERC-20
- ERC-20 assumes a single contract account holds balances and approvals; Solana SPL Token uses a **Mint Account + Token Accounts** model (one token account per holder per token type)
- Porting an ERC-20 token contract to Solana via Solang requires a fundamental redesign — it is not a "port", it is a rewrite

> [!fact] ERC-20 incompatibility confirmed in official Solang documentation across all v0.3.x releases including latest (v0.3.4).
> [!analysis] For DeFi protocols, this is a near-total blocker. The majority of EVM DeFi infrastructure (stablecoins, lending, DEX pools) is built on ERC-20 or ERC-20-derivative standards. No ERC-20 = no simple migration path.

### 2. `msg.sender` Unavailable — Fundamental Account Model Mismatch

- On Ethereum, `msg.sender` uniquely identifies the calling account in any context
- On Solana, **each contract execution uses multiple accounts** — there is no single "caller" account
- The Solana VM has no mechanism for fetching caller accounts automatically
- Solang's official workaround: use a dedicated `authority` account that must be declared as a signer in the transaction, accessed via `tx.accounts.authorityAccount.key`

**Implication for porting EVM contracts:**
- Any Solidity contract using `msg.sender` for access control (i.e., virtually every production contract) must be **redesigned**
- Common patterns (Ownable, ERC-20 `approve()`, ERC-721 `setApprovalForAll()`) all rely on `msg.sender` — all require rework
- This is not a syntactic change; it requires architectural understanding of Solana's account model

> [!fact] "There is no way to implement msg.sender" — Solang official docs v0.3.3, Solana target page.
> [!analysis] `msg.sender` is arguably the most pervasive primitive in Solidity contract security and access control. Its absence means every contract must be re-audited and re-architected for Solana. Combined with ERC-20 incompatibility, this invalidates the core value proposition of "write Solidity, deploy on Solana."

### 3. Value Transfer Limitations

- **CPI (Cross-Program Invocation) does not support value transfer** in the standard Solidity sense
- `auction.bid{value: 501}()` syntax — not permitted in Solana target
- `payable` keyword — has no effect
- `msg.value` — not supported
- `receive()` function — not permitted on Solana target

> [!fact] Confirmed in Solang docs v0.3.3: "Solana Cross Program Invocation (CPI) does not support this."
> [!analysis] Naive workaround suggested in docs (transfer SOL first, then inform callee via instruction data) is explicitly flagged as easily forgeable. Any protocol requiring ETH-value-passing semantics (auctions, escrow, payment channels) needs significant redesign.

### 4. Try-Catch Statements Don't Work

- `try-catch` statements are **not functional on the Solana target**
- If any external call or contract creation fails: the Solana runtime halts execution and **reverts the entire transaction**
- There is no error isolation — failed sub-calls cannot be caught and handled gracefully
- Additionally: **error definitions and reverts with error messages are not yet working** on Solana

> [!fact] Confirmed in Solang docs: "Try-catch statements do not work on Solana."
> [!analysis] This is particularly problematic for complex DeFi protocols that use try-catch for graceful degradation (e.g., checking multiple price oracles, fallback to backup liquidity pools). Without it, protocol resilience strategies must be redesigned.

### 5. Address Model Mismatch — 32-byte vs 20-byte

- Solana addresses are **32 bytes** (ed25519 public keys)
- Ethereum addresses are **20 bytes** (keccak256 of public key, last 20 bytes)
- Solang uses `address` type = 32-byte Solana key, not 20-byte Ethereum address
- Ethereum-style address literals (`0xE0f5206BBD...`) are **not supported** — must use Solana-format: `address"36VtvSbE6jV..."`
- Smart contracts hardcoding Ethereum addresses will not compile without modification

> [!fact] Confirmed in Solang docs v0.3.3.

### 6. Integer Size Mismatches

- Solana VM registers are **64-bit wide** — 64-bit integers (`uint64`, `int64`) are native
- `uint256` / `int256` operations (standard in Ethereum Solidity) are **split into multiple operations** on Solana
- Operations with `uint256`: multiplication, division, modulo — all significantly slower and use more compute units
- Balances and values on Solana are 64-bit: `address.balance`, `.transfer()`, `.send()` use `uint64`
- Any contract assuming 256-bit arithmetic for precision (DeFi math, fixed-point math libraries) needs performance profiling

> [!fact] Confirmed in Solang docs.
> [!analysis] Most DeFi protocols use `uint256` pervasively for precision and overflow safety. This means compute unit costs are higher than expected, and any ported contract needs careful gas profiling before mainnet deployment.

### 7. Missing EVM Builtins

- `ecrecover()` — **not available** (Solana uses ed25519, not ECDSA/secp256k1)
  - Replaced by `signatureVerify()` for ed25519 — but **cannot recover a signer from a signature** (fundamental capability difference)
  - Any contract using `ecrecover()` for signature verification (e.g., meta-transactions, EIP-712 permit patterns) is broken
- `gasleft()`, `block.gaslimit`, `tx.gasprice`, `gas()` Yul builtin — not available (Solana's compute budget model is different)
- Many Yul builtins are unavailable (see official availability table)
- `ECRECOVER` precompile is unavailable

> [!fact] "There is no ecrecover() builtin function because Solana does not use the ECDSA algorithm" — Solang docs.
> [!analysis] The absence of `ecrecover()` blocks a large class of Ethereum patterns: EIP-712 typed data signatures, EIP-2612 permit(), meta-transactions (EIP-2771), and Gnosis Safe-style multisig. These are all widely used in production protocols.

### 8. Account Management Overhead — Developer Burden

- On Solana, programs are stateless; state is stored in separate accounts
- A Solidity contract on Solana uses **two accounts**: program account (binary) + data account (storage variables)
- Instantiating a contract requires creating a data account — can be done client-side or via constructor annotations (`@payer`, `@space`, `@seed`, `@bump`)
- External function calls require the caller to **explicitly enumerate all accounts the callee needs** — accounts cannot be inferred from the contract interface
- This is a fundamental deviation from Ethereum's implicit account model

> [!fact] Solang docs v0.3.3: "When calling an external function or invoking a contract's constructor, one needs to provide the necessary accounts for the transaction."
> [!analysis] Requiring developers to enumerate dependent accounts breaks any "drop-in" mental model. Developers familiar with Ethereum's implicit state management must re-learn how to structure client-side calls — a Solana-specific skill that defeats the purpose of using Solidity as a shortcut.

### 9. Anchor Client Library Friction

- Solang compiles to Anchor-compatible IDL format
- Solidity function names are auto-converted to camelCase — inconsistency with Solidity conventions
- `.view()` calls only work on functions declared `view` or `pure`
- Return values from non-view functions are not accessible (Anchor library limitation)
- Error revert reasons are not decoded in error cases — `revert('reason')` messages lost
- Number arguments must be passed as `BN` values, not plain JavaScript `Number` — unexpected for Ethereum devs

> [!fact] Confirmed in Solang docs "Using the Anchor client library" section.
> [!analysis] Solang outputs Anchor-compatible programs but requires developers to use Anchor's JavaScript client patterns — an additional learning curve not present in Ethereum toolchains (Ethers.js, Viem).

### 10. Tooling Ecosystem Gap

- Community assessment (2025): **"Solang exists but is still underused due to lack of deep tooling integration"**
- No major IDE has native Solang support (Hardhat, Foundry, Remix — all EVM-centric)
- Debugging is limited — Solana BPF debugging is less mature than EVM debugging (no `console.log()` equivalent, limited revert traces)
- Contract verification on Solana explorers (Solscan, SolanaFM) is not standardised for Solang-compiled contracts
- No equivalent of Foundry fuzz testing, fork testing, or slither static analysis for Solang

> [!fact] "Underused due to lack of deep tooling integration" — Medium 2025 ecosystem overview.
> [!analysis] For a compiler-only approach (vs full EVM emulation), tooling is *more* important, not less — developers are accepting deeper integration with Solana's model, so they need better Solana-native tooling. The tooling gap makes Solang the worst position: loses Ethereum tooling (Hardhat/Foundry don't support SBF), doesn't gain Solana tooling (Anchor ecosystem doesn't understand Solang's Solidity syntax).

### 11. Functional Contract Upgrades — Different Pattern Required

- Ethereum Solidity uses proxy upgrade patterns (UUPS, TransparentProxy) — not directly applicable
- Solana uses `solana program deploy --program-id` for in-place upgrades (binary replacement)
- The upgrade mechanism is simpler in some ways but requires compatible data layout between versions
- Solidity upgrade patterns (`initialize()`, storage gaps, etc.) are unnecessary but developers may attempt to port them, causing confusion

> [!analysis] This is a minor limitation but reveals a broader pattern: every Solana-specific mechanism has a parallel Ethereum pattern that doesn't translate, forcing developers to unlearn before relearning.

## How it relates to Logos

> [!analysis] Analyst inference — not verified
> Solang represents the maximum feasible limitation profile for any Solidity-to-nativeVM compiler approach. The `msg.sender` absence and ERC-20 incompatibility are architectural — they can't be "fixed" without either abandoning fidelity to Solana's account model or creating an emulation layer (at which point you've re-invented Neon EVM). For LEZ: if LEZ uses a non-EVM, non-account-based VM, a Solang-style compiler for LEZ would inherit all of Solang's incompatibility categories. Only a full emulation approach (Neon-style) preserves Solidity semantics completely — at the cost of compute overhead.

See also: [[Solang]], [[Solang Adoption]], [[Neon EVM Limitations]], [[EVM vs Native SVM Tradeoffs]], [[LEZ EVM Relevance]]

## Open Questions

1. Is there a roadmap item for ERC-20 compatibility in Solang? (No evidence found)
2. Has any production protocol shipped on Solana mainnet using Solang? Which one?
3. Is there an active effort to add Foundry/Hardhat integration for Solang?
4. Could SPL Token 2022 (Solana's extended token program) be wrapped to provide ERC-20-like semantics in Solang?
5. What is Solang's maintenance status under Hyperledger post-2025? Are Solana Labs contributing upstream?

## Sources
- [Solang Solana Target Docs v0.3.3](https://solang.readthedocs.io/en/v0.3.3/targets/solana.html)
- [Inside Solana's Developer Toolbox: A 2025 Deep Dive — Medium](https://medium.com/@smilewithkhushi/inside-solanas-developer-toolbox-a-2025-deep-dive-7f7e6c4df389)
- [Minting Fungible Tokens in Solana using Solidity (Solang) — DEV Community](https://dev.to/shivamsspirit/minting-fungible-tokens-in-solana-using-solidity-solang-programming-language-50i6)
- [Deploying Solidity Smart Contracts to Solana — SolidityDeveloper.com](https://soliditydeveloper.com/solana)
- [LayerZero V2 on Solana: ERC-20 vs SPL — LayerZero Docs](https://docs.layerzero.network/v2/developers/solana/getting-started)
