# Candle

This tool provides information about current candle, including some basic stuff related to previous candles.

> This is important piece as it contains also all levels from other tools, so if you want to know if a candle is above EMA or below structure, you can use this context.
> All levels are [exported](../../levels) to values with their original names.

**Context**: `candle`

**Config**:
_No config needed_

**Flags**:
*	`$candle.green`            - green candle
*   `$candle.red`              - red candle
*   `$candle.bull-engulfing`   - green bullish engulfing candle
*   `$candle.bear-engulfing`   - red bearish engulfing candle
*   `$candle.open-gt-open`     - open higher than previous open
*   `$candle.open-lt-open`     - open lower than previous open
*   `$candle.high-gt-high`     - high higher than previous high
*   `$candle.high-lt-high`     - high lower than previous high
*   `$candle.low-gt-low`       - low higher than previous low
*   `$candle.low-lt-low`       - low lower than previous low
*   `$candle.close-gt-close`   - close higher than previous close
*   `$candle.close-lt-close`   - close lower than previous close
*   `$candle.body-gt-body`     - body bigger than previous body
*   `$candle.body-lt-body`     - body smaller than previous body
*   `$candle.up`               - bottom > previous bottom (regardless of color); meaning this candle is physically higher than previous one
*   `$candle.down`             - bottom < previous bottom (regardless of color); meaning this candle is physically lower than previous one

**Values**:
* `$candle.index`  - bar index of a candle
* `$candle.open`     - open price
* `$candle.high`     - high price
* `$candle.low`      - low price
* `$candle.close`    - close price
* `$candle.size`     - body size
* `$candle.length`   - high - low
* `$candle.top`      - top price (max of open and close)
* `$candle.bottom`   - bottom price (min of open and close)

**Levels**:
_No levels provided_
