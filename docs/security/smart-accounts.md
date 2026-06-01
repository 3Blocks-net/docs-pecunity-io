---
title: Smart Accounts
icon: shield-lock
order: 100
tags: [security, smart account, account abstraction, wallet, ERC-4337]
---

# Smart Accounts

Your Pecunity account is powered by a **smart contract account** built on ERC-4337 (Account Abstraction). This gives you the convenience of a regular web app login with the security of blockchain technology.

---

## How it works

When you sign in with Google, Facebook, email, or a passkey, Pecunity creates a smart contract wallet for you on the blockchain. This wallet is controlled by your login credentials, not by a private key you need to manage.

### What makes it different from a regular wallet?

| Feature | Traditional wallet | Pecunity smart account |
| --- | --- | --- |
| Login | Seed phrase / private key | Google, email, passkey |
| Gas fees | You pay in native token | Sponsored (free for you) |
| Batch transactions | One by one | Multiple actions in one |
| Recovery | Lose seed phrase = lose everything | Recover via login method |
| Security upgrades | Fixed | Upgradeable smart contract |

---

![Security layers](/static/smart-account-layers.svg)

## Security features

### Multi-Factor Authentication (MFA)

Add an extra layer of protection to your account. When MFA is enabled, you need to verify your identity with a second factor before sensitive operations.

### Passkeys

Add a passkey as an additional login method. Passkeys use your device's biometrics (Face ID, fingerprint, or PIN) and are phishing-resistant.

### Account transfer

If you need to move your account to a different owner, you can safely transfer ownership through the settings.

---

## Your strategy vaults

When you activate a strategy, your funds are held in a separate **strategy vault** — an independent smart contract. This means:

- Strategy funds are isolated from your main wallet balance
- Each strategy has its own vault address
- You can withdraw from a vault at any time

---

## Sponsored transactions

Pecunity sponsors your gas fees through a policy engine. You don't need to hold BNB or any other native token to interact with the blockchain. The platform covers these costs for you.
