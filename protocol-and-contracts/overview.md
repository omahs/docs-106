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

The primary contract in this framework, referred to as the EthMultiVault contract, functions similarly to a blend and advancement of the ERC-4626 and ERC-1155 standards. This contract is a pivotal element of the Intuition protocol, facilitating a wide range of functionalities and providing a robust foundation for the decentralized knowledge graph.

Aside from EthMultiVault, the AtomWallet contract is a foundational element of the Intuition protocol, acting as an abstract account (wallet) linked to a specific atom. It leverages account abstraction standard (ERC-4337) to enable advanced transaction execution and management capabilities for atom-associated operations within the blockchain environment.
