# cubrik trading kit for Claude

Turn Claude into a trading assistant connected to [cubrik](https://cubrik.ai): build a strategy, backtest it honestly, deploy it as an agent on your broker, check your risk — all from a conversation.

*🇫🇷 Version française plus bas.*

## Install (2 minutes)

1. Install the **Claude desktop app** and open the **Code** tab (or Claude Code in a terminal).
2. Open [`SETUP.md`](SETUP.md), click the **Copy raw file** button (top right of the file), paste it into Claude and press Enter.
3. Click **Yes** when Claude asks to create files (choose "allow all edits during this session" to click only once).
4. Type `/mcp`, choose **cubrik** → **Authenticate**, and sign in to cubrik on the page that opens. Then restart Claude.
5. Ask: *"Am I connected to cubrik? What's on my account?"*

## What you get

**Seven Claude skills**

| Skill | Ask for example |
|---|---|
| `cubrik-onboarding` | "Am I connected? What can my plan do?" |
| `cubrik-strategy-builder` | "Build me a gold strategy on 1h." |
| `cubrik-backtest-lab` | "Backtest it and tell me honestly if it holds up." |
| `cubrik-deploy-agent` | "Turn it into an agent on my MetaTrader account." |
| `cubrik-risk-audit` | "Check my account for risk." |
| `cubrik-daily-report` | "Give me today's report." |
| `cubrik-market-scan` | "How does gold look right now?" |

**Three skills for your cubrik account** — installed only if you say yes (ask Claude: *"Install the kit's skills in my cubrik account"*). cubrik's own chat and agents then use them, even when Claude is closed.

| Skill | What's inside |
|---|---|
| [`pre-trade-checklist`](cubrik-skills/pre-trade-checklist.md) | Risk checks before and after every trade: news windows, daily loss, losing streak, exposure, sizing, spread, a stop on every position |
| [`session-playbook`](cubrik-skills/session-playbook.md) | When gold, Nasdaq 100, EUR/USD and Bitcoin actually move — hour by hour, session by session, measured on ~23,700 hourly candles |
| [`news-playbook`](cubrik-skills/news-playbook.md) | How those markets reacted to 8 Fed decisions and the US jobs reports, with the no-entry windows to apply |

## Safety built in

- Agents are created **paused**; nothing goes live without your explicit OK.
- Claude never types a password, API key or code for you: you sign in on cubrik yourself.
- No skill can move money in or out of your accounts.

## Disclaimer

Educational tool. Not financial advice. Backtests and the statistics in these skills describe past market behaviour; they do not predict future results. Trading involves risk of loss. You set and remain responsible for the risk of any agent you run.

---

## 🇫🇷 En français

Le kit transforme Claude en assistant de trading connecté à [cubrik](https://cubrik.ai) : créer une stratégie, la backtester honnêtement, la lancer en agent sur ton broker, vérifier tes risques — en discutant.

### Installation (2 minutes)

1. Installe l'**app Claude desktop** et ouvre l'onglet **Code** (ou Claude Code dans un terminal).
2. Ouvre [`SETUP.md`](SETUP.md), clique sur le bouton **Copy raw file** (en haut à droite du fichier), colle-le dans Claude et appuie sur Entrée.
3. Clique **Yes** quand Claude demande à créer des fichiers (choisis « allow all edits during this session » pour ne cliquer qu'une fois).
4. Tape `/mcp`, choisis **cubrik** → **Authenticate**, connecte-toi à cubrik sur la page qui s'ouvre, puis redémarre Claude.
5. Demande : *« Je suis connecté à cubrik ? Qu'est-ce qu'il y a sur mon compte ? »*

### Ce que tu obtiens

- **7 skills Claude** : onboarding, création de stratégie, backtest, déploiement d'agent, audit de risque, rapport du jour, lecture du marché.
- **3 skills pour ton compte cubrik**, installés seulement si tu dis oui (*« Installe les skills du kit dans mon compte cubrik »*) : checklist de risque, horaires des marchés (données sur ~23 700 bougies), réaction aux annonces (Fed, emploi US).

### Sécurité

Les agents sont créés en pause ; rien ne part en réel sans ton accord. Claude ne tape jamais de mot de passe ni de clé à ta place.

### Avertissement

Outil éducatif. Pas un conseil financier. Les backtests et les statistiques décrivent le passé et ne prédisent pas l'avenir. Le trading comporte un risque de perte.
