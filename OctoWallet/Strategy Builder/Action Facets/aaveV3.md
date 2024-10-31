---
icon: savings
tags:
  [Aave V3, smart wallet, strategy builder, lending, borrowing, DeFi strategies]
---

# AaveV3Facet

The **AaveV3Facet** in the **StrategyBuilder** enables sophisticated strategies leveraging Aave V3's lending and borrowing functionalities. This facet allows automated management of assets by integrating liquidity provision, withdrawals, and dynamic borrowing or repayment directly from a strategy contract, automating complex actions like maintaining target health factors or borrowing according to available liquidity. Below is a comprehensive guide on how each function can be utilized within a strategy in the **StrategyBuilder**.

## Using AaveV3Facet in StrategyBuilder

The **AaveV3Facet** offers efficient and controlled interactions with Aave's protocol, empowering users to create strategies that handle collateralization, debt management, and optimized borrowing. Here's a breakdown of the primary categories and how they can be applied in strategy steps.

### 1. **Asset Supply Functions**

In strategies requiring the deployment of assets as collateral or lending capital, the following supply functions provide robust options for either ERC20 tokens or ETH:

- **Supply Tokens**:
  - Use `supply` to deposit an ERC20 token as collateral in Aave V3.
  - `supplyETH` can be used to supply ETH as collateral directly, converting it into wrapped ETH (WETH) and supplying it to the Aave pool.

#### Example Strategy Step:

For a strategy that involves gradual allocation of assets into Aave as collateral, use `supply` as a step when conditions are met.

```solidity
// Strategy Step: Supply an amount of Token A as collateral to Aave V3.
AaveV3Facet.supply(tokenA, amount);
```

### 2. **Asset Withdrawal Functions**

For strategies involving conditional liquidity retrieval, withdrawal functions allow for either partial or full removal of assets:

- **Withdraw Tokens**:
  - Use `withdraw` to redeem an ERC20 asset from Aave's lending pool.
  - `withdrawETH` can be utilized to withdraw ETH after unwrapping from WETH, suitable for strategies needing direct ETH access.

#### Example Strategy Step:

A strategy can trigger a withdrawal if the asset allocation reaches a target ratio or market conditions signal a reduction in exposure.

```solidity
// Strategy Step: Withdraw a specified amount of Token A from Aave V3.
AaveV3Facet.withdraw(tokenA, amount);
```

### 3. **Borrowing Functions**

In strategies with a borrowing component, AaveV3Facet enables borrowing of either tokens or ETH directly from the lending pool:

- **Borrow ERC20 Tokens or ETH**:
  - `borrow` allows borrowing an ERC20 token, specifying the amount and interest rate mode.
  - `borrowETH` handles ETH borrowing by withdrawing WETH and converting it back to ETH.

#### Example Strategy Step:

To automate asset acquisition, a strategy step can borrow when available liquidity or collateralization reaches a specified threshold.

```solidity
// Strategy Step: Borrow Token B at a specified interest rate mode from Aave V3.
AaveV3Facet.borrow(tokenB, amount, interestRateMode);
```

### 4. **Repayment Functions**

For strategies requiring automated debt repayment, AaveV3Facet offers flexible options for repaying ERC20 or ETH loans, either partially or fully:

- **Repay ERC20 Tokens or ETH**:
  - Use `repay` to settle debt on an ERC20 loan.
  - `repayETH` converts ETH to WETH and applies it toward debt repayment, simplifying ETH debt management.

#### Example Strategy Step:

In a strategy with conditions to lower debt during market dips, a repayment step can prevent liquidation risk by maintaining health factors.

```solidity
// Strategy Step: Repay part of the Token B debt using the specified interest rate mode.
AaveV3Facet.repay(tokenB, amount, interestRateMode);
```

### 5. **Dynamic Borrowing Functions Based on Availability**

The borrowing functions based on a percentage of available funds provide flexible options for strategies needing adjustable borrowing:

- **Borrow Percentage of Available**:
  - Use `borrowPercentageOfAvailable` to borrow a percentage of liquidity, ensuring control over portfolio leverage.
  - For ETH borrowing, `borrowPercentageOfAvailableETH` automates borrowing based on collateral availability.

#### Example Strategy Step:

A strategy step to borrow 20% of available liquidity, triggered when borrowing conditions are favorable, can dynamically increase or decrease exposure.

```solidity
// Strategy Step: Borrow 20% of available liquidity in Token B.
AaveV3Facet.borrowPercentageOfAvailable(tokenB, 20, interestRateMode);
```

### 6. **Supply Functions to Target Health Factor**

In strategies that aim to reach a specific health factor, functions are provided to calculate and supply the exact collateral needed:

- **Supply to Health Factor**:
  - Use `supplyToHealthFactor` to automatically supply enough tokens to reach a target health factor, improving collateral position.
  - `supplyETHToHealthFactor` allows ETH-based strategies to ensure target health factors are met using ETH as collateral.

#### Example Strategy Step:

Set a strategy to supply collateral as necessary when the health factor drops below a predefined safe threshold.

```solidity
// Strategy Step: Supply Token A until reaching the target health factor.
AaveV3Facet.supplyToHealthFactor(tokenA, targetHealthFactor);
```

## Implementing AaveV3Facet in StrategyBuilder

To effectively use **AaveV3Facet** in **StrategyBuilder**, each function can be represented as a **Strategy Step** with specific conditions for triggering collateral, borrowing, or repayment actions based on defined market criteria. Here’s an example strategy combining multiple functionalities:

1. **Supply Token A** as collateral when its price meets a favorable threshold.
2. **Borrow Token B** for investment purposes when collateral is sufficient.
3. **Repay Token B** debt if a profit condition is reached or to reduce exposure.
4. **Supply Additional Collateral** if health factors indicate risk.

With **AaveV3Facet** in **StrategyBuilder**, users can automate advanced DeFi strategies, optimizing for collateral safety, borrowing opportunities, and efficient debt management. This modular approach enhances risk-adjusted returns and empowers users to build custom, market-responsive strategies.
