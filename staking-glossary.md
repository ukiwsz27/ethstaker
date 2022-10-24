# Staking Glossary <!-- omit in toc -->

- [Attestation](#attestation)
- [Beacon chain](#beacon-chain)
- [Block](#block)
- [Block proposer](#block-proposer)
- [Block status](#block-status)
  - [Proposed](#proposed)
  - [Scheduled](#scheduled)
  - [Missed/skipped](#missedskipped)
  - [Orphaned](#orphaned)
- [Canonical chain](#canonical-chain)
- [Chain head](#chain-head)
- [Checkpoints](#checkpoints)
- [Committees](#committees)
- [Deposit contract](#deposit-contract)
- [Epoch](#epoch)
- [Finalization](#finalization)
  - [Finality issues](#finality-issues)
- [Fork](#fork)
- [Head vote](#head-vote)
- [Inactivity leak](#inactivity-leak)
- [Input Data](#input-data)
- [Justification](#justification)
- [Light clients](#light-clients)
- [Participation rate](#participation-rate)
- [Proof of stake (PoS)](#proof-of-stake-pos)
- [Signing](#signing)
- [Slashable offenses](#slashable-offenses)
  - [Attestation violation](#attestation-violation)
  - [Proposer violation](#proposer-violation)
- [Slasher node](#slasher-node)
- [Slot](#slot)
- [Source vote](#source-vote)
- [Sync committee](#sync-committee)
- [Target vote](#target-vote)
- [Validator](#validator)
  - [Eligible for activation & Estimated activation](#eligible-for-activation--estimated-activation)
  - [Unique index](#unique-index)
  - [Current balance & Effective balance](#current-balance--effective-balance)
- [Validator Seed Phrase / Mnemonic](#validator-seed-phrase--mnemonic)
- [Validator lifecycle](#validator-lifecycle)
    - [1. Deposited](#1-deposited)
    - [2. Pending](#2-pending)
    - [3. Active Validator](#3-active-validator)
    - [4. Slashing Validator](#4-slashing-validator)
    - [5. Exiting Validator](#5-exiting-validator)
- [Validator queue](#validator-queue)
- [Withdrawal address](#withdrawal-address)

---

## Attestation

Votes by [validators](#validator) which confirm the validity of a [block](#block). At designated times, each validator is responsible for publishing different attestations that formally declare a validator's current view of the chain, including the last finalized [checkpoint](#checkpoints) and the current [head of the chain](#chain-head).

## Beacon chain

A major part of the work of the beacon chain is storing and managing the registry of [validators](#validator) – the set of participants are responsible for running the Ethereum [Proof of Stake (PoS)](#proof-of-stake-pos) system.

This registry is used to:

- Assigns validators their duties.
- Finalizes [checkpoints](#checkpoints).
- Perform a protocol level random number generation (RNG).
- Progress the beacon chain.
- Vote on the [head of the chain](#chain-head) for the fork choice.

[Source ↗](https://notes.ethereum.org/@djrtwo/Bkn3zpwxB#High-level-overview)

## Block

A block is a bundled unit of information that include an ordered list of transactions and consensus-related information. Blocks are proposed by [Proof of Stake (PoS)](#proof-of-stake-pos) validators, at which point they are shared across the entire peer-to-peer network, where they can easily be independently verified by all other nodes. Consensus rules govern what contents of a block are considered valid, and any invalid blocks are disregarded by the network. The ordering of these blocks and the transactions therein create a deterministic chain of events with the end representing the current state of the network.

## Block proposer

A chosen [validator](#validator) by the [Beacon Chain](#beacon-chain) to propose the next [block](#block). There can only be one valid block per [slot](#slots).

## Block status

### Proposed

The block was proposed by a validator.

### Scheduled

Validators are currently submitting data.

### Missed/skipped

The proposer didn’t propose the block within the given time frame, so the block was missed/skipped.

### Orphaned

In order to understand this, let us look at the diagram below "1, 2, 3, ... ,9" represent the slots.

1. Validator at slot 1 proposes the block “a”.
2. Validator at slot 2 proposes “b”.
3. Slot 4 is being skipped because the validator didn’t propose a block (e.g.: offline).
4. At slot 5/6 a fork occurs: Validator(5) proposes a block, but validator(6) doesn’t receive this data (e.g.: the block didn’t reach them fast enough). Therefore Validator(6) proposes its block with the most recent information it sees from validator(3).
5. The [fork choice rule ↗](https://notes.ethereum.org/@vbuterin/rkhCgQteN?type=view#LMD-GHOST-fork-choice-rule) is the key here - It decides which of the available chains is the canonical one.

## Canonical chain

The canonical chain is the chain which is agreed to be the 'main' chain and not a [fork](#fork).

## Chain head

The latest block received by a validator. This does not necessarily mean it is the head of the [canonical chain](#canonical-chain).

## Checkpoints

The [Beacon Chain](#beacon-chain) has a tempo divided into [slots](#slot) (12 seconds) and [epochs](#epoch) (32 slots). The first slot in each epoch is a checkpoint. When a supermajority of validators [attests](#attestation) to the link between two checkpoints, they can be [justified](#justification) and then when another checkpoint is justified on top, they can be [finalized](#finalization).

## Committees

A group of at least 128 [validators](#validator) assigned to validate blocks in each [slot](#slot). One of the validators in the committee is the aggregator, responsible for aggregating the signatures of all other validators in the committee that agree on an attestation. Not to be confused with [sync committees](#sync-committee).

## Deposit contract

The Deposit contract is the **gateway** to Ethereum [Proof of Stake (PoS)](#proof-of-stake-pos) and is managed **through a smart contract** on Ethereum. The smart contract accepts any transaction with a minimum amount of 1 ETH and a valid [input data](#input-data). Ethereum beacon-nodes listen to the deposit contract and use the input data to credit each validator.

[_More info on the deposit contract ↗_](getting-started/deposit-process.md)

## Epoch

**1 Epoch = 32 [Slots](#slot)**  
Represents the number of 32 slots (12 seconds) and takes approximately **6.4 minutes.** Epochs play an important role when it comes to the [validator queue](#validator-queue) and [finality](#finalization).

## Finalization

In Ethereum [Proof of Stake (PoS)](#proof-of-stake-pos) at least two third of the validators have to be honest, therefore if there are two competing [epochs](#epoch) and one third of the [validators](#validator) decide to be malicious, they will receive a penalty. Honest validators will be rewarded.

In order to determine if an epoch has been finalized, validators have to agree on the latest two epochs in a row, then all previous Epochs can be considered as finalized.

### Finality issues

If there are less than 66.6% of the total possible votes (the [participation rate](#participation-rate)) in a specific epoch, the epoch cannot be [justified](#justification). As mentioned in "[Finalization](#finalization)", three justified epochs in a row are required to reach finality. As long as the chain cannot reach this state it has finality issues.

During finality issues, the validator [entry queue](#validator-queue) will be halted and new validators will not be able to join the network, however, inactive validators with less than 16 ETH balance will be exited from the network. This leads to more stability in the network and a higher participation rate, allowing the chain to eventually finalize.

## Fork

A change in protocol causing the creation of an alternative chain or a temporal divergence into two potential block paths.

## Head vote

The validator has made a timely vote for the correct [head block](#chain-head).

## Inactivity leak

If the Beacon Chain has gone more than four [epochs](#epoch) without [finalizing](#finalization), an emergency protocol called the "inactivity leak" is activated. The ultimate aim of the inactivity leak is to create the conditions required for the chain to recover finality. Finality requires a 2/3 majority of the total staked ether to agree on source and target checkpoints. If validators representing more than 1/3 of the total validators go offline or fail to submit correct [attestations](#attestation) then it is not possible for a 2/3 supermajority to finalize checkpoints. The inactivity leak lets the stake belonging to the inactive validators gradually bleed away until they control less than 1/3 of the total stake, allowing the remaining active validators finalize the chain. However large the pool of inactive validators, the remaining active validators will eventually control >2/3 of the stake. The loss of stake is a strong incentive for inactive validators to reactivate as soon as possible!

## Input Data

The Input data, also called the **deposit data**, is a user generated, 842 long sequence of characters. It represents the [validator public key](validator-keys.md) and the [withdrawal public key](validator-keys.md), which were signed with by the validator private key. The input data needs to be added to the transaction to the [deposit contract](getting-started/deposit-process.md) in order to get identified by the [beacon-chain](#beacon-chain).

[_More info about the deposit process ↗_](getting-started/deposit-process.md)

## Justification

66.6% of the total validators need to attest in favour of a block's inclusion in the [canonical chain](#canonical-chain). This condition upgrades the block to "justified". Justified blocks are unlikely to be reverted, but they can be under certain conditions.

When another block is justified on top of a justified block, it is upgraded to "finalized". Finalizing a block is a commitment to include the block in the canonical chain.

[_More info on justification ↗_](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/gasper/)

## Light clients

An Ethereum client that does not store a local copy of the blockchain, or validate blocks and transactions. It offers the functions of a wallet and can create and broadcast transactions.

## Participation rate

TODO

## Proof of stake (PoS)

A method by which a cryptocurrency blockchain protocol aims to achieve distributed consensus. PoS asks users to prove ownership of a certain amount of cryptocurrency (their "stake" in the network) in order to be able to participate in the validation of transactions.

## Signing

Demonstrating cryptographically that a message or transaction was approved by the holder of a specific private key.

## Slashable offenses

If your validator commits a slashable offense it will be force exited from the validator pool and will have ETH deducted depending on the circumstances of the event. Typically, this will be 1-2 ETH but could be [significantly more ↗](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/rewards-and-penalties/#slashing).

This is not something to be overly worried about, there are [simple steps](help/slashing-explained.md) you can take to make sure that you don't invoke a slashing event.

There are three ways a validator can be slashed, all of which amount to the dishonest proposal or attestation of blocks.

### Attestation violation

- **Double voting**: Signing two different [attestations](#attestation) in one [epoch](#epoch).
- **Surround votes**: Attesting to a block that "surrounds" another one (effectively changing history).

### Proposer violation

- **Double block proposal**: [Proposing](#block-proposer) and [signing](#signing) two different [blocks](#block) for the same [slot](#slot).

## Slasher node

The [**slasher**](slasher.md) **is its own entity** but requires a beacon-node to receive [attestations](https://kb.beaconcha.in/glossary#attestation). To find malicious activity by validators, the slashers iterates through all received attestations until a **slashable offense** has been found. Found slashings are broadcasted to the network and the next [block proposer](#block-proposer) adds the proof to the block. The block proposer receives a reward for slashing the malicious validator. However, the whistleblower (Slasher) does not receive a reward.

## Slot

**32 Slots = 1 [Epoch](#epoch)**  
A time period of **12 seconds** in which a randomly chosen validator has time to propose a block. The total number of validators is split up in [committees](#committees) and one or more individual committees are responsible to attest to each slot. One validator from the committee will be chosen to be the aggregator, while the other 127 validators are attesting. After each Epoch, the validators are mixed and merged to new committees. Each slot may or may not have a block in it as a validatory could miss their proposal (e.g. they may be offline or submit their block too late). There is a minimum of 128 validators per committee.

## Source vote

The validator has made a timely vote for the correct source [checkpoint](#checkpoint).

## Sync committee

A sync committee is a randomly selected group of [validators](#validator) that refresh every ~27 hours. Their purpose is to add their [signatures](#signing) to valid block headers. Sync committees allow [light clients](#light-clients) to keep track of the head of the blockchain without needing to access the entire validator set.

## Target vote

The validator has made a timely vote for the correct target [checkpoint](#checkpoints).

## Validator

A node in a [Proof of Stake (Pos)](#proof-of-stake-pos) system responsible for storing data, processing transactions, and adding new blocks to the blockchain. To activate validator software, you need to be able to stake 32 ETH. A validators job is to propose blocks and sign attestations. It has to be online for at least 50% of the time in order to have positive returns.

### Eligible for activation & Estimated activation

Refers to pending validators. The deposit has been recognized by the [Beacon Chain](#beacon-chain) at the timestamp of “Eligible for activation”. If there is a queue of [pending validators ↗](https://www.beaconcha.in/validators), an estimated timestamp for activation is calculated.

### Unique index

Every validator receives a unique index based on when they are added from the [validator queue](#validator-queue).

### Current balance & Effective balance

The current balance is the amount of ETH held by the validator as of now. The **effective Balance** represents a value calculated by the current balance. It is used to determine the size of a reward or penalty a validator receives. The effective balance can never be higher than 32 ETH.

In order to increase the effective balance, the validator requires “effective balance + 1.25 ETH”. In other words, if the effective balance is 20 ETH, a current balance of 21.25 ETH is required in order to have an effective balance of 21 ETH. The effective balance will adjust once it drops by 0.25 below the threshold as seen in the examples above.

Here are examples on how the effective balance changes:

- If the Current balance is 32.00 ETH – the Effective balance is 32.00 ETH.
- If the Current balance dropped from 22 ETH to 21.76 ETH – Effective balance will be **22.00 ETH**.
- If the Current balance increases to 22.25 **and** the effective balance is 21 ETH, the effective balance will increase to 22 ETH.

## Validator Seed Phrase / Mnemonic

The Seed Phrase or Mnemonic is a set of words (usually 12, 18 or 24 words long) used to generate your validator keys. Your mnemonic is the backup for your validator keys and will be the ONLY way to withdraw your ETH when the time comes and no one can help you recover your mnemonic if you lose it.

## Validator lifecycle

#### 1. Deposited

32 ETH has been deposited to the ETH1 deposit-contract and this state will be kept for around 7 hours. This offers security in case the Ethereum chain gets attacked.

#### 2. Pending

Waiting for activation on the [Beacon Chain](#beacon-chain).

Before validators enter the [validator queue](#validator-queue), they need to be voted in by other active validators. This occurs every 4 hours.

#### 3. Active Validator

Currently attesting and proposing blocks.

The validator will stay active until:

- Its balance drops below 16 ETH (ejected).
- Voluntary exit.
- It gets slashed.

#### 4. Slashing Validator

The Validator has been malicious and will be slashed and kicked out of the system.

> A _**Penalty**_ is a negative reward (e.g. for going offline).  
> A _**Slashing**_ is a large penalty (≥ 1/32 of balance at stake) and a forceful exit ... **. - Justin Drake**

#### 5. Exiting Validator

- **Ejected**: The validator balance fell below a threshold and was kicked out by the network.
- **Exited**: Voluntary exit, the withdrawal key holder has the ability to **withdraw** the current balance of the corresponding validator balance.

## Validator queue

The validator queue is a first-in-first-out queue for activating and exiting validators on the [Beacon Chain](#beacon-chain).

- Until 327680 active validators in the network, 4 validators can be activated per [epoch](#epoch). For every (4 \* 16384) = **65536** active validators, the validator **activation rate** goes up by one.
- 5 validators per epoch requires 327680 active validators which translates to 1125 validators per day.
- 6 validators per epoch requires 393216 active validators which translates to 1350 validators per day.
- 7 validators per epoch requires 458752 active validators which translates to 1575 validators per day.
- 8 validators per epoch requires 524288 active validators which translates to 1800 validators per day.
- 9 validators per epoch requires 589824 active validators which translates to 2025 validators per day.
- 10 validators per epoch requires 655360 active validators which translates to 2200 validators per day.
- Amount of activations scales with the amount of active validators and the limit is the active validator set divided by 64.

Exiting validators works in the same way, with the amount of validators that can exit the Beacon Chain per day rate limited to preserve the stability of the network.

## Withdrawal address

An address that can be optionally set when creating a validator key that will be used to withdraw staked ETH. If this address is not set at the time of key creation it can be set at the time of withdrawal instead. For more information on possible reasons for not setting the withdrawal address on key creation, [see our FAQ answer](faq.md#should-i-set-a-withdrawal-address-when-setting-up-my-solo-staking-validator).
