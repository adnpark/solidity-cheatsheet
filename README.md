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
