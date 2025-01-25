# Table of Contents

-   [Introduction to the EVM](#introduction-to-the-evm)
-   [EVM Architecture](#evm-architecture)
-   [EVM Execution Environment](#evm-execution-environment)
-   [Transaction Processing](#transaction-processing)

# Introduction to the EVM

## What is VM?

-   A virtual machine (VM) is, in general, a piece of software that simulates a full computer system—complete with its own CPU, memory, storage, and operating environment—inside another, “host” environment.
-   We can simply think of it as **“computer within a computer”**.

## What is the EVM?

-   The Ethereum Virtual Machine (EVM) is the computation engine at the heart of the Ethereum protocol.
-   It is sometimes described as a **“world computer”** because every node in the Ethereum network runs an instance of the EVM, verifying the same instructions and state transitions.

-   **Virtual Machine**: Like a computer operating system but specialized for running Ethereum’s smart contracts.
-   **Isolated Execution**: The EVM provides a **sandboxed** environment. Contracts can only access and modify Ethereum state via the EVM’s rules and resources (stack, memory, storage).

## Why Does Ethereum Need a Virtual Machine?

1. **Smart Contract Execution**:

-   Ethereum extends the concept of a blockchain from simple token transfers (like Bitcoin) to **arbitrary code** execution.
-   The EVM enforces security, ensures deterministic outcomes, and provides a uniform standard for all smart contract transactions.

2. **Deterministic & Trustless**:

-   Every node runs the same EVM code, ensuring **consensus** about the state.
-   No matter which node executes a transaction, the result is guaranteed to be the same (assuming honest consensus).

3. **Stateful Contracts**:

-   Contracts can maintain **persistent state** in Ethereum’s world state—something not natively provided by earlier blockchain systems.
-   The EVM manages how contracts read/write this state, ensuring correctness and preventing unauthorized access.

4. **Gas & Resource Management**:

-   The EVM incorporates the concept of **gas** to meter execution. This prevents infinite loops or spam, since each instruction consumes gas.
-   Users pay for the computation they trigger, aligning incentives with network resources.

## EVM vs. Traditional Computing Models

1. **World State**:

-   Unlike traditional systems where each program runs on a personal computer, the EVM’s state is **global** and replicated across all Ethereum nodes.
-   Every node has the same ledger and contract storage, guaranteeing **consistent data**.

2. **Deterministic Execution**:

-   The EVM must be fully deterministic: given the same transaction, every node must arrive at the same outcome.
-   Randomness, timing, or external system calls are heavily restricted or simulated via on-chain patterns (e.g., block hash for pseudo-randomness).

3. **Immutability & Code**:

-   Once deployed, a contract’s bytecode is **immutable** (unless using patterns like proxies).
-   The code cannot be changed after deployment, which is key to trust minimization but also demands careful design for upgrades.

# EVM Architecture

-   The Ethereum Virtual Machine (EVM) is often described as a **state machine** running atop a Harvard-style architecture.
-   This design influences how contracts store code, manage data, and interact with the rest of the blockchain.

## The EVM as a Stack-Based Machine

1. **Stack-Oriented Execution**

-   Internally, the EVM uses a **stack** to execute instructions.
-   Each instruction can push or pop 256-bit words from this stack.
-   The stack has a maximum depth of 1,024 elements—exceeding this limit reverts the transaction.

2. **Harvard Architecture vs. Von Neumann**

-   Traditional computers (von Neumann architecture) store code and data in a single memory space.
-   In the EVM (somewhat inspired by a Harvard architecture concept), **code is immutable and separate**. You can’t modify contract bytecode at runtime.
-   The contract’s **storage** is distinct from its runtime code and from the ephemeral memory region used during execution.

3. **Instruction Set**

-   The EVM has a specialized set of opcodes (e.g., `ADD`, `MUL`, `CALL`, `CREATE`, `SSTORE`, etc.).
-   Each opcode manipulates the stack, memory, or storage.
-   The EVM’s design ensures deterministic execution—every node processes instructions identically.

## The Account Model

Ethereum differs from many older blockchains (like Bitcoin) by employing an **account-based** model rather than a UTXO model. This is critical to how the EVM tracks state.

1. **Two Types of Accounts**

-   **Externally Owned Accounts (EOAs)**: Controlled by private keys (e.g., user wallets). They have no code.
-   **Contract Accounts**: Hold contract code (bytecode) and can contain persistent storage.

![account model](images/account-model.png)

1. **Account Fields**

-   **Nonce**: Number of transactions sent from an account (for EOAs) or number of contract creations performed by that account (for contract accounts).
-   **Balance**: Amount of Ether (in wei) owned by the account.
-   **Storage Root**: A hash (root of a Merkle Patricia Trie) representing the contract’s storage data.
-   **Code Hash**: A hash of the contract’s bytecode, from which code can be retrieved.

3. **Contract Code & Storage**

-   A contract’s **runtime code** is immutable after deployment.
-   Contract storage is a key-value store, mapping 256-bit slots to 256-bit values. This is where a contract’s persistent state lives.

## Global State Tree

![global state tree](images/global-state-tree.png)

1. **Merkle Patricia Trie**

-   Ethereum organizes the entire “world state” in a data structure called a **Merkle Patricia Trie (MPT)**.
-   Each account is a node in this tree, keyed by its address.
-   The trie’s root hash is stored in the block header, providing a **tamper-evident** record of the entire state.

2. **Per-Contract Storage Trie**

-   Each contract account has its own separate **storage** MPT.
-   Accessing or changing contract storage updates this sub-trie, affecting the main state root.

3. **Why a Trie?**

-   Ensures **efficient** and **verifiable** lookups of any account or storage slot.
-   Supports **light clients** or partial proofs about specific state data without revealing the entire state.

## EVM Memory Model

![evm architecture](images/evm-architecture.png)

During transaction execution, the EVM provides **ephemeral** storage areas:

1. **Memory**

-   A contiguous byte-array that resets after each transaction.
-   Typically used for **intermediate** data manipulation or ABI encoding/decoding.
-   Cost grows with **how much memory is accessed** (32-byte increments).

2. **Stack**

-   A LIFO stack for pushing/popping 256-bit words.
-   Used for **operands** of arithmetic, logical operations, etc.

3. **Transient vs. Persistent**

-   **Memory and stack** are transient: they exist only during the function execution.
-   **Contract storage** is persistent: changes remain after the transaction ends.

## Distinction Between Code and Data

1. **Code Section**

-   A contract’s deployed bytecode is immutable.
-   The EVM can only execute code (read as instructions); it cannot modify it or treat it as storage.

2. **Data (Storage, Memory, Call Data)**

-   **Storage**: persistent, contract-specific key-value store.
-   **Memory**: ephemeral, used for dynamic data within a transaction.
-   **Calldata**: the input payload of a transaction or message call, read-only.

3. **Why Is This Important?**

-   This separation enforces that smart contract logic is not self-modifying.
-   Contracts operate on **immutable code** with mutable data in a safe, isolated manner.

## Contract Calls & Message Layer

1. **Messaging**

-   Contracts communicate with each other via message calls (low-level opcodes like `CALL`, `DELEGATECALL`, `STATICCALL`).
-   Each call spawns a new, nested EVM execution context with its own memory.

2. **Depth Limitation**

-   There’s a max call depth (1024), preventing infinitely recursive calls.

3. **Gas Forwarding**

-   Each call can specify how much gas to forward to the callee, ensuring the caller can maintain enough gas to handle results or reverts.

## Putting It All Together

At a high level:

-   **Every Ethereum node** maintains a replicated copy of the global state (the Merkle trie of accounts).
-   When a user or contract triggers a transaction, the EVM on each node executes the same bytecode instructions, modifies contract storage or account balances accordingly, and yields a **deterministic result**.
-   By combining a **stack-based instruction set**, a **clear separation** of code vs. data, and cryptographic tries for storing global state, the EVM ensures **consistency, isolation, and auditability** for every smart contract operation on Ethereum.

![evm-putting-it-all-together](images/evm-putting-it-all-together.png)

# EVM Execution Environment

-   The **execution environment** in the Ethereum Virtual Machine (EVM) refers to all the **contextual data** and **runtime resources** provided to a piece of executing code during a transaction or message call.
-   Each invocation (transaction, call, `create` operation) has its own environment that dictates which opcodes are available, how they behave, and what data they can access.

## Overview of Execution Context

When the EVM runs a transaction or an internal message call, it sets up several context variables:

1. **`msg.sender`**

-   The address that initiated this call.
    For the **top-level transaction**, msg.sender is the Externally Owned Account (EOA) that signed and sent the transaction.
    For **internal calls**, msg.sender is the calling contract.

2. **`msg.value`**

-   The amount of Ether (in wei) sent along with this call/transaction.
-   If a function is not `payable`, sending Ether to it will revert.

3. **`msg.data`**

-   The raw call data (bytes) passed to the function, including the function selector plus encoded parameters.
-   In Solidity, function arguments are typically decoded from `msg.data` automatically under the hood.

4. **`tx.origin`**

-   The **original sender** of the top-level transaction (always an EOA).
-   Generally **not** recommended for authorization checks because it can be exploited via calls from malicious contracts.

5. **`gas`**

-   The amount of gas allocated to this execution context. If the code runs out of gas, it reverts.
-   You can optionally specify how much gas to forward in low-level calls like `call`/`delegatecall`.

6. **Address Variables**

-   `address(this)`: the address of the executing contract.
-   `msg.sender`: as above, the immediate caller.
-   In an **internal** `delegatecall`, `address(this)` is still the proxy (caller), not the implementation contract.

8. **Block Context (accessible via opcodes or global variables)**

-   `block.number`: The current block number.
-   `block.timestamp`: The current block’s timestamp (in seconds).
-   `block.coinbase`: The miner/validator’s address.
-   `block.difficulty` or `block.prevrandao`: In older versions, used for difficulty; now it’s used differently in PoS. A random number provided by the beacon chain.

9. **Transaction Context**

-   `tx.gasprice`: The gas price of the transaction.
-   `tx.origin`: The EOA that started the transaction (mentioned above).

These values define the **who, how much, and why** of the current EVM call frame.

## Transaction Lifecycle

A **transaction** is the external entry point into the Ethereum state transition. It leads to the following steps:

1. **Signature & Validation**

-   The network verifies the transaction’s signature (which EOA signed it) and checks if the sender’s **nonce** is correct.
-   The transaction includes gas details (gas limit, max fee per gas, etc.) which must be sufficient.

2. **State Preparation**

-   The EVM deducts the **upfront gas cost** from the sender’s balance.
-   Sets up the execution environment, populating `msg.sender`, `msg.value`, `msg.data`, etc.

3. **Execution**

-   The EVM runs the specified code (e.g., calling a contract function or deploying a contract).
-   Any errors (e.g., out of gas, revert) cause the **entire state change** to revert.

4. **Final State & Receipt**

-   If successful, any state changes (storage updates, Ether transfers) are committed.
-   Remaining gas is refunded appropriately, and a transaction **receipt** is produced with logs/events.

# Transaction Processing

-   In Ethereum, **transactions** are the mechanism by which **state transitions** happen in the EVM.
-   Whether you’re transferring Ether between accounts or invoking a smart contract, it all starts with a transaction.

## Types of Transactions

1. **Externally Owned Account (EOA) → Contract/Account**

-   A user (with a private key) initiates a transaction to call a contract or simply transfer Ether to another address.

2. **Contract → Contract (Message Calls)**

-   Within the EVM, a contract can trigger internal calls, but these are **not top-level transactions**. They’re **sub-calls within an existing transaction**.

3. **Contract Creation**

-   A special transaction without a `to` address, containing contract **creation code** in the data payload.
-   If successful, it deploys a new contract instance with its own address.

## Transaction Fields (Based on EIP-1559)

![transaction-architecture](images/transaction-architecture.png)

1. **nonce**

-   A per-account counter that ensures each transaction from a given account can only be processed once and in order.
-   A transaction is valid only if its nonce matches the account’s next expected nonce on chain.

2. **recipient**

-   The 160‑bit address that should receive the transaction. If this field is set to the “zero address” (all zeroes), the transaction is creating a contract instead of sending funds or calling a function.

3. **value**

-   The amount of Ether (in wei) being transferred to the recipient. In contract-creation transactions, this is the initial funding amount for the new contract.

4. **yParity, r, s**

-   These are the **signature** parameters used to verify that the transaction was authorized by the sender’s private key (via ECDSA).
-   `r` and `s` are outputs of the ECDSA signature algorithm.
-   `yParity` (sometimes called “v” in legacy transactions) indicates which of the two possible public key solutions was used in the ECDSA signature (it also encodes chain ID in older legacy transactions).

5. **init or data**

-   Holds code for **contract creation** (the new contract’s initialization/constructor bytecode) or **input data for a message call** (function selector + arguments for an existing contract).

6. **gasLimit**

-   The maximum amount of gas the transaction’s execution can consume. If the execution runs out of gas, it reverts. Any unused gas is refunded to the sender.

7. **type**

-   An indicator introduced by EIP‑2718 for “typed transactions.”
-   Common types:
    -   0 for legacy transactions,
    -   1 (EIP‑2930 “access list”),
    -   2 (EIP‑1559 “dynamic fee” transactions).
    -   3 (EIP‑4844 “blob” transactions).

8. **maxPriorityFeePerGas**

-   The “tip” per gas that goes to the block producer (miner or validator).
-   Under EIP‑1559, the base fee is burned, and only the priority fee (tip) is received by the block producer.

9. **maxFeePerGas**

-   The total maximum fee (base fee + priority fee) the sender is willing to pay per gas.
-   If the actual base fee is lower, the user pays less; but this caps what the user can be charged.

10. **chainId**

-   The unique identifier for the Ethereum network (e.g. 1 for Mainnet, 11155111 for Sepolia).
-   It’s used in the transaction signing process (part of EIP‑155) to prevent replay attacks on different chains.
