---
icon: issue-reopened
tags: [smart wallet, smartwallet, octo wallet, octodefi wallet, action facet]
---

# Uniswap V2 Facet

To use the **UniswapV2Facet** in the **StrategyBuilder** contract, you can integrate various token swap and liquidity functions from Uniswap V2 as steps in a strategy. This enables the creation of automated trading strategies that can perform token swaps, liquidity provision, and liquidity removal based on specified conditions, as well as percentage-based swaps and liquidity management. Below is a detailed guide on how each category of functionality can be utilized within a strategy in the **StrategyBuilder**.

## Using UniswapV2Facet in StrategyBuilder

The **UniswapV2Facet** provides flexibility in building complex strategies for DeFi operations, particularly for asset swapping and liquidity pool interactions. Here’s how each type of function can be implemented as steps in a strategy:

### 1. **Swaps for Token and ETH Interactions**

In strategies that involve trading tokens or converting between ETH and tokens, swap functions allow a variety of operations:

- **Token-to-Token Swaps**:

  - Use `swapExactTokensForTokens` to swap a fixed amount of one token for as many of another token as possible.
  - If the goal is to obtain an exact amount of output tokens, `swapTokensForExactTokens` can be used, setting a limit on how many input tokens can be spent.

- **ETH Swaps**:
  - Convert ETH into tokens using `swapExactETHForTokens`, ideal for strategies where ETH is converted into another asset.
  - Alternatively, swap for an exact amount of tokens with `swapETHForExactTokens`.
  - For the reverse (i.e., swapping tokens to ETH), use `swapExactTokensForETH` or `swapTokensForExactETH` depending on whether a fixed input or exact output is desired.

#### Example Strategy Step:

In a strategy, add a swap step where ETH is converted to tokens upon meeting a specific condition (e.g., market price threshold). This can help automate buy-ins or sell-offs.

```solidity
// Strategy Step: Swap 1 ETH for the maximum possible amount of a token, with a minimum acceptable output.
UniswapV2Facet.swapExactETHForTokens(1 ether, minTokenOut, path);
```

### 2. **Percentage-Based Swap Functions**

If a strategy is dynamically based on portfolio allocation, percentage-based swaps allow for adjusting holdings according to predefined percentages.

- **Percentage Token Swap**:
  - `swapPercentageTokensForTokens` swaps a specified percentage of a token to another token.
  - `swapPercentageTokensForETH` or `swapPercentageETHForTokens` can help strategies maintain a balanced portfolio allocation in ETH and tokens, adjusting to maintain target ratios.

#### Example Strategy Step:

For a strategy that periodically rebalances a portfolio, add a step to swap 10% of an existing token for another, ensuring the allocation is met.

```solidity
// Strategy Step: Swap 10% of a token holding to ETH.
UniswapV2Facet.swapPercentageTokensForETH(10, path);
```

### 3. **Liquidity Pool Management**

Providing and managing liquidity is a crucial part of many DeFi strategies, and UniswapV2Facet enables flexible liquidity operations.

- **Adding Liquidity**:

  - Use `addLiquidity` or `addLiquidityETH` to contribute tokens and/or ETH to Uniswap pools, generating LP tokens.
  - Percentage-based functions like `addLiquidityPercentage` or `addLiquidityETHPercentage` can help you add liquidity while keeping a portion of the original tokens or ETH balance intact.

- **Removing Liquidity**:
  - Use `removeLiquidity` and `removeLiquidityETH` for redeeming LP tokens and withdrawing the underlying assets.
  - `removeLiquidityPercentage` and `removeLiquidityETHPercentage` allow for withdrawing only a specified percentage, which can be useful in gradual exit strategies.

#### Example Strategy Step:

For strategies that aim to accumulate LP tokens as prices fluctuate, a step can be added to automatically provide liquidity when a target token ratio is met.

```solidity
// Strategy Step: Add liquidity between two tokens when their ratio meets a specified target.
UniswapV2Facet.addLiquidity(tokenA, tokenB, amountADesired, amountBDesired, minA, minB);
```

### 4. **One-Sided Liquidity Provision (Zap Functions)**

In scenarios where liquidity needs to be added without directly holding both pool assets, one-sided liquidity (zapping) is a powerful feature.

- **Zap Functions**:
  - Use `zap` for converting a single token into a liquidity position for a pair.
  - `zapETH` can be used to convert ETH into a liquidity position in a token-ETH pool.
  - These functions can be effective for strategies where single-asset liquidity addition is needed, minimizing exposure to only one of the pool assets.

#### Example Strategy Step:

In a strategy aiming to quickly capitalize on price movements, add a step to zap tokens into liquidity positions without having to manage multiple assets directly.

```solidity
// Strategy Step: Convert all of tokenA into a liquidity position in the tokenA-tokenB pool.
UniswapV2Facet.zap(tokenA, tokenB, amountIn);
```

### 5. **Retrieving Router Information**

To ensure compatibility and address usage in strategies, `uniswapV2Router` provides the Uniswap V2 router address.

#### Example Usage in Strategy

In complex strategies requiring access to multiple Uniswap functionalities or chain integrations, retrieving the router address can facilitate compatibility checks or further protocol interactions.

```solidity
address router = UniswapV2Facet.uniswapV2Router();
```

---

## Implementing UniswapV2Facet in StrategyBuilder

To incorporate **UniswapV2Facet** in the **StrategyBuilder**, each function can be designated as a **Strategy Step**. The **StrategyBuilder** conditions can then trigger swaps, liquidity provisions, or zaps based on predefined criteria, automating responses to market conditions or portfolio targets.

For example, a strategy could be set up with the following steps:

1. **Swap ETH for Token A** when a specific condition is met, like ETH surpassing a price threshold.
2. **Add Liquidity** between Token A and Token B if Token A’s allocation exceeds a certain percentage.
3. **Zap** Token A into a liquidity position with Token B as prices stabilize.

Using UniswapV2Facet in **StrategyBuilder** provides dynamic control over asset holdings, liquidity, and portfolio rebalancing, making it a versatile tool for automated DeFi strategies.
