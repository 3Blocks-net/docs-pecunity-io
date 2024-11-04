---
icon: issue-reopened
tags:
  [
    smart wallet,
    smartwallet,
    octo wallet,
    octodefi wallet,
    action facet,
    Uniswap V2,
  ]
---

# Uniswap V2 Facet

The `UniswapV2Facet` contract is designed as a facet that follows the **Diamond Standard (EIP-2535)**. This standard allows a smart contract to be modular and upgradable by integrating different "facets" that encapsulate various functionalities. By adding the `UniswapV2Facet` to a **smart contract wallet** that supports the Diamond Standard, users can extend the wallet’s capabilities to include all Uniswap V2 interaction functions provided by this facet.

With `UniswapV2Facet` in `StrategyBuilder`, users can integrate various token swap and liquidity functions from Uniswap V2 as steps in a strategy. This enables the creation of automated trading strategies that can perform token swaps, liquidity provision, and liquidity removal based on specified conditions, as well as percentage-based swaps and liquidity management.

## Functionality Overview:

- **Integration with Smart Contract Wallet**: Once the `UniswapV2Facet` is integrated into the smart contract wallet, users can access all its functions. This includes fundamental DeFi operations such as token swaps, adding and removing liquidity from token pools, and specialized actions like swapping a percentage of existing liquidity.

- **Usage in StrategyBuilder**: The `UniswapV2Facet` functions are designed to integrate seamlessly with the **StrategyBuilder**—a component within the wallet ecosystem. This allows users to create sophisticated, automated strategies that utilize Uniswap V2's liquidity features, enabling actions such as automated token swaps, liquidity management, and portfolio rebalancing based on preset criteria.

- **Direct Function Calls**: Beyond automated strategies, users or contracts with the required permissions can directly invoke any function from `UniswapV2Facet`. This grants on-demand, flexible access to Uniswap V2 operations, such as performing token swaps, adding/removing liquidity, and executing custom trading strategies.

The integration of `UniswapV2Facet` into the **StrategyBuilder** enhances the capability to manage asset portfolios dynamically, providing users with powerful tools for liquidity management, trading, and automated DeFi strategies.

## Using UniswapV2Facet in StrategyBuilder

The **UniswapV2Facet** provides flexibility in building complex strategies for DeFi operations, particularly for asset swapping and liquidity pool interactions. Here’s how each type of function can be implemented as steps in a strategy:

### 1. **Swaps for Token and ETH Interactions**

In strategies that involve trading tokens or converting between ETH and tokens, swap functions allow a variety of operations:

- **Token-to-Token Swaps**:

  - Use `swapExactTokensForTokensUniswapV2` to swap a fixed amount of one token for as many of another token as possible.
  - If the goal is to obtain an exact amount of output tokens, `swapTokensForExactTokensUniswapV2` can be used, setting a limit on how many input tokens can be spent.

- **ETH Swaps**:
  - Convert ETH into tokens using `swapExactETHForTokensUniswapV2`, ideal for strategies where ETH is converted into another asset.
  - Alternatively, swap for an exact amount of tokens with `swapETHForExactTokensUniswapV2`.
  - For the reverse (i.e., swapping tokens to ETH), use `swapExactTokensForETHUniswapV2` or `swapTokensForExactETHUniswapV2` depending on whether a fixed input or exact output is desired.

#### Example Strategy Step:

In a strategy, add a swap step where ETH is converted to tokens upon meeting a specific condition (e.g., market price threshold). This can help automate buy-ins or sell-offs.

```solidity
// Strategy Step: Swap 1 ETH for the maximum possible amount of a token, with a minimum acceptable output.
wallet.swapExactETHForTokensUniswapV2(1 ether, minTokenOut, path);
```

### 2. **Percentage-Based Swap Functions**

If a strategy is dynamically based on portfolio allocation, percentage-based swaps allow for adjusting holdings according to predefined percentages.

- **Percentage Token Swap**:
  - `swapPercentageTokensForTokensUniswapV2` swaps a specified percentage of a token to another token.
  - `swapPercentageTokensForETHUniswapV2` or `swapPercentageETHForTokensUniswapV2` can help strategies maintain a balanced portfolio allocation in ETH and tokens, adjusting to maintain target ratios.

#### Example Strategy Step:

For a strategy that periodically rebalances a portfolio, add a step to swap 10% of an existing token for another, ensuring the allocation is met.

```solidity
// Strategy Step: Swap 10% of a token holding to ETH.
wallet.swapPercentageTokensForETHUniswapV2(10, path);
```

### 3. **Liquidity Pool Management**

Providing and managing liquidity is a crucial part of many DeFi strategies, and UniswapV2Facet enables flexible liquidity operations.

- **Adding Liquidity**:

  - Use `addLiquidityUniswapV2` or `addLiquidityETHUniswapV2` to contribute tokens and/or ETH to Uniswap pools, generating LP tokens.
  - Percentage-based functions like `addLiquidityPercentageUniswapV2` or `addLiquidityETHPercentageUniswapV2` can help you add liquidity while keeping a portion of the original tokens or ETH balance intact.

- **Removing Liquidity**:
  - Use `removeLiquidityUniswapV2` and `removeLiquidityETHUniswapV2` for redeeming LP tokens and withdrawing the underlying assets.
  - `removeLiquidityPercentageUniswapV2` and `removeLiquidityETHPercentageUniswapV2` allow for withdrawing only a specified percentage, which can be useful in gradual exit strategies.

#### Example Strategy Step:

For strategies that aim to accumulate LP tokens as prices fluctuate, a step can be added to automatically provide liquidity when a target token ratio is met.

```solidity
// Strategy Step: Add liquidity between two tokens when their ratio meets a specified target.
wallet.addLiquidityUniswapV2(tokenA, tokenB, amountADesired, amountBDesired, minA, minB);
```

### 4. **One-Sided Liquidity Provision (Zap Functions)**

In scenarios where liquidity needs to be added without directly holding both pool assets, one-sided liquidity (zapping) is a powerful feature.

- **Zap Functions**:
  - Use `zapUniswapV2` for converting a single token into a liquidity position for a pair.
  - `zapETHUniswapV2` can be used to convert ETH into a liquidity position in a token-ETH pool.
  - These functions can be effective for strategies where single-asset liquidity addition is needed, minimizing exposure to only one of the pool assets.

#### Example Strategy Step:

In a strategy aiming to quickly capitalize on price movements, add a step to zap tokens into liquidity positions without having to manage multiple assets directly.

```solidity
// Strategy Step: Convert all of tokenA into a liquidity position in the tokenA-tokenB pool.
wallet.zapUniswapV2(tokenA, tokenB, amountIn);
```

## UniswapV2Facet Contract API

### Core Uniswap V2 Swap Functions

#### swapExactTokensForTokensUniswapV2

```solidity
swapExactTokensForTokensUniswapV2(uint256 amountIn, uint256 amountOutMin, address[] calldata path)
```

**Description**: Swaps an exact amount of input tokens for a minimum amount of output tokens.

- `amountIn`: The amount of input tokens.
- `amountOutMin`: The minimum acceptable amount of output tokens.
- `path`: The path of token addresses for the swap.

#### swapTokensForExactTokensUniswapV2

```solidity
swapTokensForExactTokensUniswapV2(uint256 amountOut, uint256 amountInMax, address[] calldata path)
```

**Description**: Swaps tokens to receive an exact amount of output tokens.

- `amountOut`: The exact amount of output tokens desired.
- `amountInMax`: The maximum allowable amount of input tokens.
- `path`: The token addresses for the swap path.

#### swapExactETHForTokensUniswapV2

```solidity
swapExactETHForTokensUniswapV2(uint256 amountIn, uint256 amountOutMin, address[] calldata path)
```

**Description**: Swaps an exact amount of ETH for a minimum amount of tokens.

- `amountIn`: The amount of ETH to be swapped.
- `amountOutMin`: The minimum amount of tokens to be received.
- `path`: The path of token addresses for the swap.

#### swapETHForExactTokensUniswapV2

```solidity
swapETHForExactTokensUniswapV2(uint256 amountOut, uint256 amountInMax, address[] calldata path)
```

**Description**: Swaps ETH to receive an exact amount of tokens.

- `amountOut`: The exact amount of output tokens.
- `amountInMax`: The maximum allowable amount of ETH.
- `path`: The swap path of token addresses.

#### swapExactTokensForETHUniswapV2

```solidity
swapExactTokensForETHUniswapV2(uint256 amountIn, uint256 amountOutMin, address[] calldata path)
```

**Description**: Swaps an exact amount of tokens for ETH.

- `amountIn`: The amount of input tokens.
- `amountOutMin`: The minimum amount of ETH expected.
- `path`: The token addresses for the swap path.

#### swapTokensForExactETHUniswapV2

```solidity
swapTokensForExactETHUniswapV2(uint256 amountOut, uint256 amountInMax, address[] calldata path)
```

**Description**: Swaps tokens to receive an exact amount of ETH.

- `amountOut`: The exact amount of ETH desired.
- `amountInMax`: The maximum amount of input tokens.
- `path`: The swap path of token addresses.

### Percentage-Based Swap Functions

#### swapPercentageTokensForTokensUniswapV2

```solidity
swapPercentageTokensForTokensUniswapV2(uint256 percentage, address[] calldata path)
```

**Description**: Swaps a specified percentage of tokens for other tokens.

- `percentage`: The percentage of the token balance to be swapped.
- `path`: The swap path of token addresses.

#### swapPercentageTokensForETHUniswapV2

```solidity
swapPercentageTokensForETHUniswapV2(uint256 percentage, address[] calldata path)
```

**Description**: Swaps a specified percentage of tokens for ETH.

- `percentage`: The percentage of the token balance to be swapped.
- `path`: The swap path of token addresses.

#### swapPercentageETHForTokensUniswapV2

```solidity
swapPercentageETHForTokensUniswapV2(uint256 percentage, address[] calldata path)
```

**Description**: Swaps a specified percentage of ETH balance for tokens.

- `percentage`: The percentage of the ETH balance to be swapped.
- `path`: The swap path of token addresses.

### Core Uniswap V2 Liquidity Functions

#### addLiquidityUniswapV2

```solidity
addLiquidityUniswapV2(address tokenA, address tokenB, uint256 amountADesired, uint256 amountBDesired, uint256 amountAMin, uint256 amountBMin)
```

**Description**: Adds liquidity to a token pair pool.

- `tokenA`, `tokenB`: The addresses of the tokens in the pair.
- `amountADesired`, `amountBDesired`: Desired amounts to add as liquidity.
- `amountAMin`, `amountBMin`: Minimum amounts of tokens accepted.

#### addLiquidityETHUniswapV2

```solidity
addLiquidityETHUniswapV2(address token, uint256 amountTokenDesired, uint256 amountTokenMin, uint256 amountETHDesired, uint256 amountETHMin)
```

**Description**: Adds liquidity to a token-ETH pool.

- `token`: The address of the token.
- `amountTokenDesired`, `amountETHDesired`: Desired amounts of token and ETH.
- `amountTokenMin`, `amountETHMin`: Minimum acceptable amounts.

#### removeLiquidityUniswapV2

```solidity
removeLiquidityUniswapV2(address tokenA, address tokenB, uint256 liquidity, uint256 amountAMin, uint256 amountBMin)
```

**Description**: Removes liquidity from a token pair pool.

- `tokenA`, `tokenB`: The addresses of the token pair.
- `liquidity`: Amount of liquidity to remove.
- `amountAMin`, `amountBMin`: Minimum amounts of tokens accepted.

#### removeLiquidityETHUniswapV2

```solidity
removeLiquidityETHUniswapV2(address token, uint256 liquidity, uint256 amountTokenMin, uint256 amountETHMin)
```

**Description**: Removes liquidity from a token-ETH pool.

- `token`: The address of the token.
- `liquidity`: The amount of liquidity to remove.
- `amountTokenMin`, `amountETHMin`: Minimum amounts of token and ETH accepted.

### Percentage-Based Liquidity Functions

#### addLiquidityPercentageUniswapV2

```solidity
addLiquidityPercentageUniswapV2(uint256 percentageADesired, address tokenA, address tokenB)
```

**Description**: Adds liquidity to a token pair pool based on a percentage.

- `percentageADesired`: The percentage of the available balance for `tokenA`.
- `tokenA`, `tokenB`: The token pair addresses.

#### removeLiqudityPercentageUniswapV2

```solidity
removeLiqudityPercentageUniswapV2(address tokenA, address tokenB, uint256 percentageLiquidity)
```

**Description**: Removes a percentage of liquidity from a token pair pool.

- `tokenA`, `tokenB`: The addresses of the token pair.
- `percentageLiquidity`: The percentage of liquidity to remove.

#### addLiquidityETHPercentageUniswapV2

```solidity
addLiquidityETHPercentageUniswapV2(address token, uint256 percentageETHDesired)
```

**Description**: Adds liquidity to a token-ETH pool based on a percentage of ETH.

- `token`: The address of the token.
- `percentageETHDesired`: The percentage of the ETH balance to use.

#### addLiquidityETHPercentageTokenUniswapV2

```solidity
addLiquidityETHPercentageTokenUniswapV2(address token, uint256 percentageTokenDesired)
```

**Description**: Adds liquidity to a token-ETH pool based on a percentage of the token balance.

- `token`: The address of the token.
- `percentageTokenDesired`: The percentage of the token balance to use.

#### removeLiquidityETHPercentageUniswapV2

```solidity
removeLiquidityETHPercentageUniswapV2(address token, uint256 liquidityPercentage)
```

**Description**: Removes a percentage of liquidity from a token-ETH pool.

- `token`: The address of the token.
- `liquidityPercentage`: The percentage of liquidity to remove.

### One-Sided Liquidity Functions

#### zapUniswapV2

```solidity
zapUniswapV2(address tokenA, address tokenB, uint256 amountIn)
```

**Description**: Converts a single token into a liquidity position for a token pair.

- `tokenA`, `tokenB`: The token pair for the liquidity.
- `amountIn`: The amount of the token to convert.

#### zapETHUniswapV2

```solidity
zapETHUniswapV2(address token, uint256 amountIn, bool inputETH)
```

**Description**: Converts ETH into a liquidity position for a token-ETH pair.

- `token`: The address of the token.
- `amountIn`: The amount of ETH or token to convert.
- `inputETH`: Boolean indicating if the input is ETH (`true`) or token (`false`).
