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

```
impl my_pallet::Config for Runtime {
    type TreasuryFee = Permill::from_percent(2);
}

```

### 2. Use the Parameter in Pallet Logic:
```
let fee = 1000;
let treasury_part = T::TreasuryFee::get() * fee;

```

**Dependencies**

- Token Pallet: Manages token properties and interactions with the Treasury.
- Governance Pallet (Optional): Facilitates voting on treasury-related proposals.



