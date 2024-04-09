# Session

This tool provides custom session time range your trades could be placed in. It's useful for trading in specific time range, for example in the morning or in the afternoon.

**Context**: `session`

**Config**:
*	`$ctx.session.time`  - TradingViews session time range (e.g. 1000-2200)
*   `$ctx.session.exclusive` - If true, orders are placed exclusively in active trading session; false for all the time trading

**Flags**:
*	`$session.open`  - session is active
*   `$session.is-open` - true if the session opened on the current bar
*   `$session.is-close` - true if the session closed on the current bar

**Values**:
_No values provided_

**Levels**:
_No levels provided_
