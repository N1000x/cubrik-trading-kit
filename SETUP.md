# cubrik trading for Claude Code — one-paste setup

> **How to use:** open Claude Code, paste this whole file, press Enter. Claude installs everything.
> **Mode d'emploi :** ouvre Claude Code, colle tout ce fichier, appuie sur Entrée. Claude installe tout.

---

Claude, install the cubrik trading kit on this computer. Do exactly these steps, in order, then report back
in the language I write in.

## Step 1 — connect the cubrik MCP server (all projects)

Run `claude mcp get cubrik`. If it is not found, run:

```bash
claude mcp add --transport http --scope user cubrik https://mcp.cubrik.ai/mcp
```

If it already exists, leave it as it is.

## Step 2 — install the seven skills

Create each file below at the exact path given (create the folders). Write the content between the
`=== BEGIN` and `=== END` lines exactly, without those two lines. If a file already exists, overwrite it
(it is an older version of this kit).

=== BEGIN ~/.claude/skills/cubrik-onboarding/SKILL.md
---
name: cubrik-onboarding
description: First steps with cubrik in Claude Code. Use when someone has just installed the cubrik kit, asks how to start, whether cubrik is connected, what their plan or credits are, which brokers are connected, or how to connect MetaTrader, OKX, TradingView, MoonX or another platform, or asks to install the kit's skills in their cubrik account.
---

# cubrik onboarding

Goal: in under five minutes the person knows cubrik is connected, what their account can do, and what to try first.

## 1. Is cubrik connected?

- If no cubrik tools are available (tools named `mcp__cubrik__*`), cubrik is not authorised yet. Tell the person: type `/mcp`, choose **cubrik**, then **Authenticate**, sign in on the page that opens, then come back. You cannot do this step for them. Never ask for their password, token or API key.
- If the tools are there, call `get_account` and `list_platforms`.

## 2. Explain the account in plain words

From `get_account`: plan, credits left this month and renewal date, active agents and the limit, fastest schedule allowed.
From `list_platforms`: each connected broker or exchange, whether it is a **demo** account (look at the host name, e.g. "MetaQuotes-Demo") and its last known balance.

If no platform is connected, call `connect_platform`: it lists what cubrik supports (MetaTrader, OKX, Binance, Bitget, Kraken, Coinbase, Hyperliquid, eToro, Interactive Brokers, Robinhood, MoonX, TradingView for data) and gives the link to the cubrik page where the person connects it themselves. Keys and sign-ins are entered there, never in the chat.

Check whether each account is real money or demo. A MetaTrader host such as "MetaQuotes-Demo" is demo. For OKX, `okx_system_get_capabilities` says `demo: true/false`. Say clearly which accounts are real.

## Show the pages, not just the links

When a built-in browser is available (the Claude desktop app's browser pane, tools named `*Claude_Browser*`), open the cubrik page that matches what you are talking about, so the person sees it next to the chat:
- the account and plans: https://app.cubrik.ai
- connecting a broker: the link `connect_platform` returns
- an agent: the link `test_agent` returns, or https://app.cubrik.ai/agent/detail?id=AGENT_ID

The person signs in and enters keys themselves on those pages. Never type a password, API key or code into them.

## 3. Suggest a first move

Offer four starting points, one line each:
1. "Show me ready-made agents" → `search_library` (cubrik's Library: agents backtested every week, with live results, a score and the number of copies)
2. "Build me a strategy" → skill `cubrik-strategy-builder`
3. "Backtest this idea" → skill `cubrik-backtest-lab`
4. "Check my account for risk" → skill `cubrik-risk-audit`

## 4. Install the kit's cubrik skills (once, with the person's OK)

The kit also ships skills that live **inside the person's cubrik account**, where cubrik's own trading chat and agents use them, even when Claude is closed:

| Skill | What it gives cubrik |
|---|---|
| `pre-trade-checklist` | Risk checks before and after every trade: news windows, daily loss, losing streak, exposure, sizing, spread, a stop on every position |
| `session-playbook` | When gold, Nasdaq 100, EUR/USD and Bitcoin actually move, hour by hour and session by session, measured on a year of hourly candles |
| `news-playbook` | How those markets react to the Fed and the US jobs report, with the no-entry windows to apply |

1. Explain the table above in two lines and ask: "Install these 3 skills in your cubrik account?" Do nothing without a clear yes: this writes to the person's account.
2. Call `list_skills`. If a skill with one of these names already exists, say that installing will replace it and ask again for that one.
3. Download each file from https://raw.githubusercontent.com/N1000x/cubrik-trading-kit/main/cubrik-skills/NAME.md (for example with `curl -fsSL`). Each file starts with a `---` block holding `name:` and `description:`; everything after the closing `---` is the instructions.
4. Call `save_skill` for each one with that `name`, `description` and `instructions`, `chat: true` and `agents: []`. They are switched on for cubrik's chat only: never attach them to agents without asking, because a skill changes how an agent decides.
5. Call `list_skills` again and confirm the three are there. Tell the person they can switch a skill on for an agent from the Skills page (https://app.cubrik.ai/skills) or by asking you.

If the download fails, say so and stop; never write a skill from memory.

## Rules

- Never place an order, start an agent or change a setting during onboarding, except installing the kit's cubrik skills after an explicit yes.
- Not financial advice: you help build and test the person's own ideas; they decide.
=== END

=== BEGIN ~/.claude/skills/cubrik-strategy-builder/SKILL.md
---
name: cubrik-strategy-builder
description: Turn a trading idea into precise, testable rules for cubrik. Use when someone wants to create a trading strategy, describes a setup in their own words (e.g. "buy gold when RSI is oversold"), asks what strategy to trade on a market, or wants an idea made precise before a backtest.
---

# cubrik strategy builder

Goal: a strategy written as exact rules that `run_backtest` can test and an agent can follow.

## 1. Ask only what is missing (max 5 questions, one message)

- Market(s): gold, Nasdaq, forex pair, crypto pair… (check the symbol with `list_instruments`)
- Style and timeframe: scalping (5m–15m), intraday (1h), swing (4h–1D)
- Direction: long, short or both
- Risk per trade (default 1% of balance) and leverage
- Platform it will run on (from `list_platforms`)

If the person has no idea yet, propose 2–3 classic families that fit their market and style:
trend pullback (EMA trend + RSI dip + reclaim), volatility squeeze breakout (Bollinger bandwidth then band break, with an EMA200 filter), liquidity sweep and reclaim (swing low/high taken then close back), mean reversion in range (ADX low + band re-entry).

## 2. Write the rules

Use only building blocks the backtester knows: price (open/high/low/close), SMA, EMA, RSI, ATR, highest/lowest, volume, MACD, Bollinger (upper/middle/lower/bandwidth), stochastic, ADX/+DI/−DI, VWAP, Ichimoku, OBV, CCI, MFI, parabolic SAR, Keltner, SuperTrend, rate of change, swing high/low, order blocks, fair value gaps, market structure.

Every strategy states:
- **Entry** (long, and short if both): each condition with an operator (above, below, crosses above, crosses below).
  A sequence ("a sweep, then a reclaim") uses a `within` window on the earlier condition. Without it all conditions must be true on the same bar, and many combinations then never trigger.
- **Exit**: stop loss % and take profit % of price, optional early-exit signals, optional max bars in trade.
- **Management** (optional): break-even at 1R, lock profit at 2R…
- **Risk**: % per trade, leverage.

Show the rules as a short numbered list in plain words, then offer to backtest them with `cubrik-backtest-lab`.

## Rules

- Never promise results. A strategy is a hypothesis until it has been backtested.
- Not financial advice.
=== END

=== BEGIN ~/.claude/skills/cubrik-backtest-lab/SKILL.md
---
name: cubrik-backtest-lab
description: Backtest, compare and optimise trading strategies on cubrik, and judge honestly whether one is worth running. Use when someone asks to backtest a strategy or idea, compare variants, optimise parameters, find a good strategy on a market, or asks whether a backtest result is good.
---

# cubrik backtest lab

Goal: find out honestly whether a strategy has an edge, without overfitting.

## How cubrik backtests work (know this before running anything)

- `run_backtest` queues a test and returns an id; read it with `get_backtest`. It changes nothing on any account.
- **At most 3 backtests can run at once.** Send them in batches of 3. If you get `too_many_backtests`, wait for the running ones.
- `get_backtest` does **not** return the backtest's name. Keep your own table of id → variant as soon as each `run_backtest` answers.
- **Data depth:** forex, metals and indices have no history below 1h on cubrik. For a 5m or 15m test on gold, use the perpetual `XAU/USDT:USDT` as a price proxy and say so. Crypto 1m data covers only the last 90 days. Forex/metals history is roughly the last 12 months; the result's methodology says the exact start.
- Fills: the signal is read on bar close and the trade opens at the next bar's open, with a small slippage penalty. When one bar hits both stop and target, the stop counts. Funding, swaps and broker commissions are not modelled.

## Method

0. **Look before inventing.** `search_library` shows cubrik's ready-made agents with a one-year backtest, live results since they went live, and per-market figures. If one already fits the market and style, start from it (`get_library_agent` gives its exact rules) and test variations of it.
1. **Baseline.** Run the strategy as described (default: 12 months, or 6 months for 5m/15m; 1% risk per trade).
2. **Read the numbers that matter:** number of trades, win rate, profit factor, return, max drawdown, and the **month-by-month** table.
3. **Vary one thing at a time** (a threshold, the stop/target, a trend filter, the timeframe). Batches of 3.
4. **Robustness check:** take the best version and nudge its main parameters up and down. If the neighbours collapse, the result is luck. Say so.
5. **Recent behaviour:** look at the last 3 months separately. Flag it if they are negative.

## When is a strategy acceptable?

Only call a strategy "promising" when **all** of these hold:
- At least 30 trades
- Profit factor ≥ 1.2
- Max drawdown ≤ 15% at 1% risk per trade
- Neighbouring parameter values also stay profitable
- More winning months than losing months

Otherwise say plainly that it does not have an edge yet, and suggest the next idea (often a higher timeframe: on gold, 5m strategies are usually eaten by spread, 1h works better).

## Report format

A small table per batch (name, trades, win rate, profit factor, return, max drawdown), then 2–3 lines: what worked, what failed, what to try next. End with the caveat: past results do not predict future results; not financial advice.

When a strategy passes, offer to deploy it with `cubrik-deploy-agent`.
=== END

=== BEGIN ~/.claude/skills/cubrik-deploy-agent/SKILL.md
---
name: cubrik-deploy-agent
description: Turn a tested strategy into a cubrik agent that trades on a connected broker, safely: created paused, dry-run first, started only on explicit approval. Use when someone wants to automate a strategy, create, start, pause, change or delete a cubrik agent, or change how often it runs.
---

# cubrik deploy agent

Goal: an agent that follows the strategy exactly, with nothing live until the person says yes.

## 0. Pick the kind of agent

- **From the Library (preferred when one fits):** `copy_library_agent` copies a ready-made agent into the account, paused, bound to the platforms you name by id (from `list_platforms`). These agents run a saved strategy (the same rules as their backtest), with built-in guards such as a daily loss limit, max open positions, max total margin and no new entries 30 minutes before high-impact news.
- **From a saved strategy:** a backtest saves a strategy. `check_rules_now` with its id says whether its entry and exit rules are true on the latest closed candles, symbol by symbol. Use it to show the person what the strategy sees right now.
- **From a written instruction (`create_agent`):** the most flexible option, but the agent interprets your text. Follow section 1 carefully.

## 1. Write the agent instruction

An agent does not run backtest rules mechanically: it receives your text and decides on live prices. So the instruction must be complete and unambiguous. Always include these sections:

1. **Market and platform:** exact symbol (tell it to confirm the name with the platform's symbol search), one position at a time unless asked otherwise, never touch other symbols or other agents' positions.
2. **Data:** timeframe, and "use only the last **closed** candle, ignore the one still forming". Each indicator with its exact settings.
3. **Signal:** each condition. Add "act once per closed candle; if this candle was already handled, do nothing".
4. **Order:** market order with stop loss and take profit set **in the opening order**. Size = risk % of balance ÷ distance to stop, using the contract size and lot step from symbol search, rounded down; if below the minimum lot or margin is short, do not open and report the number.
5. **Management:** break-even and profit-lock tiers if any; "the stop never moves backwards"; max time in trade.
6. **Safety:** a position without a stop gets one immediately, or is closed; a rejected order is retried at most once.
7. **Journal:** every run logs the candle values, the decision (open / hold / adjust / close / skip) and why.

## 2. Create it paused

`create_agent` with: name, `platforms` (by name), `time_zone` (the person's zone), `trigger` (how often), `max_daily_loss_pct` (propose 3%), `max_positions_per_instrument` (1). It is created **paused**.

Schedule: a 1h strategy is well served by running every 5 minutes (fast entry after the candle closes, reactive stop management). Mention that more runs use more credits.

## 3. Dry run

If a built-in browser is available, open the agent's page (https://app.cubrik.ai/agent/detail?id=AGENT_ID) so the person can watch its runs.

`test_agent` reads the market and decides as on a real run but **cannot place orders**. It can take several minutes. Read the result with `get_agent_runs`. Report what it saw and decided, and any connector errors.

## 4. Start only on an explicit yes

Before starting, tell the person: platform, demo or real account (host name), risk per trade in money, daily loss limit, schedule. Then ask. Only on a clear yes, call `start_agent` (with `run_now` if they want a run immediately). If a manual run is still in progress, the agent starts anyway and runs at its next slot.

To pause: `pause_agent`. To change: `update_agent` (pass the whole new instruction). Pausing does not close open positions: their stop and target stay on the platform, but tier management stops.

## Rules

- **Real money.** If the platform is a real-money account, say so explicitly and ask for confirmation a second time.
- Never start, resume or change a live agent without the person's explicit approval in this conversation.
- Not financial advice.
=== END

=== BEGIN ~/.claude/skills/cubrik-risk-audit/SKILL.md
---
name: cubrik-risk-audit
description: Read-only risk check of a cubrik account and its connected brokers. Use when someone asks to check their risk, their open positions, whether anything is unprotected, why they are losing, or for a health check of their trading account or agents.
---

# cubrik risk audit

Goal: spot what could hurt the account, without changing anything.

## Read

- `list_platforms` (which accounts, demo or real, balances)
- `get_portfolio` and each platform's own position and order tools (e.g. `metatrader_list_cfd_positions`, `okx_swap_get_positions`, `moonx_list_futures_positions`)
- `list_agents`, then `get_agent` for active ones (limits, today's reference balance, stops)
- `get_trade_history` / `get_trading_results` for recent closed trades

## Flag, most serious first

1. **Positions without a stop loss.** Always list them with symbol, side, size and unrealised P&L.
2. **Exposure:** margin used, several positions on the same idea (e.g. all long USD), leverage above what the person said they wanted.
3. **Losing streaks** per symbol (`consecutiveLossesBySymbol` in `get_agent`), and the daily loss versus the limit.
4. **Agents** that are active with no daily loss limit, may trade on a real-money account, or have instructions that open a position on every run without a setup.
5. **Connector problems:** a platform disconnected or reconnecting.
6. **Where the losses come from:** `get_analysis` (per platform, by its id) breaks results down by trading session (Asian / London / New York), by holding time (scalp / intraday / swing) and by weekday. It also rates each trade (good / fair / reckless). Point out the pattern, e.g. "London is profitable, New York loses everything" or "trades held over a day lose".
7. **cubrik's own advice:** `get_advice` lists suggested changes per agent. Show them. `apply_advice` changes the agent's rules, so only use it on an explicit yes.

## Report

A short list: problem → why it matters → suggested fix. Then ask which fixes to apply. **Never** close a position, move a stop, pause an agent or place an order without an explicit yes for that specific action.

Not financial advice.
=== END

=== BEGIN ~/.claude/skills/cubrik-daily-report/SKILL.md
---
name: cubrik-daily-report
description: Daily or weekly summary of a cubrik account — agents, runs, trades, P&L, open positions, upcoming high-impact news. Use when someone asks for a report, a recap, "how did my agents do", today's or this week's results, or a morning brief.
---

# cubrik daily report

## Gather

- `list_agents`, then `get_agent_runs` for each active agent (last runs, decisions, errors, credits used)
- `get_trading_results` and `get_trade_history` for the period (default: last 24h; "week" = last 7 days)
- Open positions from `get_portfolio` and the platforms' position tools
- `get_account` for credits left
- `get_economic_calendar` for high-impact events in the next 24h on the currencies traded (pass the traded symbols)
- `list_notifications` for what happened since the last report (positions opened, agent results, finished backtests, risk alerts)
- `get_market_overview` and `get_news` for a two-line market read

## Write (short, scannable)

1. **Headline:** P&L for the period, trades closed (wins / losses), equity versus the day's reference balance.
2. **Agents:** one line each: status, runs, what it did, any error.
3. **Open positions:** symbol, side, entry, stop, target, unrealised P&L. Mark any position without a stop.
4. **Next 24h:** high-impact news that could move what is open.
5. **Credits:** left this month and renewal date.

Read-only: no orders, no agent changes. Results shown are past results and do not predict future ones.
=== END

=== BEGIN ~/.claude/skills/cubrik-market-scan/SKILL.md
---
name: cubrik-market-scan
description: Read the market right now through cubrik — prices, movers, technical readings, news, upcoming events, and whether a saved strategy's setup is live. Use when someone asks what the market is doing, how gold / Bitcoin / Nasdaq / a pair looks, whether RSI or MACD says something, what news or events are coming, or whether their strategy has a signal now.
---

# cubrik market scan

## Sources (all read-only)

- **Overview and prices:** `get_market_overview` (main markets, biggest movers), `get_price` (up to 8 symbols, BASE/QUOTE). For the exact price the person would trade at, use the platform's own quote, e.g. `metatrader_get_price` (bid, ask, spread).
- **Technicals:** `moonx_get_technical_indicators` (RSI, MACD, SMA/EMA, Bollinger, stochastic, ATR, ADX, CCI, Williams %R, each with a readable signal; works for crypto and FX/CFD) or TradingView's `tradingview_mcp-tv-get-technicals-rating` (a full snapshot plus a buy/neutral/sell summary; symbols as EXCHANGE:TICKER, e.g. OANDA:XAUUSD). Only use the platforms the account has connected.
- **Candles:** `get_candles` (cubrik feed) or `tradingview_mcp-tv-get-ohlcv`. TradingView keeps only about a month of 5m history but years of 1h and daily.
- **Screeners:** `tradingview_mcp-tv-run-screener`; for crypto on OKX, `okx_market_filter`, funding rates and open interest.
- **News and events:** `get_news` (filtered by likely impact, by symbol), `get_economic_calendar` (rate decisions and big prints; `min_impact: high`).
- **Strategy signal now:** `check_rules_now` with a saved strategy id. It says, symbol by symbol, which entry conditions hold on the last closed candle.
- **Smart money (OKX):** `okx_smartmoney_get_traders_by_filter` ranks OKX's top traders. Check the drawdown in the answer yourself: the filter does not always apply.

## Answer

Lead with the answer to the question in one or two lines, then the supporting readings (price, trend, key levels, indicators), then any event in the next 48h that could move it. Describe, don't predict: say what the indicators show, never what the price will do. Not financial advice.
=== END

## Step 3 — open cubrik in your browser

If you have a built-in browser (the Claude desktop app's browser pane, tools named `*Claude_Browser*`), open https://app.cubrik.ai in it, so I can create my account or sign in, and connect my broker there. If you don't have one, give me the link instead. Never type a password, key or code for me: I sign in myself.

## Step 4 — finish

Tell me, in my language:
1. What was installed: the cubrik MCP (all projects) and the seven skills.
2. That I now need to sign in to cubrik myself: type `/mcp`, choose **cubrik**, then **Authenticate**, and sign in on the page that opens. Then restart Claude Code so the skills load.
3. What I can ask next, for example:
   - "Am I connected to cubrik? What's on my account?"
   - "Build me a gold strategy on 1h and backtest it."
   - "Check my account for risk."
   - "Give me today's report."
   - "Install the kit's skills in my cubrik account."
4. One reminder: nothing goes live without my explicit OK; this is not financial advice.
