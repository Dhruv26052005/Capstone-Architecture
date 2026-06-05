# 🔒 BluffVault Protocol Security Specifications & Guardrails

This document outlines the strict validation controls, parameters, and error handling mechanisms enforced at the Solana smart contract instructions boundary for the BluffVault program.

---

## 🛠️ Main Error Paths & Core Security Checks

Every transaction passing through the protocol runtime environment must validate against these explicit guardrails before mutating any on-chain game state:

### 1. Entry Fee Validation
* **Error Token:** `InvalidEntryFee`
* **Security Control:** Evaluates the incoming transaction layer capital. The program automatically rolls back state changes and aborts execution if the deposit amount does not exactly match the initialization configuration.

### 2. Room Capacity Verification
* **Error Token:** `RoomFull`
* **Security Control:** Enforces strict boundary rules on the player array register. The contract automatically rejects connection requests if the registered user keys have already reached the maximum room capacity limit.

### 3. Execution Sequence Control
* **Error Token:** `InvalidTurn`
* **Security Control:** Prevents out-of-turn data injection vulnerabilities. The contract asserts that the transaction signer perfectly maps to the active turn counter pointer before updating any sub-round arrays.

### 4. Double Resolution Protection
* **Error Token:** `BluffAlreadyResolved`
* **Security Control:** Blocks transaction exploitation vectors. Once a challenge or pass state is marked on a specific turn block, any secondary attempt to trigger a duplicate challenge window is instantly rejected.

### 5. Double Withdrawal Prevention
* **Error Token:** `RewardAlreadyClaimed`
* **Security Control:** Hardened financial check guarding the escrow vaults. Validates withdrawal parameters to verify assets have not been extracted previously, preventing multi-payout or pool draining exploits.

### 6. Escrow Invariant Verification
* **Error Token:** `InsufficientVaultBalance`
* **Security Control:** A strict liquidity safety check. Evaluates the Program-Derived Address (PDA) token balance to guarantee the vault has sufficient funds before releasing any pro-rata payout weights to player accounts.

### 7. Signer Authority Enforcement
* **Error Token:** `UnauthorizedSigner`
* **Security Control:** Core wallet authorization guardrail. Validates transaction signatures to ensure that only the verified private key owner of the registered wallet can authorize and sign off on active game transactions.
