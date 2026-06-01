---
title: Fee Overview
icon: note
order: 100
tags: [fees, costs, pricing, strategy fees, action fees]
---

# Fee Overview

Pecunity charges fees when strategies execute actions on the blockchain. This page explains how fees work and how much each action costs.

---

## How fees work

Each action in a strategy (swap, supply, borrow, etc.) has its own fee. The **total strategy fee** is the sum of all individual action fees.

Fees are calculated as a **percentage of the transaction amount**, with a **minimum fee** per action to cover gas and infrastructure costs.

---

## Action fees

| Action | Type | Fee | Minimum |
| --- | --- | --- | --- |
| Swap | Deposit | 0.3% | $0.40 |
| Supply to lending market | Deposit | 1.0% | $0.40 |
| Withdraw from lending market | Withdraw | 1.0% | $0.70 |
| Borrow from lending market | Deposit | 1.0% | $0.40 |
| Repay lending position | Withdraw | 1.0% | $0.70 |
| Provide liquidity | Deposit | 0.1% | $0.40 |
| Remove liquidity | Withdraw | 0.1% | $0.70 |
| Collect LP fees | Reward | 15.0% | $1.00 |

---

## One-time fees

| Action | Fee |
| --- | --- |
| Attach a strategy | $1.00 |
| Attach a rule | $0.50 |

---

![Fee distribution](/static/fee-distribution.svg)

## Fee distribution

When fees are paid, they are distributed across the ecosystem:

+++ Pay with $PEC (20% discount)
| Recipient | Share |
| --- | --- |
| **Burned** (permanently removed) | 40% |
| **Automation Server** | 27.5% |
| **Treasury** | 20% |
| **Strategy Creator** | 10% |
+++ Pay with other tokens
| Recipient | Share |
| --- | --- |
| **Burned** (converted to $PEC and burned) | 50% |
| **Automation Server** | 27.5% |
| **Strategy Creator** | 10% |
| **Treasury** | 10% |
+++

---

## Example

Suppose a strategy collects $10.00 in LP fees (15% fee = $1.50):

+++ Pay with $PEC
- Fee after 20% discount: **$1.20**
- $0.48 burned
- $0.33 to automation
- $0.24 to treasury
- $0.12 to strategy creator
+++ Pay with USDT
- Fee: **$1.50** (no discount)
- $0.75 converted to $PEC and burned
- $0.41 to automation
- $0.15 to strategy creator
- $0.15 to treasury
+++

[!ref How to reduce your fees](/fees/discounts/)
