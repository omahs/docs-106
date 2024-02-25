---
description: Contract interaction staking on an Identity.
---

# depositAtom

### depositAtom Contract Function

We can start by looking at the [depositAtom](../protocol-and-contracts/contract-documentation.md#depositatom) in our [Protocol and Contracts](broken-reference) section.

#### depositAtom

Deposits ETH into an atom vault, granting ownership of shares to the specified receiver.

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

Since this function is `payable` it accepts Ether that is sent alongside the call parameters. This can either be set via a UI input, which would allow users to choose the amount that they choose to _stake_ for their Attestation.&#x20;

### useDeposit Hook

If you're using our [Starter Templates ](../guides/starter-templates.md)we provide this hook for you in the `lib/hooks` folder. This specific hook wraps the functionality for creating an onchain Identity (Atom).

{% code title="useDeposit.tsx" lineNumbers="true" %}
```tsx
import { optimismSepolia } from 'viem/chains'
import type { Address } from 'viem'
import { usePreparedContractWriteAndWait } from './useContractWriteAndWait'
import { useMultivaultContract } from './useMultivaultContract'

export const useDeposit = ({
  vault,
  wallet,
  assets,
  isClaim,
}: {
  vault: string
  wallet: Address
  assets: bigint
  isClaim?: boolean
}) => {
  const multivault = useMultivaultContract(optimismSepolia.id)

  const functionName = isClaim ? 'depositTriple' : 'depositAtom'
  const args = [wallet, vault]
  const value = assets
  const chainId = optimismSepolia.id
  const enabled =
    vault !== undefined && assets !== undefined && wallet !== undefined

  return usePreparedContractWriteAndWait({
    ...multivault,
    functionName,
    args,
    value,
    chainId,
    enabled,
  })
}
```
{% endcode %}

This function is used for both `depositAtom` and `depositTriple`. The parameter `isClaim` determines which function is utilized.&#x20;

If you're using the contract interaction for staking on an Identity you'll want to pass a `false` value for `isClaim`.&#x20;

{% hint style="info" %}
This hook leverages the scaffolding outlined in the [Getting Started](getting-started.md) section. If you're unsure of where any of these values come from consider revisiting that page.
{% endhint %}

Let's look at a full example that also leverages our transaction reducer for handling the transaction lifecycle state:

{% code lineNumbers="true" %}
```tsx
sample-component.tsx

//imports
import { useDeposit } from '@/lib/hooks/useDeposit'
import { type TransactionReceipt, formatUnits, keccak256, toHex } from 'viem'
import { useAccount, usePublicClient, useWalletClient } from 'wagmi'

// hook setup
const publicClient = usePublicClient()
const { address } = useAccount()
  
// useDeposit() setup
  const depositHook = useDeposit({
    vault: vault_id!,
    isClaim: claimOrIdentity === 'claim',
    wallet: user.wallet as `0x${string}`,
    assets: parseUnits(val === '' ? '0' : val, 18),
  })
  
  const {
    writeAsync,
    receipt: txReceipt,
    awaitingWalletConfirmation,
    awaitingOnChainConfirmation,
    isIdle,
    isError,
    onReceipt,
    reset,
  } = mode === 'deposit' ? depositHook : redeemHook

// set up a useHandleAction() hook in your component:
 const useHandleAction = (
    actionType: string,
    writeAsync: () => Promise<WriteContractResult>,
    dispatch: (action: StakeTransactionAction) => void,
    fetchReval: FetcherWithComponents<unknown>,
    formRef: React.RefObject<HTMLFormElement>,
  ) => {
    return async () => {
      setLoading(true)
      if (typeof writeAsync === 'function') {
        try {
          await writeAsync()
          onReceipt(() => {
            fetchReval.submit(formRef.current, {
              method: 'POST',
            })
            dispatch({ type: 'TRANSACTION_COMPLETE' })
            setLoading(false)
          })
        } catch (e) {
          dispatch({
            type: 'TRANSACTION_ERROR',
            error: `Error processing ${actionType} transaction`,
          })
          setLoading(false)
        }
      }
    }
  }
```
{% endcode %}

If you decide to allow users to choose the amount they want to stake you'll want to pass that value into the `assets` parameter in the `depositHook`. The amount passed in for `assets` is the amount of value that is deposited into the vault when a user stakes.&#x20;

The decision to allow users to set their initial deposit value depends on your usecase. If you statically set this and have every deposit use the same amount you may want to consider framing Attestations and Staking as being a social signal since there isn't any way for users to specify the weight of their Attestation. Let's look at an example and the implications:

**Deposit amount being dynamically set with user input**

* 3 users go to stake on Claim A:
  * User 1 stakes .5 ETH in the positive vault (Attestation _for_)
  * User 2 stakes .1 ETH in the positive vault
  * User 3 stakes .01 ETH in the positive vault
* The decision for users to select the amount that they're staking adds the mechanic of measuring the _strength_ or the _weight_ behind their Attestation. In the example above, it can likely be inferred that User 1 has a stronger belief in the Attestation they're making.

**Deposit amount being statically set without user input:**

* 3 users go to stake on Claim B:
  * Users 1, 2, and 3 all choose to stake in the positive vault (Attestation _for_)
  * The deposit amount is statically set to .01 ETH for every deposit
* This decision removes the concepts of _weight_ or _strength_ of belief in the Attestation but still allows for Attestations to serve the purpose of a positive (or negative) social signal.

Whichever option you choose should be based on your usecase. In our Portal implementation we choose to allow users to select the deposit amount, but you can choose to do what works best for your usecase.

