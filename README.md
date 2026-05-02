# ASX Markowitz Portfolio Optimisation

A Python implementation of Modern Portfolio Theory (MPT) applied to ASX 50 stocks and ETFs. This project constructs an efficient frontier, identifies the optimal portfolio by maximising the Sharpe ratio, and visualises the risk/return tradeoff across thousands of simulated portfolios.

---

## Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/craizychicken/Portfolio_Theory.git
cd Portfolio_Theory
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the notebook
```bash
jupyter notebook notebooks/markowitz.ipynb
```

---

## Requirements

```
yfinance
pandas
numpy
matplotlib
seaborn
scipy
jupyter
```

---

## Methodology

### Universe
- **ASX 50 stocks** — top 50 Australian listed companies by market capitalisation
- **ETFs** — selected for low correlation and asset class diversification (A200, NDQ, QAU, AAA, ETHI, HBRD, QPON, CRED)

### Data Pipeline
1. Download 5 years of adjusted daily close prices via `yfinance`
2. Fill missing values in returns with 0
3. Drop any stock missing more than 5% of trading days
4. Compute simple (percentage) returns — used because portfolio return is a weighted sum of asset returns
5. Annualise returns (`× 252`) and covariance (`× 252`)

### Portfolio Construction
- **100,000 random portfolios** simulated using Dirichlet-distributed weights
  - 80% with `alpha=0.4` — concentrated portfolios to trace frontier edges
  - 20% with `alpha=5` — near-equal-weight portfolios to fill the left side
- **Efficient frontier** extracted by binning portfolios by risk and taking the maximum return in each bin
- **Optimal portfolio** identified as the portfolio with the highest Sharpe ratio

### Key Formulas

**Expected portfolio return:**

$$E(R_p) = \sum_{i=1}^{n} w_i E(R_i) = \mathbf{w}^\top \boldsymbol{\mu}$$

**Portfolio variance:**

$$\sigma_p^2 = \sum_{i=1}^{n} \sum_{j=1}^{n} w_i w_j \sigma_{ij} = \mathbf{w}^\top \boldsymbol{\Sigma} \mathbf{w}$$

**Sharpe ratio:**

$$S = \frac{E(R_p) - R_f}{\sigma_p}$$

---

## Results

### Correlation Matrix (VAT Ordered)
Stocks reordered by hierarchical clustering to reveal sector groupings. Clear correlation blocks visible across financials, materials, healthcare, and REITs. ETFs (QAU, AAA, HBRD) show near-zero correlation with equities — key diversification drivers.

### Efficient Frontier
![Efficient Frontier](outputs/efficient_frontier.png)

| Portfolio | Annual Return | Annual Std | Sharpe Ratio |
|---|---|---|---|
| Min Variance | ~5% | ~4.5% | — |
| Max Sharpe | 12.71% | 9.92% | 0.77 |

### Optimal Portfolio Weights (Top 10)

| Stock | Weight | Sector |
|---|---|---|
| BXB | 16.6% | Industrials |
| QAU | 15.4% | Gold ETF |
| IAG | 9.3% | Insurance |
| AMC | 6.4% | Packaging |
| ORG | 4.9% | Energy |
| QPON | 4.3% | Fixed Income ETF |
| TLS | 4.3% | Telecommunications |
| NAB | 4.0% | Financials |
| CAR | 3.7% | Technology |
| CPU | 2.9% | Technology |

---

## Limitations

- **In-sample optimisation** — weights are optimised on the same data used to evaluate them. Past performance does not guarantee future results
- **Estimation error** — expected returns are estimated from historical means, which are noisy and sensitive to the time period chosen
- **Random simulation** — Max Sharpe portfolio is the best randomly sampled portfolio, not the mathematical optimum. A formal solver (`scipy.minimize`) would give exact results
- **No transaction costs** — real portfolios incur brokerage, bid/ask spreads, and capital gains tax
- **Static weights** — no rebalancing is modelled; weights drift as prices change

---

## Disclaimer

This project is for **educational purposes only**. Nothing in this repository constitutes financial advice. Always consult a licensed financial adviser before making investment decisions.

---

## References

- Markowitz, H. (1952). *Portfolio Selection*. Journal of Finance, 7(1), 77–91.
- [Modern Portfolio Theory — Wikipedia](https://en.wikipedia.org/wiki/Modern_portfolio_theory)
- [ASX Listed Securities](https://www.asx.com.au)
- [yfinance Documentation](https://pypi.org/project/yfinance/)