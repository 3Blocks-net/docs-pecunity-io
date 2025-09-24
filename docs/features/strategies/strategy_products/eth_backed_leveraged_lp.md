# ETH-Backed Leveraged LP Strategy

## Introduction

This strategy leverages ETH as collateral on AaveV3 to borrow stablecoins (e.g., USDC), which are paired with ETH in a Uniswap V3 ETH/USDC liquidity pool using a concentrated price range (±10%). The impermanent loss (IL) exposure is hedged by opening and dynamically managing a short ETH perpetual position on a derivatives exchange. Advanced automation ensures continuous rebalancing of both LP exposure and short hedge for optimal performance.

- **Collateral Token:** ETH (supplied to AaveV3)
- **Borrowed:** Stablecoin (e.g., USDC at 80% LTV)
- **LP Pool:** Uniswap V3 ETH/USDC, 10% range around spot
- **Hedge:** Short ETH perp on a decentralized exchange (e.g., dYdX, Aevo, GMX)
- **Automation:** Auto-rebalance LP position and hedge, auto-compounding of LP fees

## Strategy Workflow

1. Deposit ETH as collateral on AaveV3.
2. Borrow up to 80% of ETH value in a stablecoin.
3. Provide both ETH and borrowed stablecoin as liquidity to the Uniswap V3 ETH/USDC pool, setting a narrow price range of ±10% around the current ETH/USDC price.
4. Open a short ETH perpetual position (equal to total LP ETH exposure) to hedge against ETH price movements and mitigate impermanent loss.
5. Continuously and automatically:
   - Rebalance the Uniswap LP if price drifts outside the set range (concentrated liquidity management).
   - Adjust the short position to precisely match the changing ETH exposure in the LP (hedge rebalancing).
   - Compound LP fees periodically back into the strategy.

## Automated Rebalancing Details

- **LP Range Rebalancing:** Automated bots or tools (Krystal, Orphic, Gelato) move LP positions if the ETH price exits the ±10% band, ensuring consistent fee earning.
- **Hedge Recalibration:** At each rebalance, the short position is updated to equal the notional ETH in the LP—whether that grows (due to trading) or shrinks (due to swaps or price drift).
- **Compounding:** Collected trading fees are automatically restaked into the LP position to boost yield.

## Yield (APY) Calculation

### APY Estimation (2025 conditions)

| Parameter            | Value                       |
| -------------------- | --------------------------- |
| ETH Collateral       | $10,000 in ETH              |
| LTV                  | 80% (borrow $8,000 USDC)    |
| LP Fees (±10% range) | 18–30% APY (narrow, active) |
| Borrow APY (Aave)    | -5% APY                     |
| Perp Funding (Short) | -3% APY (estimate)          |
| Net Expected APY     | 10–20% APY                  |

- **LP fee APY:** Providing concentrated liquidity within ±10% of spot increases fee capture dramatically (recent data: 18%–30% APY possible, depending on volatility and pool volume) .
- **Borrowing Cost:** At 80% LTV, borrowing USDC costs ~5% APY (Aave V3 average 2025) .
- **Perp Funding Cost:** Short hedge typically incurs ~3% APY in funding payments; may vary by exchange .
- **Net APY Formula:**

  $$
  \text{Net APY} = \text{LP Fees} - \text{Aave Borrow Rate} - \text{Perp Funding Rate}
  $$

  For example, $$ 25\% - 5\% - 3\% = 17\% $$ APY. Periodic auto-compounding can boost effective yield to ~20% APY if reinvested efficiently.

#### Key Assumptions

- The LP remains in-range most of the time due to active rebalancing.
- Sufficient liquidity for shorting ETH on perps with low slippage.
- Aave V3 and Uniswap fee rates stay consistent with summer/fall 2025 averages.
- Auto-compounding and rebalancing efficiency do not incur excessive gas or protocol costs.

## Risks

- **Liquidation Risk:** At 80% LTV, a small drop in ETH price can lead to collateral liquidation on Aave—monitor Health Factor closely or use automation for deleveraging.
- **Concentration/Volatility:** Narrow LP ranges can lead to capital being sidelined ("out of range") if ETH/USD experiences sharp moves.
- **Imperfect Hedge:** LP and hedge exposures may diverge under certain market conditions or slippage, causing tracking error.
- **Funding Rate Swings:** If short perp funding spikes, strategy net yield can drop sharply.
- **Automation Failure:** Automated bots for rebalancing/hedging must be reliable to prevent unhedged exposure or missed fee opportunities.

## Summary

This strategy uses an aggressive 80% ETH LTV to maximize LP capital efficiency in a ±10% Uniswap V3 ETH/USDC range, while employing dynamic short hedging to neutralize price risk. With proactive management, APYs of 10–20% (occasionally higher) are feasible in 2025, provided volatility and trading volume remain strong and automation is robust.

---

_Sources: Uniswap V3 and DeFi fee analytics, Galaxy Onchain Yield Reports, Aave V3 Borrow Rate Statistics, Perp DEX funding data (Q3 2025)_
