---
icon: tools
tags: [smart wallet, smartwallet, octo wallet, octodefi wallet, build contracts]
order: 5
---

## 🛠️ Build

You can build **custom action or condition contracts** for use with the **Strategy Builder Plugin** by [Octo DeFi](https://octo.fi).

### 🧱 Getting Started

1. **Open [Remix](https://remix.ethereum.org)** to start developing your contracts in the browser.
2. **Import the required interfaces** from the Strategy Builder Plugin repository:  
   👉 [https://github.com/3Blocks-net/strategy-builder-plugin](https://github.com/3Blocks-net/strategy-builder-plugin)

---

### 🧩 Action Contracts

To create a compatible **action contract**, make sure to:

- Implement the `IAction` interface from the plugin repo.
- If your action needs to **allocate additional fees to nodes**, also implement the `ITokenIdGetter` interface.

---

### ✅ Condition Contracts

To build a custom **condition contract**, you can simply:

- Inherit from the `BaseCondition` contract provided in the plugin repo.

This will ensure compatibility with the Strategy Builder Plugin and reduce the need for repetitive boilerplate code.

---

Let me know if you'd like to add an example snippet or deployment instructions!
