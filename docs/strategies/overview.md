---
title: What are Strategies?
icon: workflow
order: 100
tags: [strategies, DeFi, yield, automation, strategy builder]
---

# What are Strategies?

Strategies are automated DeFi workflows that put your assets to work. Instead of manually lending, borrowing, swapping, and collecting rewards across different protocols, a strategy handles all of this for you.

---

## How strategies work

A strategy is a sequence of DeFi actions that execute automatically based on conditions you define. For example:

1. **Supply** tokens to a lending market
2. **Borrow** against your collateral
3. **Provide liquidity** to a trading pool
4. **Collect rewards** when they accumulate
5. **Rebalance** when market conditions change

All of this happens inside a **strategy vault** — a dedicated smart contract that holds your strategy's assets separately from your main wallet.

---

## Pre-built strategies

Pecunity offers a catalog of tested strategies that you can activate with a few clicks. Each strategy in the catalog shows:

- **Name and description** — what the strategy does
- **Supported tokens** — which tokens you can use
- **Risk level** — how volatile the strategy can be
- **Historical performance** — past returns (not guaranteed)

[!ref Browse the Strategy Catalog](/strategies/strategy-catalog/)

---

![Strategy comparison](/static/strategy-comparison.svg)

## Available strategy types

| Strategy | Description | Min. deposit |
| --- | --- | --- |
| [Leveraged Yield Farming](/strategies/leveraged-yield/) | Borrow to amplify your liquidity position | $100 |
| [Wick and Wait](/strategies/wick-and-wait/) | Provide concentrated liquidity and wait for price wicks | $100 |
| [Delta Neutral](/strategies/delta-neutral/) | Hedge spot with a short perpetual position | $250 |

---

## Protocols used

Strategies are built on top of established DeFi protocols on **BNB Smart Chain**:

- **PancakeSwap v3** — decentralized exchange for swaps and liquidity
- **Aave v3** — lending and borrowing market
- **APX Finance** — perpetual trading

---

## Fees

Each strategy action incurs a small fee. You can reduce fees by paying with $PEC or by locking $PEC tokens.

[!ref Fee Overview](/fees/overview/)
