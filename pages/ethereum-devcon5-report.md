---
layout: default
title: Ethereum Devcon 5 Report
category: software developer
toc: true
---

Ethereum 1.0
---------

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

## EVM

* 256-bit Virtual Machine
    * maximun **2<sup>256</sup>** &asymp; **10<sup>77</sup>** addressess(accounts)



## Account State

   | Property | Description | Remarks |
   |----------|-------------|---------|
   | **`nonce`**  | # of tx. sent for EOA or # of contracts created by the account for contract account | scalar value  |
   | **`balance`** | the # of Wei (10<sup>-18</sup> Ether) | scalar value  |
   | **`storageRoot`** | A 256-bit hash of the root node of a Merkle Patricia tree |   |
   | **`codeHash`** | The hash of the EVM code of this account | immutable |

* [Ethereum Yellowpaper ](https://ethereum.github.io/yellowpaper/paper.pdf)

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


Ethereum 2.0
---------

* [The Roadmap to Serenity](https://media.consensys.net/the-roadmap-to-serenity-bc25d5807268)
* [Ethereum 2.0 (Serenity) Phases](https://docs.ethhub.io/ethereum-roadmap/ethereum-2.0/eth-2.0-phases/)
* [State of Ethereum Protocol #1](https://media.consensys.net/state-of-ethereum-protocol-1-d3211dd0f6) (Aug 29, 2018)
* [State of Ethereum Protocol #2: The Beacon Chain](https://media.consensys.net/state-of-ethereum-protocol-2-the-beacon-chain-c6b6a9a69129) (Oct 27, 2018)


![abc](https://miro.medium.com/max/1744/0*ufBJ5S8YX0IGIqng)


Solidity Secure Programming
---------

* [ConsenSys Diligence](https://diligence.consensys.net/)
* [MythX](https://mythx.io/)
* [Mythril](https://github.com/ConsenSys/mythril)
* [VeriSol](https://github.com/microsoft/verisol)
* [Slither](https://github.com/crytic/slither)
* [Securify](https://securify.chainsecurity.com/)
* [VerX](http://verx.ch/)
* [Solhint](https://protofire.github.io/solhint/)


My Schedule
---------

   | Date | Sessions | Speakers | Description | Remarks |
   | ---- | -------- | -------- | ----------- | ------- |
   | 2019/10/08 09:05 | [Formal Verification of Smart Contracts and Protocols: What, Why, How](https://devcon.org/agenda?talk=recON87t6ffg2NFw8) |   |   |   |
   | 2019/10/11 09:10 | [Ethereum 2.0 Phase 1 & 2 Developer Experience](https://devcon.org/agenda?talk=recFhu9NOO4tYsZs4) |   |   |   |