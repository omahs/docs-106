---
description: Protocol documentation for the Intuition EthMultiVault.sol smart contract.
---

# EthMultiVault.sol

Intuition's smart contracts are central to its blockchain architecture, handling critical on-chain activities such as the creation of Atoms (also known as Identities) and Triples (also known as Claims), as well as the staking processes on these elements. These contracts are integral to the economic interactions involved in creating and staking, ensuring a seamless and secure execution of these operations. Beyond these functions, the smart contracts might also govern various other components integral to the Intuition system. These include Atom Claiming/Recovery, management of off-chain Atom Data, and various registries such as Bonding Curve, Primitive, Schema, Algorithm, and Interpretation Registries.

Acting as the Verifiable Data Registry, these smart contracts store crucial information like URIs of Atoms/Triples, the intricate relationships between them, and current stakeholder positions. They essentially form the nodes and edges of the knowledge graph. In contrast, the off-chain Verifiable Data Registry maintains the properties of these nodes and edges, creating a comprehensive and interconnected data structure.

Key components of the on-chain state of the knowledge graph include:

1. Atom wallets and vaults, along with their respective shares and balances.
2. AtomData, typically represented as bytes, usually pointing to a URI for off-chain data.
3. Triples and their constituent Atoms.
4. User positions on Atoms and Triples, reflecting current stakes and indicating who is attesting to what.

The primary contract in this framework, referred to as the MultiVault contract, functions similarly to a blend and advancement of the ERC-4626 and ERC-1155 standards. This contract is a pivotal element of the Intuition protocol, facilitating a wide range of functionalities and providing a robust foundation for the decentralized knowledge graph.

### EthMultiVault.sol

**Inherits:** IEthMultiVault, Initializable, ReentrancyGuardUpgradeable

**Author:** 0xIntuition

This is the core contract for the Intuition protocol.

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
