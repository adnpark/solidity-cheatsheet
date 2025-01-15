# Solidity Master Cheatsheet

Welcome to the Solidity Master Cheatsheet—created especially for new Solidity developers! Whether you’re just starting to explore the fundamentals of smart contract programming or need a convenient reference while building your DApps, this guide has you covered.

_This cheatsheet is based on version 0.8.29_

# Table of Contents

- [Solidity Master Cheatsheet](#solidity-master-cheatsheet)
- [Table of Contents](#table-of-contents)
- [Getting Started](#getting-started)
- [Specifying compiler version](#specifying-compiler-version)
- [Basic Data Types](#basic-data-types)
  - [Summary](#summary)
  - [`bool`](#bool)
  - [Integers: `uint` and `int`](#integers-uint-and-int)
  - [`address` and `address payable`](#address-and-address-payable)
  - [`bytes` and `bytesN`](#bytes-and-bytesn)
  - [`string`](#string)
- [Variables \& Visibility](#variables--visibility)
  - [State Variables](#state-variables)
    - [Constants](#constants)
    - [Immutable Variables](#immutable-variables)
  - [Local Variables](#local-variables)
  - [Global (Built-in) Variables](#global-built-in-variables)
  - [Visibility Keywords](#visibility-keywords)
    - [Visibility Accessible By Common Use Cases](#visibility-accessible-by-common-use-cases)
    - [`public`](#public)
    - [`external`](#external)
    - [`internal`](#internal)
    - [`private`](#private)
  - [Best Practices for Visibility](#best-practices-for-visibility)
- [Functions](#functions)
  - [Basic Syntax](#basic-syntax)
  - [Visibility](#visibility)
- [State Mutability: view, pure, and payable](#state-mutability-view-pure-and-payable)
  - [Return Values](#return-values)
  - [Function Parameters and Data Location](#function-parameters-and-data-location)
  - [Overloading and Overriding](#overloading-and-overriding)
  - [Internal vs External Calls](#internal-vs-external-calls)
  - [Gas Considerations](#gas-considerations)
  - [Best Practices for Functions](#best-practices-for-functions)
- [Control Flow](#control-flow)
  - [If / Else Statements](#if--else-statements)
  - [`require`](#require)
  - [For Loops](#for-loops)
  - [While Loops](#while-loops)
  - [Do-While Loops](#do-while-loops)
  - [Break and Continue](#break-and-continue)
  - [Best Practices for Loops](#best-practices-for-loops)
    - [Vanila Loop](#vanila-loop)
    - [Array Loop](#array-loop)
- [Error Handling](#error-handling)
  - [`require` vs `revert` vs `assert`](#require-vs-revert-vs-assert)
    - [`require(condition, "Error Message")`](#requirecondition-error-message)
    - [`revert("Error Message")`](#reverterror-message)
    - [`assert(condition)`](#assertcondition)
    - [Key Points:](#key-points)
  - [Custom Errors (\>=0.8.4)](#custom-errors-084)
    - [Benefits:](#benefits)
  - [`try/catch` for External Calls](#trycatch-for-external-calls)
  - [Best Practices for Error Handling](#best-practices-for-error-handling)
- [Arrays, Mappings, Structs, Enums](#arrays-mappings-structs-enums)
  - [Arrays](#arrays)
    - [Fixed-size Arrays](#fixed-size-arrays)
    - [Dynamic Arrays](#dynamic-arrays)
    - [Declaring and Using Arrays](#declaring-and-using-arrays)
      - [Storage vs. Memory](#storage-vs-memory)
      - [Accessing Elements](#accessing-elements)
      - [Length](#length)
      - [Push and Pop (Dynamic Arrays in Storage)](#push-and-pop-dynamic-arrays-in-storage)
      - [Gas Considerations:](#gas-considerations-1)
  - [Mappings](#mappings)
  - [Structs](#structs)
  - [User Defined Value Types](#user-defined-value-types)
    - [Motivation](#motivation)
    - [Syntax](#syntax)
    - [Wrapping and Unwrapping](#wrapping-and-unwrapping)
    - [Operations and Conversions](#operations-and-conversions)
  - [Enums](#enums)
    - [Enum Advantages:](#enum-advantages)
  - [Best Practices and Tips](#best-practices-and-tips)
- [Modifiers](#modifiers)
  - [Syntax](#syntax-1)
  - [Anatomy of a Modifier](#anatomy-of-a-modifier)
  - [Multiple Modifiers on One Function](#multiple-modifiers-on-one-function)
- [Events](#events)
  - [Key Characteristics](#key-characteristics)
  - [Declaring Events](#declaring-events)
  - [Emitting Events](#emitting-events)
  - [Viewing Events Off-Chain](#viewing-events-off-chain)
  - [Indexed vs. Non-Indexed Parameters](#indexed-vs-non-indexed-parameters)
  - [Common Use Cases](#common-use-cases)
  - [Simple Example](#simple-example)
  - [Best Practices](#best-practices)
- [Contract Inheritance \& Interfaces](#contract-inheritance--interfaces)
  - [Contract Inheritance](#contract-inheritance)
    - [Single Inheritance](#single-inheritance)
    - [Multiple Inheritance](#multiple-inheritance)
    - [Overriding Functions](#overriding-functions)
    - [Constructors in Inheritance](#constructors-in-inheritance)
  - [Interfaces](#interfaces)
    - [Implementing an Interface](#implementing-an-interface)
    - [Using Interfaces](#using-interfaces)
    - [Combining Inheritance and Interfaces](#combining-inheritance-and-interfaces)
    - [Abstract Contracts](#abstract-contracts)
    - [Diamond Inheritance and the “Linearization of Base Contracts”](#diamond-inheritance-and-the-linearization-of-base-contracts)
  - [Best Practices](#best-practices-1)
- [Libraries](#libraries)
  - [Key Characteristics of Libraries](#key-characteristics-of-libraries)
  - [Library Function Types](#library-function-types)
    - [Example: Internal Library](#example-internal-library)
    - [Example: External Library](#example-external-library)
  - [Library for Struct Extensions](#library-for-struct-extensions)
  - [Best Practices](#best-practices-2)
- [Payable, Fallback, and Receive](#payable-fallback-and-receive)
  - [`payable` Keyword](#payable-keyword)
  - [`receive()` Function](#receive-function)
  - [`fallback()` Function](#fallback-function)
  - [Best Practices](#best-practices-3)
- [Data Locations: `storage`, `memory`, `calldata`](#data-locations-storage-memory-calldata)
  - [`storage`: Persistent, On-Chain Data](#storage-persistent-on-chain-data)
  - [`memory`: Temporary, In-Function Workspace](#memory-temporary-in-function-workspace)
  - [`calldata`: Read-Only External Input](#calldata-read-only-external-input)
  - [Best Practices](#best-practices-4)
- [Transient Storage](#transient-storage)
- [Sending Ether](#sending-ether)
  - [Overview](#overview)
    - [transfer](#transfer)
    - [send](#send)
    - [call](#call)
    - [Best Practices](#best-practices-5)
- [Function Selector](#function-selector)
- [Call \& Delegatecall](#call--delegatecall)
- [Create, Create2, Create3, and CreateX](#create-create2-create3-and-createx)
- [ABI Encode \& Decode](#abi-encode--decode)
- [Bitwise Operations](#bitwise-operations)
- [Assembly](#assembly)
- [References](#references)

# Getting Started

```solidity
// SPDX-License-Identifier: MIT
// Filename: HelloWorld.sol
// The function `greet()` will return the message "Hello, Solidity!"
pragma solidity ^0.8.29;

contract HelloWorld {
    string public message = "Hello, Solidity!";

    function greet() external view returns (string memory) {
        return message;
    }
}
```

# Specifying compiler version

```solidity
pragma solidity 0.8.29; // The contract must be compiled with exactly version 0.8.29

pragma solidity ^0.8.29; // Any version greater than or equal to 0.8.29, but strictly less than 0.9.0

pragma solidity >=0.8.0 <0.9.0; // Any version greater than or equal to 0.8.0, but strictly less than 0.9.0
```

**Best Practice**

-   When releasing production contracts, you may want to pin to a narrower range or exact version for complete determinism.
-   During development, using ^0.8.x can be more convenient.

# Basic Data Types

## Summary

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

## `bool`

-   Stores a single bit of information (true or false).
-   Default value is false if not initialized.

## Integers: `uint` and `int`

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

## `address` and `address payable`

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

## `bytes` and `bytesN`

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

## `string`

-   A dynamically sized UTF-8 encoded data type typically used for text.
-   In practice, string is very similar to bytes (both are dynamically sized), but string is meant for text, while bytes is better for raw binary data.
-   Default value is an empty string "".

```solidity
string public greeting = "Hello, World!";
```

# Variables & Visibility

In Solidity, variables are categorized based on **where** they are declared and **how** they can be accessed:

1. State Variables
2. Local Variables
3. Global (Built-in) Variables
4. Visibility Keywords

## State Variables

-   **Declared inside** a contract but **outside** of any function.
-   **Stored permanently** on the blockchain as part of the contract’s state (in storage).
-   **Gas cost**: Writing and updating state variables costs gas. Reading is cheaper but not free.
-   **Initialization**: If not explicitly initialized, they are given default values (e.g., 0 for integers, false for booleans, address(0) for addresses).

```solidity
pragma solidity ^0.8.29;

contract MyContract {
    // State variables
    uint256 public count;         // defaults to 0
    bool public isActive = true;

    // ...
}
```

### Constants

-   Constants are declared using the `constant` keyword.

```solidity
uint256 public constant constantVariable = 10;
```

### Immutable Variables

-   Immutable variables are declared using the `immutable` keyword.
-   They are initialized once and cannot be changed after that.

```solidity
uint256 public immutable immutableVariable = 10;
```

**Best Practice**

-   Mark state variables as `public` only if you need external read access.
-   Use `private` or `internal` for variables that should not be directly accessible outside the contract.
-   Use `immutable` for variables that should not be changed after initialization.

## Local Variables

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

## Global (Built-in) Variables

These are pre-defined variables and functions that give information about the blockchain, transaction, or message context. Examples include:

-   `msg.sender` — the address that called the function.
-   `msg.value` — how much Ether (in wei) was sent with the call.
-   `msg.data` — the data sent with the call.
-   `block.timestamp` — the current block timestamp (a.k.a. Unix epoch time).
-   `block.number` — the current block number.
-   `block.chainid` — the chain ID of the blockchain.
-   `tx.gasprice` — the gas price of the transaction.

These variables are read from the environment and cannot be directly overwritten. They do not require a declaration like normal variables.

For more global variables, see [here](https://docs.soliditylang.org/en/v0.8.29/units-and-global-variables.html#units-and-globally-available-variables).

Example:

```solidity
function whoCalledMe() public view returns (address) {
    // msg.sender is a global variable
    return msg.sender;
}
```

## Visibility Keywords

In Solidity, visibility determines which parts of the contract or external entities can access a function or state variable.

### Visibility Accessible By Common Use Cases

| Visibility   | Accessible By                                                                                    | Common Use Cases                                                                      |
| ------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| **public**   | - Externally (via transactions or other contracts) <br/> - Internally within the contract itself | Functions/variables that need to be read or called externally                         |
| **external** | - Externally only (cannot be called internally without `this.`)                                  | Functions intended solely for external interaction (e.g., an API for Dapp users)      |
| **internal** | - Only within this contract or inheriting contracts                                              | Helper functions and state variables used by derived contracts                        |
| **private**  | - Only within this specific contract                                                             | Sensitive logic or state variables that shouldn't be accessed even by child contracts |

**Note**

-   `public` and `private` keywords are used to define the visibility of state variables and functions.
-   `external` and `internal` keywords are used to define the visibility of functions only.

### `public`

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

### `external`

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

### `internal`

-   Accessible **only** within the contract and child contracts that inherit from it.
-   Not part of the public ABI, so cannot be called externally.
-   Useful for shared logic across parent-child relationships.

```solidity
function internalHelper() internal pure returns (uint256) {
    return 42;
}
```

### `private`

-   **Only** accessible within the same contract.
-   Not accessible in derived contracts.
-   Typically used for sensitive or low-level logic that you don’t want child contracts to override or manipulate directly.

```solidity
bool private privateVariable = true;

function privateHelper() private pure returns (uint256) {
    return 123;
}
```

## Best Practices for Visibility

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

# Functions

Solidity functions define the behavior of your smart contract. They can be used to read or modify the contract’s state, interact with other contracts, or perform computations.

## Basic Syntax

```solidity
function functionName(Type param1, Type param2) [visibility] [stateMutability] returns (ReturnType) {
    // function body
}
```

Where:

-   `functionName` is the identifier (name) of the function.
-   `param1, param2` are parameters with specified types.
-   `visibility` can be `public`, `external`, `internal`, or `private`.
-   `stateMutability` includes `view`, `pure`, `payable`, or can be omitted if the function modifies state.
-   `returns (ReturnType)` specifies the output type(s) (can be multiple).

## Visibility

As covered in [Visibility Keywords](#visibility-keywords), a function’s visibility determines who can call it. The most common visibilities for functions are:

-   `public`: callable from outside and inside the contract
-   `external`: callable from outside only (or via `this.functionName()` inside)
-   `internal`: callable only inside this contract and derived contracts
-   `private`: callable only inside this contract

# State Mutability: view, pure, and payable

1. `view`

-   The function can read state variables but cannot modify them.

```solidity
function getCount() public view returns (uint256) {
    return count;  // reading a state variable
}
```

2. `pure`

-   The function cannot read or modify state variables (nor use `this.balance` or `block.number` etc.).
-   Ideal for pure math or utility functions.

```solidity
function addNumbers(uint256 a, uint256 b) public pure returns (uint256) {
    return a + b;
}
```

3. `payable`

-   The function can accept Ether sent to it.
-   Without `payable`, the function will reject any Ether transfer.

```solidity
function deposit() public payable {}
```

## Return Values

You can return one or more values from a function. There are multiple ways to do so:

1. Return Single Value

```solidity
function getNumber() public pure returns (uint256) {
    return 42;
}
```

2. Return Multiple Values

```solidity
function getValues() public pure returns (uint256, bool) {
    return (100, true);
}
```

3. Named Returns

-   You can name your return variables for clarity.

```solidity
function namedReturn() public pure returns (uint256 count, bool status) {
    count = 10;
    status = true;
}
```

-   This can sometimes make the code more readable but is optional.

## Function Parameters and Data Location

For parameters of reference types (e.g., `string`, `bytes`, `arrays`, `structs`), you must specify the data location (memory, storage, or calldata):

-   `memory`: non-persistent, used for local copies within a function.
-   `calldata`: non-persistent, immutable/read-only, only used in **external** function parameters. It’s more gas-efficient than memory for external calls because it doesn’t copy the data.
-   `storage`: persists in the contract’s state storage (rarely used as a parameter location, mostly used for state variables). Can be used for internal or private function if you want to pass a reference to an existing storage variable.

Example using calldata:

```solidity
function concatStrings(
    string calldata str1,
    string calldata str2
) external pure returns (string memory) {
    return string(abi.encodePacked(str1, str2));
}
```

## Overloading and Overriding

-   **Overloading**: Defining multiple functions with the same name but different parameters.

```solidity
function setValue(uint256 _value) public {
    // ...
}

function setValue(uint256 _value, bool _flag) public {
    // ...
}
```

-   **Overriding**: When a function in a child contract overrides a function from a parent contract.
    -   The parent function must be marked as `virtual`.
    -   The child function must use `override`.

```solidity
contract Parent {
    function greet() public virtual pure returns (string memory) {
        return "Hello from Parent";
    }
}

contract Child is Parent {
    function greet() public pure override returns (string memory) {
        return "Hello from Child";
    }
}
```

## Internal vs External Calls

-   When you call a function internally in Solidity, it uses jump instructions without creating a new message call. This is more gas-efficient.
-   When you call an external function from within the same contract (e.g., this.myExternalFunction()), it triggers a new contract call. This is less gas-efficient and changes msg.sender to the contract itself.

## Gas Considerations

1. Function Complexity:

    - Avoid excessive loops or large data copy operations within a single function.
    - If possible, break down large operations into smaller functions or use off-chain solutions for heavy computations.

2. Function Parameters:

    - For external functions, using calldata for parameters instead of memory is cheaper in many cases.
    - Passing large arrays around increases gas due to data copy overhead.

## Best Practices for Functions

-   Use public or external only if the function needs to be called from outside. Otherwise, consider internal or private.
-   Separate reading (view/pure) and state-changing functions for clarity and potential performance benefits.
-   Overloading can be handy, but use it sparingly; it can cause confusion if the parameter differences are subtle.
-   Overriding is essential for inheritance hierarchies. Make sure to mark parent functions as virtual and child overrides as override.

# Control Flow

Control flow in Solidity is largely similar to other languages like JavaScript, C, or Python. You can use if/else, for, while, and do-while loops to direct program execution.

## If / Else Statements

Syntax:

```solidity
function checkValue(uint256 _x) public pure returns (string memory) {
    if (_x > 100) {
        return "Greater than 100";
    } else if (_x == 100) {
        return "Exactly 100";
    } else {
        return "Less than 100";
    }
}
```

## `require`

`require` statements revert the transaction immediately if a condition is not met:

```solidity
require(condition, "Error message");
```

This is often used for input validation or access control checks.

## For Loops

```solidity
function sumArray(uint256[] memory _arr) public pure returns (uint256) {
    uint256 total = 0;
    for (uint256 i = 0; i < _arr.length; i++) {
        total += _arr[i];
    }
    return total;
}
```

## While Loops

```solidity
function decrement(uint256 _x) public pure returns (uint256) {
    while (_x > 0) {
        _x--;
    }
    return _x; // returns 0
}
```

## Do-While Loops

Solidity also supports do-while loops, which execute the loop body at least once before checking the condition:

```solidity
function doWhileExample(uint256 _x) public pure returns (uint256) {
    uint256 counter = _x;

    do {
        counter--;
    } while (counter > 0);

    return counter;
}
```

## Break and Continue

Like many languages, Solidity provides `break` and `continue` statements for early termination or skipping an iteration in loops.

-   `break`: Exits the loop immediately.
-   `continue`: Skips the remaining statements in the current iteration and moves to the next iteration.

```solidity
function loopWithBreak(uint256 _x) public pure returns (uint256) {
    for (uint256 i = 0; i < _x; i++) {
        if (i == 5) break; // exits loop when i equals 5
        if (i == 3) continue; // skips remaining statements when i equals 3
        // ...
    }
}
```

## Best Practices for Loops

### Vanila Loop

**Don't use >= and <= in the condition**

```solidity
// 37975 gas (+347)
function loop_lte() public returns (uint256 sum) {
    for(uint256 n = 0; n <= 99; n++) {
        sum += n;
    }
}
```

-   The EVM has the opcodes LT, GT and EQ for comparison, but there are no convenient LTE or GTE opcodes for the operation we are doing.
-   Therefore each time the condition n <= 99 is checked, 3 instructions must be executed: LT n 99,EQ n 99 and OR to check if either one of those returned true, which leads to extra gas cost.

**Increment the variable in an unchecked block**

```solidity
// 32343 gas (-5285)
function loop_unchecked_plusplus() public returns (uint256 sum) {
    for(uint256 n = 0; n < 100;) {
        sum += n;
        unchecked {
            n++;
        }
    }
}
```

-   Since version 0.8 Solidity implements safety checks for all integer arithmetic, including overflow and underflow guards
-   `n++` Solidity will insert extra code to handle the case if `n` would overflow after incrementing it
-   We can skip the overflow check by using `unchecked` block
-   Use only if you are sure the variables will never overflow

**Just write it in assembly**

```solidity
// 26450 gas (-11178)
function loop_assembly() public returns (uint256) {
    assembly {
        let sum := 0
        for {let n := 0} lt(n, 100) {n := add(n, 1)} {
            sum := add(sum, n)
        }
        mstore(0, sum)
        return(0, 32)
    }
}
```

-   Removed the declaration uint256 sum from the function header in order to escape Solidity's type system as much as possible
-   Using assembly is the most gas-efficient way to write a loop, but it's also the most complex and harder to read and maintain. So make sure to use it only when necessary.

### Array Loop

**"Cache" the array's length for the loop condition**

```solidity
// 25182 gas (-230)
function loopArray_cached(uint256[] calldata ns) public returns (uint256 sum) {
    uint256 length = ns.length;
    for(uint256 i = 0; i < length;) {
        sum += ns[i];
        unchecked {
            i++;
        }
    }
}
```

-   We know the length won't change during execution and we can reduce the number of ns.length calls to just 1 for a modest reduction in gas.

# Error Handling

## `require` vs `revert` vs `assert`

### `require(condition, "Error Message")`

-   Verifies **input conditions** or **other external conditions**.
-   Commonly used at the start of a function to validate function inputs, access control, or other preconditions (e.g., ensuring msg.value is sufficient).

```solidity
require(msg.sender == owner, "Not the owner");
require(amount > 0, "Amount must be greater than 0");
```

-   If the condition fails, it reverts with the provided error message.
-

### `revert("Error Message")`

-   **Explicitly** trigger a revert from any part of the code.
-   Often used to handle more complex logic or custom flows.

```solidity
if (balance < amount) {
    revert("Insufficient balance to withdraw");
}
```

### `assert(condition)`

-   Used to check for internal errors and invariants that should never fail.
-   If an assert fails, it indicates a bug in your code (e.g., an invariant has been violated).
-   From Solidity 0.8.x onwards, failing assert also reverts the transaction but uses a different kind of error—often highlighting a critical code issue.

```solidity
assert(totalSupply == sumOfAllBalances);
```

### Key Points:

-   `require` is for **invalid user inputs** or **external conditions**.
-   `revert` is a **manual** revert trigger.
-   `assert` is for **internal invariants**—if it fails, something is fundamentally wrong.

## Custom Errors (>=0.8.4)

-   Custom errors allow you to define error types with optional parameters.
-   They are more **gas-efficient** than revert strings because the encoded error data is smaller.

```solidity
error Unauthorized(address caller);
error InvalidAmount(uint256 requested, uint256 available);

function restrictedAction() public view {
    if (msg.sender != owner) {
        revert Unauthorized(msg.sender);
    }
    // ...
}

function withdraw(uint256 amount) public {
    if (amount > balances[msg.sender]) {
        revert InvalidAmount(amount, balances[msg.sender]);
    }
    // ...
}
```

### Benefits:

-   Saves gas compared to long string messages.
-   Provides typed parameters, making debugging easier off-chain.
-   Especially useful in large or complex contracts.

## `try/catch` for External Calls

-   When calling **external** functions or doing contract creations, you can use `try/catch` to handle failures without reverting your entire function:

```solidity
interface DataFeed { function getData(address token) external returns (uint value); }

contract FeedConsumer {
    DataFeed feed;
    uint errorCount;
    function rate(address token) public returns (uint value, bool success) {
        // Permanently disable the mechanism if there are
        // more than 10 errors.
        require(errorCount < 10);
        try feed.getData(token) returns (uint v) {
            return (v, true);
        } catch Error(string memory /*reason*/) {
            // This is executed in case
            // revert was called inside getData
            // and a reason string was provided.
            errorCount++;
            return (0, false);
        } catch Panic(uint /*errorCode*/) {
            // This is executed in case of a panic,
            // i.e. a serious error like division by zero
            // or overflow. The error code can be used
            // to determine the kind of error.
            errorCount++;
            return (0, false);
        } catch (bytes memory /*lowLevelData*/) {
            // This is executed in case revert() was used.
            errorCount++;
            return (0, false);
        }
    }
}
```

-   You can choose to handle or re-revert. This can be used to do partial rollbacks or alternate logic.

## Best Practices for Error Handling

-   **Use require for Input Validation**
    -   Validate function arguments, check `msg.value`, verify permissions, etc.
-   **Keep Revert Messages Short**
    -   Revert strings add to the bytecode size and runtime cost.
    -   If you need to pass detailed data, consider custom errors.
-   **Use custom errors for More Complex Logic**
    -   Save gas and convey structured data (like addresses, amounts) in the revert reason.
-   **Fail Fast**
    -   Place `require` checks at the top of your functions to prevent unnecessary computations.

# Arrays, Mappings, Structs, Enums

-   Arrays: Sequential lists of elements of the same type.
-   Mappings: Key-value data structure that does not store keys (only values).
-   Structs: Custom types that group different data fields together.
-   Enums: Defines a finite set of possible states.

## Arrays

### Fixed-size Arrays

-   Their size must be known at compile time.
-   You cannot change their length afterward.

```solidity
uint256[3] public fixedArray = [1, 2, 3];
```

### Dynamic Arrays

-   Size can be modified at runtime using built-in methods like push and pop.
-   Declared with empty brackets: `uint256[]`.

```solidity
uint256[] public dynamicArray;
```

### Declaring and Using Arrays

#### Storage vs. Memory

-   State arrays (declared at the contract level) live in **storage** (on-chain).
-   Function parameters and local arrays can be declared in **memory** or **calldata** (for external functions).
-   `calldata` is read-only and more gas-efficient for external function parameters.

#### Accessing Elements

```solidity
uint256[] public numbers;

function addNumber(uint256 num) external {
    numbers.push(num); // dynamic array
}

function getNumber(uint256 index) external view returns (uint256) {
    return numbers[index];
}
```

#### Length

-   For a dynamic array, `arrayName.length` gives the current length.
-   You can also set a new length (in storage arrays) by assigning: `numbers.length = newLength;` (this can truncate or extend the array).

#### Push and Pop (Dynamic Arrays in Storage)

-   `push(value)` appends a new element at the end.
-   `pop()` removes the last element (reducing the array length by one).
-   Time complexity of `push` and `pop` is O(1).

```solidity
function removeLast() external {
    numbers.pop();
}
```

#### Gas Considerations:

-   Each push, pop, and indexing operation in storage arrays incurs gas costs.
-   Minimizing on-chain array size or using more efficient data structures can be crucial for large data sets.

## Mappings

-   A mapping is a reference type that stores key-value pairs.
-   Mappings in Solidity:
    -   **Do not allow iteration** over keys.
    -   **Do not store** or keep track of the keys themselves—only the values and their hashed keys.
    -   Are often used for **fast lookups** (constant time access).

```solidity
mapping(address => uint256) public balances;

function deposit() external payable {
    balances[msg.sender] += msg.value;
}
```

-   **KeyType**: Usually a built-in type like `address`, `uint256`, or `bytes32`. (Structs, mappings, or dynamically-sized arrays cannot be used as keys.)
-   **ValueType**: Can be any type, including another mapping or a struct.

```solidity
// Nested mapping
mapping(address => mapping(address => uint256)) public allowance;

function approve(address spender, uint256 amount) external {
    allowance[msg.sender][spender] = amount;
}
```

## Structs

-   **Structs** let you define custom data types that group multiple variables (of possibly different types).
-   They help in making code more readable and organizing complex data.

```solidity
struct User {
    string name;
    uint256 age;
    address wallet;
}

User[] public users; // dynamic array of User structs

function createUser(string memory _name, uint256 _age) external {
    // Option 1: Direct struct creation
    User memory newUser = User(_name, _age, msg.sender);
    users.push(newUser);

    // Option 2: Struct with named fields
    users.push(User({name: _name, age: _age, wallet: msg.sender}));
}
```

## User Defined Value Types

-   User-Defined Value Types allow you to create a custom type name that wraps an existing built-in value type (like `uint256`, `int128`, `bytes32`, etc.).
-   This feature was introduced in Solidity 0.8.8 and provides a way to give **semantic meaning** to a primitive type, helping catch logic errors and improving code readability.

### Motivation

-   **Type Safety & Readability**: By assigning a descriptive name to a built-in type, you can differentiate between, say, a `UserId` and a `Balance`, even if they’re both `uint256` under the hood.
-   **Compile-Time Checks**: Conversions between different user-defined value types (and between the user-defined type and its underlying type) require **explicit** wrapping/unwrapping. This helps avoid mixing up incompatible values in your code.

### Syntax

```solidity
type TypeName is UnderlyingType;
```

-   `TypeName`: The name of your new type (PascalCase by convention).
-   `UnderlyingType`: Any built-in value type (e.g., `uint256`, `int128`, `bool`, `bytes32`, etc.).

```solidity
type UserId is uint256;
type Price is uint128;
type Flag is bool;
```

### Wrapping and Unwrapping

-   Wrapping: Converting an underlying type to a user-defined type.
-   Unwrapping: Converting a user-defined type back to its underlying type.

```solidity
uint256 rawId = 123;
// Wrap a uint256 into a UserId
UserId userId = UserId.wrap(rawId);

// Unwrap a UserId to a uint256
uint256 unwrappedId = UserId.unwrap(userId);

// ❌ This will fail: Type uint256 is not implicitly convertible to UserId
UserId invalidConversion = 123;
```

### Operations and Conversions

By default, **no** arithmetic or comparison operations are defined for user-defined value types. If you need to perform operations on your custom type, you can:

1. **Unwrap** your custom type, perform the operations on the underlying type, then **re-wrap** the result.
2. Define **inline** or **library** functions that handle the logic for your custom type.

```solidity
pragma solidity ^0.8.29;

type Distance is uint256;

library DistanceLib {
    function add(Distance a, Distance b) internal pure returns (Distance) {
        // unwrap, add, then re-wrap
        return Distance.wrap(Distance.unwrap(a) + Distance.unwrap(b));
    }

    function greaterThan(Distance a, Distance b) internal pure returns (bool) {
        return Distance.unwrap(a) > Distance.unwrap(b);
    }
}

contract Road {
    using DistanceLib for Distance;

    // store total length of roads
    Distance public totalDistance;

    function addDistance(Distance d) external {
        totalDistance = totalDistance.add(d);
        // internally uses DistanceLib.add
    }

    function compareDistances(Distance a, Distance b) external pure returns (bool) {
        return a.greaterThan(b);
    }
}

```

## Enums

-   Enums define a finite list of constant values, improving code clarity when dealing with limited states.
-   Internally, enums map to uint8 by default (or the smallest fitting unsigned integer, though conceptually it can occupy a full storage slot). Each value corresponds to an index, starting at `0`.

```solidity
enum Status {
    Pending,    // 0
    Shipped,    // 1
    Delivered,  // 2
    Canceled    // 3
}

Status public currentStatus;

function setStatusShipped() external {
    currentStatus = Status.Shipped;
}

function cancelOrder() external {
    currentStatus = Status.Canceled;
}

function getStatus() external view returns (Status) {
    return currentStatus;
}
```

-   **Converting Enums**:
    -   You can cast an integer to an enum if it’s a valid index.
    -   Example: `Status s = Status(1); // s == Status.Shipped`
    -   Be careful with casting, as invalid casts can lead to out-of-range enum values, which can corrupt state.

### Enum Advantages:

-   Makes state transitions self-documenting.
-   Avoids using magic numbers (`0, 1, 2, 3`) or strings for statuses.
-   Typically used for order states, workflow stages, or roles with limited states.

## Best Practices and Tips

1. **Use Arrays for Ordered Collections**

-   Great for sequential data like lists of user IDs.
-   Remember that iterating over large arrays on-chain is costly.

2. **Use Mappings for Fast Lookups**

-   Ideal for key-value pairs like balances or allowances.
-   You cannot iterate over a mapping, so maintain auxiliary data if you need a list of keys.

3. **Structs for Grouped Data**

-   Keep related data fields together.
-   This improves readability and can reduce storage slot usage if packed correctly (e.g., storing small-size types in one slot).

4. **Guard Against Invalid Enum Values**

-   If you do type-casting from integers to enums, ensure you handle out-of-range values.
-   In Solidity 0.8.x, you get a revert on overflow, but be explicit about checking valid indices if you do custom casting.

5. **Avoid Large Arrays in Storage**

-   If you expect an unbounded number of elements, consider a more efficient approach like indexing or partial off-chain storage.
-   Or provide a mechanism to paginate through array elements or handle them in batches.

6. **Be Aware of Storage Collisions**

-   When dealing with nested data structures or inherited contracts, be sure you understand how Solidity’s storage layout works.
-   Typically, using the standard approach (top-level declarations) avoids collisions.

# Modifiers

-   Modifiers are a powerful feature in Solidity that allow you to augment or condition the execution of functions.
-   Think of them as wrappers around your functions that can run additional code (e.g., access checks, state validations) before (and/or after) the function body is executed.

## Syntax

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Caller is not owner");
    _;
}

function sensitiveAction() public onlyOwner {
    // This code runs after the `onlyOwner` checks
    // ...
}
```

1. When `sensitiveAction()` is called, Solidity first executes the code in the `onlyOwner` modifier.
2. If all checks (e.g., `require`) pass, it proceeds to execute the body of `sensitiveAction()`.
3. If any check fails, it reverts and never calls the function body.

## Anatomy of a Modifier

-   A modifier can contain code before and after the special `_` (underscore):

```solidity
modifier checkValue(uint256 _value) {
    // Code executed before the function body
    require(_value > 0, "Value must be greater than zero");

    _; // The function body is inserted here

    // Code executed after the function body
    emit ValueChecked(_value);
}

function doSomething(uint256 amount) public checkValue(amount) {
    // function body
}
```

the compiled code effectively looks like:

```solidity
function doSomething(uint256 amount) public {
    require(amount > 0, "Value must be greater than zero");

    // function body (original code of doSomething)

    emit ValueChecked(amount);
}
```

## Multiple Modifiers on One Function

-   You can apply multiple modifiers to a single function. The modifiers will execute in order, left to right:

```solidity
modifier onlyOwner() { /* ... */ _; }
modifier whenNotPaused() { /* ... */ _; }

function specialAction() public onlyOwner whenNotPaused {
    // function body
}
```

-   Be mindful of the order, especially if the modifiers change state.
-   Also ensure your modifiers don’t conflict or replicate the same checks unnecessarily.

# Events

-   Events in Solidity are mechanisms that allow smart contracts to emit logs during execution.
-   These logs are stored on the blockchain as part of the transaction receipt, but do not directly consume contract storage.
-   Off-chain applications (like DApps, block explorers, etc.) can listen for these events to react or update their interfaces.

## Key Characteristics

-   **Logs vs. Storage**: Events produce logs that are cheaper than storing data in contract state, because they’re kept in the transaction receipt.
-   **Indexed Parameters**: You can mark up to **3 parameters** as indexed, enabling more efficient filtering on the client side.
-   **Not Accessible On-Chain**: Event data (logs) are **not** accessible to other contracts. You can’t read events from within Solidity; they’re purely for off-chain usage.

## Declaring Events

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
```

-   Up to three parameters can be labeled as `indexed`.
-   `indexed` parameters are **indexed** in the event log, which makes them searchable by off-chain tools.

## Emitting Events

```solidity
function transfer(address _to, uint256 _amount) external {
    // ... perform transfer logic ...
    emit Transfer(msg.sender, _to, _amount);
}
```

## Viewing Events Off-Chain

-   Web3 Libraries (Viem, ethers.js) allow you to listen for these events or query past events by searching transaction logs.

Example(in Viem):

```typescript
import { parseAbiItem } from "viem"
import { publicClient } from "./client"

publicClient.watchEvent({
    address: "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
    event: parseAbiItem(
        "event Transfer(address indexed from, address indexed to, uint256 value)"
    ),
    args: {
        from: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
        to: "0xa5cc3c03994db5b0d9a5eedd10cabab0813678ac"
    },
    onLogs: (logs) => console.log(logs)
})
```

## Indexed vs. Non-Indexed Parameters

-   Indexed parameters are **searchable** in the logs and stored as topics.
-   Non-indexed parameters are stored in the log’s data section, which is not easily searchable but can be read once you retrieve the log.

For example, in `event Transfer(address indexed from, address indexed to, uint256 value)`;:

-   `from` => topic[1]
-   `to` => topic[2]
-   `value` => part of the data payload

-   You can filter logs by indexed topics like `from = 0x1234...abcd`, but you cannot directly filter by `value` because it’s not indexed.

```typescript
const filter = await publicClient.createContractEventFilter({
    abi: wagmiAbi,
    address: "0xfba3912ca04dd458c843e2ee08967fc04f3579c2",
    eventName: "Transfer",
    args: {
        from: "0xd8da6bf26964af9d7eed9e03e53415d37aa96045",
        to: "0xa5cc3c03994db5b0d9a5eedd10cabab0813678ac"
    }
})
```

## Common Use Cases

1. Token Transfers
    - ERC20 tokens emit a `Transfer` event whenever tokens move from one address to another.
2. State Changes
    - Logging changes of ownership, updates to config variables, or changes in contract status.
3. Debugging
    - Emitting certain events during testing can help you trace contract execution.
4. Off-Chain Processing
    - Subgraphs (e.g., The Graph) or other indexing services use events to build databases of contract activity, enabling complex queries and analytics.

## Simple Example

```solidity
pragma solidity ^0.8.21;

contract Payment {
    event Deposit(address indexed sender, uint256 amount);
    event Withdraw(address indexed recipient, uint256 amount);

    function deposit() external payable {
        require(msg.value > 0, "No ether sent");
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) external {
        require(amount > 0, "Zero withdrawal");
        require(address(this).balance >= amount, "Insufficient contract balance");

        (bool success, ) = payable(msg.sender).call{value: amount}("");
        require(success, "Withdraw failed");

        emit Withdraw(msg.sender, amount);
    }
}
```

## Best Practices

-   Emit Events for Important State Changes

    -   Especially for actions that off-chain services or UIs need to track: token transfers, ownership changes, auctions ending, etc.

-   Index Only What’s Needed

    -   Up to three indexed parameters can be used. Indexing more variables doesn’t exist (the rest must be unindexed).
    -   Too many indexed parameters or large data can increase log size and gas cost.

-   Avoid Emitting Too Many Events in a Single Transaction

    -   Each event adds to the gas cost. If you’re in a loop, consider alternative approaches or batch events if possible.

-   Be Aware of Event Ordering

    -   The order in which you emit events is the order they appear in the logs. Off-chain listeners often rely on log order to interpret data.

# Contract Inheritance & Interfaces

-   Solidity supports an **object-oriented programming** model where **contracts** can **inherit** from one or more base contracts.
-   This allows for code reuse and hierarchical organization of functionality.
-   **Interfaces** in Solidity define the required function signatures (and optionally events) without providing an implementation, allowing for standardized interaction between different contracts.

## Contract Inheritance

Inheritance is achieved using the `is` keyword. A derived (child) contract can access and override the functions and state variables of its parent (base) contract(s).

### Single Inheritance

```solidity
contract Parent {
    string public parentName = "Parent";

    function greet() public pure returns (string memory) {
        return "Hello from Parent";
    }
}

contract Child is Parent {
    function sayHello() public view returns (string memory) {
        // Access parent's state variable
        return parentName;
    }
}
```

-   `Child` inherits everything from `Parent` except **constructor**, **private** state variables/functions, and **internal** data can only be accessed within `Child`.

### Multiple Inheritance

A contract can inherit from multiple base contracts using comma separation:

```solidity
contract A {
    function foo() public pure returns (string memory) {
        return "A";
    }
}

contract B {
    function bar() public pure returns (string memory) {
        return "B";
    }
}

// Child inherits both A and B
contract C is A, B {
    // C now has both foo() from A and bar() from B
}
```

### Overriding Functions

-   Parent functions must be marked `virtual` to allow them to be overridden.
-   Child overrides must use `override`.

```solidity
contract Parent {
    function greet() public pure virtual returns (string memory) {
        return "Hello from Parent";
    }
}

contract Child is Parent {
    // Overriding greet()
    function greet() public pure override returns (string memory) {
        return "Hello from Child";
    }
}
```

-   If you have multiple levels (e.g., `Grandparent -> Parent -> Child`), and `Child` wants to override a function from `Grandparent`, you typically chain `override(Base, Grandparent)` if needed.

```solidity
contract Child is Parent, Grandparent {
    function greet() public pure override(Parent, Grandparent) returns (string memory) {
        return "Hello from Child";
    }
}
```

### Constructors in Inheritance

-   If a parent contract has a **constructor**, the child must **explicitly** call it (unless it has a parameterless constructor).
-   If multiple parents have constructors, each must be called with the needed arguments in the child’s constructor.

```solidity
contract Parent1 {
    uint256 public x;

    constructor(uint256 _x) {
        x = _x;
    }
}

contract Parent2 {
    uint256 public y;

    constructor(uint256 _y) {
        y = _y;
    }
}

contract Child is Parent1, Parent2 {
    // Must call Parent1,2 constructor
    constructor(uint256 _childValue) Parent1(_childValue) Parent2(_childValue) {
        // additional child init
    }
}
```

## Interfaces

An **interface** in Solidity is like a contract but with these restrictions:

1. **No** state variables or constructor definitions.
2. **All functions must be `external`** (or public in older versions of Solidity, but typically we use `external`).
3. **No** function implementations—**only signatures**.
4. Usually includes events that implementing contracts should emit.

Purpose: They define a **standard** for other contracts to implement, ensuring compatibility without forcing an implementation approach.

```solidity
interface ICounter {
    function increment() external;
    function getCount() external view returns (uint256);
    event Counted(address indexed caller, uint256 newCount);
}
```

### Implementing an Interface

-   A contract **implements** an interface by providing **all** the functions declared in the interface and matching their signatures exactly.

```solidity
contract Counter is ICounter {
    uint256 private count;

    function increment() external override {
        count++;
        emit Counted(msg.sender, count);
    }

    function getCount() external view override returns (uint256) {
        return count;
    }
}
```

### Using Interfaces

Contracts can call interface functions to interact with external contracts that implement them:

```solidity
contract Caller {
    function doIncrement(ICounter _counter) external {
        _counter.increment();
    }

    function readCount(ICounter _counter) external view returns (uint256) {
        return _counter.getCount();
    }
}
```

### Combining Inheritance and Interfaces

You can mix inheritance and interfaces. For example, you could have a base abstract contract that implements some parts of an interface and leaves others for child contracts.

```solidity
interface IVault {
    function deposit(uint256 amount) external;
    function withdraw(uint256 amount) external;
}

abstract contract VaultBase is IVault {
    // partially implement deposit logic
    // but keep withdraw abstract or add some logic

    function deposit(uint256 amount) external virtual override {
        // partial logic
    }
}

contract MyVault is VaultBase {
    // Must override deposit if not fully implemented
    // Must implement withdraw
    function deposit(uint256 amount) external override {
        // full deposit logic
    }

    function withdraw(uint256 amount) external override {
        // implement withdraw logic
    }
}
```

-   `VaultBase` is an abstract contract because it might not fully implement all methods of `IVault`.
-   `MyVault` completes the implementation.

### Abstract Contracts

**Abstract contracts** are those that cannot be deployed directly because they have at least one function without an implementation (`virtual` function). They serve as base contracts.

-   Typically contain shared logic and placeholders for functions to be defined in child contracts.
-   Marked with `abstract` keyword in Solidity 0.6.x+.

```solidity
abstract contract Animal {
    function speak() public virtual returns (string memory);
}

contract Dog is Animal {
    function speak() public pure override returns (string memory) {
        return "Woof!";
    }
}
```

-   `Animal` is abstract because `speak()` has no implementation.
-   `Dog` provides an implementation for `speak()`.

### Diamond Inheritance and the “Linearization of Base Contracts”

-   Solidity uses the **C3 linearization** algorithm to resolve conflicts in multiple inheritance.
-   If multiple parent contracts have the **same function** name, you must explicitly **override** and specify which base contract’s implementation to use.

```solidity
contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

contract B is A {
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}

// D inherits from both B and C
contract D is B, C {
    // Must override foo() again
    function foo() public pure override(B, C) returns (string memory) {
        // decide which parent's implementation to call, or write new logic
        // it will return "C"
        return super.foo(); // picks the rightmost parent's override by default (C) in linearization
    }
}
```

-   The `override(B, C)` explicitly states you are overriding `foo()` from both `B` and `C`.
-   `super.foo()` calls the next-most derived override in the linearization.

## Best Practices

-   **Use Inheritance for Shared Logic**

    -   Common functionality (e.g., access control, utility functions) can be placed in base contracts for easy reuse.

-   **Favor Composition over Deep Inheritance**

    -   Avoid extremely deep inheritance chains—they can become confusing and error-prone.
    -   Sometimes, libraries or composition patterns are clearer.

-   **Mark Functions as `virtual` and `override`**

    -   Always use `virtual` in the base contract to signal it can be overridden.
    -   Always use `override` in child contracts to signal you’re overriding a parent function.

-   **Use Interfaces for Inter-Contract Communication**

    -   Define an interface that external contracts must implement.
    -   This reduces dependencies, as your contract does not rely on the internal details of the other contract.

-   **Separate Interfaces into Their Own Files**

    -   This aids clarity and reusability.
    -   Keep your interface definitions minimal—only the required function signatures and events.

-   **Avoid Conflicting State in Multiple Inheritance**

    -   Overlapping storage layouts from multiple parents can cause confusion or even storage collisions.
    -   Ensure each parent contract uses distinct state variable slots (this usually happens naturally if you’re just inheriting standard contracts without complicated overrides).

# Libraries

-   In Solidity, **libraries** are special types of contracts intended to provide reusable code.
-   They can be thought of as utility or helper modules that other contracts can call without needing to implement the same logic repeatedly.
-   Libraries help keep code DRY (Don’t Repeat Yourself), save gas, and improve readability.

## Key Characteristics of Libraries

-   **No Ether**

    -   Libraries **cannot** hold Ether (i.e., they cannot have a balance).
    -   Any attempt to send Ether to a library will fail.

-   **No State Variables**

    -   Libraries cannot store or modify their own state; they are stateless.

-   **No Inheritance**

    -   Libraries cannot be inherited or extend other contracts (nor can you inherit from a library).

-   **Linked at Compile Time or Deployment Time**
    -   **Internal library functions** are typically inlined or statically called.
    -   **External library functions** can be deployed and then “linked” to your contract.

## Library Function Types

Solidity libraries support **two** ways of using their functions:

-   **Internal Functions**

    -   If a library function is declared `internal`, it is inlined into the calling contract’s bytecode at compile time (similar to macros).
    -   This reduces overhead since no external call is made.

-   **External Functions**

    -   If a library function is declared `public` or `external`, the calling contract will **delegatecall** into the library at runtime.
    -   This can save bytecode size in the calling contract but introduces a small overhead for each external call.
    -   You must **deploy** the library contract first and link it before deploying the final contract.

### Example: Internal Library

Most libraries are used as **internal** because it’s more gas-efficient (no external call) and simpler to manage.

```solidity
// MathLib.sol
pragma solidity ^0.8.29;

library MathLib {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function multiply(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }
}

// TestMath.sol
pragma solidity ^0.8.21;

import "./MathLib.sol";

contract TestMath {
    function testAdd(uint256 x, uint256 y) public pure returns (uint256) {
        return MathLib.add(x, y);
    }

    function testMultiply(uint256 x, uint256 y) public pure returns (uint256) {
        return MathLib.multiply(x, y);
    }
}
```

-   `MathLib.add` and `MathLib.multiply` are called from `TestMath` without an external call because they’re `internal` functions in a library.
-   The compiler will inline or statically link these calls, increasing `TestMath`’s bytecode size, but saving on runtime gas costs.

### Example: External Library

You can define a library with public or external functions. In that case, your contract calls the library via delegatecall at runtime.

```solidity
// ExternalLib.sol
pragma solidity ^0.8.29;

library ExternalLib {
    function externalAdd(uint256 a, uint256 b) external pure returns (uint256) {
        return a + b;
    }
}
```

To use this library in another contract:

```solidity
pragma solidity ^0.8.29;

import "./ExternalLib.sol";

contract UseExternalLib {
    // The compiler inserts a reference that must be linked to the ExternalLib deployed address
    function compute(uint256 x, uint256 y) public pure returns (uint256) {
        return ExternalLib.externalAdd(x, y);
    }
}
```

-   **Deployment**: You must first deploy `ExternalLib` and then link its address into `UseExternalLib`’s bytecode.
-   At runtime, calls to `ExternalLib.externalAdd` happen via DELEGATECALL.

## Library for Struct Extensions

-   A popular pattern is to write libraries that extend built-in types or custom structs with `using for`.
-   This adds **library functions** as if they were member functions of the type.

```solidity
pragma solidity ^0.8.29;

library ArrayUtils {
    function findIndex(uint256[] storage arr, uint256 value) internal view returns (int256) {
        for (uint256 i = 0; i < arr.length; i++) {
            if (arr[i] == value) {
                return int256(i);
            }
        }
        return -1; // Not found
    }
}

contract MyArray {
    using ArrayUtils for uint256[];  // "using for" directive

    uint256[] private data;

    function addValue(uint256 value) external {
        data.push(value);
    }

    function findValue(uint256 value) external view returns (int256) {
        // We can now call findIndex() as if it's a member of data
        return data.findIndex(value);
    }
}
```

-   The `using ArrayUtils for uint256[];` directive allows you to call `data.findIndex(value)` directly.
-   This improves readability and encapsulates utility logic for arrays.

## Best Practices

-   **Keep Libraries Focused**

    -   A library should do **one thing** well—e.g., math operations, array helpers, address utilities, etc.

-   **Avoid Large External Libraries for Single Contracts**

    -   If you’re only using a library for a single contract and that library has few functions, it might be simpler and cheaper to make them internal.

-   **Mark Functions `pure` or `view` Where Possible**

    -   Avoid unintentional state modifications in libraries unless that’s the library’s explicit purpose.
    -   This also ensures your library is safer and can be reasoned about more easily.

-   **`using for` Pattern**

    -   Great for readability and a more object-oriented feel.
    -   Clarifies which types are extended by the library.

-   **Security**

    -   Libraries can mutate the caller’s storage if called with `delegatecall`. Ensure your library code is trusted and thoroughly reviewed.

-   **Solady, OpenZeppelin and Other Standard Libraries**

    -   Before writing your own, check if established libraries (e.g., Solady, OpenZeppelin) cover your needs. This reduces risk and leverages well-audited code.

# Payable, Fallback, and Receive

-   Smart contracts in Solidity can receive Ether if they have specific functions set up.
-   Before Solidity `0.6.x`, there was only a single fallback function.
-   From `0.6.x` onward, Solidity introduced a split: a dedicated `receive()` function to handle plain Ether transfers, and a `fallback()` function to handle non-matching function calls (and optionally Ether if `receive()` is not found).

## `payable` Keyword

1. `payable` is required for functions to receive Ether.
2. It can apply to constructors and functions.
3. If a function or constructor is not marked `payable`, it will reject any incoming Ether with a revert.

```solidity
pragma solidity ^0.8.21;

contract PayableExample {
    // A payable constructor allows you to deploy the contract with initial Ether
    constructor() payable {
        // Contract can receive Ether on deployment
    }

    // A payable function can receive Ether
    function deposit() external payable {
        // Funds are added to contract balance
    }

    // This function is NOT payable, so it cannot receive Ether
    function nonPayableFunction() external {
        // ...
    }
}
```

-   Attempting to send Ether (e.g., via `msg.value`) to `nonPayableFunction()` will revert.
-   Conversely, calling `deposit()` without sending Ether is valid (but `msg.value` would be `0`).

## `receive()` Function

-   **Introduced** in Solidity `0.6.x` as a **special** function to receive plain Ether with no data (i.e., calls with an empty calldata).
-   It can be declared `external payable` with **no** arguments and **no** return value.

```solidity
receive() external payable {
    // custom logic or leave it empty
}
```

-   If someone sends Ether to your contract without calling a specific function (like a simple `transfer()` or `send()` from an external account), the `receive()` function is triggered if it is defined.
-   If you want your contract to accept plain Ether transfers, define a `receive()` function (or a fallback that is payable).

**Behavior:**

-   If a contract **does not** define `receive() external payable` but does define a `fallback() external payable`, then sending Ether with no data triggers the `fallback()` function.
-   If neither function is payable, the contract cannot receive Ether outside of a payable function call and will revert.

## `fallback()` Function

-   The `fallback()` function is a special function in Solidity, called when:
    -   A function that doesn’t exist in the contract is called.
    -   No function signature matches the calldata.
    -   If the call has data but there is no matching function.

```solidity
fallback() external [payable] {
    // custom logic
}
```

-   `fallback()` is **not** required to be `payable` by default. If you want it to receive Ether when no function matches, mark it `payable`. Otherwise, calls sending Ether will revert.

**Use Cases:**

-   **Proxy/Forwarding**: This is common in proxy contracts where all unknown calls are forwarded to another contract.
-   **Emergency catch-all**: If a user or contract calls a function that does not exist, you can handle or revert gracefully.
-   **Receiving Ether + Data**: If you need to handle Ether transfers that include data, you can do so in `fallback()` if `receive()` is not suitable or is absent.

## Best Practices

-   **Decide if you want to accept Ether**:
    -   If yes, define a `receive()` function (or a `payable fallback()`).
    -   If no, either omit them or revert in `fallback()`.
-   **Keep Fallback/Receive Minimal**:
    -   They can be triggered frequently, and often with limited gas (2,300).
    -   Complex logic can lead to out-of-gas errors.
    -   For reentrancy safety, minimize state changes or use reentrancy guards.
-   **Test for Edge Cases**:
    -   Test calls with valid function signatures, invalid ones, random data, zero-length data, and with Ether attached.
    -   Make sure you get the intended behavior (especially for proxies or other fallback patterns).
-   **Event Logging**:
    -   If your contract receives Ether, consider emitting an event to easily track inbound transfers.

# Data Locations: `storage`, `memory`, `calldata`

-   In Solidity, **complex data types**—like arrays, structs, and mappings—require specifying a **data location**.
-   This tells the compiler where the data physically resides. The three main data locations are:

1. `storage`: Persistent on-chain storage (the contract’s state).
2. `memory`: Temporary, in-memory area used within function execution. Not persisted on-chain.
3. `calldata`: Read-only area for function arguments in external functions. It directly references call data without copying it into memory.

## `storage`: Persistent, On-Chain Data

-   **Location**: The contract’s long-term memory on the Ethereum blockchain.
-   **Persistence**: Any changes to data in storage are permanent and cost **significant gas**.
-   **State Variables**: By default, state variables declared at the contract level (e.g., `uint256 public count;`) reside in `storage`.

```solidity
contract MyContract {
    // This array is in storage (part of contract state).
    uint256[] public numbers;

    function storeValue(uint256 value) external {
        numbers.push(value); // Modifying storage costs gas
    }
}
```

**Key Points:**

-   **Expensive to Modify**: Writing to `storage` is the costliest operation because you’re permanently updating the blockchain state.
-   **Reference Types in Storage**: Arrays, structs, and mappings can be stored in `storage`. Modifying their contents is also costly.
-   **Assignments in Storage**: When you do `storageRef = someStateVar;`, you create a reference pointing to the same data. Changes to one reflect in the other.

## `memory`: Temporary, In-Function Workspace

-   **Location**: A transient area used for the duration of a function call.
-   **Lifecycle**: Data in `memory` is wiped after the function executes.
-   **Cost**: Reading/writing `memory` is cheaper than `storage` (though not free), but the data does not persist.

```solidity
function incrementValuesMemory(uint256[] memory arr)
    public
    pure
    returns (uint256[] memory)
{
    // We can mutate the array elements because 'arr' is a mutable copy in memory.
    for (uint256 i = 0; i < arr.length; ++i) {
        arr[i] += 10;
    }
    // Returning the changed array
    return arr;
}
```

## `calldata`: Read-Only External Input

-   **Location**: A special data location for external function parameters.
-   **Read-Only**: You cannot modify data in calldata; it’s immutable.
-   **Gas Optimization**: Using calldata for external function parameters (instead of memory) can save gas, because Solidity can read directly from the transaction call data instead of making a full copy in memory.

```solidity
function sumArray(uint256[] calldata arr) external pure returns (uint256) {
    uint256 sum = 0;
    for (uint256 i = 0; i < arr.length; i++) {
        sum += arr[i];
    }
    return sum;
}
```

-   Any attempt to modify arr (e.g., `arr[i] = ...`) will fail to compile.

## Best Practices

-   **Use `calldata` for External Read-Only Params**:
    -   If you only need to read from parameters, mark them as `calldata` to save gas (no copying into memory).
-   **Avoid Large Copies**:
    -   Copying large arrays from `storage` to `memory` can be expensive. Consider streaming or chunking if you need partial data.
-   **Use `storage` Wisely**:
    -   Minimize writes to `storage`. If possible, only store essential data. Reading from and especially writing to `storage` is the biggest cost in Solidity.

# Transient Storage

-   Transient storage is a new feature introduced to Ethereum through EIP-1153.
-   It provides a special type of storage for Ethereum smart contracts that:

    -   Exists only **during a single transaction**.
    -   Automatically resets at the end of the transaction.
    -   Does not incur persistent storage costs (unlike regular storage, which is expensive because it remains on-chain indefinitely).

-   Transient storage is implemented through two new opcodes:

    -   `TSTORE`: Temporarily stores a value in transient storage.
    -   `TLOAD`: Retrieves a value from transient storage.

```solidity
contract ReentrancyGuard {
    bytes32 constant SLOT = 0;

    modifier nonreentrant() {
        assembly {
            if tload(SLOT) { revert(0, 0) }
            tstore(SLOT, 1)
        }
        _;
        assembly {
            tstore(SLOT, 0)
        }
    }
}
```

-   One practical example of transient storage is using it for a reentrancy guard.
-   Normally, you’d store a boolean flag in contract storage to indicate that a function is currently being executed.
-   With transient storage, you can use the `TSTORE` and `TLOAD` opcodes instead, which let you store and read this flag during the transaction without writing to storage.
-   This not only makes the guard cheaper to implement, but also avoids cluttering your contract’s persistent state.

**Differences from Existing Data Locations**
| Data Location | Persistence | Cost | Typical Usage |
| --------------------- | ---------------------------- | ------------------ | ------------------------------------------------------------ |
| **storage** | Persists across transactions | High | State variables, permanent contract data |
| **memory** | Temporary (function scope) | Medium | Local variables, function operations |
| **calldata** | Read-only, external inputs | Low | External function parameters |
| **Transient Storage** | Only within one transaction | Lower than storage | Ephemeral data used across calls within the same transaction |

# Sending Ether

## Overview

| **Method**   | **Gas Forwarded**                        | **On Failure**             | **Return Type**                | **Recommended Usage**             |
| ------------ | ---------------------------------------- | -------------------------- | ------------------------------ | --------------------------------- |
| **transfer** | 2,300 gas (fixed)                        | Auto-reverts               | No return (reverts)            | Not recommended                   |
| **send**     | 2,300 gas (fixed)                        | Does not revert            | bool (success/fail)            | Not recommended                   |
| **call**     | All remaining gas (or a specified value) | Does not revert by default | (bool, bytes) (success + data) | Recommended with reentrancy guard |

### transfer

-   Forwards a **fixed 2,300 gas** to the recipient’s fallback/receive function.
-   If the call fails (e.g., the fallback function uses more than 2,300 gas or reverts), `transfer` will automatically revert the entire transaction.
-   If future Ethereum upgrades raise the fallback gas cost or the logic changes, `transfer` might break.

```solidity
function transfer(address payable _to, uint256 _amount) external {
    _to.transfer(_amount);
    // If it fails, entire function reverts automatically
}
```

### send

-   Also forwards 2,300 gas to the recipient’s fallback function.
-   Does not revert on failure by default. Instead, it returns false if the call fails.
-   Generally considered obsolete in favor of `transfer`, or `call`.

```solidity
function send(address payable _to, uint256 _amount) external {
    bool success = _to.send(_amount);
    require(success, "Send failed");
}
```

### call

```solidity
(bool success, bytes memory data) = recipient.call{value: amount, gas: gasAmount}("");

```

-   Forwards all remaining gas by default, or a specified amount if you use `gas: gasAmount`.
-   Does not automatically revert; you must handle the success boolean.
-   `call` is more future-proof and flexible. Recommended for most use cases with reentrancy guards or checks-effects-interactions pattern.

Simple Ether send:

```solidity
function flexibleCall(address payable _to, uint256 _amount) external {
    (bool success, ) = _to.call{value: _amount}("");
    require(success, "Call failed");
}
```

Calling a function with data:

```solidity
function callFunction(address _contract, bytes memory _data, uint256 _amount) external {
    (bool success, bytes memory returnedData) = _contract.call{value: _amount}(_data);
    require(success, "Low-level call failed");
    // returnedData may contain a return value
}
```

### Best Practices

-   `transfer` and `send` are no longer recommended in most modern Solidity patterns because of their 2,300 gas limit and potential for breakage if fallback logic changes.
-   `call` with proper error handling (checking the return boolean) and reentrancy protections is the current best practice for sending Ether.

# Function Selector

# Call & Delegatecall

# Create, Create2, Create3, and CreateX

# ABI Encode & Decode

# Bitwise Operations

# Assembly

# References

-   [Solidity Docs](https://docs.soliditylang.org/en/latest/)
-   [Solidity by Example](https://solidity-by-example.org/)
-   [Solidity Cheatsheet and Best practices](https://github.com/manojpramesh/solidity-cheatsheet/)
-   [Solidity Gas Optimization Techniques: Loops](https://hackmd.io/@totomanov/gas-optimization-loops#Solidity-Gas-Optimization-Techniques-Loops)
-   [Gas Optimization In Solidity: Strategies For Cost-Effective Smart Contracts](https://hacken.io/discover/solidity-gas-optimization/)
