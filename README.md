# Solidity Cheatsheet

Welcome to the Solidity Cheat Sheet—created especially for new Solidity developers! Whether you’re just starting to explore the fundamentals of smart contract programming or need a convenient reference while building your DApps, this guide has you covered.

_This cheatsheet is based on version 0.8.28_

### References

-   [Solidity Docs](https://docs.soliditylang.org/en/latest/)
-   [Solidity by Example](https://solidity-by-example.org/)
-   [Solidity Cheatsheet and Best practices](https://github.com/manojpramesh/solidity-cheatsheet/)

## Table of Contents

-   [Solidity Cheatsheet](#solidity-cheatsheet)
    -   [References](#references)
    -   [Table of Contents](#table-of-contents)
    -   [Getting Started](#getting-started)
    -   [Specifying compiler version](#specifying-compiler-version)
    -   [Basic Data Types](#basic-data-types)
        -   [Summary](#summary)
        -   [`bool`](#bool)
        -   [Integers: `uint` and `int`](#integers-uint-and-int)
        -   [`address` and `address payable`](#address-and-address-payable)
        -   [`bytes` and `bytesN`](#bytes-and-bytesn)
        -   [`string`](#string)
    -   [Variables \& Visibility](#variables--visibility)
        -   [State Variables](#state-variables)
        -   [Local Variables](#local-variables)
        -   [Global (Built-in) Variables](#global-built-in-variables)
        -   [Visibility Keywords](#visibility-keywords)
            -   [Visibility Accessible By Common Use Cases](#visibility-accessible-by-common-use-cases)
            -   [`public`](#public)
            -   [`external`](#external)
            -   [`internal`](#internal)
            -   [`private`](#private)
            -   [Best Practices for Visibility](#best-practices-for-visibility)

## Getting Started

```solidity
// SPDX-License-Identifier: MIT
// Filename: HelloWorld.sol
// The function `greet()` will return the message "Hello, Solidity!"
pragma solidity ^0.8.28;

contract HelloWorld {
    string public message = "Hello, Solidity!";

    function greet() external view returns (string memory) {
        return message;
    }
}
```

## Specifying compiler version

```solidity
pragma solidity 0.8.28; // The contract must be compiled with exactly version 0.8.28

pragma solidity ^0.8.28; // Any version greater than or equal to 0.8.28, but strictly less than 0.9.0

pragma solidity >=0.8.0 <0.9.0; // Any version greater than or equal to 0.8.0, but strictly less than 0.9.0
```

**Best Practice**

-   When releasing production contracts, you may want to pin to a narrower range or exact version for complete determinism.
-   During development, using ^0.8.x can be more convenient.

## Basic Data Types

### Summary

| Type                | Description                                                | Example                                         |
| ------------------- | ---------------------------------------------------------- | ----------------------------------------------- |
| **bool**            | Boolean type, true or false                                | `bool isReady = true;`                          |
| **uint**            | Unsigned integer (by default 256 bits, i.e. `uint256`)     | `uint256 count = 10;`                           |
| **int**             | Signed integer (by default 256 bits, i.e. `int256`)        | `int256 temperature = -5;`                      |
| **address**         | 20-byte Ethereum address (e.g. `0xABC123...`), non-payable | `address owner = msg.sender;`                   |
| **address payable** | Same as address, but can receive Ether                     | `address payable wallet = payable(msg.sender);` |
| **bytes**           | Dynamically sized byte array                               | `bytes data = hex"001122";`                     |
| **bytesN**          | Fixed-size byte array of length N (1 ≤ N ≤ 32)             | `bytes32 hash = keccak256(...);`                |
| **string**          | Dynamically sized UTF-8 data                               | `string name = "Alice";`                        |

### `bool`

-   Stores a single bit of information (true or false).
-   Default value is false if not initialized.

### Integers: `uint` and `int`

-   uint stands for unsigned integer and does not allow negative values.
    -   Range for uint256 is 0 to 2^256 - 1.
    -   You can also specify smaller sizes like uint8, uint16, ..., uint256 (increments of 8 bits).
-   int stands for signed integer and allows negative values.
    -   Range for int256 is -(2^255) to (2^255 - 1).
    -   Similarly, you can specify int8, int16, ..., int256.
-   Default value for both uint256 and int256 is 0.

```solidity
// Unsigned integer
uint256 public totalSupply = 10000;
// Signed integer
int256 public temperature = -25;
```

**Best Practice**

-   Use uint256 unless you have a specific reason to use a smaller size (like uint128, etc.).
    -   Smaller types can save storage (gas) if you can tightly pack multiple variables in a struct or the same storage slot.
-   Avoid using signed integers if you only deal with non-negative values.

### `address` and `address payable`

-   An address type holds a 20-byte value.
-   address has built-in attributes like:
    -   balance (returns the account’s balance in wei),
    -   code and codehash (get contract code or its hash),
    -   and methods like call, delegatecall, staticcall (low-level).
-   address payable is a special variant that allows sending Ether via methods like transfer or send.

**Important**

In Solidity ^0.8.0, you must explicitly convert an address to address payable if you want to send Ether:

```solidity
address payable receiver = payable(someAddress);
```

### `bytes` and `bytesN`

bytes: dynamically sized array of bytes.

-   Good for arbitrary-length binary data.
-   More expensive to store than fixed-size arrays due to dynamic nature.

bytesN: fixed-size array of length N (where 1 <= N <= 32).

-   Commonly used for storing hashes (bytes32) or other fixed-length data (e.g., signatures).
-   bytes32 is a popular choice for storing keccak256 hashes.

```solidity
// Dynamically sized
bytes public data = hex"DEADBEEF";

// Fixed-size, exactly 32 bytes
bytes32 public myHash = keccak256(abi.encodePacked("Solidity"));
```

### `string`

-   A dynamically sized UTF-8 encoded data type typically used for text.
-   In practice, string is very similar to bytes (both are dynamically sized), but string is meant for text, while bytes is better for raw binary data.
-   Default value is an empty string "".

```solidity
string public greeting = "Hello, World!";
```

## Variables & Visibility

In Solidity, variables are categorized based on **where** they are declared and **how** they can be accessed:

1. State Variables
2. Local Variables
3. Global (Built-in) Variables
4. Visibility Keywords

### State Variables

-   **Declared inside** a contract but **outside** of any function.
-   **Stored permanently** on the blockchain as part of the contract’s state (in storage).
-   **Gas cost**: Writing and updating state variables costs gas. Reading is cheaper but not free.
-   **Initialization**: If not explicitly initialized, they are given default values (e.g., 0 for integers, false for booleans, address(0) for addresses).

```solidity
pragma solidity ^0.8.28;

contract MyContract {
    // State variables
    uint256 public count;         // defaults to 0
    bool public isActive = true;

    // ...
}
```

**Best Practice**

-   Mark state variables as `public` only if you need external read access.
-   Use `private` or `internal` for variables that should not be directly accessible outside the contract.

### Local Variables

-   **Declared and used within function scope** (including function parameters).
-   **Stored in memory or stack**, not in contract storage (unless explicitly specified otherwise).
-   **Cheaper than state variables** because they’re only used temporarily during function execution.

```solidity
function multiplyByTwo(uint256 _x) public pure returns (uint256) {
    // Local variable
    uint256 result = _x * 2;
    return result; // This value is not saved on-chain
}
```

**Note**

-   Local variables are destroyed after the function call ends.
-   For arrays, structs, or strings passed as function parameters, you often must specify memory or calldata (in external functions) to define the data location.

### Global (Built-in) Variables

These are pre-defined variables and functions that give information about the blockchain, transaction, or message context. Examples include:

-   `msg.sender` — the address that called the function.
-   `msg.value` — how much Ether (in wei) was sent with the call.
-   `msg.data` — the data sent with the call.
-   `block.timestamp` — the current block timestamp (a.k.a. Unix epoch time).
-   `block.number` — the current block number.
-   `block.chainid` — the chain ID of the blockchain.
-   `tx.gasprice` — the gas price of the transaction.

These variables are read from the environment and cannot be directly overwritten. They do not require a declaration like normal variables.

For more global variables, see [here](https://docs.soliditylang.org/en/v0.8.28/units-and-global-variables.html#units-and-globally-available-variables).

Example:

```solidity
function whoCalledMe() public view returns (address) {
    // msg.sender is a global variable
    return msg.sender;
}
```

### Visibility Keywords

In Solidity, visibility determines which parts of the contract or external entities can access a function or state variable.

#### Visibility Accessible By Common Use Cases

| Visibility   | Accessible By                                                                                    | Common Use Cases                                                                      |
| ------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| **public**   | - Externally (via transactions or other contracts) <br/> - Internally within the contract itself | Functions/variables that need to be read or called externally                         |
| **external** | - Externally only (cannot be called internally without `this.`)                                  | Functions intended solely for external interaction (e.g., an API for Dapp users)      |
| **internal** | - Only within this contract or inheriting contracts                                              | Helper functions and state variables used by derived contracts                        |
| **private**  | - Only within this specific contract                                                             | Sensitive logic or state variables that shouldn't be accessed even by child contracts |

**Note**

-   `public` and `private` keywords are used to define the visibility of state variables and functions.
-   `external` and `internal` keywords are used to define the visibility of functions only.

#### `public`

-   A `public` state variable automatically generates a getter function. For example:

```solidity
uint256 public count;
```

This allows reading `count` externally. The contract ABI will have a function `count()` that returns the variable’s value.

```solidity
function getCount() public view returns (uint256) {
    return count;
}
```

-   A `public` function can be called from:
    -   Outside via a transaction or another contract
    -   Inside by other functions within the same contract

#### `external`

-   Functions only callable from outside the contract (or via `this.functionName(...)`).
-   Typically used to indicate a function is part of the contract’s external interface.
-   Slightly more gas-efficient if you don’t plan to call that function from within the contract.

```solidity
function externalFunction() external view returns (uint256) {
    return address(this).balance;
}

// Inside another function in the same contract, you'd have to call: (but not recommended)
this.externalFunction();
```

#### `internal`

-   Accessible **only** within the contract and child contracts that inherit from it.
-   Not part of the public ABI, so cannot be called externally.
-   Useful for shared logic across parent-child relationships.

```solidity
function internalHelper() internal pure returns (uint256) {
    return 42;
}
```

#### `private`

-   **Only** accessible within the same contract.
-   Not accessible in derived contracts.
-   Typically used for sensitive or low-level logic that you don’t want child contracts to override or manipulate directly.

```solidity
bool private privateVariable = true;

function privateHelper() private pure returns (uint256) {
    return 123;
}
```

#### Best Practices for Visibility

1. Explicitly Specify Visibility

    - In Solidity, the default function visibility is internal if not specified.
    - Always define visibility (public, external, internal, private) for every function and state variable to avoid confusion and ensure clarity in your code.

2. Use external for Functions Called Externally Only

    - If a function is never intended to be called internally, mark it external.
    - external functions can be slightly more gas-efficient than public because Solidity handles arguments differently for external calls.

3. Restrict Access Whenever Possible

    - Follow the principle of least privilege.
    - Use private or internal whenever you don’t need external or inherited access.
    - This minimizes the contract’s attack surface and reduces the likelihood of unintended behavior.
