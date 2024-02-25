---
description: Contract interaction for creating a Claim (Triple).
---

# createTriple

## createTriple

Creates an onchain Claim. In the context of onchain interactions, a Claim is known as a Triple. You should consider pairing this with [Creating a Claim](../api/claim/create-a-claim.md) for offchain Claim creation. The Intuition Portal user journey for creating a Claim combines both the offchain and onchain interactions.&#x20;

If you haven't already worked through the scaffolding in the [Getting Started](https://app.gitbook.com/o/xYyeoT5KBfRZxYH5NYQb/s/9RbcQCLDAZejj4yGMuAM/contract-interactions/getting-started) section you'll want to do that before continuing.

#### createTriple Contract Function <a href="#createatom-contract-function" id="createatom-contract-function"></a>

We can start by looking at the [createTriple](../protocol-and-contracts/contract-documentation.md#createtriple) function in our [Protocol and Contracts](broken-reference) section.

#### createTriple

Creates a triple from three given atom IDs and returns its vault ID. Requires a payment of ETH to cover the triple cost.

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

#### useCreateTriple Hook <a href="#usecreateatom-hook" id="usecreateatom-hook"></a>

If you're using our [Starter Templates](../guides/starter-templates.md) we provide this hook for you in the `lib/hooks` folder.&#x20;

This specific hook wraps the functionality for creating an onchain Claim (Triple).

{% code title="useCreateTriple.tsx" lineNumbers="true" %}
```tsx
import { type GetContractReturnType } from 'viem'
import { optimismSepolia } from 'viem/chains'
import { useContractWriteAndWait } from './useContractWriteAndWait'
import { useMultivaultContract } from './useMultivaultContract'

export const useCreateTriple = () => {
  const multivault = useMultivaultContract(
    optimismSepolia.id,
  ) as GetContractReturnType

  return useContractWriteAndWait({
    ...multivault,
    functionName: 'createTriple',
  })
}
```
{% endcode %}

If you're looking to generalize this pattern for use with other contracts, you'll want to switch out the ABI and be sure that the `functionName` matches a function in your contract. In this hook, the `functionName: 'createTriple'` matches the `createTriple` function in our contract.

{% hint style="info" %}
This hook leverages the scaffolding outlined in the [Getting Started](getting-started.md) section. If you're unsure of where any of these values come from consider revisiting that page.
{% endhint %}

Once this hook pattern is established you can use it in your contract interactions. The following example is a snippet from how we structure our `createTriple` call for the onchain creation step in the Create Claim Flow in our Portal.

Let's look at a full example that also leverages our transaction reducer for handling the transaction lifecycle state:

{% code title="sample-component.tsx" lineNumbers="true" %}
```tsx
  //imports
  import { useCreateTriple } from '@/lib/hooks/useCreateTriple'
  import { formatUnits } from 'viem'
  import { useAccount, usePublicClient, useWalletClient } from 'wagmi'
  
  // hook setup
  const publicClient = usePublicClient()
  const { address } = useAccount()
    
  // useCreateTriple() setup
  const {
    writeAsync: writeCreateTriple,
    awaitingWalletConfirmation,
    awaitingOnChainConfirmation,
  } = useCreateTriple()

  // example async function leveraging useCreateAtom
  async function handleOnChainCreateTriple({
    subjectVaultId,
    predicateVaultId,
    objectVaultId,
  }: {
    subjectVaultId: string
    predicateVaultId: string
    objectVaultId: string
  }) {
    if (
      !awaitingOnChainConfirmation &&
      !awaitingWalletConfirmation &&
      writeCreateTriple &&
      address
    ) {
      try {
        dispatch({ type: 'CONFIRM_TRANSACTION' })
        const claimTx = await writeCreateTriple({
          args: [subjectVaultId, predicateVaultId, objectVaultId],
          value: feeCalculation,
        })
        dispatch({ type: 'TRANSACTION_PENDING' })
        if (claimTx !== undefined && claimTx.hash) {
          logger('claimTx hash', claimTx.hash)
          const claimTxReceipt = await publicClient.waitForTransactionReceipt({
            hash: claimTx.hash,
          })
          dispatch({
            type: 'TRANSACTION_COMPLETE',
            txHash: claimTx.hash,
            txReceipt: claimTxReceipt.toString(),
          })
          logger('claim receipt', claimTxReceipt)
          toast.custom(
            (_) => (
              <ClaimToast
                toastType="success"
                txHash={claimTxReceipt.transactionHash}
              />
            ),
            { duration: 5000 },
          )
        }
      } catch (error) {
        console.error('error', error)
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
            (t) => <ClaimToast toastType="error" errorMessage={errorMessage} />,
            { duration: 5000 },
          )
          return
        }
      }
    }
  }
```
{% endcode %}

This comprehensive example is an example of our full onchain Claim creation. You can remove pieces from this to simplify the interaction as shown in our [Remix Starter Template](https://github.com/0xIntuition/app-template-remix).&#x20;
