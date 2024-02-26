---
description: >-
  Documentation for the IEthMultiVault.sol Interface events in our core
  EthMultiVault.sol smart contract.
---

# IEthMultiVault.sol Events

### Interface Information

**Author:** 0xIntuition

Events described in the interface of the EthMultiVault contract, used for managing many ERC4626 style vaults in a single contract.

### Events

#### Deposit

Emitted upon the minting of shares in the vault by depositing assets.

```solidity
event Deposit(
        address indexed sender,
        address indexed receiver,
        uint256 vaultBalance,
        uint256 assets,
        uint256 shares,
        uint256 id
);
```

**Logged Parameters:**

* **`sender`**: Address of the initiator of the deposit.
* **`receiver`**: Address of the beneficiary receiving the minted shares.
* **`vaultBalance`**: The total assets held in the vault after the deposit.
* **`assets`**: The total amount of assets transferred in the deposit.
* **`shares`**: The total shares transferred as a result of the deposit.
* **`id`**: Vault ID associated with the deposit event.

#### Withdraw

Emitted upon the withdrawal of assets from a vault through redeeming shares.

```solidity
event Withdraw(
    address indexed sender,
    address indexed owner,
    uint256 vaultBalance,
    uint256 assets,
    uint256 shares,
    uint256 exitFee,
    uint256 id
);
```

**Logged Parameters:**

* `sender`: Address of the initiator of the withdrawal.
* `receiver`: Address of the beneficiary receiving the minted shares.
* `vaultBalance`: The total assets remaining in the vault post-withdrawal.
* `assets`: The amount of assets that were transferred out of the vault.
* `shares`: The total number of shares that were redeemed in this transaction.
* `id`: The unique identifier of the vault from which assets are withdrawn.

#### AtomCreated

Emitted upon creation of an Atom.

```solidity
event AtomCreated(
    address indexed creator,
    address indexed atomWallet,
    bytes atomData,
    uint256 vaultID
);
```

**Logged Parameters:**

* `creator`: Address of the Atom creator.
* `atomWallet`: Address of the Atom's associated abstract account.
* `atomData`: The Atom's respective string data.
* `vaultID`: The vault ID of the Atom.

#### TripleCreated

Emitted upon creation of a Triple.

```solidity
event TripleCreated(
    address indexed creator,
    uint256 subjectId,
    uint256 predicateId,
    uint256 objectId,
    uint256 vaultID,
    uint256 positiveVaultId
);
```

\
**Logged Parameters:**

* `creator`: Address of the Triple creator.
* `subjectId`: The Triple's respective subject Atom.
* `predicateId`: The Triple's respective predicate Atom.
* `objectId`: The Triple's respective object Atom.
* `vaultID`: The vault ID of the triple.

####

