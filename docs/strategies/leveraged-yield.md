---
title: Leveraged Yield Farming
icon: graph
order: 90
tags: [leveraged yield farming, LYF, leverage, yield, Aave, PancakeSwap, liquidity]
---

# Leveraged Yield Farming

Leveraged Yield Farming (LYF) amplifies your returns by borrowing additional tokens to increase your liquidity position. This strategy combines lending (Aave v3) with liquidity provision (PancakeSwap v3).

---

![Leveraged Yield Farming flow](/static/lyf-strategy-flow.svg)

## How it works

1. **Supply** your chosen base token (e.g., BNB) to Aave v3 as collateral
2. **Borrow** a quote token (e.g., USDT) against your collateral
3. **Provide liquidity** with both tokens on PancakeSwap v3
4. **Collect rewards** — trading fees and farming rewards accumulate automatically
5. **Rebalance** — when the price moves out of range, the strategy repositions your liquidity
6. **Monitor health factor** — the strategy watches your Aave position to avoid liquidation

---

## Setup steps

### Step 1: Choose your base token

Select which token you want to leverage:

| Token | Network |
| --- | --- |
| USDT | BNB Smart Chain |
| BNB | BNB Smart Chain |
| BTC | BNB Smart Chain |
| ETH | BNB Smart Chain |

**Minimum deposit:** $100

### Step 2: Choose your quote token

Select the token to pair with your base token for the liquidity pool. Available options depend on your base token choice.

### Step 3: Choose your risk level

| Risk level | Description |
| --- | --- |
| **Low** | Lower volatility with steadier performance |
| **Medium** | Balanced risk and reward |
| **Mid-High** | Higher volatility, higher potential swings |
| **High** | Highest volatility and risk |

Higher risk means more aggressive borrowing and tighter liquidity ranges, which can lead to higher returns but also increases the chance of liquidation or impermanent loss.

---

## What to know

!!!warning
Leveraged positions carry risk. If the price of your collateral drops significantly, your Aave position may be liquidated. The strategy monitors your health factor and attempts to rebalance, but extreme market movements can still cause losses.
!!!

- The strategy automatically collects LP rewards from PancakeSwap's MasterChef
- A portion of rewards is used to pay the automation fee
- Your funds are held in a dedicated vault, separate from your main wallet
