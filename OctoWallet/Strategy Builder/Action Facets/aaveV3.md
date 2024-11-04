---
icon: savings
tags:
  [Aave V3, smart wallet, strategy builder, lending, borrowing, DeFi strategies]
---

# AaveV3Facet

The `AaveV3Facet` contract is designed as a facet that follows the **Diamond Standard (EIP-2535)**. This standard allows a smart contract to be modular and upgradable by integrating different "facets" that encapsulate various functionalities. By adding the `AaveV3Facet` to a **smart contract wallet** that supports the Diamond Standard, users can extend the wallet’s capabilities to include all AAVE V3 interaction functions provided by this facet.

With `AaveV3Facet` in `StrategyBuilder`, users can automate advanced DeFi strategies, optimizing for collateral safety, borrowing opportunities, and efficient debt management. This modular approach enhances risk-adjusted returns and empowers users to build custom, market-responsive strategies

## Functionality Overview:

- **Integration with Smart Contract Wallet**: Once the `AaveV3Facet` is added to the smart contract wallet, all its functions become accessible. This includes core operations such as supplying, borrowing, and repaying assets, as well as specialized functions like borrowing a percentage of available liquidity or maintaining a target health factor.
- **Usage in StrategyBuilder**: The functions in this facet can be leveraged by an automated **StrategyBuilder** that is part of the wallet ecosystem. This enables the creation of complex, automated strategies that utilize AAVE V3’s financial tools, such as automated supply and borrow operations based on predefined conditions.
- **Direct Function Calls**: In addition to automated strategies, any function from `AaveV3Facet` can be called directly if the user or contract has appropriate access to the wallet. This allows for on-demand, flexible interaction with AAVE V3, including actions like supplying or withdrawing specific assets or managing debt positions.

By integrating `AaveV3Facet`, the smart contract wallet gains comprehensive access to AAVE V3’s DeFi capabilities, enhancing both manual and automated financial management.

## Using AaveV3Facet

The **AaveV3Facet** offers efficient and controlled interactions with Aave's protocol, empowering users to create strategies that handle collateralization, debt management, and optimized borrowing. Here's a breakdown of the primary categories and how they can be applied in strategy steps.

### 1. **Asset Supply Functions**

In strategies requiring the deployment of assets as collateral or lending capital, the following supply functions provide robust options for either ERC20 tokens or ETH:

- **Supply Tokens**:
  - Use `supplyAaveV3` to deposit an ERC20 token as collateral in Aave V3.
  - `supplyETHAaveV3` can be used to supply ETH as collateral directly, converting it into wrapped ETH (WETH) and supplying it to the Aave pool.

#### Example Strategy Step:

For a strategy that involves gradual allocation of assets into Aave as collateral, use `supplyAaveV3` as a step when conditions are met.

```solidity
// Strategy Step: Supply an amount of Token A as collateral to Aave V3.
wallet.supplyAaveV3(tokenA, amount);
```

### 2. **Asset Withdrawal Functions**

For strategies involving conditional liquidity retrieval, withdrawal functions allow for either partial or full removal of assets:

- **Withdraw Tokens**:
  - Use `withdrawAaveV3` to redeem an ERC20 asset from Aave's lending pool.
  - `withdrawETHAaveV3` can be utilized to withdraw ETH after unwrapping from WETH, suitable for strategies needing direct ETH access.

#### Example Strategy Step:

A strategy can trigger a withdrawal if the asset allocation reaches a target ratio or market conditions signal a reduction in exposure.

```solidity
// Strategy Step: Withdraw a specified amount of Token A from Aave V3.
wallet.withdrawAaveV3(tokenA, amount);
```

### 3. **Borrowing Functions**

In strategies with a borrowing component, AaveV3Facet enables borrowing of either tokens or ETH directly from the lending pool:

- **Borrow ERC20 Tokens or ETH**:
  - `borrowAaveV3` allows borrowing an ERC20 token, specifying the amount and interest rate mode.
  - `borrowETHAaveV3` handles ETH borrowing by withdrawing WETH and converting it back to ETH.

#### Example Strategy Step:

To automate asset acquisition, a strategy step can borrow when available liquidity or collateralization reaches a specified threshold.

```solidity
// Strategy Step: Borrow Token B at a specified interest rate mode from Aave V3.
wallet.borrowAaveV3(tokenB, amount, interestRateMode);
```

### 4. **Repayment Functions**

For strategies requiring automated debt repayment, AaveV3Facet offers flexible options for repaying ERC20 or ETH loans, either partially or fully:

- **Repay ERC20 Tokens or ETH**:
  - Use `repayAaveV3` to settle debt on an ERC20 loan.
  - `repayETHAaveV3` converts ETH to WETH and applies it toward debt repayment, simplifying ETH debt management.

#### Example Strategy Step:

In a strategy with conditions to lower debt during market dips, a repayment step can prevent liquidation risk by maintaining health factors.

```solidity
// Strategy Step: Repay part of the Token B debt using the specified interest rate mode.
wallet.repayAaveV3(tokenB, amount, interestRateMode);
```

### 5. **Dynamic Borrowing Functions Based on Availability**

The borrowing functions based on a percentage of available funds provide flexible options for strategies needing adjustable borrowing:

- **Borrow Percentage of Available**:
  - Use `borrowPercentageOfAvailableAaveV3` to borrow a percentage of liquidity, ensuring control over portfolio leverage.
  - For ETH borrowing, `borrowPercentageOfAvailableETHAaveV3` automates borrowing based on collateral availability.

#### Example Strategy Step:

A strategy step to borrow 20% of available liquidity, triggered when borrowing conditions are favorable, can dynamically increase or decrease exposure.

```solidity
// Strategy Step: Borrow 20% of available liquidity in Token B.
wallet.borrowPercentageOfAvailableAaveV3(tokenB, 20, interestRateMode);
```

### 6. **Supply Functions to Target Health Factor**

In strategies that aim to reach a specific health factor, functions are provided to calculate and supply the exact collateral needed:

- **Supply to Health Factor**:
  - Use `supplyToHealthFactorAaveV3` to automatically supply enough tokens to reach a target health factor, improving collateral position.
  - `supplyETHToHealthfactorAaveV3` allows ETH-based strategies to ensure target health factors are met using ETH as collateral.

#### Example Strategy Step:

Set a strategy to supply collateral as necessary when the health factor drops below a predefined safe threshold.

```solidity
// Strategy Step: Supply Token A until reaching the target health factor.
wallet.supplyToHealthFactorAaveV3(tokenA, targetHealthFactor);
```

## AaveV3Facet Contract API

### Core AAVE V3 Functions

#### supplyAaveV3

```solidity
function supplyAaveV3(address asset, uint256 amount) external
```

**Description**: Supplies a specified asset to the Aave V3 pool.

- `asset`: The address of the asset to be supplied. (Type: `TokenAddress`)
- `amount`: The amount of the asset to supply. (Type: `TokenAmount`,{token: `asset`})

#### supplyETHAaveV3

```solidity
function supplyETHAaveV3(uint256 amount) external
```

**Description**: Supplies ETH to the Aave V3 pool.

- `amount`: The amount of ETH to supply. (Type: `TokenAmount`,{token: ETH})

#### withdrawAaveV3

```solidity
function withdrawAaveV3(address asset, uint256 amount) external
```

**Description**: Withdraws a specific amount of an asset from the Aave V3 pool.

- `asset`: The address of the asset to be withdrawn. (Type: `TokenAddress`)
- `amount`: The amount of the asset to withdraw. (Type: `TokenAmount`,{token: `asset`})

#### withdrawETHAaveV3

```solidity
function withdrawETHAaveV3(uint256 amount) external
```

**Description**: Withdraws a specific amount of ETH from the Aave V3 pool.

- `amount`: The amount of ETH to withdraw. (Type: `TokenAmount`,{token: ETH})

#### borrowAaveV3

```solidity
function borrowAaveV3(address asset, uint256 amount, uint256 interestRateMode) external
```

**Description**: Borrows a specified amount of an asset from the Aave V3 pool.

- `asset`: The address of the asset to borrow. (Type: `TokenAddress`)
- `amount`: The amount of the asset to borrow. (Type: `TokenAmount`,{token: `asset`})
- `interestRateMode`: The interest rate mode (1 for stable, 2 for variable). Default is 2.

#### borrowETHAaveV3

```solidity
function borrowETHAaveV3(uint256 amount, uint256 interestRateMode) external
```

**Description**: Borrows a specified amount of ETH from the Aave V3 pool.

- `amount`: The amount of ETH to borrow. (Type: `TokenAmount`,{token: ETH})
- `interestRateMode`: The interest rate mode (1 for stable, 2 for variable). Default is 2.

#### repayAaveV3

```solidity
function repayAaveV3(address asset, uint256 amount, uint256 interestRateMode) external
```

**Description**: Repays a borrowed amount of an asset to the Aave V3 pool.

- `asset`: The address of the asset to repay. (Type: `TokenAddress`)
- `amount`: The amount of the asset to repay. (Type: `TokenAmount`,{token: `asset`})
- `interestRateMode`: The interest rate mode used for the loan (1 for stable, 2 for variable). Default is 2.

#### repayETHAaveV3

```solidity
function repayETHAaveV3(uint256 amount, uint256 interestRateMode) external
```

**Description**: Repays a borrowed amount of ETH to the Aave V3 pool.

- `amount`: The amount of ETH to repay. (Type: `TokenAmount`,{token: ETH})
- `interestRateMode`: The interest rate mode used for the loan (1 for stable, 2 for variable). Default is 2.

---

### Special AAVE V3 Functions

#### borrowPercentageOfAvailableAaveV3

```solidity
function borrowPercentageOfAvailableAaveV3(address asset, uint256 percentage, uint256 interestRateMode) external
```

**Description**: Borrows a specified percentage of the available liquidity for a particular asset from the Aave V3 pool.

- `asset`: The address of the asset to borrow. (Type: `TokenAddress`)
- `percentage`: The percentage of the available liquidity to borrow (e.g., 50 for 50%). (Type: `Percentage`,{percentageFactor: 10000})
- `interestRateMode`: The interest rate mode (1 for stable, 2 for variable). Default is 2.

#### borrowPercentageOfAvailableETHAaveV3

```solidity
function borrowPercentageOfAvailableETHAaveV3(uint256 percentage, uint256 interestRateMode) external
```

**Description**: Borrows a specified percentage of the available liquidity for ETH from the Aave V3 pool.

- `percentage`: The percentage of the available liquidity to borrow (e.g., 50 for 50%). (Type: `Percentage`,{percentageFactor: 10000})
- `interestRateMode`: The interest rate mode (1 for stable, 2 for variable). Default is 2.

#### supplyToHealthFactorAaveV3

```solidity
function supplyToHealthFactorAaveV3(address asset, uint256 targetHealthFactor) external
```

**Description**: Supplies an asset to the Aave V3 pool until the target health factor is reached.

- `asset`: The address of the asset to supply. (Type: `TokenAddress`)
- `targetHealthFactor`: The target health factor to achieve (e.g., 1.5 for 150%). (Type: `Decimal`,{decimals: 1e18})

#### supplyETHToHealthfactorAaveV3

```solidity
function supplyETHToHealthfactorAaveV3(uint256 targetHealthFactor) external
```

**Description**: Supplies ETH to the Aave V3 pool until the target health factor is reached.

- `targetHealthFactor`: The target health factor to achieve (e.g., 1.5 for 150%). (Type: `Decimal`,{decimals: 1e18})

---

### External View Functions

#### aaveV3Pool

```solidity
function aaveV3Pool() external view returns (address)
```

**Description**: Returns the address of the Aave V3 pool contract.

#### WETH

```solidity
function WETH() external view returns (address)
```

**Description**: Returns the address of the WETH contract.

#### aaveV3PriceOracle

```solidity
function aaveV3PriceOracle() external view returns (address)
```

**Description**: Returns the address of the Aave V3 price oracle contract.
