---
icon: play
tags: [smart wallet, smartwallet, pecunity, action contract]
order: 10
---

# Actions

## 🧩 Action Contract Overview

An **Action Contract** defines a modular, reusable operation that can be executed within the strategy-builder-plugin. These contracts implement the `IAction` interface and encapsulate logic for creating one or more `PluginExecution` instructions.

Each Action does **not** execute logic directly. Instead, it returns a set of low-level instructions that specify:

- **`target`** – the address to call
- **`data`** – the calldata for the call (function selector and arguments)
- **`value`** – the ETH value (if any) to send with the call

This design allows strategies to **simulate, analyze, or chain** actions before actually executing them, offering flexibility and composability.

---

## 📦 Example: `SendCoinAction`

```solidity
contract SendCoinAction is IAction {
    function sendCoins(uint256 amount, address to) external pure returns (PluginExecution[] memory) {
        PluginExecution ;
        executions[0] = PluginExecution({
            target: to,
            data: '',
            value: amount
        });
        return executions;
    }
}
```

### 🔍 What It Does

The `SendCoinAction` contract defines a simple action: sending ETH (`amount`) to a specified address (`to`). When `sendCoins` is called, it doesn't actually send the funds itself. Instead, it **returns** a `PluginExecution` array describing **what** should be executed.

---

## 💰 Fee Handling with `ITokenGetter`

For flexible **fee assessment and handling**, you can optionally implement the `ITokenGetter` interface. This is particularly useful in combination with the **FeeController**, where it’s important to determine the token involved in a specific action for calculating fees.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

interface ITokenGetter {
    function getTokenForSelector(bytes4 selector, bytes memory params) external view returns (address);
}
```

### ✳️ How It Works

- `getTokenForSelector(...)` receives:
  - the **function selector** of the action being called
  - the **encoded parameters** for that function
- It returns the **token address** that should be used for fee calculation or tracking.

This can be implemented **directly within an Action contract**, or as a **separate helper contract** if you want to keep responsibilities decoupled.

### 🔄 Integration Flow

1. A strategy calls an Action method.
2. The FeeController intercepts the call and uses `ITokenGetter` to determine the token involved.
3. Fees are calculated or applied accordingly, based on that token.

### 🧠 Why Optional?

Not all actions involve tokens, and not all need fee logic. The interface is designed to be **opt-in**—you only implement it if your Action requires token-aware fee logic.

---

## ✅ Key Benefits

- **Composable:** Easily chain multiple actions together.
- **Auditable:** Actions can be inspected or simulated before execution.
- **Modular:** Developers can build a library of reusable actions for different on-chain operations.
- **Flexible Fee Handling:** Optional `ITokenGetter` support enables advanced use cases without overloading the core action logic.
