# OptionSizer

Options position sizing calculator for NIFTY and SENSEX index options, built as an offline-first iPhone web app.

- **v2.0** (default): ATR + DTE based stop-loss correction, lots sized from the corrected SL, R:R targets with realised R:R.
- **v1.1**: the original %-based calculator (NIFTY, BANKNIFTY, FINNIFTY, SENSEX), still available from the toggle at the top.

## 🚀 Open the App

👉 https://cells-2696-cells.github.io/Options-sizer

## 📲 Install on iPhone

1. Open the link above in **Safari**
2. Tap the **Share ↑** button
3. Tap **Add to Home Screen**
4. Tap **Add**

Works fully offline once installed. After an update, close the app and open it again once to load the new version.

---

## Using v2.0

### 1. Start of day
Set once each morning, then **Save as default**:

| Setting | Notes |
|---|---|
| Instrument | NIFTY (lot 65, Tuesday expiry) or SENSEX (lot 20, Thursday expiry) |
| Risk per trade | Fixed ₹ amount you are willing to lose on one trade |
| Expiry / DTE | Calculated automatically. Tap to override (auto → 0 → 1 → 2 → 3+) when an exchange holiday moves the expiry |

### 2. Stop loss

**Auto (ATR)**: default
1. Tap the strike's position in the option chain: ITM+2, ITM+1, ATM, OTM+1, OTM+2
2. Enter the entry premium
3. Enter the swing SL (previous swing low of the premium)
4. Enter ATR(14) from the option's 1-minute chart, read at the entry candle

**Manual (%)**: enter the SL as a % of premium, same as v1.1.

### 3. Profit estimator
- **R:R**: 1:2 (default), 1:3, 1:4, 1:5
- **% gain**: target as a % rise in premium, same as v1.1

The SL and target are shown side by side at the top of the results.

---

## How the SL is calculated (Auto mode)

```
Corrected SL = Swing SL − k × ATR
k            = strike factor × DTE factor
```

| Strike position | Strike factor |
|---|---|
| ITM+2 | 0.75 |
| ITM+1 | 0.85 |
| ATM | 1.00 |
| OTM+1 | 1.15 |
| OTM+2 | 1.30 |

| Days to expiry | DTE factor |
|---|---|
| 3 or more | 1.00 |
| 1–2 | 1.15 |
| Expiry day | 1.30 |

Then:
- **Round-number rule**: if the SL lands within 0.25 above a multiple of 5 (e.g. 70.10), it moves to just below it (69.95), because stops cluster at round levels.
- **Tick rounding**: rounded down to the 0.05 tick.

**Why:** a stop placed exactly at the swing low sits where everyone's stops are, and a normal 1-minute wick sweeps it. The ATR buffer puts the stop outside that noise. OTM strikes and expiry days are noisier, so they get a wider buffer.

### ATR settings
On the option's 1-minute chart: **ATR, length 14, smoothing RMA**. Use the value at the entry candle and do not update it during the trade.

---

## Lots, target and realised R:R

| Output | Formula |
|---|---|
| Lots | floor( risk per trade ÷ ((entry − corrected SL) × lot size) ) |
| Target (R:R, Auto) | entry + R × (entry − **swing SL**) |
| Target (R:R, Manual) | entry + R × (entry − SL) |
| Realised R:R | (target − entry) ÷ (entry − **corrected SL**) |

The target is planned on the swing SL, which is your chart-based risk. The realised R:R shows what that target is worth against the wider corrected SL. It turns amber below 1:1.5.

### Warnings
- **SL closer than 1.5 ATR to entry**: the stop is inside normal noise. Consider skipping.
- **1 lot exceeds your risk**: shows 0 lots. No trade at this SL.
- **Swing SL above entry premium**: invalid input.

### Worked example
NIFTY 23050 PE, 25 Sep 2026: entry 75.90, swing SL 70.35, ATR 2.92, ATM, DTE 2, risk ₹10,000.

| | Value |
|---|---|
| k | 1.00 × 1.15 = 1.15 |
| Corrected SL | 70.35 − 1.15 × 2.92 = 66.99 → **66.95** |
| Risk per unit | 8.95 pts (3.1 ATR) |
| Lots | 10,000 ÷ (8.95 × 65) = **17** |
| Target at 1:2 | 75.90 + 2 × 5.55 = **87.00** |
| Realised R:R | 11.10 ÷ 8.95 = **1:1.24** |

The premium dipped to 70.15, taking out a stop at the swing low of 70.35, and then rallied to 92. The corrected SL stayed in the trade.

---

## Releases

| Version | What's new |
|---------|-----------|
| v2.0.0 | ATR + DTE corrected SL, strike position factor, auto DTE, lots from corrected SL, R:R targets with realised R:R, SL/target highlight row, v1.1 ⇄ v2.0 toggle |
| v1.1.0 | Save all inputs as persistent defaults |
| v1.0.0 | Initial release |

See [CHANGELOG.md](CHANGELOG.md) for details.
