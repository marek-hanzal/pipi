# Improved MACD

This is improved version of MACD with some extra features, which enables a bit more accurate signals.

**Context**: `macd2`

**Config**:
*	`$ctx.macd2.length`     - MACD length
*   `$ctx.macd2.signal`     - signal length
*   `$ctx.macd2.impulse`    - how many bars (in flat histogram) to wait to generate impulse signal
*   `$ctx.macd2.dmz`        - demilitarized zone - marks part of histogram, where no strong signals are generated; _be careful using this value as it may change on the marker you're on, and also by time on the same market_
*   `$ctx.macd2.flat`       - what is considered being flat market (in histogram); this value should be kept somehow low (like 0.1, 0.2 or so)

**Flags**:
*	`$macd2.dmz`            - true - demilitarized zone is active (no strong signals)
*   `$macd2.flat`           - true - flat histogram
*   `$macd2.up.low`         - weak uptrend signal
*   `$macd2.up.high`        - strong uptrend signal
*   `$macd2.up.impulse`     - uptrend impulse signal (after flat histogram)
*   `$macd2.down.low`       - weak downtrend signal
*   `$macd2.down.high`      - strong downtrend signal
*   `$macd2.down.impulse`   - downtrend impulse signal (after flat histogram)
*   `$macd2.long-dmz`       - long signal generated in demilitarized zone
*   `$macd2.long`           - long signal generated outside of DMZ; should be considered as most strong buy signal (of this indicator)
*   `$macd2.short-dmz`      - short signal generated in demilitarized zone
*   `$macd2.short`          - short signal generated outside of DMZ; should be considered as most strong sell signal (of this indicator)

**Values**:
_No values provided_

**Levels**:
_No levels provided_
