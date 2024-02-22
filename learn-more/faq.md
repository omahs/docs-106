---
description: Frequently Asked Questions.
---

# ❓ FAQ

<details>

<summary>What is Intuition?</summary>

Intuition is a composition of decentralized identity, decentralized data, and decentralized finance primitives into a full-stack solution that allows anyone to claim and discover anything about anything.&#x20;

Intuition enables and incentivizes users to create attestations – or cryptographically signed messages – about subjects and their decentralized identifiers (DIDs), storing the information in a way where the resulting information is easily navigable, queryable, and leveraged by other applications.

</details>

<details>

<summary>How does it work?</summary>

Intuition’s protocol and middleware allow developers to easily integrate attestation infrastructure into their own applications, which can permissionlessly create or consume data from Intuition’s shared knowledge graph.

Intuition will also be launching its own application built atop the protocol, functioning as a knowledge graph explorer, and is planning to have human-readable Intuition data regarding wallet addresses displayed directly in popular web wallets.&#x20;

</details>

<details>

<summary>Why does Intuition exist?</summary>

We presently rely on a fundamentally flawed system of siloed, centralized platforms such as Yelp, LinkedIn, and Twitter to inform our intuition about the world around us. These platforms shape our perceptions and decisions with fragmented, incomplete, and occasionally misleading information.&#x20;

By adopting the appropriate decentralized systems and incentives, we can foster more precise, transparent, and comprehensive understanding

</details>

<details>

<summary>Will user data be encrypted / private?</summary>

Yes and no. Intuition believes that some claims should be public, while others should be private. If a user wants to make or receive entirely-private attestations, they can easily do so. However, since the Intuition system is designed to encompass all things - not just people - some data may need to be public by default, as many entities are not able to “selectively disclose” data (such as an ephemeral concept that is not controlled by anyone).&#x20;

To address this, users can create an attestation either entirely off-chain or create an on-chain attestation that references an encrypted off-chain attestation or piece of data. This approach allows users to take advantage of Intuition's on-chain functionality, such as token mechanics, while still maintaining privacy.

</details>

<details>

<summary>How will Intuition ensure that attestations are not misleading/false? </summary>

Intuition does not engage in censorship or determine the truthfulness of attestations. The platform allows users to make claims about anything, but with certain restrictions in place to prevent malicious claims. Intuition provides users with tooling to help weigh data and make informed decisions, leveraging the metadata available through the platform and a user's social/trust graph.&#x20;

However, it is ultimately up to the individual user to determine what is true or false. Intuition is designed to enable the creation of arbitrary logic that can be programmed on top of verifiable data, specific to a user's needs.

With that being said, anyone is free to build additional tooling or design methods for separating the signal from the noise and/or guiding users in the proper direction. For example, MetaMask might require 10 approved auditors to declare a smart contract secure before granting a blue checkmark. Similarly, a user might require a certain number of attestations from trusted individuals before making a decision about a restaurant, or a DAO may require a certain threshold of attestations about an individual before extending an undercollateralized loan through a DeFi protocol.

</details>

<details>

<summary>What am I signing?</summary>

You will sign a message proving you have ownership of the associated ETH address, using both [SIWE](https://docs.login.xyz/sign-in-with-ethereum/quickstart-guide/creating-siwe-messages) & [DIDSession](https://did.js.org/docs/api/classes/did\_session.DIDSession/).   This is also used to instantiate a new DID Session and issue API keys to new developer users.

```json
{
    "domain": "window.location.host",
    "address": "0x0",
    "statement": "Sign in to Intuition",
    "uri": "window.location.origin",
    "version": "1",
    "nonce": "bqnxrfyv",
    "issuedAt": "2024-01-01T00:00:00.000Z",
    "expirationTime": "2024-02-01T00:00:00.000Z",,
    "chainId": "11155420",
}
```

</details>
