---
name: news-playbook
description: How gold, Nasdaq 100, EUR/USD and Bitcoin react to the big US releases (Fed rate decisions, the monthly jobs report, inflation), measured on a year of hourly candles, with the no-entry windows to apply. Use before trading near a scheduled release, when setting a news window for an agent, when someone asks whether to trade the Fed / NFP / CPI, or when explaining a sudden spike.
---

# News playbook

Answer in the language the person writes in.

Measured by cubrik on TradingView hourly candles (OANDA for gold, Nasdaq 100 and EUR/USD; Binance for BTC/USDT), October 2025 to October 2026. "×normal" compares the release hour with the same hour on ordinary days. Past reactions describe how these markets have tended to move; they do not predict the next release or its direction.

Always read the live schedule with `get_economic_calendar` (`min_impact: high`, with the traded symbols): dates move, and holidays or a government shutdown can delay a release.

## Fed rate decisions (FOMC)

Released at 14:00 New York time (18:00 UTC in US summer time, 19:00 UTC in winter), followed 30 minutes later by the press conference. Eight meetings measured, from 29 Oct 2025 to 16 Sep 2026 (only five for EUR/USD and six for Bitcoin, whose data starts later).

| Market | Release hour vs normal (median) | Range over the 2 hours (median) | Biggest 2-hour range | Second hour reversed the first |
|---|---|---|---|---|
| Gold | ×2.5 | 1.9% (≈ $76) | 3.7% (17 Jun 2026) | 3 of 8 |
| Nasdaq 100 | ×2.0 | 1.1% | 2.8% (29 Jul 2026) | 4 of 8 |
| EUR/USD | ×3.5 | 0.68% (≈ 76 pips) | 1.02% (17 Jun 2026) | 3 of 5 |
| Bitcoin | ×2.0 | 1.8% | 3.2% (17 Jun 2026) | 3 of 6 |

- **No reliable direction.** Four hours after the decision, gold was up after four meetings and down after four, with moves from 0.1% to 4.4%.
- **Fake first moves are common.** In about 40–60% of meetings the hour after the decision went the other way from the decision hour (the press conference often reverses the first reaction).

## US jobs report (Non-Farm Payrolls)

Usually the first Friday of the month at 08:30 New York time (12:30 UTC in summer, 13:30 UTC in winter). Measured on the months where the release did print that Friday (holiday and shutdown months removed).

| Market | Release hour vs normal Friday (median) | Absolute move 4 hours later (median) | Largest release-hour range |
|---|---|---|---|
| Gold | ×1.9 | 1.1% | 2.2% (4 Sep 2026) |
| Nasdaq 100 | ×1.5 | — (the cash market opens one hour later) | 1.1% (6 Mar 2026) |
| EUR/USD | ×2.2 | 0.2% | 0.49% (5 Jun 2026) |
| Bitcoin | ×1.4 | 1.7% | 2.7% (4 Sep 2026) |

- When the report surprises, the release hour can reach ×2.5 to ×3.5 normal (gold 2.2% on 4 Sep 2026, EUR/USD 0.49% on 5 Jun 2026).
- On 5 Jun 2026 gold fell 3.0% in the four hours after the report: the first move kept going.

## Inflation (CPI, PCE) and other high-impact prints

US CPI comes out mid-month at 08:30 New York time; PCE at the end of the month. They were not measured individually here because their dates vary, but they belong with the jobs report in the "big print" group and are treated the same way below.

## No-entry windows to apply

| Event | No new position | Notes |
|---|---|---|
| Fed decision + press conference | 60 min before → 90 min after | Covers the decision and the press-conference reversal |
| CPI, PCE, jobs report, Fed chair speech | 30 min before → 60 min after | |
| Other high-impact release on the traded currency | 30 min before → 30 min after | |
| Medium impact | No window; size normally | Mention it in the journal |

- **Open positions keep their stop and target through the event.** Do not remove a stop "to survive the spike". Do not move it closer just before the release either: the spread widens and the first tick can skip it.
- **Size down if you must hold through it.** A stop smaller than the median release range above (gold: ≈1.9% over two hours on Fed days) is likely to be hit by the reaction alone.
- **Wait for the second hour.** If a strategy trades the reaction, wait for the first hourly candle after the release (and after the press conference for the Fed) to close, then apply the normal rules: the first move reversed in roughly half of the Fed meetings measured.
- **Map the currency to the instrument.** USD events move gold, the Nasdaq, the S&P 500, Bitcoin and every USD pair. EUR events move EUR/USD. Check the calendar for every currency the agent trades.

## In a cubrik agent

Set the agent's own **"Quiet time before high-impact news"** limit (60 minutes covers every row above), and keep the rule in the instruction or in a skill so the agent also respects the after-release part, which the limit does not cover.

This playbook describes past behaviour; it is not financial advice and does not predict future moves.
