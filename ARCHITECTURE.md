# 🏗️ Technical Protocol Architecture Spec

This document outlines the visual layout bounds, program boundaries, and persistent memory footprints used by the BluffVault smart contract environment.

---

## 📊 Visual Protocol Flow & Component Matrix

The following architecture chart maps the core interactions between user wallets and the non-custodial Solana runtime layers. 

*(GitHub will automatically render this as a dynamic flowchart diagram on your repository page)*

```mermaid
graph TD
    classDef wallet fill:#ffffff,stroke:#4a5568,stroke-width:2px,border-radius:5px;
    classDef program fill:#ebf8ff,stroke:#2b6cb0,stroke-width:2px,stroke-dasharray: 5 5;
    classDef storage fill:#e6fffa,stroke:#319795,stroke-width:2px;

    W1[Player 1: Bluffer Wallet]:::wallet
    W2[Player 2: Responder Wallet]:::wallet

    subgraph Solana_On_Chain_Layer [Solana BPF Runtime Environment]
        BP[BluffVault Core Anchor Program]:::program
        GS_PDA[PDA Account: MatchRoom State Account]:::storage
        EV_PDA[PDA Account: Secure Escrow Vault]:::storage
    end

    %% Flow Steps
    W1 -- "1. commit_stake [Sends Hash + Claim String]" --> BP
    BP -- "2. Write Hash & Public Claim to Storage" --> GS_PDA
    W2 -- "3. Read Live Context Stream via RPC Node" --> GS_PDA
    W2 -- "4. respond_stake [Flags Challenge or Pass Status]" --> BP
    BP -- "5. Mutate Response State Flag & Freeze Pipeline" --> GS_PDA
    W1 -- "6. reveal_stake [Sends Raw Stake + Secret Salt]" --> BP
    BP -- "7. Execute Runtime Hash Verification Match" --> BP
    BP -- "8. Mutate Vault Point Array Weights" --> GS_PDA
    
    %% Capital Operations
    W1 & W2 -- "Join & Initialization Deposit" --> BP
    BP --> EV_PDA
    GS_PDA -- "Round 5 Complete: Read Allocation Weights" --> BP
    EV_PDA -- "Release Pro-Rata Settlement Tokens" --> W1 & W2
