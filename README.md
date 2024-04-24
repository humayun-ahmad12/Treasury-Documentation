# Treasury Setup

The Treasury pallet provides a designated pool of funds within the blockchain, allowing stakeholders, such as token holders, to contribute. It facilitates proposing, approving, and spending these funds for activities related to blockchain enhancement.

## Overview

The Treasury pallet is a component of the Substrate blockchain framework, designed to manage the funds collected by the blockchain. These can be from transaction fees, fines (e.g., from slashing), inefficiencies, or other surpluses.

It serves to manage funds accumulated through transaction fees, slashes, or other means, and can be utilized to fund community proposals, developer work, and more.

### Key Features

- **Community-driven decisions**: Empowers communities to make collective financial decisions.
- **Transparency**: Visible treasury balance and fund usage.
- **Flexibility**: Supports a variety of community beneficial projects.

## Use Cases

- **Community Funding**: Addresses the lack of built-in mechanisms for fund management in traditional blockchains.
- **Democratic Decision-Making**: Stakeholders propose and vote on fund utilization, ensuring community involvement.
- **Transparency and Security**: Blockchain recording of all transactions ensures transparency and reduces misuse risks.
- **Flexibility and Long-Term Growth**: Supports diverse initiatives promoting sustainable development.
- **Incentivization**: Encourages community and developer engagement through grants and rewards.

## Treasury Inflow and Outflow

**Inflows**
- **Transaction Fees**: 80% of transaction fees go to the Treasury.
- **Staking Inefficiencies**: Include funds from staking inefficiencies.
- **Slashes**: Funds from slashing malpractices.
- **Transfers**: General fund transfers into the Treasury.

**Outflows**
- **Burned Tokens**: 1% of available funds are burned each spend period.
- **Proposals & Bounties**: Major share of outflows, approved by governance, disbursed post spend period.

## Relation to Tokenomics

The treasury pallet is integral to the blockchain's tokenomics, influencing token distribution, supply and demand dynamics.

## Integration in Blockchain

### Step 1: Adding the Treasury Pallet

Modify `Cargo.toml`:

```toml
pallet-treasury = { version = '3.0.0', default-features = false, features = ['runtime-benchmarks'] }
```

### Step 2: Update Runtime Configuration

Import and configure in `runtime/src/lib.rs`:

```rs
pub use pallet_treasury;
construct_runtime!(
    Treasury: pallet_treasury::{Pallet, Call, Storage, Config, Event<T>},
);
```

### 1. Set the Value in Runtime Configuration:

```rs
impl my_pallet::Config for Runtime {
    type TreasuryFee = Permill::from_percent(2);
}

```

### 2. Use the Parameter in Pallet Logic:
```rs
let fee = 1000;
let treasury_part = T::TreasuryFee::get() * fee;

```

**Various pallet dependencies on the Treasury**

| Pallet                 | Dependency Type                    | Description                                                                                           |
|------------------------|------------------------------------|-------------------------------------------------------------------------------------------------------|
| `pallet_bounties`      | Direct Dependency                  | Uses Treasury funds to manage bounties, including funding and slashing.                               |
| `pallet_tips`          | Direct Dependency                  | Uses Treasury for tips management, including funding for tips and their payouts.                      |
| `pallet_treasury`      | Self                               | Core functionality of managing funds, spending proposals, and revenue.                                |
| `pallet_staking`       | Interaction through Events/Callbacks| May interact for slashing and rewards, which could be redirected or impacted by Treasury.             |
| `pallet_alliance`      | Funds Management                   | Manages deposits and potentially uses funds for managing alliances.                                   |
| `pallet_child_bounties`| Direct Dependency                  | Manages child bounties funding through the Treasury.                                                  |
| `pallet_society`       | Indirect Dependency                | Manages funds that might interact with the Treasury for rewards and slashes.                          |
| `pallet_identity`      | Interaction for Slashing           | Uses Treasury as a destination for slashed funds in identity mismanagement.                           |
| `DealWithFees struct`  | Fee Redistribution                 | Configured to split transaction fees, sending a portion to the Treasury.                              |





