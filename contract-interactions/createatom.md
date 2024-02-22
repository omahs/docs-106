---
description: Contract interaction for creating an Identity (Atom).
---

# createAtom

Creates an onchain Identity. In the context of  onchain interactions, an Identity is known as an Atom. You should consider pairing this with [Creating an Identity](../api/identities/create-an-identity.md) for offchain Identity creation. The Intuition Portal user journey for creating a Identity combines both the offchain and onchain interactions.

If you haven't already worked through the scaffolding in the [Getting Started](getting-started.md) section you'll want to do that before continuing.

{% hint style="info" %}
While you can create an Identity through onchain interactions only, it's recommended to combine this with an offchain create via our API and then passing the resulting `identity_id` into the `createAtom` for the most robust user experience and alignment with our protocol and knowledge graph.
{% endhint %}

### createAtom Contract Function

We can start by looking at the [createAtom function](../protocol-and-contracts/contract-documentation.md#createatom) in our [Protocol and Contracts](broken-reference) section.

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

This signature is flexible and allows for any string to be passed in as the `atomData`.&#x20;

If you're interested in aligning with the process we use in our Portal app, you'll want to create an offchain Identity via an API call and then pass in the `identity_id` that you get back in the response for the successfully created Identity. Following this flow ensures that the `identity_id` has a `did` Document for the associated metadata that is stored on IPFS.&#x20;

### useCreateAtom Hook

If you're using our [Remix Starter Template](https://github.com/0xIntuition/app-template-remix) we provide this hook for you in the `lib/hooks` folder. This specific hook wraps the functionality for creating an onchain Identity (Atom).

{% code title="useCreateAtom.tsx" lineNumbers="true" %}
```tsx
import { type GetContractReturnType } from 'viem'
import { optimismSepolia } from 'viem/chains'
import { useContractWriteAndWait } from './useContractWriteAndWait'
import { useMultivaultContract } from './useMultivaultContract'

export const useCreateAtom = () => {
  const multivault = useMultivaultContract(
    optimismSepolia.id,
  ) as GetContractReturnType

  return useContractWriteAndWait({
    ...multivault,
    functionName: 'createAtom',
  })
}
```
{% endcode %}

If you're looking to generalize this pattern for use with other contracts, you'll want to switch out the ABI and be sure that the `functionName` matches a function in your contract. In this hook, the `functionName: 'createAtom'` matches the `createAtom` function in our contract.

{% hint style="info" %}
This hook leverages the scaffolding outlined in the [Getting Started](getting-started.md) section. If you're unsure of where any of these values come from consider revisiting that page.
{% endhint %}

Once this hook pattern is established you can use it in your contract interactions. The following example is a snippet from how we structure our `createAtom` call for the onchain creation step in the Create Identity Flow in our Portal.

Let's look at a full example that also leverages our transaction reducer for handling the transaction lifecycle state:

{% code title="sample-component.tsx" lineNumbers="true" %}
```tsx
//imports
import { useCreateAtom } from '@/lib/hooks/useCreateAtom'
import { type TransactionReceipt, formatUnits, keccak256, toHex } from 'viem'
import { useAccount, usePublicClient, useWalletClient } from 'wagmi'

// hook setup
const publicClient = usePublicClient()
const { address } = useAccount()
  
// useCreateAtom() setup
const {
    writeAsync: writeCreateAtom,
    awaitingWalletConfirmation,
    awaitingOnChainConfirmation,
} = useCreateAtom()

// example async function leveraging useCreateAtom
async function handleOnChainCreateAtom({ atomData }: { atomData: string }) {
    if (
      !awaitingOnChainConfirmation &&
      !awaitingWalletConfirmation &&
      writeCreateAtom &&
      address
    ) {
      try {
        dispatch({ type: 'TRANSACTION_APPROVE' })
        const identityTx = await writeCreateAtom({
          args: [keccak256(toHex(atomData))],
          value: atomCost,
        })
        dispatch({ type: 'TRANSACTION_APPROVE_COMPLETE' })
        dispatch({ type: 'TRANSACTION_PROGRESS' })
        if (identityTx !== undefined && identityTx.hash) {
          dispatch({ type: 'TRANSACTION_PROGRESS_PROGRESS' })
          const identityTxReceipt =
            await publicClient.waitForTransactionReceipt({
              hash: identityTx.hash,
            })
          handleIdentityTxReceiptReceived(identityTxReceipt)
          dispatch({
            type: 'TRANSACTION_COMPLETE',
            txHash: identityTx.hash,
            txReceipt: identityTxReceipt,
          })
          toast.custom(
            (_) => (
              <IdentityToast
                toastType="success"
                txHash={identityTxReceipt.transactionHash}
              />
            ),
            { duration: 5000 },
          )
        }
      } catch (error) {
        logger('error', error)
        if (error instanceof Error) {
          let errorMessage = 'Error in onchain transaction.'
          if (error.message.includes('insufficient')) {
            errorMessage =
              'Insufficient funds. Please add more OP to your wallet and try again.'
          }
          if (error.message.includes('rejected')) {
            errorMessage = 'Transaction rejected. Try again when you are ready.'
          }
          dispatch({
            type: 'TRANSACTION_ERROR',
            error: errorMessage,
          })
          toast.custom(
            (t) => (
              <IdentityToast toastType="error" errorMessage={errorMessage} />
            ),
            { duration: 5000 },
          )
          return
        }
      }
    }
  }
```
{% endcode %}

This comprehensive example is an example of our full onchain Identity creation. You can remove pieces from this to simplify the interaction as shown in our [Remix Starter Template](https://github.com/0xIntuition/app-template-remix).&#x20;
