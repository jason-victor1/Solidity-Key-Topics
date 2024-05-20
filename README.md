# Solidity Programming Overview

This is an overview of key topics in Solidity programming. These notes have sections covering fundamental concepts to help me understand and implement Solidity smart contracts.

## Table of Contents
1. [Contract Structure](#contract-structure)
2. [Variables](#variables)
3. [Types](#types)
4. [Functions](#functions)
5. [Visibility](#visibility)
6. [Modifiers](#modifiers)
7. [Custom Modifiers](#custom-modifiers)
8. [Constructors](#constructors)
9. [Global Variables](#global-variables)
10. [Operators](#operators)
11. [Conditionals](#conditionals)
12. [Arrays](#arrays)
13. [Mappings](#mappings)
14. [Structs](#structs)
15. [Events](#events)
16. [Ether](#ether)
17. [Errors](#errors)
18. [Inheritance](#inheritance)
19. [Calling Other Contracts](#calling-other-contracts)
20. [Interfaces](#interfaces)

## Contract Structure
Solidity contracts are similar to classes in object-oriented programming. A contract can contain state variables, functions, function modifiers, events, and struct types.

Example:
```solidity
pragma solidity ^0.8.0;

contract ContractStructureSolid {
    // State variables
    uint256 public data;

    // Function
    function setData(uint256 _data) public {
        data = _data;
    }

    // Function modifier
    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    // Event
    event DataChanged(uint256 newData);

    // Struct type
    struct MyStruct {
        uint256 value;
        string name;
    }
}
```

## Variables
Variables in Solidity store data and come in several types:
- **State variables**: Stored permanently on the blockchain.
- **Local variables**: Exist only within function execution.
- **Global variables**: Provide information about the blockchain and transaction context.

Example:
```solidity
pragma solidity ^0.8.0;

contract VariablesSolid {
    // State variable
    uint256 public stateVariable = 100;

    // Global variables
    address public owner = msg.sender;
    uint256 public currentBlockNumber = block.number;

    // Function with local variables
    function calculate(uint256 a, uint256 b) public pure returns (uint256) {
        // Local variable
        uint256 result = a + b;
        return result;
    }
}
```

## Types
Solidity supports various data types:
- **Value types**: Includes `uint`, `int`, `bool`, `address`, and fixed-size `bytes`.
- **Reference types**: Includes `string`, `array`, `struct`, and `mapping`.

## Functions
Functions are executable units of code. They can:
- Read and modify the contract’s state.
- Accept input parameters and return values.
- Be declared with visibility specifiers like `public`, `private`, `internal`, and `external`.

## Visibility
Visibility specifiers determine the accessibility of functions and state variables:
- **public**: Accessible both internally and externally.
- **private**: Accessible only within the contract.
- **internal**: Accessible within the contract and derived contracts.
- **external**: Accessible only externally.

## Modifiers
Modifiers are used to change the behavior of functions. They can be used for:
- Restricting access.
- Validating conditions before executing a function.

## Custom Modifiers
Custom modifiers allow you to define reusable code that can be inserted into functions. Example:
```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Not owner");
    _;
}
```

## Constructors
Constructors are special functions that are executed once when the contract is deployed. They are used to initialize state variables.

## Global Variables
Solidity provides global variables that give information about the blockchain:

- `msg.sender`: Address of the sender.
- `msg.value`: Amount of ether sent with a message.
- `block.timestamp`: Current block timestamp.
- `block.number`: Current block number.

## Operators
Solidity supports various operators:

- **Arithmetic operators**: `+`, `-`, `*`, `/`, `%`
- **Comparison operators**: `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Logical operators**: `&&`, `||`, `!`
- **Bitwise operators**: `&`, `|`, `^`, `<<`, `>>`

## Conditionals
Solidity uses typical control flow structures like `if`, `else`, and `else if` to make decisions.

## Arrays
Arrays are used to store sequential data. They can be:

- **Static**: Fixed size.
- **Dynamic**: Size can change.

## Mappings
Mappings are key-value stores for storing associative arrays. Example:
```solidity
mapping(address => uint) public balances;
```

## Structs
Structs allow you to create custom data types by grouping variables. Example:
```solidity
struct Person {
    string name;
    uint age;
}
```
## Events
Events are used to log information on the blockchain. They can be listened to by off-chain applications. Example:
```solidity
event Transfer(address indexed from, address indexed to, uint256 value);
```

## Ether
Ether is the native cryptocurrency of Ethereum. Solidity allows contracts to handle ether:

- **Sending ether**: Using functions like `transfer`, `send`, or `call`.
- **Receiving ether**: By marking functions as `payable`.

## Errors
Solidity uses `require`, `assert`, and `revert` to handle errors and exceptions:

- **require**: Checks for conditions and reverts on failure, refunding remaining gas.
- **assert**: Used for internal errors and conditions that should never be false.
- **revert**: Explicitly revert the transaction.

## Inheritance
Solidity supports inheritance, allowing contracts to inherit functions and state variables from parent contracts. Example:
```solidity
contract A {
    function foo() public {}
}

contract B is A {
    function bar() public {}
}
```

## Calling Other Contracts
Contracts can interact with other contracts by:

- Instantiating the contract.
- Using low-level calls.
- Using interfaces.

## Interfaces
Interfaces define the function signatures that a contract must implement. They do not contain any implementation. Example:
```solidity
interface MyInterface {
    function myFunction() external;
}
```

