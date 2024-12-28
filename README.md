# Solidity Cheatsheet

Welcome to the Solidity Cheat Sheet—created especially for new Solidity developers! Whether you’re just starting to explore the fundamentals of smart contract programming or need a convenient reference while building your DApps, this guide has you covered.

_This cheatsheet is based on version 0.8.28_

### References

-   [Solidity Docs](https://docs.soliditylang.org/en/latest/)
-   [Solidity by Example](https://solidity-by-example.org/)
-   [Solidity Cheatsheet and Best practices](https://github.com/manojpramesh/solidity-cheatsheet/)

## Table of Contents

-   [Getting Started](#getting-started)
-   [Specifying compiler version](#specifying-compiler-version)
-   [Basic Data Types](#basic-data-types)
-   [Variables & Visibility](#variables-and-visibility)
-   [Functions](#functions)
-   [Control Flow](#control-flow)
-   [Global Variables & Block Data](#global-variables-and-block-data)
-   [Error Handling](#error-handling)
-   [Arrays, Mappings, Structs, Enums](#arrays-mappings-structs-enums)
-   [Modifiers](#modifiers)
-   [Events](#events)
-   [Contract Inheritance & Interfaces](#contract-inheritance-and-interfaces)
-   [Libraries](#libraries)
-   [Abstract Contracts](#abstract-contracts)
-   [Payable, Fallback, and Receive](#payable-fallback-and-receive)
-   [Gas & Optimization Tips](#gas-and-optimization-tips)
-   [Useful Patterns](#useful-patterns)
-   [Further Reading](#further-reading)

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

-   Best Practice: When releasing production contracts, you may want to pin to a narrower range or exact version for complete determinism. During development, using ^0.8.x can be more convenient.Best Practice: When releasing production contracts, you may want to pin to a narrower range or exact version for complete determinism. During development, using ^0.8.x can be more convenient.

## Basic Data Types

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

contract DataTypes {
    bool public isReady;
    uint256 public count;
    int256 public temperature;
    address public owner;
    bytes32 public dataHash;
    string public welcome;

    constructor() {
        isReady = true;
        count = 100;
        temperature = -10;
        owner = msg.sender;
        dataHash = keccak256(abi.encodePacked("test"));
        welcome = "Hello, Solidity!";
    }
}
```
