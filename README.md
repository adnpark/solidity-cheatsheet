# Solidity Cheatsheet

Welcome to the Solidity Cheat Sheet—created especially for new Solidity developers! Whether you’re just starting to explore the fundamentals of smart contract programming or need a convenient reference while building your DApps, this guide has you covered.

_This cheatsheet is based on version 0.8.29_

# Table of Contents

- [Solidity Cheatsheet](#solidity-cheatsheet)
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

**Best Practice**

-   Mark state variables as `public` only if you need external read access.
-   Use `private` or `internal` for variables that should not be directly accessible outside the contract.

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

# References

-   [Solidity Docs](https://docs.soliditylang.org/en/latest/)
-   [Solidity by Example](https://solidity-by-example.org/)
-   [Solidity Cheatsheet and Best practices](https://github.com/manojpramesh/solidity-cheatsheet/)
-   [Solidity Gas Optimization Techniques: Loops](https://hackmd.io/@totomanov/gas-optimization-loops#Solidity-Gas-Optimization-Techniques-Loops)
