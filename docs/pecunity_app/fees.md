---
title: Fees
icon: person
order: 96
---

We currently charge fees for the following features:

## 1. Marketplace Fees

For every NFT trade on our marketplace, a **2% fee** is applied.

## 2. Strategy Execution Fees

When executing a strategy, an individual fee is charged. The fee depends on the structure of the strategy and the specific actions performed.

Examples:

- **Supplying on the Lending Market:** relatively low fee
- **Rewards Collection:** higher fee

The **total fee** for a strategy is the sum of the fees for each individual action and must be paid upon executing the strategy.

### Payment with PEC (Platform Token)

If the total fee is paid in PEC, it is distributed as follows:

- **25%** burned
- **15%** to the Strategy Creator
- **20%** to the Automation Server
- **40%** to the Development Company

### Payment with Other Tokens

If the total fee is paid with other tokens, it is distributed as follows:

- **35%** burned in the form of $PEC
- **15%** to the Strategy Creator
- **20%** to the Automation Server
- **30%** to the Development Company

# Action Fees

Each action has a type and a corresponding **minimum fee** that must be paid, regardless of the percentage fee.

| Action                                | Type     | Fee  | Minimum Fee |
| ------------------------------------- | -------- | ---- | ----------- |
| Swap                                  | Deposit  | 0.3% | $0.40       |
| Supply MoneyMarket                    | Deposit  | 0.1% | $0.40       |
| Withdraw MoneyMarket                  | Withdraw | 0.2% | $0.70       |
| Provide Liquidity                     | Deposit  | 0.1% | $0.40       |
| Decrease Liquidity                    | Withdraw | 0.1% | $0.70       |
| Collect Fees from Providing Liquidity | Reward   | 10%  | $1.00       |
