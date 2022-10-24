# FAQ <!-- omit in toc -->

- [Can I withdraw my ETH at any time?](#can-i-withdraw-my-eth-at-any-time)
- [I proposed a block! What did I earn?](#i-proposed-a-block-what-did-i-earn)
- [Should I set a withdrawal address when setting up my solo staking validator?](#should-i-set-a-withdrawal-address-when-setting-up-my-solo-staking-validator)
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

## Should I set a withdrawal address when setting up my solo staking validator?

Setting a withdrawal address when creating your validator keys can be useful as you won't need to set it again when withdrawals are enabled.

The [Staking Deposit CLI](staking-glossary.md#staking-deposit-cli) can set a withdrawal address during deposit json creation. If a user opts not to do this - usually simply by omission - then it sets the hash of the withdrawal pub key instead. Sometime in the future - possibly with Shanghai/Capella hard fork - there will be a tool that takes the mnemonic and signs a message to effect a one-time change from v0 credentials - withdrawal key - to v1 credentials: Withdrawal address.

And that’s it. Once your validator uses v1 credentials the withdrawal address is fixed and can’t be changed. In the current design, skimming is automatic, and so are full withdrawals: Full withdrawal just happens after exit is completed.

A tool to export the withdrawal key will likely not be created, and it’d also not be very useful. You need the withdrawal key at most twice:

- Once to generate the signing key (only if no withdrawal address was set at that time).
- Once more to sign a message to set one.

In both cases the key can be generated inside the CLI tool, be used for its purpose, and then be discarded again without ever being written to disk.

However, there are some cases to be aware of that make it beneficial to **not** set a withdrawal address at the start:

- If you plan to migrate your validator to a pool e.g. (Rocketpool) in the future, then you won't be able to perform this migration if you set a [withdrawal address](staking-glossary.md#withdrawal-address) when you created your validator keys. You would have to wait for withdrawals to be enabled, potentially wait in the withdrawal queue, then re-stake your ETH, potentially waiting in the activation queue as well!

## What happens if I lose my validator keys?

If there's a catastrophic failure of your validator and you lose your validator keys, don't panic! These can be easily recovered as long as you still have your [validator seed phrase / mnemonic](staking-glossary.md#validator-seed-phrase). Simply follow the same steps you used when you first generated your validator keys, and install them on a new validator machine.

> Be 100% certain that any previous machines will not come back online as this will lead to a [slashing event](staking-glossary.md#slashable-offenses).

## What happens if I lose my validator seed phrase / mnemonic?

If you lose your seed phrase, the one used to generate the validator keys, then unfortunately your staked ETH is most likely unrecoverable.

However, if you had set a withdrawal address, then the validator keys are enough to sign a voluntary-exit, which causes a withdrawal to that address. There is also a special case if you have a pre-signed voluntary-exit message, but that's likely only used by staking services and only noted here for completeness.
