---
layout: default
title: Ethereum Devcon 5 Report
category: software developer
toc: true
---

# Ethereum 1.0

## Mainnet Statistics

   | Item | Value | Remarks |
   |------|-------|---------|
   | Started At |  2015-06-30  | > 51 Months |
   | Node Number  | ~ 7,000 | 2019 |
   | Account Number | > 77,000,000 | 2019/10/13 |
   | Block Number | > 8,700,000 | 2019/10/15 |
   | Block Time   | ~ 13 sec    | 2019/10/15 |
   | Transaction Number | > 560,000,000 | 2019/10/15 |
   | Throughput   | ~ 9 TPS     | 2019/10/15 |
   | Full Node Size | ~ 200 MB  | Current State + All Txs |
   | Archive Node Size | ~ 3.2 TB | Current State + All Txs + Snapshots |

* [Etherscan](https://etherscan.io/)
    * [Etherscan > Charts & Stats](https://etherscan.io/charts)
    * [Ethereum Genesis Block](https://etherscan.io/block/0)
    * [Ethereum Unique Address Growth Rate](https://etherscan.io/chart/address)
* [Bitcoin Overtakes Ethereum in Node Numbers](https://www.trustnodes.com/2019/01/09/bitcoin-overtakes-ethereum-in-node-numbers) (January 9, 2019)
* [Are Ethereum Full Nodes Really Full? An Experiment.](https://medium.com/@marcandrdumas/are-ethereum-full-nodes-really-full-an-experiment-b77acd086ca7)
* [Where is the state data stored?](https://ethereum.stackexchange.com/questions/359/where-is-the-state-data-stored) (Jan 22 '16)

## Hard Forks

* Hard Forks (Network Upgrade) : a change to the underlying Ethereum protocol, creating new rules to improve the system.

   | Fork | When | Block # | Description | Remarks |
   |------|------|---------|-------------|---------|
   | **Olympic** | 2015-05-09 | N/A | The final PoC testnet |   |
   | **Frontier** | 2015-06-30 | 0 | The 1st official public mainnet | Block Reward, Gas  |
   | **Homestead** | 2016-03-14 | 1,150,000 | The first planned hard fork | Solidity, Mist wallet |
   | **DAO Fork** | 2016-06-20 | 1,192,000 | To restore $50 million stolen in DAO hack | Ethereum Class |
   | **Metropolis: Byzantium** | 2017-10-16 | 4,370,000 | EIP 100, EIP 658, EIP 649 |   |
   | **Metropolis: Constantinople and St. Petersburg** | 2019-02-28 | 7,280,000 | Smart Contract Verification, State Channel, Block Reward Reduction (3ETH -> 2ETH) |   |
   | **Istanbul** | Oct 2019 |   | PoW -> ProPoW | planned |
   | **Serenity** | 2020 ~   |   | PoS, Beacon Chain, Shard Chains, eWASM | Ethereum 2.0, planned, phased (0/1/2) |

* [A Short History of Ethereum](https://media.consensys.net/a-short-history-of-ethereum-a8fdc5b4362c)
* [The Constantinople Upgrade: What You Need to Know](https://media.consensys.net/the-constantinople-hard-fork-what-you-need-to-know-d438a91dec3f)
* [Ethereum Constantinople/St. Petersburg Upgrade Announcement](https://blog.ethereum.org/2019/02/22/ethereum-constantinople-st-petersburg-upgrade-announcement/)(February 22, 2019)
* [Byzantium HF Announcement](https://blog.ethereum.org/2017/10/12/byzantium-hf-announcement/)(October 12, 2017)
* [Hard Fork No. 4: Spurious Dragon](https://blog.ethereum.org/2016/11/18/hard-fork-no-4-spurious-dragon/)(November 18, 2016)
* [Istanbul Hard Fork — Everything you need to know](https://medium.com/nuo-news/istanbul-hard-fork-everything-you-need-to-know-about-it-3a29738934e5)

## EVM

* 256-bit Virtual Machine
    * maximun **2<sup>256</sup>** &asymp; **10<sup>77</sup>** addressess(accounts)

### Account State

   | Property | Description | Remarks |
   |----------|-------------|---------|
   | **`nonce`**  | # of tx. sent for EOA or # of contracts created by the account for contract account | scalar value  |
   | **`balance`** | the # of Wei (10<sup>-18</sup> Ether) | scalar value  |
   | **`storageRoot`** | A 256-bit hash of the root node of a Merkle Patricia tree |   |
   | **`codeHash`** | The hash of the EVM code of this account | immutable |

* [Ethereum Yellowpaper ](https://ethereum.github.io/yellowpaper/paper.pdf)


# Ethereum 2.0

* [The Roadmap to Serenity](https://media.consensys.net/the-roadmap-to-serenity-bc25d5807268)
* [Ethereum 2.0 (Serenity) Phases](https://docs.ethhub.io/ethereum-roadmap/ethereum-2.0/eth-2.0-phases/)
* [State of Ethereum Protocol #1](https://media.consensys.net/state-of-ethereum-protocol-1-d3211dd0f6) (Aug 29, 2018)
* [State of Ethereum Protocol #2: The Beacon Chain](https://media.consensys.net/state-of-ethereum-protocol-2-the-beacon-chain-c6b6a9a69129) (Oct 27, 2018)


<img src="https://miro.medium.com/max/1744/0*ufBJ5S8YX0IGIqng" alt="Ethereum 2.0 Architecture" width=1024 border=1 style="border-width:1"/>

## Key Concepts

### Becon Chain

* Key function (from "[State of Ethereum Protocol #2: The Beacon Chain](https://media.consensys.net/state-of-ethereum-protocol-2-the-beacon-chain-c6b6a9a69129)")
    * Manage the PoS protocol for itself and all of the shard chains
        * Managing validators and their stakes
        * Nominating the chosen block proposer for each shard at each step
        * Organising validators into committees to vote on the proposed blocks
        * Applying the consensus rules
        * Applying rewards and penalties to validators

* Functions (from "[Ethereum 2.0: A Complete Guide. Scaling Ethereum — Part Two: Sharding.](https://medium.com/chainsafe-systems/ethereum-2-0-a-complete-guide-scaling-ethereum-part-two-sharding-902370ac3be)")
    * Manages the validator registry.
    * Provides the randomness (through a RANDAO and later a VDF)
        * RANDAO : provide unpredictability and unstoppability
        * VDF : provide unbiasability
    * Provides finality through casper FFG
    * Keeps track of shard cross-links
    * Stores execution environment contracts (In phase 2)

### Casper

* For each proposed block, validators stake a portion of their coins on a block which they think will be a valid addition to the chain.
* If that block gets appended, it will be added to the chain and any validator who bet on it will be granted an award proportional to their bet. Since there is no block award in a PoS system, validators are rewarded with transaction fees.
* A validator must not publish two distinct votes for the same target height.
* A validator must not vote within the span of its other votes.
* Any validator who violates either of these commandments is considered malignant, and their entire deposit (not just their stake) is slashed.
* Validators who’s nodes go offline are also punished in order to increase availability and reduce censorship of transactions.

* For each block, an active validator is selected to propose a new block and the other active validators must all vote on its validity.
* Attestations can be votes for both a shard block as well as proof of stake votes for a block on the beacon chain.
* Once enough attestations for each shard block are collected, the block is appended and a crosslink to the beacon chain is created to keep it up to date.

* A validator is an entity (person or company) who can propose new blocks for the blockchain, or someone who confirms another validator's proposal.
* A validator who proposes is called a proposer, while a validator who validates the proposal is called an attester.
* When some validators are picked to be the committee, they are responsible for attesting to a state, i.e. building the next block.
* One __slot__ means one proposal of a block and attestations from other validators that this block is A-OK.
* A set of slots during which all the randomly picked validators have had the opportunity to make an attestation is called a __cycle__.

#### Casper FFG (Casper the Friendly Finality Gadget)
#### Casper CBC (Casper the Friendly GHOST: Correct-by-Construction)

#### Validator

* Anyone who wishes to be a validator must make a deposit of exactly 32 Eth.
* The deposit will generate a receipt and will likely contain an ID that indicates the shard to which the validator is assigned
* The node will become a pending validator on the beacon chain.
* After a period of time, they will become an active validator who can participate in the PoS protocol.
* Active validators will take turns to propose new blocks and vote on their validity.

### Crosslink

* Periodically, the current state (the “combined data root”) of each shard gets recorded in a Beacon Chain block as a crosslink.
* When the Beacon Chain block has been finalised, the corresponding shard block is considered finalised
* A set of signatures from a committee attesting to a block in a shard chain, which can be included into the Beacon Chain.
* The main means by which the Beacon Chain "learns about" the updated state of shard chains.

### Sharding

* User accounts will all be specific to a certain shard.
* Transactions within shards will be relatively simple and similar to Eth 1.0 transactions, while transactions between shards will require an added layer of complexity.
* In order to insure efficiency and security, interactions between individual shards need to follow a special protocol.
* Sharding would increase Ethereum’s throughput and network speed by a factor of over 1000. The addition of layer two solutions such as zk-rollups and plasma will increase this number still further.

### Double Verification Process

* Validators are periodically and randomly assigned to a shard.
* Validators vote on the validity of each transaction package.
* A separate committee on the beacon chain must verify this vote using a sharding manager smart contract.
* If the second vote also passes, the transaction package will be appended to the mainchain and become part of the public ledger, establishing an immutable cross-link to the transaction group on that shard.
* After a certain period, the validators on each shard are relieved of their duty and return to a larger pool. They are replaced by new validators drawn from this same pool and randomly selected by the beacon chain.

### Plasma Chains

* Plasma chains are __semi-independent blockchains__ that can be registered to, and commit transactions on, the Ethereum mainchain.
* Plasma chains use a tool called MapReduce in conjunction with a merkle trie construction to facilitate quick and easy fraud verification in the event of a Byzantine component (bad actor)
* Plasma chains even include a “roll-back” feature that will be activated if a “bonded truth of spend” request fails due to a fraud attempt by a maleficent actor.
* Eth 2.0 will feature both solutions so users will be able to choose whether to use the sharded mainnet, a plasma chain, or both, depending on their unique individual needs and preferences.

# Solidity Secure Programming

* [ConsenSys Diligence](https://diligence.consensys.net/)
* [MythX](https://mythx.io/)
* [Mythril](https://github.com/ConsenSys/mythril)
* [VeriSol](https://github.com/microsoft/verisol)
* [Slither](https://github.com/crytic/slither)
* [Securify](https://securify.chainsecurity.com/)
* [VerX](http://verx.ch/)
* [Solhint](https://protofire.github.io/solhint/)


# My Schedule

   | Date | Sessions | Speakers | Description | Remarks |
   | ---- | -------- | -------- | ----------- | ------- |
   | 2019/10/08 09:05 | [Formal Verification of Smart Contracts and Protocols: What, Why, How](https://devcon.org/agenda?talk=recON87t6ffg2NFw8) |   |   |   |
   | 2019/10/11 09:10 | [Ethereum 2.0 Phase 1 & 2 Developer Experience](https://devcon.org/agenda?talk=recFhu9NOO4tYsZs4) |   |   |   |
