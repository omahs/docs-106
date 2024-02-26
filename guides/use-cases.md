---
description: Potential use cases for inspiration.
---

# Use Cases

We're excited to see what unique integrations that builders come up with for building on, extending, and populating the Intuition knowledge graph.

We'll be adding more use cases over time and as we expand our resources for building on Intuition. These initial use cases are intended to serve as inspiration and starting points for exploring what's possible. We welcome feedback and would love to see what you build!

{% hint style="success" %}
We've thought about the use cases presented in both categories and have thought about how they can be built. If you're interested in exploring this with us, [please contact us](../learn-more/contact-us.md) or find us in person at EthDenver at Booth 506 from Wednesday 2/28 to Sunday 3/3!
{% endhint %}

### Protocol Integrations

While the use cases in both categories are all technically protocol integrations, this first group of ideas relate to extension via protocol composition. These use cases fulfill specific purposes, solve a novel problem, or can be launch points for collaboration between protocols.

Creating specific integrations is an effective and important way to contribute to Intuition and help populate the Intuition knowledge graph, improving the impact and power for all Intuition users.&#x20;

Several of these integration proof of concepts rely on inferring a user's address from their Identity. There is a specific workaround to keep in mind for this:

{% hint style="info" %}
Our current Beta doesn't include a specific concept of User Identities. This makes it tricky to infer a user's address from their Identity. As a workaround for a proof of concept, you can set a user address as either the `display_name` or include it in the `description`.\
\
Taking this approach allows for testing out the logic of this integration as it's a clear path to obtaining a user's address from the associated Identity.
{% endhint %}

The following are a list of such use cases that we've thought about that we'd love to see built!&#x20;

#### Intuitive Splits

* Leverage Intuition primitives alongside [0xSplits](https://splits.org/) to programmatically create Splits from Intuition Identities that meet specific criteria.

#### Intuition Powered DAOs

* Leverage Intuition primitives to programmatically create a DAO based on specific criteria.
* We'd love to see this built tailored to meet the needs of different DAO governance frameworks:
  * Moloch DAOs
    * [Moloch v3 Contract Docs](https://moloch.daohaus.fun/)
    * [DAOhaus Developer Docs: Contracts](https://docs.daohaus.club/contracts)
  * Aragon DAOs
    * [Aragon OSx Developer Portal](https://devs.aragon.org/)
  * Governor DAOs
    * [Tally: OpenZeppelin Governor Docs](https://docs.tally.xyz/user-guides/tally-contract-compatibility/openzeppelin-governor)

#### Intuition and Hats

* Leverage Intuition and [Hats Protocol](https://www.hatsprotocol.xyz/) to issue Hats based on users meeting specified criteria

#### Intuition and Farcaster&#x20;

* Create a Farcaster [Frame](https://docs.farcaster.xyz/learn/what-is-farcaster/frames) that integrates Intuition primitives.

#### Intuition Access Gating

* General access gating based on an Intuition Claim.

### Populating Intuition

This category of use cases are for building apps and integrations that can contribute to, and extend, the Intuition knowledge graph.

Intuition becomes more powerful for all users and builders as the knowledge graph grows. Identities play a critical role in this, and adding robust Identity data is a high value way of contributing to Intuition.

Having our knowledge graph be populated with robust, quality Identity data is essential for creating quality Claims that represent the _relationship_ between Identities.

The example use cases and integrations in this category relate to populating the knowledge graph either through programmatic creation of Identities or Claims.

#### Intuitive Open Source Contributions

* Leverage the [GitHub API](https://docs.github.com/en) to get a list of open source projects that meet specific criteria and then programmatically create Identities.

