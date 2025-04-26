# Bitcoin Analytics DAO - Staking & Governance Protocol

## Overview

**Bitcoin Analytics DAO** introduces a decentralized staking and governance system built on the **Stacks blockchain**, aligning with Bitcoin's security principles. This protocol enables STX holders to stake their assets, earn rewards based on commitment levels, and actively participate in governance proposals.

Participants are incentivized through a tiered rewards system, with voting power proportional to their stake and time commitment.

## Key Features

- **Tiered Staking System**  
  Stake STX tokens and ascend through reward tiers based on the total stake amount and lock duration.
  
- **Time-Based Multipliers**  
  Locking tokens for longer periods increases reward rates.

- **Governance Mechanism**  
  Token holders with sufficient voting power can create and vote on proposals that steer the DAO's future.

- **Security Alignment**  
  Built on Stacks, secured by Bitcoin finality through Proof of Transfer (PoX).

- **Emergency and Administrative Controls**  
  Contract owner privileges include pausing/resuming operations and activating emergency modes.

## Technical Specifications

### Token

- **ANALYTICS-TOKEN**: Fungible token (`ANALYTICS-TOKEN`) defined with initial supply `u0`.

### Core Contract Variables

- `contract-paused`: Boolean flag to pause critical operations.
- `emergency-mode`: Boolean flag to enable emergency functionalities.
- `stx-pool`: Accumulated pool of staked STX tokens.

### Staking Mechanics

- **Base Reward Rate**: `5%` annually (`base-reward-rate = u500`)
- **Bonus Rate for Locking**:
  - No lock: 1.0x
  - 1-month lock: 1.25x
  - 2-month lock: 1.5x

- **Minimum Stake**: `1,000,000` microSTX (`1 STX`).

### Tier System

| Tier | Minimum Stake | Reward Multiplier | Features Enabled |
|:----:|:--------------:|:-----------------:|:-----------------:|
| 1    | 1 STX          | 1.00x              | Basic Governance  |
| 2    | 5 STX          | 1.50x              | Extended Features |
| 3    | 10 STX         | 2.00x              | Full Access       |

### Cooldown & Unstaking

- **Cooldown Period**: 24 hours (1440 blocks) before completing an unstake after initiation.

### Governance Parameters

- **Proposal Creation Threshold**: Minimum 1 STX in voting power.
- **Voting Period**: Between 100 and 2880 blocks.
- **Proposal Validity**: Proposals must have at least 10 characters and up to 256 characters.

## Core Functions

### Public Functions

- `initialize-contract`  
  Set up initial tier levels.
  
- `stake-stx (amount, lock-period)`  
  Stake STX tokens with optional lock-up for higher rewards.

- `initiate-unstake (amount)`  
  Start the cooldown period for unstaking.

- `complete-unstake`  
  Complete the unstaking process after the cooldown.

- `create-proposal (description, voting-period)`  
  Submit a new governance proposal.

- `vote-on-proposal (proposal-id, vote-for)`  
  Vote for or against an active proposal.

- `pause-contract` / `resume-contract`  
  Pause or resume staking and proposal operations.

### Read-Only Functions

- `get-contract-owner`
- `get-stx-pool`
- `get-proposal-count`

### Internal (Private) Functions

- `get-tier-info (stake-amount)`  
  Determine tier based on the stake.

- `calculate-lock-multiplier (lock-period)`  
  Reward boost based on lock duration.

- `calculate-rewards (user, blocks)`  
  Reward calculation based on stake, tier, and time.

- `is-valid-description (desc)`  
  Validate proposal descriptions.

- `is-valid-lock-period (lock-period)`  
  Check if lock period is accepted.

- `is-valid-voting-period (period)`  
  Validate voting period duration.

## Error Codes

| Code | Meaning |
|:----:|:--------|
| 1000 | Not authorized |
| 1001 | Invalid protocol parameters |
| 1002 | Invalid amount |
| 1003 | Insufficient STX balance |
| 1004 | Cooldown still active |
| 1005 | No active stake |
| 1006 | Below minimum stake requirement |
| 1007 | Contract is paused |

## Deployment Notes

- Only the `CONTRACT-OWNER` can initialize, pause, or resume the contract.
- Ensure STX token balances are correctly handled when calling staking/unstaking functions.
- Reward rates and tier structures can be adjusted via future protocol upgrades, governed by DAO proposals.

## Security Considerations

- Contract includes an **emergency mode** for critical operations during incidents.
- Follows **best practices** for decentralized governance and staking, including cooldown mechanisms to prevent sudden withdrawals.
