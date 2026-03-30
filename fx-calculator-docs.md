# FX Risk & Reward Calculator — Documentation

**File:** `fx.html`  
**Last Updated:** 2026-03-30  
**Hosted at:** [francismesina.github.io/fx.html](https://francismesina.github.io/fx.html)

---

## Overview

A single-page, no-dependency forex trading tool with three panels:

1. **Lot Size Calculator** — computes position size based on risk parameters
2. **Calculator** — general-purpose arithmetic calculator
3. **Market Clocks** — live session times for major forex markets

---

## Panel 1: Lot Size Calculator

### Inputs

| Field | Description |
|---|---|
| Account Balance ($) | Your total account equity in USD |
| Risk Method | **Fixed $** — risk a fixed dollar amount; **% of Bal** — risk a percentage of balance |
| Risk Value | Dollar amount or percentage depending on method selected |
| Stop Loss (Pips) | Distance in pips from entry to stop loss |
| Target (Pips) | Distance in pips from entry to take profit |
| Asset Type | Selects pip value per standard lot (see Asset Types below) |
| Contract Size | Standard / Mini / Micro — scales pip value and lot size |

### Outputs

| Output | Description |
|---|---|
| Recommended Lot Size | Position size to use in your broker order |
| Risk Amount | Actual USD at risk based on inputs |
| Potential Profit | Expected USD profit if TP is hit |
| Pip Value / Lot | USD value of 1 pip for the selected contract size |
| R:R Ratio | Risk-to-reward ratio (e.g. 1:1.65) |

### Formulas

```
Risk USD       = Fixed $ input  OR  (Balance × Risk%)

Lot Size       = (Risk USD ÷ SL Pips ÷ Pip Value per Std Lot) × Contract Multiplier

Potential Profit = (Lot Size ÷ Contract Multiplier) × TP Pips × Pip Value per Std Lot

R:R Ratio      = TP Pips ÷ SL Pips

Pip Value/Lot  = Pip Value per Std Lot ÷ Contract Multiplier
```

### Contract Size Multipliers

| Contract | Units | Multiplier |
|---|---|---|
| Standard | 100,000 | 1 |
| Mini | 10,000 | 10 |
| Micro | 1,000 | 100 |

> XM micro account uses **Micro (1k units)**. Lot sizes will be in the 10–50 range for small accounts.

---

## Asset Types

### Forex Majors (Fixed pip values)

| Asset | Pip Value (Std Lot) | Pip Value (Micro Lot) | Notes |
|---|---|---|---|
| EURUSD, GBPUSD, AUDUSD, NZDUSD | $10.00 | $0.10 | Quote currency = USD. Exact. |
| XAUUSD (Gold) | $1.00 | $0.01 | XM contract: 100 oz × $0.01/pip |
| XAGUSD (Silver) | $5.00 | $0.05 | XM contract: 5,000 oz × $0.001/pip |
| USOIL / BRENT (Oil) | $10.00 | $0.10 | XM contract: 1,000 barrels × $0.01/pip |

### Dynamic Pip Value Assets (Live Rate Fetched)

These pairs have USD as the **base** currency, so pip value depends on the current exchange rate. Values are fetched live on page load.

| Asset | Formula | Approx Pip Value (Std Lot) |
|---|---|---|
| JPY Pairs (USDJPY, EURJPY, GBPJPY) | `1000 ÷ USD/JPY rate` | ~$7.00 |
| USDCAD | `10 ÷ USD/CAD rate` | ~$7.25 |
| USDCHF | `10 ÷ USD/CHF rate` | ~$11.50 |

---

## Live Rate Fetching

**API:** [Frankfurter API](https://www.frankfurter.app/)  
**Endpoint:** `https://api.frankfurter.app/latest?from=USD&to=CAD,CHF,JPY`  
**Triggered:** On every page load  
**No API key required.** CORS-enabled. Rates sourced from the European Central Bank (ECB), updated daily.

### Behavior

- On success: dropdown labels update to show live pip value (e.g. `USDCAD ($7.21/pip, live)`), status shows green confirmation with rate date
- On failure (offline / API down): falls back to static approximate values, status shows red warning

---

## Panel 2: Calculator

A standard arithmetic calculator for quick manual calculations (e.g. computing pip distances from price levels).

**Keyboard support** (click the calculator card first to focus it):

| Key | Action |
|---|---|
| `0–9`, `.` | Input digits |
| `+`, `-`, `*`, `/` | Operators |
| `Enter` or `=` | Evaluate |
| `Backspace` | Delete last character |
| `C`, `Escape` | Clear |

---

## Panel 3: Market Clocks

Displays live local time for the four major forex trading sessions, updated every second.

| Market | Timezone |
|---|---|
| New York | America/New_York |
| London | Europe/London |
| Tokyo | Asia/Tokyo |
| Sydney | Australia/Sydney |

---

## Changelog

| Date | Change |
|---|---|
| 2026-03-30 | Added USDCAD, USDCHF, Silver, Oil to asset type dropdown with `<optgroup>` labels |
| 2026-03-30 | Added live rate fetching via Frankfurter API for JPY, CAD, CHF pairs |
| 2026-03-30 | Added rates status indicator below asset type dropdown |
| *(earlier)* | Initial build: lot size calculator, inline calculator, market clocks |

---

## Broker Notes (XM Micro Account)

- Symbol suffix `m#` (e.g. `EURUSDm#`) confirms micro account on XM
- Always select **Micro (1k units)** as contract size
- Leverage (e.g. 1000:1) does **not** affect lot size calculation — only determines how much margin is reserved
- For EURUSD micro: pip value = **$0.10 per lot** ✓
