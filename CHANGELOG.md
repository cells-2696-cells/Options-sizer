# Changelog

## [2.0.0] - 2026-09-28

### Added
- **ATR stop-loss correction (Auto mode, default):** Corrected SL = Swing SL − k × ATR, rounded down to the 0.05 tick, stepped below round levels (multiples of 5).
- **Strike position selector:** ITM+2, ITM+1, ATM, OTM+1, OTM+2 set the strike factor in k.
- **Automatic expiry and DTE:** NIFTY Tuesday, SENSEX Thursday, rolls to next week after 15:30 on expiry day. Tap to override for holiday weeks.
- **Manual mode:** SL as % of premium, same as v1.1.
- **R:R profit estimator:** 1:2 (default) to 1:5, with the target based on the swing SL risk.
- **Realised R:R:** reward measured against the corrected SL risk; amber below 1:1.5.
- **Highlighted SL / target row** at the top of the results.
- **Start-of-day card:** instrument, risk per trade, expiry.
- **Warnings:** SL within 1.5 ATR of entry, 1 lot over budget, swing SL above entry.
- **Version toggle** in the header to switch between v1.1 and v2.0.

### Changed
- v2.0 instruments limited to NIFTY (lot 65) and SENSEX (lot 20).
- Lots are sized from the corrected SL. When 1 lot exceeds the risk budget, 0 lots are shown with a warning instead of forcing 1.
- "Target premium" in the profit card replaced by "Exit value" (the target moved to the highlight row).
- Service worker cache renamed to `optionsizer-v2.0.0`.

### Unchanged
- v1.1 calculator, its saved defaults, and offline support.

## [1.1.0]
- Save all inputs as persistent defaults.

## [1.0.0]
- Initial release.
