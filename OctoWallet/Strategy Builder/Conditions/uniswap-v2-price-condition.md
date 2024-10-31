---
icon: issue-reopened
tags: [smart wallet, smartwallet, octo wallet, octodefi wallet, codnition]
---

# Uniswap V2 Price Condition

The **UniswapV2PriceCondition** contract is a condition module that can be integrated into the **StrategyBuilder** contract, allowing strategies to execute only when a specific price condition on Uniswap V2 is met. This contract evaluates whether the price of a specified token pair meets a target based on an input amount, with conditions for prices being **lower**, **equal**, or **greater** than a threshold value. Below is a guide on setting up and using this price condition within the StrategyBuilder.

---

## Using UniswapV2PriceCondition in StrategyBuilder

The **UniswapV2PriceCondition** can be set up to dynamically trigger actions in a strategy when certain price conditions are met. Here’s a step-by-step guide on how to set up and use this condition in a strategy:

### 1. **Setting Up a Price Condition**

To create a price-based condition that will activate a strategy, follow these steps:

- **Define the Path**: Choose the token pair for which the price will be monitored. The `path` array specifies the route of tokens, with at least two addresses (e.g., `[tokenA, tokenB]`).
- **Specify the Condition Type**: Select the type of condition:

  - **LOWER**: Triggers when the output amount for a given input amount falls below the specified threshold.
  - **EQUAL**: Triggers when the output amount matches the target exactly.
  - **GREATER**: Triggers when the output amount surpasses the target.

- **Set the Thresholds**:
  - `amountIn`: The input amount (in tokenA) to convert.
  - `amountOut`: The target output amount (in tokenB) used to evaluate the condition.

#### Example Setup

To add a condition that triggers when the price of 1 ETH is greater than 3000 DAI, the following parameters might be used:

- `path`: `[ETH, DAI]`
- `condition`: `GREATER`
- `amountIn`: `1 ETH` (specified in wei)
- `amountOut`: `3000 DAI` (also in the appropriate units)

This setup could be executed as follows:

```solidity
// Add a condition where 1 ETH should yield more than 3000 DAI.
PriceCondition memory condition = PriceCondition(
    path: [address(ETH), address(DAI)],
    condition: Condition.GREATER,
    amountIn: 1 ether,
    amountOut: 3000 * 10**18
);

UniswapV2PriceCondition.addCondition(conditionId, condition);
```

### 2. Integration as a Conditional Step in StrategyBuilder

The **UniswapV2PriceCondition** contract can be integrated into **StrategyBuilder** by setting it up as a structured condition check. This allows for branching logic in the strategy execution path based on the price condition’s results. Below are the key steps and structure for using this condition in the strategy builder.

In the **StrategyBuilder**, each conditional step in a strategy can reference **UniswapV2PriceCondition** using the `Condition` struct. This struct specifies:

- The `conditionAddress` (address of the **UniswapV2PriceCondition** contract).
- A unique `id` for the condition.
- `result1`: The next step to execute if the condition evaluates to `1`.
- `result0`: The next step if the condition evaluates to `0`.

By setting up the branching in this way, a strategy can move to different steps depending on whether the condition result is `1` (condition met) or `0` (condition unmet). If `result1` or `result0` are `0`, then no further steps are executed, effectively ending the automation sequence.

#### Example Structure of a Condition in the Strategy

The `Condition` struct enables branching by assigning next steps based on condition results:

```solidity
struct Condition {
    address conditionAddress; // UniswapV2PriceCondition address
    uint16 id;                // Unique ID of the price condition
    uint16 result1;           // Index of next step if condition result is 1
    uint16 result0;           // Index of next step if condition result is 0
}
```

This setup is used to control the flow within a strategy's automation by directing which steps to execute based on the result of the condition.

#### Execution Flow in StrategyBuilder

The `executeAutomation` function is designed to:

1. Retrieve the automation configuration and its condition details.
2. Check the condition result using `_checkCondition`.
3. Based on `conditionResult`, follow the path defined in `result1` or `result0` to determine the next step or stop.

Here’s how the `executeAutomation` function operates in **StrategyBuilder**:

```solidity
function executeAutomation(uint16 id) internal automationIsActive(id) {
    // Retrieve the automation setup
    Automation memory automation = diamondStorage().automations[id];

    // Check the condition status (1 or 0)
    (uint8 conditionResult,) = _checkCondition(automation.condition);

    // Execute the strategy if condition result is 1
    if (conditionResult == 1) {
        executeStrategy(automation.strategyId);
        emit AutomationExecuted(id);
    }

    // Update condition if it's set to be updateable
    if (ICondition(automation.condition.conditionAddress).isUpdateable(address(this), automation.condition.id)) {
        ICondition(automation.condition.conditionAddress).updateCondition(automation.condition.id);
    }
}
```

1. **Condition Check**: Calls `_checkCondition` to retrieve `conditionResult`.
2. **Execution Based on Condition**: If `conditionResult` is `1`, it executes the strategy via `executeStrategy`.
3. **Conditional Updates**: If the condition is marked as updatable, it updates the condition after execution.

### Example Use Case in Automation

Consider an automation where a strategy step should be triggered only when the price of 1 ETH is above 3000 DAI:

- Set `conditionAddress` to the **UniswapV2PriceCondition** contract address.
- Use `id` to identify this specific price condition.
- Define `result1` to point to the next step in the strategy if the price is above the target, or set `result1 = 0` to end execution.
- Similarly, set `result0` to specify the next step if the condition is unmet.

By structuring conditions this way, **StrategyBuilder** can dynamically adjust its execution flow based on real-time price data, creating a highly responsive strategy framework. This conditional approach facilitates more complex automation sequences, supporting branching logic that’s triggered by external market conditions.

### 3. **Managing Conditions**

Conditions in **UniswapV2PriceCondition** are stored per user and `id`, which means:

- Multiple conditions can be created by a single user, each with a unique `id`.
- Use `deleteCondition` to remove conditions that are no longer needed to free up memory.

Conditions that need periodic checking or one-time validation can be managed by calling `checkCondition` within the **StrategyBuilder** each time the strategy is executed.

---

## Benefits of Using UniswapV2PriceCondition in Strategies

- **Automatic Trade Execution**: Set up price triggers to automatically swap, add, or remove liquidity based on target price levels.
- **Enhanced Flexibility with Custom Conditions**: Control whether a strategy step should proceed by defining detailed conditions based on real-time price changes.
- **Precision Portfolio Management**: Strategies can be configured to act only at specific price points, ensuring optimal trade execution within desired price ranges.

In summary, **UniswapV2PriceCondition** offers powerful conditional capabilities to **StrategyBuilder**, enabling strategies that are responsive to real-time price conditions. This setup allows for complex, automated strategies on Uniswap V2 that can adapt to price movements and execute actions only when specific price thresholds are met.
