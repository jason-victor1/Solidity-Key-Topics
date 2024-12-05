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
// SPDX-License-Identifier: MIT
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
// SPDX-License-Identifier: MIT
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

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AllTypesExample {
    // Value Types
    uint public myUint = 123;
    int public myInt = -123;
    bool public myBool = true;
    address public myAddress = 0x1234567890123456789012345678901234567890;
    bytes32 public myBytes = "FixedSizeBytes";
    
    // Reference Types
    string public myString = "Hello, Solidity!";
    uint[] public myArray = [1, 2, 3, 4];
    uint[3] public myFixedArray = [10, 20, 30];

    struct Person {
        string name;
        uint age;
    }

    Person public person = Person("Alice", 30);

    mapping(address => uint) public balances;

}
```

## Functions
Functions are executable units of code. They can:
- Read and modify the contract’s state.
- Accept input parameters and return values.
- Be declared with visibility specifiers like `public`, `private`, `internal`, and `external`.

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ExampleFunctions {
    // State variable to store a number
    uint256 private number;

    // Constructor function to initialize the contract's state
    constructor(uint256 _initialNumber) {
        number = _initialNumber;
    }

    // Public function to set a new value for the number (modifies state)
    function setNumber(uint256 _newNumber) public {
        number = _newNumber;
    }

    // Public function to get the current number (reads state)
    function getNumber() public view returns (uint256) {
        return number;
    }

    // Internal function to double the current number (modifies state)
    function doubleNumber() internal {
        number *= 2;
    }

    // External function to multiply the number by a given multiplier (modifies state)
    function multiplyNumber(uint256 multiplier) external {
        number *= multiplier;
    }

    // Private function to add a specific value to the number (modifies state)
    function addToNumber(uint256 value) private {
        number += value;
    }

    // Public function to test private function (calls private function)
    function testAddToNumber(uint256 value) public {
        addToNumber(value);
    }
}
```


## Visibility
Visibility specifiers determine the accessibility of functions and state variables:
- **public**: Accessible both internally and externally.
- **private**: Accessible only within the contract.
- **internal**: Accessible within the contract and derived contracts.
- **external**: Accessible only externally.

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract VisibilityExample {
    // State variables with different visibility
    string public publicVariable = "Accessible both internally and externally";
    string private privateVariable = "Accessible only within this contract";
    string internal internalVariable = "Accessible within this and derived contracts";

    // Public function
    function getPublicVariable() public view returns (string memory) {
        return publicVariable;
    }

    // Private function
    function getPrivateVariable() private view returns (string memory) {
        return privateVariable;
    }

    // Internal function
    function getInternalVariable() internal view returns (string memory) {
        return internalVariable;
    }

    // External function
    function setPublicVariable(string memory newValue) external {
        publicVariable = newValue;
    }
}

// A derived contract showing internal access
contract DerivedExample is VisibilityExample {
    function getInternalFromDerived() public view returns (string memory) {
        return getInternalVariable(); // Accessing internalVariable from parent contract
    }
}
```

## Modifiers
Modifiers are used to change the behavior of functions. They can be used for:
- Restricting access.
- Validating conditions before executing a function.

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ModifierExample {
    address public owner;
    bool public paused;

    constructor() {
        owner = msg.sender; // Setting the deployer as the owner
        paused = false;
    }

    // Modifier to restrict access to the owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Not the owner");
        _;
    }

    // Modifier to check if the contract is not paused
    modifier whenNotPaused() {
        require(!paused, "Contract is paused");
        _;
    }

    // Function to update owner (only callable by the current owner)
    function changeOwner(address newOwner) public onlyOwner {
        owner = newOwner;
    }

    // Function to toggle pause state (only callable by the owner)
    function togglePause() public onlyOwner {
        paused = !paused;
    }

    // Function to perform an action (only when not paused)
    function performAction() public whenNotPaused {
        // Function logic here
    }
}
```

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

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ConstructorExample {
    string public greeting;
    address public owner;

    // Constructor function to initialize variables
    constructor(string memory _greeting) {
        greeting = _greeting; // Initialize the greeting
        owner = msg.sender;   // Set the deployer as the owner
    }
}
```

## Global Variables
Solidity provides global variables that give information about the blockchain:

- `msg.sender`: Address of the sender.
- `msg.value`: Amount of ether sent with a message.
- `block.timestamp`: Current block timestamp.
- `block.number`: Current block number.

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract GlobalVariablesExample {
    address public lastCaller;
    uint public lastSentValue;
    uint public lastBlockTimestamp;
    uint public lastBlockNumber;

    // Function to demonstrate global variables
    function callMe() public payable {
        lastCaller = msg.sender;         // Address of the caller
        lastSentValue = msg.value;       // Ether sent with the call
        lastBlockTimestamp = block.timestamp; // Timestamp of the current block
        lastBlockNumber = block.number;  // Current block number
    }
}
```

## Operators
Solidity supports various operators:

- **Arithmetic operators**: `+`, `-`, `*`, `/`, `%`
- **Comparison operators**: `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Logical operators**: `&&`, `||`, `!`
- **Bitwise operators**: `&`, `|`, `^`, `<<`, `>>`

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OperatorsExample {
    function arithmeticOperators(uint a, uint b) public pure returns (uint, uint, uint, uint, uint) {
        return (a + b, a - b, a * b, a / b, a % b);
    }

    function comparisonOperators(uint a, uint b) public pure returns (bool, bool, bool, bool, bool, bool) {
        return (a == b, a != b, a < b, a > b, a <= b, a >= b);
    }

    function logicalOperators(bool x, bool y) public pure returns (bool, bool, bool) {
        return (x && y, x || y, !x);
    }

    function bitwiseOperators(uint a, uint b) public pure returns (uint, uint, uint, uint, uint) {
        return (a & b, a | b, a ^ b, a << 1, b >> 1);
    }
}
```

## Conditionals
Solidity uses typical control flow structures like `if`, `else`, and `else if` to make decisions.

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ConditionalsExample {
    function checkNumber(uint x) public pure returns (string memory) {
        if (x < 10) {
            return "Less than 10";
        } else if (x == 10) {
            return "Exactly 10";
        } else {
            return "Greater than 10";
        }
    }
}
```

## Arrays
Arrays are used to store sequential data. They can be:

- **Static**: Fixed size.
- **Dynamic**: Size can change.

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ArraysExample {
    // Static array with fixed size
    uint[5] public staticArray = [1, 2, 3, 4, 5];

    // Dynamic array
    uint[] public dynamicArray;

    // Add an element to the dynamic array
    function addElement(uint element) public {
        dynamicArray.push(element);
    }

    // Get an element from the dynamic array
    function getElement(uint index) public view returns (uint) {
        require(index < dynamicArray.length, "Index out of bounds");
        return dynamicArray[index];
    }

    // Get the length of the dynamic array
    function getLength() public view returns (uint) {
        return dynamicArray.length;
    }
}
```

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

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract EtherExample {
    // Event to log received ether
    event EtherReceived(address sender, uint256 amount);

    // Event to log sent ether
    event EtherSent(address recipient, uint256 amount);

    // Function to receive ether - must be marked as payable
    receive() external payable {
        emit EtherReceived(msg.sender, msg.value);
    }

    // Fallback function to handle calls with no data but ether
    fallback() external payable {
        emit EtherReceived(msg.sender, msg.value);
    }

    // Function to send ether using transfer
    function sendEtherViaTransfer(address payable recipient) external payable {
        require(msg.value > 0, "Must send some ether");
        recipient.transfer(msg.value);
        emit EtherSent(recipient, msg.value);
    }

    // Function to send ether using send
    function sendEtherViaSend(address payable recipient) external payable {
        require(msg.value > 0, "Must send some ether");
        bool success = recipient.send(msg.value);
        require(success, "Send failed");
        emit EtherSent(recipient, msg.value);
    }

    // Function to send ether using call
    function sendEtherViaCall(address payable recipient) external payable {
        require(msg.value > 0, "Must send some ether");
        (bool success, ) = recipient.call{value: msg.value}("");
        require(success, "Call failed");
        emit EtherSent(recipient, msg.value);
    }

    // Function to check the contract's balance
    function getContractBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
```

## Errors
Solidity uses `require`, `assert`, and `revert` to handle errors and exceptions:

- **require**: Checks for conditions and reverts on failure, refunding remaining gas.
- **assert**: Used for internal errors and conditions that should never be false.
- **revert**: Explicitly revert the transaction.

Example:
```solidity
pragma solidity ^0.8.0;

contract ErrorHandlingExample {
    uint public balance;

    function deposit(uint amount) public {
        require(amount > 0, "Deposit amount must be greater than zero");
        balance += amount;
    }

    function withdraw(uint amount) public {
        require(amount <= balance, "Insufficient funds");
        balance -= amount;
        assert(balance >= 0); // Ensures the balance invariant
    }

    function specialFunction(uint value) public pure {
        if (value < 100) {
            revert("Value must be at least 100");
        }
    }
}
```

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

Example:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// Called Contract
contract Callee {
    uint public value;

    function setValue(uint _value) external {
        value = _value;
    }

    function getValue() external view returns (uint) {
        return value;
    }
}

// Caller Contract
contract Caller {
    // Example 1: Instantiating the contract
    function callSetValueDirectly(address calleeAddress, uint _value) public {
        Callee callee = Callee(calleeAddress); // Instantiate the Callee contract
        callee.setValue(_value); // Call setValue on the instantiated contract
    }

    // Example 2: Using low-level calls
    function callSetValueLowLevel(address calleeAddress, uint _value) public {
        bytes memory data = abi.encodeWithSignature("setValue(uint256)", _value); // ABI encoding of function and argument
        (bool success, ) = calleeAddress.call(data); // Low-level call
        require(success, "Low-level call failed");
    }

    // Example 3: Using interfaces
    function callSetValueUsingInterface(ICallee calleeContract, uint _value) public {
        calleeContract.setValue(_value); // Call setValue via the interface
    }
}

// Interface for the Callee Contract
interface ICallee {
    function setValue(uint _value) external;
    function getValue() external view returns (uint);
}
```

## Interfaces
Interfaces define the function signatures that a contract must implement. They do not contain any implementation. Example:
```solidity
interface MyInterface {
    function myFunction() external;
}
```

