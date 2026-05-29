---
title: Buy Crypto
icon: credit-card
order: 90
tags: [buy, fiat, on-ramp, DFX, bank transfer, deposit, USDT, BTC]
---

# Buy Crypto

You can buy crypto directly within Pecunity using a bank transfer. This feature is powered by **DFX**, a regulated Swiss on-ramp service.

---

```mermaid
graph LR
    A["Open<br/>Buy Crypto"] --> B["Select Token<br/>USDT or BTC"]
    B --> C["Complete<br/>Bank Transfer"]
    C --> D["Tokens arrive<br/>in your wallet"]
    style A fill:#1A3FBA,color:#fff,stroke:none,rx:8
    style B fill:#4568D0,color:#fff,stroke:none,rx:8
    style C fill:#81A1EE,color:#fff,stroke:none,rx:8
    style D fill:#A4BDFA,color:#1E2A4A,stroke:none,rx:8
```

## How it works

1. Open **Buy Crypto** from the sidebar menu
2. Select the token you want to buy (USDT or BTC)
3. Follow the DFX widget instructions to complete your bank transfer
4. Once the payment is processed, the tokens appear in your Pecunity wallet

!!!
Your smart account wallet must be deployed before you can use the Buy Crypto feature. If it hasn't been deployed yet, Pecunity will guide you through this step automatically.
!!!

---

## Supported tokens

| Token | Network |
| --- | --- |
| USDT (Tether) | BNB Smart Chain |
| BTC (Bitcoin BEP-20) | BNB Smart Chain |

---

## Sell crypto

You can also sell your crypto back to fiat currency using the **Sell Crypto** feature in the sidebar. The same DFX service handles the off-ramp process.

---

## Alternative: Transfer from another wallet

If you prefer, you can send tokens directly from any external wallet to your Pecunity address. Your address is available in **Settings** or by clicking **Receive** in the Collectibles section.

Pecunity operates on the **BNB Smart Chain**. Make sure to send tokens on the correct network to avoid losing funds.
