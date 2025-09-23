---
title: Strategy Builder
icon: tools
order: 92
---

The **Strategy Builder** is the tool that allows you to create your own strategies and publish them if you wish.  
Using **drag & drop**, you can move and connect components. By linking these components together, a full strategy is built.

When building a strategy, there are two main types of components:

---

## Action

An **Action** is a blockchain operation, such as:

- Performing a swap
- Adding collateral
- Borrowing assets

Each Action can be connected to other components, forming a sequence of steps.

- Every Action has **one input** and **one output**.
- After each Action, only one component can follow.

---

## Condition

A **Condition** can be used in two ways:

1. **As a trigger** for a strategy

   - The strategy will only run if the condition is **true**.
   - In this case, the condition has only one output: **true**.

2. **As a branch inside a strategy**
   - The condition can be **true or false**.
   - Depending on the outcome, different Actions or further Conditions can follow.

---

With these components, you can design flexible and powerful strategies tailored to your needs.
