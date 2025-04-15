---
icon: package
tags: [strategy, builder, octo, octodefi, strategies]
order: 100
---

# Strategy Builder Plugin

## Overview

The StrategyBuilderPlugin is a modular smart contract built on the ERC6900 standard, designed to automate and execute advanced DeFi strategies seamlessly. This plugin integrates with modular smart accounts, allowing users to create, manage, and automate complex financial strategies such as vault management, borrowing, leveraged yield farming, and more.

## Key Features

✅ **DeFi Strategy Automation** – Users can define and execute automated strategies involving lending, borrowing, staking, and farming.  
✅ **Modular & Extensible** – Built within the ERC6900 ecosystem, enabling compatibility with other modular account plugins.  
✅ **Smart Execution & Conditions** – Allows strategies to execute based on predefined conditions, optimizing yield and efficiency.  
✅ **Fee Management Integration** – Implements IFeeController and IFeeHandler to ensure seamless fee handling for automated transactions.  
✅ **Security & Permissions** – Utilizes strategy validation to ensure only authorized actions are executed within a user’s smart account.

## How It Works

- **Strategy Creation** – Users define custom strategies that include multiple DeFi actions (e.g., borrowing, staking, swapping).
- **Automated Execution** – The plugin triggers actions based on conditions (e.g., interest rate thresholds, collateral ratios).
- **Fee Handling** – Ensures fair and transparent fee structures using external controllers and handlers.
- **Smart Account Integration** – Seamlessly integrates with modular smart accounts, leveraging ERC6900’s flexible permissioning system.

## Use Cases

- **Automated Yield Farming** – Deploy capital across multiple DeFi protocols for optimal returns.
- **Vault Management** – Automatically rebalance or reinvest assets in DeFi vaults.
- **Leverage Strategies** – Execute leveraged borrowing and farming with automated risk management.
- **Automated Liquidation Protection** – Prevent unnecessary liquidations by setting up stop-loss mechanisms.

## Why StrategyBuilderPlugin?

By leveraging the ERC6900 modular account standard, this plugin enhances DeFi automation, making complex financial strategies accessible, secure, and highly efficient for institutional and individual users.

Absolutely! Here's a more detailed, polished, and structured version of your **Fee Handling** section that integrates seamlessly with your existing documentation. It adds depth, clarity, and flow while keeping it developer- and user-friendly:

---

## 🔧 Core Contracts

There are four core smart contracts that make up the heart of the system:

- 🖥️ **PriceOracle**  
  Fetches real-time price data from the Pyth network.

- ⚙️ **FeeController**  
  Manages the fee configuration for each individual function selector used in strategy execution.

- 💸 **FeeHandler**  
  Distributes the total collected fee among recipients based on predefined percentages at the end of strategy execution.

- 🧩 **StrategyBuilderPlugin**  
  An ERC-6900 compatible plugin contract that provides the main functionality of the strategy builder—allowing you to create, execute, and manage strategies.

---

### 🧬 Source Code

You can find the source code for each core contract below:

| 📝 Contract              | 📌 Description                         | 🔗 Source Code Link                                                                                                               |
| ------------------------ | -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| 🖥️ PriceOracle           | Fetches price data from Pyth           | [PriceOracle.sol](https://github.com/3Blocks-net/strategy-builder-plugin/blob/main/contracts/PriceOracle.sol)                     |
| ⚙️ FeeController         | Manages function-specific fees         | [FeeController.sol](https://github.com/3Blocks-net/strategy-builder-plugin/blob/main/contracts/FeeController.sol)                 |
| 💸 FeeHandler            | Handles fee distribution               | [FeeHandler.sol](https://github.com/3Blocks-net/strategy-builder-plugin/blob/main/contracts/FeeHandler.sol)                       |
| 🧩 StrategyBuilderPlugin | Strategy builder core logic (ERC-6900) | [StrategyBuilderPlugin.sol](https://github.com/3Blocks-net/strategy-builder-plugin/blob/main/contracts/StrategyBuilderPlugin.sol) |

---

### ⚙️ Strategy Execution Flow

When a strategy is triggered—either by an automation service or a user—the execution process follows a structured sequence coordinated by the `StrategyBuilderPlugin` and related contracts:

1. **Initial Execution Check**  
   The `StrategyBuilderPlugin` first checks the `ConditionContract` defined in the strategy’s `ActionStruct`. This ensures that the overall strategy conditions (such as price thresholds, time-based logic, or on-chain states) are met before execution proceeds.

2. **Step-by-Step Processing**  
   If the initial condition check passes, the plugin begins processing the strategy’s steps in sequence.

   For each step:

   - The plugin checks whether a condition is defined.
   - If a condition exists, it queries the `ConditionContract` to determine the result:
     - If the result is **true (1)**: all actions within the step are executed, and the plugin jumps to the **next step assigned for a true result**.
     - If the result is **false (0)**: the plugin **skips** the step’s actions and jumps to the **next step assigned for a false result**.
   - If no condition is defined, the step's actions are executed unconditionally, and the flow proceeds to the next sequential step.

3. **Action Execution**  
   Each action within a step is executed individually, using the function call and data defined in the strategy configuration. Actions can include interacting with DeFi protocols, transferring tokens, or calling external contracts.

4. **Completion**  
   Once all relevant steps and actions have been executed, the strategy run concludes. Any post-execution logic (such as logging, event emission, or fee-related calls) is handled at this stage, depending on the strategy's structure and configuration.

---

## 🧮 Fee Handling in StrategyBuilderPlugin

The `StrategyBuilderPlugin` incorporates a flexible and transparent fee mechanism designed to fairly compensate stakeholders involved in strategy execution. The system revolves around two dedicated contracts: `IFeeController` and `IFeeHandler`, which together manage fee calculation, token handling, and revenue distribution.

---

### 🔧 Fee Management Components

- **`IFeeController`**  
  Handles the logic for calculating fees per action. It supports dynamic configurations per function selector and ensures that the appropriate observation tokens are used for balance tracking.

- **`IFeeHandler`**  
  Responsible for collecting the total calculated fees, handling token conversions (if necessary), and distributing the fees to all designated recipients based on predefined percentage splits.

---

### 📐 How Fees Are Calculated

Each step in a strategy may include one or more actions, and each action incurs its own fee. The `StrategyBuilderPlugin` interacts with `IFeeController` before and after action execution to determine fees based on **actual on-chain impact**.

The fee calculation follows this process:

1. **Observation Token Selection**  
   Before each action is executed, the `FeeController` provides the relevant **observation token** used to track balance changes (e.g., a token being swapped or deposited).

2. **Pre- and Post-Execution Balance Check**  
   The `StrategyBuilderPlugin` records the user’s balance of the observation token **before** and **after** executing the action. The difference (delta) is used to determine the **volume of activity** the action caused.

3. **Dynamic Fee Calculation**  
   Using the delta and the function selector of the action, the `FeeController` computes the fee amount based on the following parameters:

   - **Action Type** – Each function selector (e.g., swap, borrow, deposit) has a unique fee configuration.
   - **Transaction Volume** – The delta is converted to a USD-equivalent value using the `PriceOracle`, allowing for volume-based fees.
   - **Minimum Fee** – A default minimum fee is applied for actions where volume is not trackable or falls below a threshold.

4. **User-Defined Max Fee Cap**  
   Before execution, users can define a **maximum acceptable fee**. The contract enforces this cap to protect users from unexpectedly high charges, especially during volatile market conditions.

---

### 💸 Fee Payment & Token Conversion

Fees can be paid in a range of **approved ERC-20 tokens** or in **ETH**, depending on what the user allows. The process works as follows:

1. **USD-Based Fee Reference**  
   All fee calculations are initially done in **USD** terms for consistency and fairness across different token markets.

2. **Conversion via Price Oracle**  
   The USD-denominated fee is converted into the chosen payment token using real-time data from the `PriceOracle`.

3. **Fee Transfer & Security**  
   The payment is then **collected from the user’s wallet** or strategy context using `IFeeHandler`. Only **whitelisted payment tokens** are accepted to prevent incompatibility or malicious assets.

---

### 📤 Fee Distribution

After the entire strategy has been executed and all action-level fees have been calculated:

- The total fee amount is sent to `IFeeHandler`.
- The `FeeHandler` splits the fee based on predefined **percentage allocations**, which may include:
  - vault
  - strategy creator
  - token burns
  - Third-party automation services

This modular architecture allows flexibility for protocols to configure and adjust revenue models without touching the core strategy logic.

---

### ✅ Summary

The fee system in `StrategyBuilderPlugin` is:

- **Modular** – Separated into specialized contracts for flexibility.
- **Transparent** – Users can preview and cap fees.
- **Fair** – Based on real on-chain volume, not flat fees.
- **Secure** – Supports only approved tokens and verified oracles.
- **Configurable** – Adapts to different strategies, actions, and user preferences.

---

## Example: Leveraged Yield Farming Strategy on Aave + Uniswap

### **Objective:**

We want to maximize yield farming returns by:

1. Borrowing stablecoins from Aave using ETH as collateral.
2. Swapping stablecoins for LP tokens on Uniswap.
3. Generate fees while providing liquidity.

### **Strategy Steps:**

| Step | Action                          | Protocol Used |
| ---- | ------------------------------- | ------------- |
| 1️⃣   | Deposit ETH as collateral       | Aave          |
| 2️⃣   | Borrow USDC against ETH         | Aave          |
| 3️⃣   | Swap 50% of USDC for ETH        | Uniswap       |
| 4️⃣   | Provide liquidity (USDC-ETH LP) | Uniswap       |

## How This Works in StrategyBuilderPlugin

### **1️⃣ Defining the Strategy**

Each step in the strategy corresponds to an action within the smart contract. The user specifies:

- Which DeFi protocol to interact with (Aave, Uniswap, etc.).
- Which asset to deposit, borrow, swap, or stake.
- Conditions for execution (e.g., minimum collateral ratio).

### **2️⃣ Setting Up Execution Conditions**

Conditions can be defined to automate decision-making:

- **Trigger conditions:** Execute swaps only if USDC price drops below a threshold.
- **Health factor monitoring:** If Aave's collateral ratio is too low, automatically repay debt to avoid liquidation.
- **Yield optimization:** Reinvest farm rewards only if they exceed a minimum threshold.

### **3️⃣ Executing the Strategy**

Once the strategy is deployed and approved, the contract can execute steps automatically based on the conditions defined.

---

## Summary

The StrategyBuilderPlugin enables DeFi automation by:
✅ **Chaining multiple DeFi actions into a single strategy.**  
✅ **Defining conditions & triggers for execution.**  
✅ **Enhancing capital efficiency through automation.**  
✅ **Reducing manual intervention by managing positions dynamically.**

---

## Strategy Builder API

### Strategy Management

- **createStrategy**: Creates and stores a new strategy with defined steps and actions, associated with a unique `id`.

  ```solidity
  function createStrategy(uint32 id, address creator, StrategyStep[] calldata steps) external;
  ```

- **deleteStrategy**: Removes a strategy by `id`, making it inactive.

  ```solidity
  function deleteStrategy(uint32 id) external;
  ```

- **executeStrategy**: Executes a strategy by processing each step in sequence, checking conditions, and performing actions as specified.
  ```solidity
  function executeStrategy(uint32 id) external;
  ```

### Automation Management

- **createAutomation**: Links an automation to a specific condition and strategy, enabling the strategy to be triggered based on the condition's outcome.

  ```solidity
  function createAutomation(uint32 id, uint32 strategyId, address paymentToken, uint256 maxFeeInUSD, Condition calldata condition) external;
  ```

- **deleteAutomation**: Removes an automation, deactivating it.

  ```solidity
  function deleteAutomation(uint32 id) external;
  ```

- **executeAutomation**: Checks the condition of an automation and, if true, triggers the linked strategy.
  ```solidity
  function executeAutomation(uint32 id, address wallet, address beneficary) external;
  ```

This contract enables the creation of complex automated workflows that integrate with external triggers and conditions, making it suitable for applications like decentralized finance (DeFi) and automated contract management.
