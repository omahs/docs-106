---
description: >-
  A deep dive into how some of the key properties and actions within our smart
  contracts work
---

# Deep Dive

### Overview

The EthMultiVault system facilitates the creation of Atoms and Triples, which are essential for building and expanding the Intuition Knowledge Graph. This system allows users to contribute knowledge and insights, leveraging Ethereum (ETH) as a medium of exchange and interaction.

### Atoms

**Atom**: A fundamental unit within the Intuition ecosystem, representing the smallest piece of knowledge. Atoms can embody various forms of data, such as subjects, predicates, objects, or even Triples themselves. They serve as the building blocks for constructing more complex knowledge representations.

Key Features of an Atom:

1. Each Atom is uniquely identified, typically through a URI.
2. Atoms are associated with Ethereum addresses or Smart Contract Wallets.
3. Detailed descriptions and metadata about Atoms are stored on decentralized storage solutions like the Ceramic Network.

While initially, Shares in Atom Vaults cannot be purchased directly, users can acquire Shares by staking on Triples that incorporate specific Atoms. Future iterations may introduce direct Share purchase options for Atoms.

### Creating Atoms

Atom creation involves several steps, including:

* Initiating a dedicated Vault for the Atom.
* Assigning an Atom Identity and generating a corresponding DID using the did:pkh standard.
* Ensuring the Atom's uniqueness within the system.
* Storing detailed Atom metadata on external, decentralized registries for accessibility and transparency.

The creation of an Atom necessitates a fee, which is utilized for:

* Allocating Shares within the Atom's Vault.
* Registering the Atom within the Intuition system for tracking and utilization purposes.

### Triples

**Triple**: Represents a structured knowledge statement within the Intuition ecosystem, formulated as {Subject, Predicate, Object}. Triples are composed of three Atoms and articulate semantic relationships or assertions (see [semantic triple](https://en.wikipedia.org/wiki/Semantic\_triple)).

Triples enable users to craft detailed and meaningful statements, enriching the knowledge graph with semantically structured data. The creation of a Triple involves specifying three constituent Atoms and ensuring the Triple's hash uniqueness to maintain data integrity.

### Creating Triples

The Triple creation process requires users to:

* Supply three Atoms as the Triple's Subject, Predicate, and Object.
* Guarantee the Triple's uniqueness through a unique hash.
* Stake ETH on the Triple, if desired, to express concurrence with its assertion.

When creating a triple, logical and semantic coherence is encouraged to ensure the graph's overall quality and utility.

### Vaults

Vaults are central to the EthMultiVault architecture, managing the economic aspects of knowledge representation. Each Atom and Triple is linked to a Vault, which administers the ETH stakes and Shares related to the knowledge entity.

Conforming to the ERC4626 Tokenized Vault Standard, we extend the functionality to our EthMultiVault standard. ERC4626 is a standard of yield-bearing vaults which represent shares of a single underlying ERC-20 Token. Core functionality of 4626 Vaults includes depositing and withdrawing tokens, as well as reading balances.

By depositing ETH, a user receives pro-rata shares (minus fees) representing ownership of those assets. These shares can then be redeemed for the deposit assets plus any generated rewards at a later point.

<div>

<figure><img src="../.gitbook/assets/vault_deposit_math.png" alt=""><figcaption></figcaption></figure>

 

<figure><img src="../.gitbook/assets/vault_withdrawal_math.png" alt=""><figcaption></figcaption></figure>

</div>

### Atom & Triple Staking

Staking involves depositing ETH into Vaults corresponding to Atoms or Triples, acquiring Shares in return. This process signifies a user's support for the knowledge entity's validity and grants them a stake in the entity's economic activity, such as accruing fees from future interactions.

In conclusion, the EthMultiVault system presents a robust framework for decentralized knowledge representation, underpinned by economic incentives. By facilitating the creation and engagement with Atoms and Triples, it underlies a participatory and economically driven knowledge graph within the Intuition ecosystem.
