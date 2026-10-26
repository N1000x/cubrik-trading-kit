---
name: session-playbook
description: When each market actually moves — the trading sessions (Asia, London, New York) for gold, Nasdaq 100, EUR/USD and Bitcoin, measured on a year of hourly candles. Use when choosing the hours an agent should trade, setting a session filter, sizing a stop for the time of day, explaining why a market is quiet or wild right now, or when someone asks "when is the best time to trade X".
---

# Session playbook

Answer in the language the person writes in.

Measured by cubrik on TradingView hourly candles (OANDA for gold, Nasdaq 100 and EUR/USD; Binance for BTC/USDT). Ranges are high minus low as a percentage of the opening price, averaged over weekdays. Past behaviour describes how these markets have tended to move; it does not predict the next session.

| Market | Period measured | Hourly bars | Average daily range |
|---|---|---|---|
| Gold (XAU/USD) | 20 Oct 2025 → 26 Oct 2026 | 6,019 | 2.51% (≈ $100 at $4,000) |
| Nasdaq 100 | 20 Oct 2025 → 26 Oct 2026 | 6,013 | 1.84% (≈ 535 points at 29,000) |
| EUR/USD | 5 Feb 2026 → 26 Oct 2026 | 4,500 | 0.53% (≈ 59 pips at 1.12) |
| Bitcoin | 1 Jan 2026 → 26 Oct 2026 | 7,166 | 3.72% (≈ $3,000 at $80,000) |

## Sessions (all hours in UTC)

| Session | UTC hours | Gold | Nasdaq 100 | EUR/USD | Bitcoin |
|---|---|---|---|---|---|
| Asia | 00:00–07:00 | 1.36% · 58% | 0.70% · 41% | 0.22% · 44% | 1.76% · 51% |
| London morning | 07:00–12:00 | 0.93% · 40% | 0.63% · 37% | 0.26% · 53% | 1.38% · 41% |
| London/NY overlap | 12:00–16:00 | 1.36% · 59% | 1.15% · 66% | 0.30% · 60% | 2.12% · 61% |
| NY afternoon | 16:00–21:00 | 0.99% · 42% | 0.89% · 49% | 0.22% · 42% | 1.58% · 45% |
| Late / rollover | 21:00–24:00 | 0.52% · 22% | 0.35% · 20% | 0.09% · 18% | 0.94% · 26% |

Read each cell as "average range of the session · share of the day's full range that the session covered". Shares add up to more than 100% because sessions overlap the same highs and lows.

## Hour by hour (UTC, average range of one hourly candle)

| Market | Four busiest hours | Four quietest hours |
|---|---|---|
| Gold | 14:00 (0.71%), 13:00 (0.71%), 01:00 (0.66%), 15:00 (0.64%) | 20:00 (0.35%), 21:00 (0.34%), 22:00 (0.32%), 04:00 (0.30%) |
| Nasdaq 100 | 14:00 (0.70%), 13:00 (0.57%), 15:00 (0.56%), 16:00 (0.46%) | 05:00 (0.24%), 02:00 (0.23%), 03:00 (0.21%), 04:00 (0.20%) |
| EUR/USD | 14:00 (0.156%), 12:00 (0.155%), 13:00 (0.155%), 15:00 (0.141%) | 04:00 (0.058%), 22:00 (0.053%), 23:00 (0.053%), 21:00 (0.047%) |
| Bitcoin | 14:00 (1.18%), 15:00 (1.09%), 13:00 (0.99%), 16:00 (0.83%) | 21:00 (0.54%), 04:00 (0.53%), 10:00 (0.52%), 23:00 (0.50%) |

## Weekdays (average daily range)

| Market | Mon | Tue | Wed | Thu | Fri |
|---|---|---|---|---|---|
| Gold | 2.35% | 2.51% | 2.57% | 2.58% | 2.53% |
| Nasdaq 100 | 1.73% | 1.86% | 1.81% | 2.04% | 1.78% |
| EUR/USD | 0.53% | 0.50% | 0.57% | 0.55% | 0.50% |
| Bitcoin | 3.74% | 3.52% | 3.75% | 3.62% | 3.95% |

## What the numbers say

1. **13:00–16:00 UTC is the engine room for all four markets.** It is the London/New York overlap plus the US data releases (12:30–13:30 UTC) and the US stock open (13:30 UTC in summer, 14:30 UTC in winter). Breakout strategies get most of their follow-through here; tight-stop strategies get stopped out most here.
2. **Gold also moves at 01:00 UTC.** The Asian open (China, Singapore) gives gold its third busiest hour of the day. Nasdaq and EUR/USD are at their quietest at that time.
3. **21:00–24:00 UTC is dead for all four.** Spreads widen at the daily rollover (around 21:00–22:00 UTC) on forex and CFD brokers. Avoid opening new positions there unless the strategy is built for it.
4. **The London morning does not tell you where New York goes.** The overlap continued the London morning's direction on 48% of days for gold, 50% for Nasdaq, 52% for EUR/USD and 51% for Bitcoin: a coin flip. Do not assume a London trend carries into New York.
5. **Day of week matters far less than hour of day.** The busiest weekday is 10–18% wider than the calmest, while the busiest hour is two to three times wider than the quietest. Thursday is the busiest day for the Nasdaq; Monday is the calmest for gold and Nasdaq; Friday is the busiest for Bitcoin.

## How to use it

- **Session filter for an agent.** For a breakout or trend strategy, allow new entries 12:00–17:00 UTC (plus 00:00–03:00 UTC for gold). For mean-reversion in a range, the quiet hours fit better, but check the spread first.
- **Stop sizing by hour.** A stop smaller than one average hourly range is likely to be hit by noise at that hour. For gold, one 13:00–15:00 UTC candle averages 0.7% (≈ $28): a $10 stop at that hour is inside the noise; at 04:00 UTC the same candle averages 0.3% (≈ $12).
- **Convert to the person's time.** Always give hours in the person's own time zone as well as UTC. Daylight saving moves session opens by one hour: London uses UTC+1 from the last Sunday of March to the last Sunday of October; New York uses UTC−4 from the second Sunday of March to the first Sunday of November. Most of the measured period was in summer time.
- **News overrides sessions.** A quiet hour with a major release is not quiet. Combine with the `news-playbook` skill.

This playbook describes past behaviour; it is not financial advice and does not predict future moves.
