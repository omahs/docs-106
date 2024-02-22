---
description: Overview of the Intuition Protocol and its core core smart contracts.
---

# Overview

Intuition's smart contracts are central to its blockchain architecture, handling critical on-chain activities such as the creation of Atoms (also known as Identities) and Triples (also known as Claims), as well as the staking processes on these elements. These contracts are integral to the economic interactions involved in creating and staking, ensuring a seamless and secure execution of these operations. Beyond these functions, the smart contracts might also govern various other components integral to the Intuition system. These include Atom Claiming/Recovery, management of off-chain Atom Data, and various registries such as Bonding Curve, Primitive, Schema, Algorithm, and Interpretation Registries.

Acting as the Verifiable Data Registry, these smart contracts store crucial information like URIs of Atoms/Triples, the intricate relationships between them, and current stakeholder positions. They essentially form the nodes and edges of the knowledge graph. In contrast, the off-chain Verifiable Data Registry maintains the properties of these nodes and edges, creating a comprehensive and interconnected data structure.

Key components of the on-chain state of the knowledge graph include:

1. Atom wallets and vaults, along with their respective shares and balances.
2. AtomData, typically represented as bytes, usually pointing to a URI for off-chain data.
3. Triples and their constituent Atoms.
4. User positions on Atoms and Triples, reflecting current stakes and indicating who is attesting to what.

The primary contract in this framework, referred to as the MultiVault contract, functions similarly to a blend and advancement of the ERC-4626 and ERC-1155 standards. This contract is a pivotal element of the Intuition protocol, facilitating a wide range of functionalities and providing a robust foundation for the decentralized knowledge graph.

### Pausability

The EthMultiVault contract incorporates a mechanism to pause specific methods, ensuring user security and safety, and facilitating safe resolutions in emergency situations. This feature is paramount for maintaining the integrity of operations and safeguarding assets under unforeseen circumstances. The methods that can be paused include:

* `createAtom`
* `createAtomCompressed`
* `batchCreateAtom`
* `batchCreateAtomCompressed`
* `createTriple`
* `batchCreateTriple`
* `depositAtom`
* `depositTriple`
* `deployAtomWallet`
* `redeemAtom`
* `redeemTriple`

This pausability feature is crucial for preemptive measures against potential vulnerabilities or during critical updates to ensure uninterrupted and secure service. You can learn more about the methods mentioned above here.

### Emergency Mode

Upon activating the pause function, the contract automatically transitions into emergency mode. This mode is a critical feature that pauses all functions listed in the Pausability section and activates specific methods for emergency withdrawals of assets from atom and triple vaults. Emergency withdrawals are executed in batches and are solely performed by the contract admin. This measure is a last resort, employed when the contract cannot be upgraded safely or in scenarios where an immediate intervention is required for asset protection.

### Upgradeability

The current architecture of our EthMultiVault contract utilizes the EIP-1967 Transparent Proxy pattern, specifically through the `TransparentUpgradeableProxy` by OpenZeppelin. This pattern ensures that the contract remains transparent and upgradeable, aligning with our commitment to transparency and community trust. The upgrade process is managed by a separate ProxyAdmin contract, which is presently controlled by an address associated with our team. However, plans are underway to transition this mechanism to a multisig arrangement, potentially incorporating prominent community members. This evolution aims to bolster trust and decentralize administrative control, enhancing the security and resilience of the contract's upgradeability.

### Latest Deployments

The latest deployment of the Intuition protocol contracts has been executed on the OP Sepolia testnet. Below are the addresses of the key smart contracts in the system:

**Optimism Sepolia**

* **EthMultiVault Proxy**: [https://sepolia-optimism.etherscan.io/address/0x3A169e3b7EadAB76870D4c510a9DaA3a75618b69](https://sepolia-optimism.etherscan.io/address/0x3A169e3b7EadAB76870D4c510a9DaA3a75618b69)
* **EthMultiVault Implementation**: [https://sepolia-optimism.etherscan.io/address/0xd2057bF9EB265897CEA5658b7013C3b3f9D779EA](https://sepolia-optimism.etherscan.io/address/0xd2057bF9EB265897CEA5658b7013C3b3f9D779EA)
* **ProxyAdmin**: [https://sepolia-optimism.etherscan.io/address/0xc4082Ba087Fd36FA199cA27d03A36EE947A9Cb46](https://sepolia-optimism.etherscan.io/address/0xc4082Ba087Fd36FA199cA27d03A36EE947A9Cb46)

These deployments are a testament to our ongoing efforts to test, iterate, and validate the functionalities of our contracts in a controlled environment, ensuring robustness and reliability before mainnet deployment.
