---
title: The Merge
description: What is the Merge?
sidebar_position: 4
---

# The Merge

:::info

The Merge was executed on **September 15, 2022**.

:::

[The Merge](https://ethereum.org/en/upgrades/merge/) was an Ethereum upgrade that merged the [Beacon Chain](https://ethereum.org/en/upgrades/beacon-chain/) into Ethereum Mainnet, turning Mainnet into a combination of an [execution layer and consensus layer](#execution-and-consensus-clients). The Merge transitioned Mainnet from proof of work to [proof of stake consensus](Proof-of-Stake.md).

You can run Teku as a consensus client with:

- Any execution client on Mainnet.
- Any execution client on a testnet.
- Besu on Mainnet.
- Besu on a testnet.

## Execution and consensus clients

After The Merge, a full Ethereum Mainnet node is a combination of an execution client (previously called an [Ethereum 1.0](https://blog.ethereum.org/2022/01/24/the-great-eth2-renaming/) client) and a consensus client (previously called an [Ethereum 2.0](https://blog.ethereum.org/2022/01/24/the-great-eth2-renaming/) client).

Execution and consensus clients communicate with each other using the [Engine API](https://besu.hyperledger.org/en/latest/HowTo/Interact/APIs/Engine-API/).

![Ethereum Merge node](../images/Execution-Consensus-Clients.png)

### Execution clients

Execution clients, such as [Besu](https://besu.hyperledger.org/en/stable/), manage the execution layer, including executing transactions and updating the world state. Execution clients serve [JSON-RPC API](https://besu.hyperledger.org/en/stable/Reference/API-Methods/) requests and communicate with each other in a peer-to-peer network.

### Consensus clients

Consensus clients, such as Teku, contain beacon node and validator client implementations. The beacon node is the primary link to the [Beacon Chain](https://ethereum.org/en/upgrades/beacon-chain/) (consensus layer). The validator client performs [validator duties](Proof-of-Stake.md) on the consensus layer. Consensus clients serve [REST API](../Reference/Rest_API/Rest.md) requests and communicate with each other in a peer-to-peer network.

## What happened during The Merge

Before The Merge, the execution and consensus clients' configurations were updated to listen for a certain total terminal difficulty (TTD) to be reached.

The consensus layer enabled the Merge configuration (Bellatrix) before reaching the TTD. Once the execution layer blocks reached the TTD, the Beacon Chain merged into Ethereum Mainnet, and Ethereum transitioned to a proof of stake network.

:::tip

After The Merge, a Mainnet node operator must run both an execution client and a beacon node at the same time. To become a validator, you must also run a validator client (either [in the same process as the beacon node](../HowTo/Get-Started/Run-Teku.md#start-the-clients-in-a-single-process) or [separately](../HowTo/Get-Started/Run-Teku.md#run-the-clients-separately)).

:::

After The Merge, validators earn rewards for performing [validator duties](Proof-of-Stake.md), and fee recipients earn rewards for the inclusion of execution layer transactions.
