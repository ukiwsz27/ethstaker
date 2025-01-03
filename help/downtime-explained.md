# I'm worried about downtime - Explained

{% hint style="info" %}
The Ethereum network is designed for solo home stakers in mind. This means that the protocol is very forgiving if a validator has downtime or is offline.
{% endhint %}

You shouldn't worry about downtime, but understanding what happens when your validator is offline can help you to gain confidence as a solo home staker.

> If a validator is offline and not executing its duties, it will be penalized at a rate slightly lower than the rewards for the same period of time.

## Example Scenario

- You start your solo staking home validator with 32 ETH.
- Everything is going well and after a few months your validator balance is 32.5 ETH.
- Then... your validator goes offline! 🚨
  - If this happens for real, check out the "[My validator is offline! What do I do?](validator-offline.md)" guide.

### Inactivity Leak

- As soon as your validator is no longer participating in the network it will start [leaking](../staking-glossary.md#inactivity-leak) ETH.
  - When you are offline, for each missed attestation the inactivity leak might be around -0.000011 ETH (the inactivity leak is slightly less than a successful attestation).
  - For a normal successful attestation you might be rewarded with 0.000014 ETH.
- If you have a catastrophic failure and you are not able to get your validator back online for 5 days, then it will take about 5 days of being back online to get back to the same balance as when the failure occurred.

### Missed Block Proposals

If you are offline, you will not be able to produce a block. But how often do block proposals occur for a single validator? Currently, on average, a validator will propose a block every 2-3 months.

So, in this example scenario, even if you are offline for 5 days, there's only a small chance you would miss a block proposal. But what happens if you miss a block proposal?

If you miss your block proposal, the [slot](staking-glossary.md#slot) that should have contained your block will be empty. Other than the lost [rewards](rewards/chain-rewards.md) from missing the block proposal, there are **no penalties or slashing** that occurs from a missed block proposal.

### Can I be slashed for being offline?

No. Realistically, the only condition that can cause a [slashing event](staking-glossary.md#slashable-offenses) is if you run your validator's keys on two nodes at the same time (such as a failover / redundancy setup, where your backup node accidentally turns on while your main node is still running). Don't let this happen, and you won't get slashed. **Slashing cannot occur from being offline for maintenance**.

## What if I can't recover my validator and/or I want to stop staking?

In the event that you can't recover your validator or you decide you want to stop staking, you have the option to exit your validator. Even though withdrawals are not currently enabled, you can still exit your validator from the network. This means that, while you won't be able to get your validator balance back right away (until withdrawals are enabled), you won't receive any penalties for being offline once the validator exits the [withdrawal queue](../staking-glossary.md#validator-queue). Exiting a validator is currently a one way process. For details on how to exit your validator, [check out our guide](../tutorials/how-to-exit-a-validator.md).

## Conclusion

Being a solo validator is an important responsibility to ensure the long term health of the Ethereum network. At EthStaker our goal is to help as many people as possible [#stakefromhome ↗](https://twitter.com/search?q=%23stakefromhome) and this information is provided to show that downtime and being offline is not something to be overly worried about.
