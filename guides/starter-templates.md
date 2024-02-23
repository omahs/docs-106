---
description: A collection of our starter templates.
---

# Starter Templates

We provide starter templates to kickstart your journey building on Intuition. Our goal is allow builders to focus on what matters and dive right in to build the core functionality of your app.

We'll be adding additional starter templates for other frameworks and with additional functionality. Keep checking back!

### Starters Overview

We focused on handling a lot of the tedious steps related to scaffolding a web3 app.&#x20;

#### Authentication Ready

Our starter templates integrate framework-specific auth strategies such as [remix-auth](https://github.com/sergiodxa/remix-auth) and [next-auth](https://github.com/sergiodxa/remix-auth). Your app will have authentication using [DID](https://w3c.github.io/did-core/) session authorization, and you can use your [Intuition API Key](../getting-started/dev-quick-start.md#getting-your-api-key) to interact with our [API](broken-reference).&#x20;

#### web3 Scaffolding

In addition to our authentication, we include tools and patterns for onchain interactions. We leverage [RainbowKit](https://www.rainbowkit.com/), [wagmi](https://www.rainbowkit.com/), and [viem](https://www.rainbowkit.com/) for onchain interactions and [Sign in with Ethereum](https://lucide.dev/) paired with template-specific auth strategy.

In addition to this each starter includes our viem compatible ABI for our [EthMultiVault](../protocol-and-contracts/contract-documentation.md) contract. You can read more about this in our [contract interactions](broken-reference) section.

#### Efficient Styling

We include [Tailwind](https://tailwindcss.com/), [shadcn/ui](https://www.npmjs.com/package/@didtools/cacao), and [Lucide Icons ](https://lucide.dev/)for styling. Shadcn provides beautiful components that can be added to your app via the [CLI](https://ui.shadcn.com/).&#x20;

We've battletested this styling stack and have found it to be a huge productivity boost. Our starters include all of the wiring and configuration needed to work with these libraries.

**Focus on What Matters**

By handling authentication, scaffolding onchain interactions, and focusing on efficient styling, our starters let you channel your energy and creativity into building the unique features of your app. We handled these pieces so that you don't have to!

#### Technologies Used

* [TypeScript](https://www.npmjs.com/package/@didtools/cacao), [ESLint](https://www.npmjs.com/package/@didtools/cacao), and [Prettier](https://www.npmjs.com/package/@didtools/cacao) out of the box
* [Tailwind](https://tailwindcss.com/), [shadcn/ui](https://www.npmjs.com/package/@didtools/cacao), and [Lucide Icons ](https://lucide.dev/)for styling
* [RainbowKit](https://www.rainbowkit.com/), [wagmi](https://www.rainbowkit.com/), and [viem](https://www.rainbowkit.com/) for onchain interactions
* [Sign in with Ethereum](https://lucide.dev/) paired with template-specific auth strategy
* [@didtools/cacao](https://www.npmjs.com/package/@didtools/cacao) and [did-session](https://did.js.org/docs/api/modules/did\_session/) for [DID](https://w3c.github.io/did-core/) management
* [Zod](https://www.npmjs.com/package/@didtools/cacao) for schema validation

### Specific Starters

We'll be adding more of these so keep checking back!

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Remix and Vite Template</td><td><a href="../.gitbook/assets/remix-vite-tile-cover.png">remix-vite-tile-cover.png</a></td><td><a href="https://github.com/0xIntuition/app-template-remix-vite">https://github.com/0xIntuition/app-template-remix-vite</a></td></tr><tr><td>Remix Template</td><td><a href="../.gitbook/assets/remix-tile-cover.png">remix-tile-cover.png</a></td><td><a href="https://github.com/0xIntuition/app-template-remix">https://github.com/0xIntuition/app-template-remix</a></td></tr><tr><td>Next.js Template</td><td><a href="../.gitbook/assets/next-tile-cover.png">next-tile-cover.png</a></td><td><a href="https://github.com/0xIntuition/app-template-next-page-router">https://github.com/0xIntuition/app-template-next-page-router</a></td></tr></tbody></table>
