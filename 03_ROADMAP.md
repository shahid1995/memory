# StrikeNova — Roadmap

This roadmap is a living plan. Completed items indicate known development history; they should be reconciled with the actual application repository before being treated as verified production state.

## Completed / Historical Foundations

- Repository audit / foundation
- Risk calculation changes
- Price-domain payoff engine work
- Scenario and time analysis engine
- Greek foundation and live-vs-model analytics
- IV analytics and volatility data foundation
- Generic Greek/IV analytics and statistical condition engine
- Server-authoritative paper trading and portfolio foundation
- Performance analytics
- Capital and margin foundation

## Active / Next Major Research & Product Domains

### GEX
Study, validate and productize Gamma Exposure analytics where evidence supports it.

### POS / Gap Prediction
Research Vibhore Gupta-style POS concepts and develop an independently validated or improved approach for next-session gap direction.

### Scalping
Research high-confidence small-point setups suitable for larger position sizing while maintaining explicit risk controls. No predictive claims without validation.

### Best Strike Selection
Develop an option-buyer-oriented strike selection feature that evaluates candidate strikes and presents expected outcomes, risk/reward and relevant market conditions.

### Option Price Projection
Provide a model for estimating an option's price at a target underlying/index level using appropriate Greeks/IV/time assumptions and clearly communicate model limitations.

### Trading Journal
Continue improving the trading journal using established product patterns and trader workflow research.

### Watchlist / Alerts
Evolve watchlist and alerts into a dedicated product area rather than keeping them as a secondary option-chain feature.

## Longer-Term Product Areas

- Advanced options analytics
- Institutional/positioning analytics
- Strategy research and validation
- Broker integrations
- Controlled live trading
- Risk management
- Portfolio analytics
- User accounts/subscriptions if commercially appropriate
- Infrastructure scaling

## Roadmap Rule

A feature should move from research to production only after its requirements, architecture, data availability, validation and operational risks are understood.
