---
description: >-
  Documentation for the IEthMultiVault.sol Interface in our core
  EthMultiVault.sol smart contract.
---

# IEthMultiVault.sol Interface

**Author:** 0xIntuition

### Functions

#### getTripleAtoms

Return the underlying atom vault ids given a Triple vault ID.

```solidity
function getTripleAtoms(
        uint256 id
) external view returns (uint256, uint256, uint256);
```

* **Inputs**:
  * `uint256 id`: Vault ID
* **Outputs:**
  * `uint256, uint256, uint256`: Underlying Atom vaults for the Triple.

#### isTriple

Mapping to designate if a vault ID is a Triple.

```solidity
function isTriple(uint256 id) external view returns (bool);
```

* **Inputs**:
  * `uint256 id`: Vault ID
* **Outputs:**
  * `bool`: True/false for whether vault ID is a Triple.

#### tripleAtomShares

Return Triple atom shares given Triple ID, Atom ID, and account address.

```solidity
function tripleAtomShares(
        uint256 id,
        uint256 atomId,
        address account
) external view returns (uint256);
```

* **Inputs:**
  * `uint256 id`: Vault ID
  * `uint256 atomId`: Atom ID of the Atom
  * `address account`: Account address of the account.
* **Outputs:**
  * `uint256`: Triple Atom shares

#### assertTriple

Returns `true` for Triple vaults and `false` for Atom vaults. Designates if provided vault ID is a Triple.

```solidity
function assertTriple(uint256 id) external view returns (bool);
```

* **Inputs:**
  * `uint256 id`: ID of the vault to check
* **Output:**
  * `bool`: `true` if provided vault ID is a Triple; `false` otherwise

#### getCounterIdFromTriple

Returns the Triple ID for the given counter-Triple ID.

```solidity
function getCounterIdFromTriple(uint256 id) external returns (uint256);
```

* **Inputs**:
  * `uint256 id`: Counter Triple ID
* **Outputs**:
  * `uint256`: Triple ID

#### tripleHashFromAtoms

Returns the corresponding hash for the given RDF triple, given the Atoms that make up the Triple.

```solidity
function tripleHashFromAtoms(
        bytes memory subject,
        bytes memory predicate,
        bytes memory object
) external pure returns (bytes32);
```

* **Inputs:**
  * `bytes memory subject`:&#x20;
  * `bytes memory predicate`:
  * `bytes memory object`:
* **Outputs:**
  * `bytes32`: Corresponding hash for the given RDF Triple (subject, predicate, object)

#### convertToShares

Returns the amount of shares that would be exchanged with the vault for the amount of assets provided.

```solidity
function convertToShares(
        uint256 assets,
        uint256 id
) external view returns (uint256 shares);
```

* **Inputs**:
  * `uint256 assets`: Amount of assets
  * `uint256 id`: Vault ID
* **Outputs**:
  * `uint256 shares`: Amount of shares for the amount of assets provided.

#### convertToAssets

Returns the amount of assets that would be exchanged with the vault for the amount of shares provided.

```solidity
function convertToAssets(
        uint256 shares,
        uint256 id
) external view returns (uint256 assets);
```

* **Inputs:**
  * `uint256 shares`: Amount of shares
  * `uint256 id`: Vault ID
* **Outputs**:
  * `uint256 assets`: Amount of assets for the amount of shares provided.

#### previewDeposit

Simulates the effects of depositing assets at the current block.

```solidity
function previewDeposit(
        uint256 assets,
        uint256 id
) external view returns (uint256 shares);
```

* **Inputs:**
  * `uint256 assets`: The amount of assets to deposit.
  * `uint256 id`: Vault ID
* **Outputs:**
  * `uint256 shares`: The amount of shares from depositing the amount of assets

#### previewRedeem

Simulates the effects of redeeming shares at the current block.

```solidity
function previewRedeem(
        uint256 shares,
        uint256 id
) external view returns (uint256 assets, uint256 exitFees);
```

* **Inputs:**
  * `uint256 shares`: The amount of shares to redeem.
  * `uint256 id`: Vault ID
* **Outputs:**
  * `uint256 assets`: The amount of assets from redeeming the amount of shares
  * `uint256 exitFees`: Any fees associated with redeeming the amount of shares.

#### maxRedeem

Max amount of shares that can be redeemed from the _owner_ balance through a redeem call.

```solidity
function maxRedeem(
        address owner,
        uint256 id
) external view returns (uint256 shares);
```

* **Inputs:**&#x20;
  * `address owner`: The _owner's_ address
  * `uint256 id`: Vault ID
* **Outputs:**
  * `uint256 shares`: The _max amount_ of shares that can be redeemed.

