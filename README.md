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


# Treasury Pallet Overview

The Treasury Pallet is designed to manage the funds accumulated by a blockchain project. It handles proposals for spending these funds, including their approval and rejection by the governing body.

## Treasury Pallet Components

### Proposal
A suggestion to allocate funds from the treasury pot to a beneficiary. Proposals are submitted by users and include details such as the amount of funds requested and the beneficiary account.

### Beneficiary
An account that receives funds from a proposal if it is approved by the governance system.

### Deposit
A security bond deposited by the proposer when making a proposal. This deposit may be returned or slashed depending on whether the proposal is accepted or rejected.

### Pot
The unspent funds accumulated by the treasury. These funds are gathered from various sources like transaction fees, inefficiencies, penalties, and other means specified by the blockchain's governance.

## High-Level Features

- **Proposal Creation**: Users can submit proposals to request funds from the treasury.
- **Proposal Rejection/Approval**: Proposals can be approved or rejected by the council or other governance mechanisms.
- **Proposal Tracking**: The system provides mechanisms to track the status and history of proposals.
- **Spending**: Execution of successful proposals leading to disbursement of funds.

## How It Works

### Origins
The ability to interact with the treasury pallet begins with designated origins, typically including governance roles like a council.

### Governance
The governance system, often embodied by a council, plays a critical role in managing proposals. It provides oversight and makes final decisions on fund disbursement.

### Extrinsics
Treasury functionality is exposed through extrinsics that allow users to submit proposals, and for councils to approve, reject, or execute spending.

### Proposal ID
Each proposal is assigned a unique identifier which is used to track its progress through submission, approval, execution, and potential archival.

## Usage in Substrate/Polkadot

1. **Proposal Submission**: A proposer introduces a Treasury proposal through a user interface, specifying the desired amount and the beneficiary.
2. **Collaboration**: Council members and other stakeholders discuss the proposal, often off-chain, to determine its viability and impact.
3. **Motion Creation**: A council member may create a collective motion to propose the approval of the Treasury proposal.
4. **Voting**: Council members vote on the collective motion, either in favor (aye) or against (nay).
5. **Approval Execution**: If the motion passes, the council executes the approval, moving the proposal into the approved state.
6. **Funding Disbursement**: Approved proposals wait in the queue until funds are available in the treasury pot for spending.

These steps ensure that treasury funds are managed transparently and responsibly, aligning with the broader goals of the blockchain ecosystem.




# Blockchain Transaction Handling

## Inflow Transactions

Inflow transactions typically refer to transactions that increase the balance of an account. These can happen through:

### Transfers to an Account
This is managed by the `pallet_balances`, which is a core component for handling tokens in Substrate blockchains. The relevant code isn't explicitly highlighted in the provided script, but it would involve `transfer`, `transfer_keep_alive`, and similar functions that allow tokens to be sent to an account.

### Rewards and Staking
Handled by `pallet_staking` and potentially other modules like `pallet_rewards` or custom implementations that manage distribution of block rewards or other incentives.

### Treasury and Incentive Disbursements
`pallet_treasury` might handle inflow transactions in the form of funding grants or other payouts approved by governance mechanisms.

### Other Inflows
Pallets like `pallet_lottery`, `pallet_vesting`, or `pallet_bounties` could also initiate inflow transactions as they handle payouts and prize distributions.

## Outflow Transactions

Outflow transactions are those that decrease the balance of an account, which can be seen in:

### Payments for Transactions
Handled primarily through the `pallet_transaction_payment`, which deals with fees associated with processing transactions. This includes the deduction of transaction fees from the account initiating a transaction.

### Slashing in Staking
Managed by the `pallet_staking`, where validators or nominators can have their stakes slashed due to misbehavior or security breaches.

### Treasury Funding
Contributions to the treasury through taxation or fees, configured in the runtime (though specific parameters were not detailed in the provided code).

### Other Outflows
Pallets managing specific fund outflows like `pallet_multisig` (for multi-signature account operations), `pallet_proxy` (for proxy-based operations that might involve fee deductions), or custom logic that might involve payments or penalties.

## Code Areas

Here are specific areas in the provided code that handle inflows and outflows:

### Inflows
- Look at methods in `pallet_balances` (for transfers and receipts) and `pallet_staking` (for rewards).

### Outflows
- `pallet_transaction_payment` for transaction fees.
- Slashing logic in `pallet_staking`.
- Fee deductions in `pallet_multisig` and `pallet_proxy`.





