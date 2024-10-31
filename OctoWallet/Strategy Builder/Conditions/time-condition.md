---
icon: clock
tags:
  [
    smart wallet,
    smartwallet,
    octo wallet,
    octodefi wallet,
    condition,
    time-based,
  ]
---

# TimeCondition

The **TimeCondition** contract is a time-based condition module that can be integrated into the **StrategyBuilder** contract, allowing strategies to execute only when a specific time condition is met. This contract schedules actions based on specified timestamps and intervals, with options for one-time or recurring execution. Below is a guide on setting up and using **TimeCondition** within **StrategyBuilder**.

---

## Using TimeCondition in StrategyBuilder

The **TimeCondition** can be set up to dynamically trigger actions in a strategy when certain time conditions are met. Here’s a step-by-step guide on how to set up and use this condition in a strategy:

### 1. **Setting Up a Time Condition**

To create a time-based condition that will activate a strategy, follow these steps:

- **Define Execution Timestamp**: Specify the `execution` time when the condition will first trigger. This timestamp is checked against the current block timestamp.
- **Set Update Interval (`delta`)**: Define the interval between recurring triggers. If set to `0`, the condition is one-time only.
- **Mark as Updateable**: Specify whether the condition is **updateable**, meaning it will recur at each `execution` + `delta` interval.

#### Example Setup

To add a condition that triggers every 24 hours, starting from a specified timestamp:

- `execution`: Starting timestamp for the condition to first execute.
- `delta`: `86400` (seconds in 24 hours) for daily recurrence.
- `updateable`: `true` to allow recurring execution.

The setup could look like this:

```solidity
// Add a condition to trigger daily starting from a specified timestamp.
TimeCondition.Condition memory condition = TimeCondition.Condition(
    execution: startTimestamp,
    delta: 86400, // 24 hours in seconds
    updateable: true
);

TimeCondition.addCondition(conditionId, condition);
```

### 2. **Integration as a Conditional Step in StrategyBuilder**

The **TimeCondition** contract can be integrated into **StrategyBuilder** by setting it up as a structured condition check. This allows for branching logic in the strategy execution path based on the time condition’s results. Below are the key steps and structure for using this condition in the **StrategyBuilder**.

In the **StrategyBuilder**, each conditional step in a strategy can reference **TimeCondition** using the `Condition` struct. This struct specifies:

- `conditionAddress`: The address of the **TimeCondition** contract.
- `id`: A unique ID for the condition.
- `result1`: The next step to execute if the condition evaluates to `1` (time met).
- `result0`: The next step if the condition evaluates to `0` (time not met).

By setting up the branching in this way, a strategy can proceed differently based on whether the time condition has been met. If `result1` or `result0` are `0`, then no further steps are executed, effectively ending the automation sequence.

#### Example Structure of a Condition in the Strategy

The `Condition` struct enables branching by assigning next steps based on condition results:

```solidity
struct Condition {
    address conditionAddress; // TimeCondition contract address
    uint16 id;                // Unique ID of the time condition
    uint16 result1;           // Index of next step if condition result is 1
    uint16 result0;           // Index of next step if condition result is 0
}
```

This setup is used to control the flow within a strategy's automation by directing which steps to execute based on the result of the condition.

#### Execution Flow in StrategyBuilder

The `executeAutomation` function in **StrategyBuilder** is designed to:

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
3. **Conditional Updates**: If the condition is marked as updateable, it updates the condition after execution.

### Example Use Case in Automation

Consider an automation where a strategy step should execute every 24 hours:

- Set `conditionAddress` to the **TimeCondition** contract address.
- Use `id` to identify this specific time condition.
- Define `result1` to point to the next step in the strategy if the time is met, or set `result1 = 0` to end execution.
- Similarly, set `result0` to specify the next step if the condition is unmet.

This time-based conditional setup allows **StrategyBuilder** to dynamically adjust its execution flow, creating a responsive strategy framework with time-sensitive branching logic.

### 3. **Managing Conditions**

Conditions in **TimeCondition** are stored per user and `id`, which means:

- Multiple conditions can be created by a single user, each with a unique `id`.
- Use `deleteCondition` to remove conditions that are no longer needed to free up memory.

Conditions that require periodic checking or one-time validation can be managed by calling `checkCondition` within **StrategyBuilder** each time the strategy is executed.

---

## Benefits of Using TimeCondition in Strategies

- **Scheduled Execution**: Set up time-based triggers to execute strategy steps at precise intervals.
- **Flexible Control**: Supports one-time and recurring conditions, allowing strategies to act at specific intervals or one-time events.
- **Enhanced Automation**: Time-sensitive strategies enable portfolio actions and adjustments on a set schedule without manual intervention.

In summary, **TimeCondition** offers powerful time-based capabilities for **StrategyBuilder**, enabling strategies that respond to time-based triggers. This setup facilitates complex, automated strategies that execute actions only at specific times, creating a responsive automation system for time-sensitive strategies.
