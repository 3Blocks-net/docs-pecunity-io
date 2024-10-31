---
icon: repo-forked
tags: [strategy, builder, octo, octodefi, strategies]
---

# Strategy Builder

## Overview

The `StrategyBuilder` contract leverages the `StrategyBuilderLib` library to manage and execute complex, automated strategies with multiple steps, conditions, and actions. This contract provides a straightforward interface for defining and controlling strategies, as well as automating their execution based on external conditions.

## Core Concepts

### 1. **Creating a Strategy and Executing It**

Using `StrategyBuilder`, users can create strategies, each containing steps that specify conditions and actions to be executed. Strategies can be executed step-by-step, with each step following a predefined flow based on condition evaluations.

### 2. **Automating Strategy Execution**

The contract also allows strategies to be automated through external triggers. Automations enable strategies to run when specific conditions are met, enabling flexibility and external control over execution flow.

## Building a Strategy

To build a strategy in `StrategyBuilder`, follow these steps:

### Step 1: Define Actions

An action specifies a command or function call that a step in the strategy will execute. Actions are defined by:

- **selector**: The function selector of the action to be called.
- **parameter**: The encoded parameters for the function call.

Example:

```solidity
StrategyBuilderLib.Action memory action1 = StrategyBuilderLib.Action({
    selector: bytes4(keccak256("functionName(uint256)")),
    parameter: abi.encode(123)
});
```

### Step 2: Define Conditions

Each step in a strategy may be contingent on a condition that determines which next step to proceed to. Conditions are defined by:

- **conditionAddress**: Address of the condition contract to check.
- **id**: Identifier for the condition within the contract.
- **result1**: The next step to proceed to if the condition returns `true`.
- **result0**: The next step to proceed to if the condition returns `false`.

Example:

```solidity
StrategyBuilderLib.Condition memory condition1 = StrategyBuilderLib.Condition({
    conditionAddress: address(conditionContract),
    id: 1,
    result1: 2,  // Go to step 2 if true
    result0: 0   // Stop if false
});
```

### Step 3: Create Strategy Steps

Each step in the strategy will contain a condition and a list of actions. Steps are added in sequence to the strategy.

Example:

```solidity
StrategyBuilderLib.StrategyStep memory step1 = StrategyBuilderLib.StrategyStep({
    condition: condition1,
    actions: new StrategyBuilderLib.Action
});
step1.actions[0] = action1; // Add action to the step
```

### Step 4: Add Strategy to Storage

Once the steps are defined, create a new strategy and add it to storage using the `addStrategy` function. Each strategy is associated with an ID and a creator's address.

Example:

```solidity
StrategyBuilderLib.StrategyStep[] memory steps = new StrategyBuilderLib.StrategyStep[](1);
steps[0] = step1;

StrategyBuilder.addStrategy(strategyId, creatorAddress, steps);
```

### Step 5: Execute the Strategy

To execute the strategy, call the `executeStrategy` function, which will process each step sequentially based on the specified conditions and actions.

Example:

```solidity
StrategyBuilder.executeStrategy(strategyId);
```

### Automate Strategy Execution with Conditions

To trigger strategy execution based on external conditions:

1. **Define Automation** with a linked condition and strategy ID:

   ```solidity
   StrategyBuilderLib.Condition memory autoCondition = StrategyBuilderLib.Condition({
       conditionAddress: address(conditionContract),
       id: 2,
       result1: 1, // Step 1 if true
       result0: 0  // Stop if false
   });

   StrategyBuilder.activateAutomation(automationId, strategyId, autoCondition);
   ```

2. **Execute Automation** when the condition is met:
   ```solidity
   StrategyBuilder.executeAutomation(automationId);
   ```

## Strategy Builder API

### Strategy Management

- **addStrategy**: Creates and stores a new strategy with defined steps and actions, associated with a unique `id`.

  ```solidity
  function addStrategy(uint16 id, address creator, StrategyStep[] calldata steps) internal;
  ```

- **deleteStrategy**: Removes a strategy by `id`, making it inactive.

  ```solidity
  function deleteStrategy(uint16 id) internal;
  ```

- **executeStrategy**: Executes a strategy by processing each step in sequence, checking conditions, and performing actions as specified.
  ```solidity
  function executeStrategy(uint16 id) internal;
  ```

### Automation Management

- **activateAutomation**: Links an automation to a specific condition and strategy, enabling the strategy to be triggered based on the condition's outcome.

  ```solidity
  function activateAutomation(uint16 id, uint16 strategy, Condition calldata condition) internal;
  ```

- **deleteAutomation**: Removes an automation, deactivating it.

  ```solidity
  function deleteAutomation(uint16 id) internal;
  ```

- **executeAutomation**: Checks the condition of an automation and, if true, triggers the linked strategy.
  ```solidity
  function executeAutomation(uint16 id) internal;
  ```

## Example Usage

### Define and Execute a Simple Strategy

1. **Create a strategy** with steps, conditions, and actions:
   ```solidity
   StrategyBuilder.addStrategy(strategyId, creatorAddress, strategySteps);
   ```
2. **Execute the strategy** to process its steps:
   ```solidity
   StrategyBuilder.executeStrategy(strategyId);
   ```

### Automate Strategy Execution Based on Conditions

1. **Activate automation** for a strategy with a specific condition:
   ```solidity
   StrategyBuilder.activateAutomation(automationId, strategyId, autoCondition);
   ```
2. **Execute automation** to trigger the strategy if the condition is met:
   ```solidity
   StrategyBuilder.executeAutomation(automationId);
   ```

This contract enables the creation of complex automated workflows that integrate with external triggers and conditions, making it suitable for applications like decentralized finance (DeFi) and automated contract management.
