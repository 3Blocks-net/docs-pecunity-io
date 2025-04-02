---
icon: repo-forked
tags: [strategy, builder, octo, octodefi, strategies]
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

## Fee Handling in StrategyBuilderPlugin

The StrategyBuilderPlugin implements a structured fee system to ensure fair and transparent payments for executing automated strategies. Fees are managed through two key components:

- **IFeeController** – Responsible for calculating fees based on transaction type, action volume, and predefined fee structures.
- **IFeeHandler** – Manages fee collection, token conversions, and distribution to beneficiaries.

### **How Fees Are Calculated**

Each strategy consists of multiple steps, and every step incurs a separate fee based on:

- **Action Type** – Different DeFi actions (e.g., deposit, borrow, swap) have predefined fee structures.
- **Transaction Volume** – Fees are dynamically calculated based on asset amounts involved.
- **Minimum Fees** – If an action does not track volume, a minimum fee applies.

Before execution, the contract verifies that the total fee does not exceed the user-defined maximum fee limit, preventing unexpected overcharges.

### **Fee Payment & Token Handling**

- Users can pay fees in **ERC-20 tokens** or **ETH**.
- The contract converts the fee from USD value to the selected payment token using **price oracles**.
- Fees are transferred to beneficiaries (e.g., protocol, developers) using **IFeeHandler**.
- Only **approved tokens** can be used for payment, ensuring security and compatibility.

### **Security & Restrictions**

✅ **Max Fee Protection** – The system ensures fees never exceed the user’s predefined maximum.  
✅ **Token Whitelisting** – Only pre-approved tokens can be used to pay for execution.  
✅ **Oracle Verification** – Ensures accurate price conversion when fees are paid in non-stable assets.

By leveraging this **modular and automated fee management system**, the StrategyBuilderPlugin ensures **cost efficiency, transparency, and secure execution of DeFi strategies.**

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
