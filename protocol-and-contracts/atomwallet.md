---
description: >-
  Smart contract documentation for the AtomWallet.sol, one of the Intuition
  protocol's core contracts.
---

# AtomWallet

### Overview

The AtomWallet contract is a foundational element of the Intuition protocol, acting as an abstract account (wallet) linked to a specific atom. It leverages account abstraction standard (ERC-4337) to enable advanced transaction execution and management capabilities for atom-associated operations within the blockchain environment.

### Inherits

* `BaseAccount` from `@account-abstraction/contracts` package, which provides the basic framework for the account abstraction functionalities.
* Uses `ECDSA` from OpenZeppelin for cryptographic operations, particularly signature verification.

### Dependencies

* `IEntryPoint` from `@account-abstraction/contracts`: Interface for the EntryPoint contract, which facilitates external interactions with the AtomWallet.
* `UserOperation:` User operation struct, also imported from `@account-abstraction/contracts`. Contains key transaction-related data, including nonce, sender, calldata, gas limit, maxFeePerGas, maxPriorityFeePerGas, etc.
* `Errors`: Custom error handling library created by the Intuition protocol.

### Contract Information

* **Author**: 0xIntuition
* **License**: MIT
* **Solidity Version**: ^0.8.18

### State Variables

**owner**

The owner of the AtomWallet, capable of executing transactions.

```solidity
address public owner;
```

**\_entryPoint**

The EntryPoint contract's address, facilitating external calls and deposits.

```solidity
IEntryPoint private immutable _entryPoint;
```

### Functions

Here's the combined information for each function in the AtomWallet contract, detailing the function interfaces along with their short descriptions, inputs, and outputs.

#### constructor

Initializes a new AtomWallet with a specified EntryPoint and owner.

```solidity
constructor(IEntryPoint anEntryPoint, address anOwner);
```

* **Inputs**:
  * `IEntryPoint anEntryPoint`: The address of the `EntryPoint` contract.
  * `address anOwner`: The address of the wallet owner.
* **Outputs**: None.

#### entryPoint

Returns the EntryPoint associated with this AtomWallet.

```solidity
function entryPoint() public view virtual override returns (IEntryPoint);
```

* **Inputs**: None.
* **Outputs**:
  * `IEntryPoint`: The associated EntryPoint contract address.

#### execute

Executes a single transaction from the AtomWallet. Only the owner or the EntryPoint can call this function.

```solidity
function execute(address dest, uint256 value, bytes calldata func) external;
```

* **Inputs**:
  * `address dest`: The destination address for the transaction.
  * `uint256 value`: The value (in wei) to be sent with the transaction.
  * `bytes calldata func`: The data payload of the transaction.
* **Outputs**: None.

#### executeBatch

Executes a batch of transactions from the AtomWallet. It ensures that the `dest` and `func` arrays have matching lengths and that only the owner or EntryPoint initiates the execution.

```solidity
function executeBatch(address[] calldata dest, bytes[] calldata func) external;
```

* **Inputs**:
  * `address[] calldata dest`: An array of destination addresses for the transactions.
  * `bytes[] calldata func`: An array of data payloads for each transaction, matching the `dest` array in length.
* **Outputs**: None.

#### \_validateSignature (internal)

Validates the signature of a UserOperation. This function overrides the BaseAccount template method `_validateSignature`.

```solidity
function _validateSignature(
    UserOperation calldata userOp, 
    bytes32 userOpHash
) internal virtual override returns (uint256 validationData);
```

* **Inputs**:
  * `UserOperation calldata userOp`: The user operation to validate.
  * `bytes32 userOpHash`: The hash of the user operation.
* **Outputs**:
  * `uint256 validationData`: A numeric value indicating the result of the signature validation. Returns `0` if validation succeeds or `SIG_VALIDATION_FAILED` value (which should be 1) if it fails.

#### \_call (internal)

Performs a low-level call to a target address with specified value and data.

```solidity
function _call(address target, uint256 value, bytes memory data) internal;
```

* **Inputs**:
  * `address target`: The target address for the call.
  * `uint256 value`: The amount of ether (in wei) to send.
  * `bytes memory data`: The data payload of the call.
* **Outputs**: None.

#### getDeposit

Returns the current deposit amount for this account in the EntryPoint.

```solidity
function getDeposit() public view returns (uint256);
```

* **Inputs**: None.
* **Outputs**:
  * `uint256`: The current deposit amount in the EntryPoint for this account.

#### addDeposit

Deposits additional funds to the AtomWallet's account in the EntryPoint.

```solidity
function addDeposit() public payable;
```

* **Inputs**: None. (The deposit amount is specified by `msg.value`)
* **Outputs**: None.

#### Withdraw Deposit

Withdraws a specified amount from the AtomWallet's deposit to a given address. Only the owner can initiate the withdrawal.

```solidity
function withdrawDepositTo(address payable withdrawAddress, uint256 amount) public;
```

* **Inputs**:
  * `address payable withdrawAddress`: The address to send the withdrawn funds to.
  * `uint256 amount`: The amount to withdraw.
* **Outputs**: None.

### Modifier Descriptions

The contract does not explicitly define custom modifiers, but it uses conditions within functions to restrict access to the owner and to validate transaction requests against the EntryPoint or the owner's authority.

### Error Handling

Utilizes the custom `Errors` library for custom error messages, ensuring clarity and specificity in error handling related to the AtomWallet operations.

### Usage Scenarios

The `AtomWallet` contract is designed for:

* Secure execution of transactions linked to an atom within the Intuition protocol.
* Batch processing of multiple transactions for efficiency and convenience.
* Managing deposits for transaction fees in the EntryPoint, allowing for flexible account funding and withdrawal.
