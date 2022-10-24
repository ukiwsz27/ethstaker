# FAQ <!-- omit in toc -->

- [Can I withdraw my ETH at any time?](#can-i-withdraw-my-eth-at-any-time)
- [I proposed a block! What did I earn?](#i-proposed-a-block-what-did-i-earn)
- [What happens if I lose my validator keys?](#what-happens-if-i-lose-my-validator-keys)
- [What happens if I lose my validator seed phrase / mnemonic?](#what-happens-if-i-lose-my-validator-seed-phrase--mnemonic)

---

## Can I withdraw my ETH at any time?

Withdrawals are not currently enabled on the [beacon chain](staking-glossary.md#beacon-chain). This means that any ETH deposited will be locked in the staking contract until a future time (expected to be in 2023 but this time frame is an estimate) when an upgrade to the network allows withdrawals.

If your validator proposes a block, then some of those rewards are immediately available to you in the form of [priority fees](rewards/chain-rewards.md#priority-fees) and [MEV](rewards/chain-rewards.md#mev) (if you are using an [MEV-Boost](validator-clients/mev-boost.md) relay).

In future, when withdrawals have been enabled, you will be able to withdraw your ETH by exiting your validator and waiting in the [withdrawal queue](staking-glossary.md#validator-queue).

## I proposed a block! What did I earn?

Validators that participate in securing the [beacon chain](staking-glossary.md#beacon-chain) and execute "duties" get rewarded for this by new issuance of ETH. In addition, validators receive priority fees paid by users, and optionally MEV, Maximal Extractable Value.

See a detailed explanation here: [How does my validator earn ETH?](rewards/chain-rewards.md)

## What happens if I lose my validator keys?

If there's a catastrophic failure of your validator and you lose your validator keys, don't panic! These can be easily recovered as long as you still have your [validator seed phrase / mnemonic](staking-glossary.md#validator-seed-phrase). Simply follow the same steps you used when you first generated your validator keys, and install them on a new validator machine.

> Be 100% certain that any previous machines will not come back online as this will lead to a [slashing event](staking-glossary.md#slashable-offenses).

## What happens if I lose my validator seed phrase / mnemonic?

If you lose your seed phrase, the one used to generate the validator keys, then unfortunately your ETH and stake is unrecoverable.
