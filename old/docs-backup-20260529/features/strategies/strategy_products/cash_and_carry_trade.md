# Cash-and-Carry Strategy

## Strategy Overview

This strategy uses a delta-neutral position on the HYPE token with two key components:

- **Long Position:** Invest $75,000 in a Pendle Principal Token (PT) for HYPE, representing a fixed yield of 12% APY. The PT token locks in yield on HYPE, providing a steady income stream.
- **Short Position:** Open a leveraged short position on HYPE perpetual contracts with $25,000 capital at 3x leverage (equivalent to $75,000 exposure) on a decentralized perpetual futures exchange (Perp DEX). The short position offsets the long exposure, aiming to achieve delta-neutrality and capture a high funding rate of approximately 30% APR.

Together, the longs and shorts offset price risk on HYPE while generating income from fixed PT yields and high perpetual funding payments.

---

## Capital Allocation Summary

| Position       | Capital | Leverage | Effective Exposure | Yield / Funding Rate APY |
| -------------- | ------- | -------- | ------------------ | ------------------------ |
| Pendle PT Long | $75,000 | 1x       | $75,000            | 12% APY (fixed yield)    |
| Perp Dex Short | $25,000 | 3x       | $75,000            | ~30% APR (funding rate)  |

---

## APR Calculation

### Step 1: Pendle PT Yield (HYPE)

- $75,000 × 12\% = $9,000$ annual yield from HYPE PT token

### Step 2: Perpetual Short Funding Payments

- Position size on perp = $25,000 × 3 = $75,000$
- Funding received from short position = $75,000 × 30\% = $22,500$ annual

### Step 3: Combine Yields and Capital

- Total capital deployed: $100,000$
- Total annual return: $9,000 + 22,500 = 31,500$
- **Overall APR:**

$$
\frac{31,500}{100,000} = 31,5\% \text{ APR}
$$

---

## Strategy Characteristics

- **Delta Neutral:** Long and short positions offset price movement risk in HYPE token.
- **Yield Generation:** Income generated from fixed PT yields on the long side plus funding payments on leveraged short.
- **Leverage Use:** 3x leverage on the short position to match long exposure.
- **Risk Management:** Active monitoring and rebalancing to maintain delta-neutrality, manage funding rate changes, and mitigate liquidation risk.
- **Capital Efficiency:** Optimizes returns by combining fixed income and leveraged perp funding on HYPE token.

---

## Summary

This delta-neutral cash-and-carry strategy on HYPE token combines $75k in yield-bearing Pendle PT tokens at 12% fixed APY with a 3x leveraged $25k short perpetual position capturing ~30% funding APR, delivering an overall estimated APR of about 31% on $100k principal, while minimizing directional price risk.

---

_APR estimates are based on current market data for HYPE PT yields and perpetual funding rates in 2025._
