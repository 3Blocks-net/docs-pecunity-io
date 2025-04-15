---
icon: code-square
tags: [smart wallet, smartwallet, octo wallet, octodefi wallet, condition]
order: 9
---

# Condition

## 📘 Condition Contracts Overview– Strategy Builder Plugin

**Condition Contracts** are a core part of the Strategy Builder Plugin, enabling smart, rule-based execution of automations and strategies on-chain. These contracts allow users to define custom logic that must be fulfilled before any on-chain action can proceed.

---

### 🧠 What Are Condition Contracts?

Condition Contracts are smart contracts that **check the state of the blockchain** and compare it against user-defined parameters. They are used to **control whether a strategy or automation is allowed to execute**.

Each condition is **stored per user** and includes logic to determine whether it is currently fulfilled.

---

### 🔍 How It Works

1. **Create a Condition**  
   A user defines and stores their own condition (e.g., a timestamp, threshold, or token balance) in a contract.

2. **Link It to a Strategy or Trigger**  
   The condition is then attached to either a strategy or a specific automation (trigger).

3. **Check the Condition**  
   When the Strategy Builder Plugin evaluates the condition, it will call:

   ```solidity
   checkCondition(address wallet, uint32 id)
   ```

   - Returns `1` if the condition is fulfilled.
   - Returns `0` if it is not.

4. **Update the Condition (Optional)**  
   Some conditions are marked as **updateable**, meaning they can change over time based on custom logic (e.g., rolling time intervals, price changes, etc.). If a condition is updateable, the plugin or user can call:
   ```solidity
   updateCondition(id)
   ```

---

### 🔐 Condition Lifecycle & Deletion

Conditions can only be deleted if they are **not currently used**:

- In an **automation trigger**
- Or as part of a **strategy comparison**

This ensures strategies don’t reference invalid or deleted conditions.

---

### ✅ Summary

| Feature                   | Description                                                                 |
| ------------------------- | --------------------------------------------------------------------------- |
| **Custom Logic**          | Each condition can implement any on-chain validation logic.                 |
| **Per-User Storage**      | Conditions are stored per wallet, allowing isolated, user-defined behavior. |
| **True/False Evaluation** | Returns `1` (true) or `0` (false) when evaluated by the plugin.             |
| **Safe Deletion**         | Conditions can only be removed if no strategy or trigger uses them.         |
| **Optional Updates**      | Some conditions can be updated dynamically based on their design.           |

---

### 🧱 Example Implementations

- `TimeCondition`: Allows execution only after a certain timestamp and can optionally reschedule itself.
- `ThresholdCondition`: Triggers when a value (e.g., token price, balance) exceeds a user-defined threshold.
- `BalanceCondition`: Checks if a wallet holds a minimum amount of a specific token.

---

Condition Contracts give your strategy powerful control over **when** and **under what circumstances** it can act.

Let us know if you want example code, visuals, or advanced use cases!
