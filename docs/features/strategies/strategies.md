---
title: Strategies
icon: database
order: 94
---

Pecunity Strategies are the core product of Pecunity and the primary use case of the platform.  
They enable users to generate returns in the DeFi space. There are two ways to participate:

- **Choose from a selection of pre-built strategies**
- **Create a custom strategy using the Strategy Builder via drag & drop**

Created strategies can also be published by the community and made available to other users.

---

## Types of Strategies

Users can choose from different strategy types, each with its own focus and return profile. Most strategies aim to generate yields and display an expected annual percentage yield (APY).

### Long Strategy

- Invests “long” in one or more tokens
- Benefits from price increases in addition to generated yields

### Short Strategy

- Invests “short” in one or more tokens
- Benefits from falling prices in addition to generated yields

### Delta-Neutral Strategy

- **Goal:** Generate yields while minimizing exposure to price fluctuations of individual tokens
- Remains largely independent of overall market movements

---

## Risk Levels

In addition to type, strategies differ in their risk profiles:

- **Low Risk**

  - Expected return: ~5–10%
  - More conservative strategies

- **Medium Risk**

  - Expected return: up to ~20%

- **High Risk**
  - Expected return: above 20%
  - Highly speculative, should be used with caution

---

## Example Strategies Available at Launch

| Strategy Name                            | Description                                                                                                                                                                                                                                                                                  | Est. APY | Risk   |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------ |
| **USD Yield Booster**                    | Uses the decentralized perpetual exchange GMX on Arbitrum and Avalanche to provide liquidity with USDC and generate returns. ETH price risk is neutralized through hedging on Aave. Designed to achieve a delta-neutral yield primarily from trading fees, liquidations, and borrowing fees. | 18–21%   | Medium |
| **Leverage-Yield with sUSDE**            | Combines stablecoin lending with leverage on Aave. The goal is to boost sUSDE yields by reusing collateral multiple times. Investors benefit from a compounding effect without having to add capital manually.                                                                               | 22%      | Medium |
| **Leverage-Yield with sUSDE (_E-Mode_)** | Combines stablecoin lending with leverage on Aave with active E-Mode. The goal is to boost sUSDE yields by reusing collateral multiple times. Investors benefit from a compounding effect without having to add capital manually.                                                            | 55%      | High   |
| **Liquidity Provision with Hedging**     | Combines liquidity provision (LP) on Uniswap V3 with a hedging mechanism via perpetuals (e.g., on GMX, dYdX, or Perpetual Protocol). Designed to earn trading fees from LPing while mitigating impermanent loss with a hedge position.                                                       | 22–28%   | Medium |
| **ETH-Backed Leveraged LP Strategy**     | This strategy has a LONG exposure on ETH and use ETH as collateral on Aave V3 to borrow stablecoins, which are paired with ETH in a concentrated Uniswap V3 liquidity pool.                                                                                                                  | 20%      | Medium |

Additional strategies will be continuously made available, allowing users to select the ones that best match their **risk profile** and **yield expectations** across selected tokens.

[!ref Leverage-Yield Strategy with sUSDE on Aave](./strategy_products/leveraged_yield_strategy_sUSDE.md)

[!ref ETH-Backed Leveraged LP Strategy](./strategy_products/eth_backed_leveraged_lp.md)

---

## Fee Model & $PEC Token

Each strategy execution incurs a fee, based on the transaction volume.  
The utility token **$PEC** directly benefits from these fees:

- If fees are paid in tokens such as **USDT** or **USDC**, a portion is converted into **$PEC** and then **burned**.
- If fees are paid directly in **$PEC**, the fee is lower , but a small portion is still burned.

This ensures that with every strategy execution, the **value and scarcity of $PEC** are continuously supported in the long term.
