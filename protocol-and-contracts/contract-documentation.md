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

#### tripleAtomShares

```solidity
public tripleAtomShares;
```

### Mappings

```solidity
mapping(uint256 => VaultState) public vaults;
```

```solidity
mapping(uint256 => bytes) public atoms;
```

```solidity
mapping(bytes32 => uint256) public AtomsByHash;
```

```solidity
mapping(uint256 => uint256[3]) public triples;
```

```solidity
mapping(bytes32 => uint256) public TriplesByHash;
```

```solidity
mapping(uint256 => bool) public isTriple;
```

```solidity
mapping(uint256 => mapping(uint256 => mapping(address => uint256)))
```

### Write Functions

#### createAtom

```solidity
function createAtom(
    bytes calldata atomUri
) external payable nonReentrant returns (uint256 id);
```

* **Inputs**:
  * `bytes calldata atomData`: The data of the atom to be created.
* **Outputs**:
  * `uint256 id`: The vault ID of the created atom.
* **Description**:
  * Creates an atom with the given atom data and returns its vault ID. Requires a payment of ETH to cover the atom cost.

#### createTriple

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
* **Description**:
  * Creates a triple from three given atom IDs and returns its vault ID. Requires a payment of ETH to cover the triple cost.

#### depositAtom

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
* **Description**:
  * Deposits ETH into an atom vault, granting ownership of shares to the specified receiver.

#### depositTriple

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
* **Description**:
  * Deposits ETH into a triple vault, granting ownership of shares to the specified receiver.

#### redeemAtom

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
* **Description**:
  * Redeems assets from an atom vault in exchange for a specified number of shares. The assets are sent to the specified receiver.

#### redeemTriple

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
* **Description**:
  * Redeems assets from a triple vault in exchange for a specified number of shares. The assets are sent to the specified receiver.





### Read Functions

