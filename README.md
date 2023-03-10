# Optimism On-Chain Governance

This repo conatins the code for the Optimism On-Chain Governance system.

## Motivation

Optimism is moving towards on-chain governance process. The initial version on the governance system is used for signling purposes only and is designed to be upgraded in the future.

## Overview

The governance contract is deployed behind an upgradeable proxy. The proxy is owned by the admin address, which is currently [the Optimism multisig](https://optimistic.etherscan.io/address/0x2501c477D0A35545a387Aa4A3EEe4292A9a8B3F0). The admin address can upgrade the implementation to a new version, and transfer ownership to a new address.

The core governor implementation is based on OpenZeppelin's [Governor](https://docs.openzeppelin.com/contracts/4.x/api/governance) contract.

The contract has been modified to support the following features:

- Quorum is calculation includes `for`, `against`, and `abstain` votes
- When executed, proposals don't perform any on-chain actions
- A special manager address has certain superpowers (see below)

## Roles

The contracts in this repo are designed to be used by the following roles:

- **admin** is the top-level owner of the governance system. The admin address can only do the following:

  - Upgrade implementation to a new version
  - Transfer admin role to a new address (inlcuding renouncing ownership)
  - Can't have any other roles (i.e. can't be a manager and can't interact with the governance system in any other way)

- **manager** address used in day-to-day operations of the governance system. The manager address has the following superpowers:

  - Create proposals
  - Mark passed proposals as executed (without performing on-chain actions)
  - Update any proposal's `endBlock` (to extend the voting period)
  - Update voting delay (number of blocks before a proposal can be voted on)
  - Update voting period (number of blocks a proposal can be voted on)
  - Update proposal threshold (number of votes required to create a proposal)
  - Update quorum (number of votes required to pass a proposal, snapshotted at the time of proposal creation)

- **voter** is any address that has OP tokens delegated to it

  - Vote on proposals
  - Delegate votes to another address (via OP token contract)

## Deployment

- [`0xcDF27F107725988f2261Ce2256bDfCdE8B382B10`](https://optimistic.etherscan.io/address/0xcdf27f107725988f2261ce2256bdfcde8b382b10) - Optimism Governance Proxy
- [`0x9d66365f4910c522f937c9e6dd4a192f6de89636`](https://optimistic.etherscan.io/address/0x9d66365f4910c522f937c9e6dd4a192f6de89636) - Implementation (v1)

## See Also

- [OPerating Manual of the Optimism Collective](https://github.com/ethereum-optimism/OPerating-manual)
- [Agora](https://twitter.com/nounsagora)
