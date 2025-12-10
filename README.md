# Building a Managed Futures Trend-Following Strategy

## Overview

This project develops a full managed-futures (CTA) trend-following system using a multi-asset dataset spanning FX, Equities, Fixed Income, and Commodities from 1969–2014. The strategy builds on Time-Series Momentum (TSMOM) and is progressively enhanced with volatility targeting, cross-sectional ranking, shrinkage-based covariance estimation, and risk-budgeting across asset classes.

The result is a robust trend-following framework that outperforms static benchmarks and aligns closely with institutional CTA behavior.

## Objective

- Construct a baseline TSMOM strategy using monthly multi-asset returns.  
- Improve the signal through optimal lookback windows, volatility estimation, and cross-sectional ranking.  
- Apply realistic portfolio construction: volatility targeting, shrinkage covariance, and risk budgeting.  
- Compare performance to static benchmarks and industry CTA indices (SG Trend).  
- Evaluate robustness, predictive validity, and out-of-sample stability.

## Dataset

- **Asset Classes:** FX, EQ, FI, COMM  
- **Data:** Monthly returns (1969–2014)  
- **Instrument Mapping:** IDs, names, currencies, asset classes  
- **Filtering:** Months with <10 instruments removed  
- **Handling Missing Data:** Ignored in cross-sectional averages
  
## Baseline Strategy (TSMOM)

The baseline strategy uses:

- **12-month cumulative return** as the momentum signal  
- **Look-ahead safe shifting** (signal uses t–1 data)  
- **Positioning:**  
  - Long if past 12-month return > 0  
  - Short if < 0  
- **Volatility Scaling:**  
  - Full-sample vol, annualized  
  - Target asset vol = **40%**  
- **Portfolio Vol Targeting:** Achieved through risk-scaling  
- **Benchmark:** Static long-only with equal magnitude

Performance 

| Strategy | Ann Return | Ann Vol | Sharpe |
|----------|------------|---------|--------|
| TSMOM | 14.65% | 13.71% | 1.07 |
| Static | 9.22% | 18.30% | 0.50 |

## Sensitivity Analysis (V1)

To find stronger parameters, the strategy is tested across:

- Momentum lookback windows: **1–12 months**  
- Target vol levels: **10%–100%**  
- Vol estimation methods: full-sample, rolling, expanding  

Heatmaps show the best zone at:

- **11-month momentum lookback**  
- **12-month rolling volatility window**  
- **14% portfolio volatility target**

Optimized performance:

| Strategy | Ann Return | Ann Vol | Sharpe |
|----------|------------|---------|--------|
| Optimal V1 | 18.91% | 11.60% | 1.63 |
| Static | 6.23% | 15.46% | 0.40 |

## Cross-Sectional Enhancement

Momentum’s predictive power appears stronger in *relative* terms:  
Assets with high momentum outperform their lower-ranked counterparts.

The upgraded rule:

- Long if sign positive **and** percentile ≥ 60  
- Short if sign negative **and** percentile ≤ 40  

Results:

| Portfolio | Ann Return | Ann Vol | Sharpe |
|-----------|------------|---------|--------|
| Long | 6.73% | 9.75% | 0.69 |
| Short | -2.43% | 11.18% | -0.22 |

This confirms that cross-sectional momentum meaningfully strengthens the signal.

## Volatility Forecasting & Risk Estimation

Rolling windows:

- Best predictor of next-month volatility → **9-month window**

EWMA comparison:

- Best halflife → **3–4 months**

Both methods show significant predictive power across assets.

## Shrinkage, Covariance, and Risk Control

Removing shrinkage increases raw returns but raises:

- Leverage  
- Turnover  
- Transaction costs  
- Instability  

Adding **asset-class risk budgeting** stabilizes exposure and reduces concentration in Commodities (page 18).

## Final Optimal Strategy (V2)

Combines:

- Rolling 9-month volatility  
- 11-month TSMOM signal  
- Cross-sectional ranking  
- Ledoit–Wolf shrinkage  
- Asset-class risk budgeting  
- Portfolio target vol: **14%**

Performance:

| Metric | TSMOM V2 | Static |
|--------|-----------|--------|
| Ann Return (Net) | **15.66%** | 5.63% |
| Ann Vol | 11.85% | 15.59% |
| Sharpe (Net) | **1.32** | 0.36 |

## Industry Benchmark Comparison (SG Trend)

| Strategy | Sharpe | Corr w/ SG Trend |
|----------|--------|-------------------|
| TSMOM Base | 0.91 | 0.63 |
| TSMOM V1 | **1.52** | 0.64 |
| TSMOM V2 | **1.42** | 0.67 |
| SG Trend | 0.47 | 1.00 |

The strategy behaves similarly to institutional CTAs but with far stronger returns.

## Out-of-Sample Performance

Final OOS results:

| Strategy | Ann Return | Ann Vol | Sharpe |
|----------|------------|---------|--------|
| TSMOM V2 | 5.05% | 9.59% | 0.53 |
| SG Trend | 2.74% | 11.57% | 0.24 |

Even out-of-sample, V2 remains robust and aligned with real-world CTA behavior.

## Key Takeaways

- Time-Series Momentum is robust across 45+ years and 4 major asset classes.  
- Momentum’s power is **cross-sectional**, not just directional.  
- Rolling volatility (9m) and EWMA (3–4m) provide strong forecasting ability.  
- Shrinkage and risk-budgeting stabilize signals and reduce noise.  
- Enhanced TSMOM (V2) achieves **higher Sharpe, lower vol**, and **CTA-like behaviour**.

## Conclusion

A carefully engineered TSMOM strategy—combining momentum, cross-sectional ranking, smart volatility estimation, and robust risk controls—produces strong, persistent returns across decades. The final strategy rivals or exceeds institutional trend-following benchmarks while maintaining low correlation to traditional long-only exposures.

