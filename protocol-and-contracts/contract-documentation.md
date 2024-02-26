---
description: Protocol documentation for the Intuition EthMultiVault.sol smart contract.
---

# EthMultiVault.sol

### EthMultiVault.sol

**Inherits:**&#x20;

* IEthMultiVault,&#x20;
* Initializable, ReentrancyGuardUpgradeable & PausableUpgradeable from OpenZeppelin

**Author:** 0xIntuition

Core contract of the Intuition protocol. Manages the creation and management of vaults associated to Atoms & Triples.

### State Variables

#### count

ID of the last vault to be created

```solidity
  uint256 public count;
```

#### VaultState

```solidity
 struct VaultState {
        uint256 totalAssets;
        uint256 totalShares;
        // address -> balanceOf, amount of shares an account has in a vault
        mapping(address => uint256) balanceOf;
    }
```

#### VaultFees

```solidity
struct VaultFees {
        // entry fee for vault 0 is considered the default entry fee
        uint256 entryFee;
        // exit fee for each vault, exit fee for vault 0 is considered the default exit fee
        uint256 exitFee;
        // protocol fee for each vault, protocol fee for vault 0 is considered the default protocol fee
        uint256 protocolFee;
    }
```

### Mappings

```solidity
mapping(uint256 => VaultState) public vaults;
```

```solidity
mapping(uint256 => VaultFees) public vaultFees;
```

#### Vault ID -> Atom Data

```solidity
mapping(uint256 => bytes) public atoms;
```

#### Hash -> Atom ID

```solidity
mapping(bytes32 => uint256) public AtomsByHash;
```

#### Triple ID -> Vault IDs

```solidity
mapping(uint256 => uint256[3]) public triples;
```

#### Hash -> Triple ID

```solidity
mapping(bytes32 => uint256) public TriplesByHash;
```

#### Vault Id -> (Is Triple)

```solidity
mapping(uint256 => bool) public isTriple;
```

#### Triple ID -> Atom ID -> Account Address -> Atom Balance

```solidity
mapping(uint256 => mapping(uint256 => mapping(address => uint256)))
```

### Write Methods

#### createAtom

Creates an atom with the given atom data and returns its vault ID. Requires a payment of ETH to cover the atom cost.

```solidity
function createAtom(
    bytes calldata atomUri
) external payable nonReentrant returns (uint256 id);
```

* **Inputs**:
  * `bytes calldata atomData`: The data of the atom to be created.
* **Outputs**:
  * `uint256 id`: The vault ID of the created atom.

#### createTriple

Creates a triple from three given atom IDs and returns its vault ID. Requires a payment of ETH to cover the triple cost.

```solidity
function createTriple(
    uint256 subjectId,
    uint256 predicateId,
    uint256 objectId
) external payable returns (uint256 id);
```

* **Inputs**:
  * `uint256 subjectId`: Vault ID of the subject atom.
  * `uint256 predicateId`: Vault ID of the predicate atom.
  * `uint256 objectId`: Vault ID of the object atom.
* **Outputs**:
  * `uint256 id`: The vault ID of the created triple.

#### depositAtom

Deposits ETH into an atom vault, granting ownership of shares to the specified receiver.

```solidity
function depositAtom(
   address receiver,
   uint256 id
) external payable returns (uint256 shares);
```

* **Inputs**:
  * `address receiver`: The address to receive the shares.
  * `uint256 id`: The vault ID of the atom.
* **Outputs**:
  * `uint256 shares`: Amount of shares minted.

#### depositTriple

Deposits ETH into a triple vault, granting ownership of shares to the specified receiver.

```solidity
function depositTriple(
   address receiver,
   uint256 id
) external payable returns (uint256 shares);
```

* **Inputs**:
  * `address receiver`: The address to receive the shares.
  * `uint256 id`: The vault ID of the triple.
* **Outputs**:
  * `uint256 shares`: Amount of shares minted.

#### redeemAtom

Redeems assets from an atom vault in exchange for a specified number of shares. The assets are sent to the specified receiver.

```solidity
function redeemAtom(
   uint256 shares,
   address receiver,
   uint256 id
) external returns (uint256 assets);
```

* **Inputs**:
  * `uint256 shares`: Amount of shares to redeem.
  * `address receiver`:
* **Outputs**:
  * `uint256 assets`: The amount of assets (ETH) withdrawn.

#### redeemTriple

Redeems assets from a triple vault in exchange for a specified number of shares. The assets are sent to the specified receiver.

```solidity
function redeemTriple(
    uint256 shares,
    address receiver,
    uint256 id
) external returns (uint256 assets);
```

* **Inputs**:
  * `uint256 shares`: Amount of shares to redeem.
  * `address receiver`: Address to receive the assets.
  * `uint256 id`: The vault ID of the triple.
* **Outputs**:
  * `uint256 assets`: The amount of assets (ETH) withdrawn.

### Read Methods

#### vaults

A mapping that provides information about all vaults, including total assets and shares.

```solidity
struct VaultState {
    uint256 totalAssets;
    uint256 totalShares;
    mapping(address => uint256) balanceOf;
}

mapping(uint256 => VaultState) public vaults;
```

* **Inputs**:
  * `uint256 id`: Vault ID.
* **Outputs**:
  * Returns a `VaultState` struct for the corresponding vault ID.

#### previewDeposit

Simulates the deposit process, giving an estimate of the shares that would be minted from a specified amount of assets.

```solidity
function previewDeposit(
    uint256 assets,
    uint256 id
) public view returns (uint256 shares);
```

* **Inputs**:
  * `uint256 assets`: Amount of assets to deposit.
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `uint256 shares`: Estimated number of shares to be minted from the deposit.

#### previewRedeem

Simulates the redemption process, estimating the amount of assets that would be returned for a specified number of shares from a given vault.

```solidity
function previewRedeem(
    uint256 shares,
    uint256 id
) public view returns (uint256 assets, uint256 exitFees);
```

* **Inputs**:
  * `uint256 shares`: Amount of shares to redeem.
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `uint256 assets`: Estimated amount of assets to be returned.

#### maxRedeem

Returns the maximum number of shares that can be redeemed from the specified vault by the given owner.

```solidity
function maxRedeem(
    address owner,
    uint256 id
) external view returns (uint256 shares);
```

* **Inputs**:
  * `address owner`: Address of the account to check.
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `uint256 shares`: Maximum amount of shares that can be redeemed by the specified owner.

#### getVaultBalance

Retrieves the balance of a particular user in a specified vault.

```solidity
function getVaultBalance(
        uint256 vaultId,
        address user
) external view returns (uint256);
```

* **Inputs**:
  * `uint256 vaultId`: ID of the vault.
  * `address user`: Address of the user.
* **Outputs**:
  * `uint256`: Balance of the user in the specified vault.

#### currentSharePrice

Calculates and returns the current price per share in the specified vault.

```solidity
function currentSharePrice(
    uint256 id
) external view returns (uint256 price);
```

* **Inputs**:
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `uint256 price`: Current share price in the vault.

#### convertToAssets

Converts a specified number of shares to its equivalent amount in assets for a given vault.

```solidity
function convertToAssets(
    uint256 shares,
    uint256 id
) public view returns (uint256 assets);
```

* **Inputs**:
  * `uint256 shares`: Number of shares.
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `uint256 assets`: Equivalent amount of assets for the given shares.

#### convertToShares

Converts a specified amount of assets to its equivalent number of shares in a given vault.

```solidity
function convertToShares(
    uint256 assets,
    uint256 id
) public view returns (uint256 shares);
```

* **Inputs**:
  * `uint256 assets`: Amount of assets.
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `uint256 shares`: Equivalent amount of shares for the given assets.

#### vaultFees

A mapping that provides information about the fees associated with each vault.

```solidity
struct VaultFees {
    uint256 entryFee;
    uint256 exitFee;
    uint256 protocolFee;
}

mapping(uint256 => VaultFees) public vaultFees;
```

* **Inputs**:
  * `uint256 id`: Vault ID.
* **Outputs**:
  * Returns a `VaultFees` struct for the corresponding vault ID.

#### getVaultStates

Returns the states (including total assets and shares) of all vaults.

```solidity
function getVaultStates() external view returns (Types.VaultState[] memory states);
```

* **Inputs**: None.
* **Outputs**:
  * `Types.VaultState[] memory states`: An array of `VaultState` structures for all vaults.

#### isTriple

Checks if a given vault ID represents a triple.

```solidity
mapping(uint256 => bool) public isTriple;
```

* **Inputs**:
  * `uint256 id`: Vault ID.
* **Outputs**:
  * `bool`: Indicates whether the specified vault ID corresponds to a triple.
