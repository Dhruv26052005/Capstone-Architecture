# 🎭 BluffVault Protocol

BluffVault is a 100% decentralized, non-custodial, peer-to-peer psychological bluffing game built natively on the Solana blockchain using the **Anchor Framework**. 

The protocol operates independently of any centralized server-side environments, Web2 middlewares, or off-chain tracking databases. Absolute interaction privacy (hiding actual wagers) is handled deterministically via an on-chain cryptographic **Commit-Reveal Scheme** executing directly between the user's client wallet interface and the Solana runtime.

---

## 🎯 Protocol Purpose & Core Mechanics

* **Trustless Escrow Environment:** Secures entry fees ($5.00 worth of tokens/lamports) and dynamic round wagers safely in an isolated program-derived container.
* **Asymmetric Data Privacy:** Implements client-side commitment hashing to allow players to bluff or declare honesty without leaking raw ledger records to opponents scanning block explorers.
* **Automated Decentralized Arbitration:** Resolves disputes, scores metrics, handles timeout slashing, and distributes final reward pools pro-rata based entirely on verified cryptographic states.

---

## 🛠️ On-Chain Instructions & Workflow Sequences

1. **`initialize_match(ctx, match_id)`**
   Allocates memory and deploys a clean `MatchRoom` state instance on-chain.
2. **`join_match(ctx)`**
   Locks exactly $5.00 safety entry fee from the client wallet into the `EscrowVault` PDA and registers their `Pubkey`.
3. **`commit_stake(ctx, commitment_hash, public_claim)`**
   Saves the 32-byte hash block of the hidden move and shifts the action flag to the target responder.
4. **`respond_stake(ctx, action_flag)`**
   Allows the subsequent player to read the state via an RPC stream and write a `Pass` or `Challenge` flag.
5. **`reveal_stake(ctx, raw_stake, secret_salt)`**
   The bluffer reveals their raw input. The contract computes a runtime hash verification match (`Keccak256(raw_stake + salt) == active_commitment`). Settle points array.
6. **`enforce_timeout(ctx)`**
   Security mechanism: If any player stalls the transaction rotation beyond 15 seconds, any participant can call this function to slash their safety deposit.
7. **`settle_and_claim(ctx)`**
   Executes on Round 5 completion. Reads point arrays and issues pro-rata winnings back to player wallets.

---

## 📂 Repository Documentation Structure

To understand the core technical details of this protocol, please refer to our modular architecture and security specifications:
* 👉 **[ARCHITECTURE.md](./ARCHITECTURE.md):** Complete On-Chain Account layouts, Byte Schema tables, and the Mermaid.js flow diagram.
* 👉 **[SECURITY.md](./SECURITY.md):** Detailed security guardrails, validation checks, and strict custom error paths.
