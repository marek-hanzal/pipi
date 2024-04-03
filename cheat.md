## Intro

This is a cheat sheet for the PiPi Script Language. For examples, see for example [US500 strategies](us500%2F15min).

## Action

This describes available options in actions.

### profit

Profit sets profit price. By itself it does nothing else (for entering a trade, you have to specify `long` or `short` keyword).

> At least value must be set, the rest is optional.

**Code:**
`profit		[value]([modifier]) [+ | -] [value]([modifier])`

**Description:**
- `profit`	     - the keyword
- `[value]`	     - may be constant (`0.3`) or value from a context (`s:day-fibbonachi.100.low`)
- `([modifier])` - optional modifier (for example `s:day.size(0.5)`); this multiplies the value by the modifier
- `[+ | -]`	     - optionally you can calculate values (for example `s:day.size(0.5) + 0.3`)

**Example:**
`profit		s:day-fibbonachi.100.low	-	s:day.size(0.5)`

**Meaning:**
Set `profit` to a base value of 100% fibbonachi level's low (`s:day-fibbonachi.100.low`), but subtracted by a modifier of half (`(0.5)`) of the day's size (`s:day.size`). 

**Example:**
`profit		b:candle.top	+	1.5`

**Meaning:**
Just set the profit to 1.5 points above the candle's top (regardless of green/red candle).

### loss

The same as `profit`, only use `loss` keyword.

###	long

Opens long position; `profit` and `loss` may not be specified (for example, you can close the trade by another pattern).

### short

Opens short position; `profit` and `loss` may not be specified (for example, you can close the trade by another pattern).

### close

Closes all trades; useful if you want to run the trade without profit/loss and close it by the rule.

### set

Set various variables, when a pattern matches; see [Set section](#set-variables) for more details.

## Set (variables)

## Context

- `s:` for state - this is a state, which persists between bars
- `b:` for bar - this is a state, which is calculated for each bar

### Global Context

**Context**: `#`

There is a special global context, which indicators use for configuration and you may use it for...
...whatever you want.

## Levels

**Flags:**
- `above`                        - candle is above the level
- `below`                        - candle is below the level
- `inside-[open/high/low/close]` - candle's OHLC is inside the level (for example `inside-open`)
- `body-over`                    - candle's body is over the level (both green and red are handled correctly)
- `body-inside`                  - candle's body is inside the level
- `wicks-over`                   - candle's wicks are over the level

**Values:**
- `low`		- low value
- `high`	- high value
- `size`	- size
- `level`   - middle value - `(high + low) / 2`

**Usage - Flags:**
- Format on the candle: `flag	b:candle.[level].[flag]`
- Find level name (for example `s:day-fibbonachi.30`)
- On a candle, use `b:candle.s.day-fibbonachi.30.above`
- This means - candle must be above fibb 38.2% level
- Be careful - level name has prefix `s:` or `b:`, candle uses dot notation (`s.` or `b.`)

**Usage - Values:**
- Format on the candle: `value	[left]	[> | >= | < | <= | == | !=]		[right]`
- [left] or [right] [level].[value]; for example `value b:candle.low >= s:day-fibbonachi.100.low`
- This means - candle's low must be greater or equal to fibb 100% level's low

## Indicators

Not everything is an indicator in the true sense, it could be just value provider (for example Trade only provides information about current
position).

### Day High/Low

Tracks daily high and low prices.

**Context**: `day`

**Config:**
- `s:#.factor`        - factor used to calculate day's factor (used for level size calculation)

**Flags:**
- `b:day.change`      - new day/new low/new high
- `b:day.new-day`     - new day
- `b:day.higher-high` - new day's higher high
- `b:day.lower-low`   - new day's lower low

**Values:**
- `s:day.factor`      - day's factor used to calculate level size (for example for fibbo and so on);
  						the reason is to keep levels somehow in sync with day's price range 
- `s:day.size`        - day's price range

### Day Fibbonachi

This one works in cooperation with day's low/high. Tracks fibbonachi levels during the day.

**Config:**
- `s:#.factor`           - affected by this config, through the day's factor

**Levels:**
- `s:day-fibbonachi.0`	 - day's high
- `s:day-fibbonachi.20`  - 23.6%
- `s:day-fibbonachi.30`  - 38.2%
- `s:day-fibbonachi.50`  - 50%
- `s:day-fibbonachi.60`  - 61.8%
- `s:day-fibbonachi.70`  - 78.6%
- `s:day-fibbonachi.100` - day's low

### Candle

Tracks a lot of information about a candle.

**Context**: `candle`

**Config:**
_No config required._

**Flags:**
- `b:candle.red`       - red candle
- `b:candle.green`     - green candle

**Values:**
- `b:candle.[open/high/low/close]`      - candle's OHLC; for example `b:candle.low`

### Trade

Here are interesting things about current trade.

**Context**: `trade`

**Config:**
_No config needed._

**Flags:**
- `b:trade.entry`       		- order is placed
- `b:trade.exit`       		    - order is closed
- `s:trade.long`      		    - long position
- `s:trade.short`      		    - short position

**Values:**
- `s:trade.profit`              - profit price
- `s:trade.loss`                - loss price
- `s:trade.length`              - duration of a trade

### MACD

You know this one.

**Context**: `macd`

**Config:**
- `s:#.macd.length`            - length (int)
- `s:#.macd.signal`            - signal length (int)
- `s:#.macd.impulse`           - how many bars must be in "flat" zone to trigger impulse
- `s:#.macd.dmz`               - when in DMZ, long/short signals are not triggered (this may be quite large number, depending on your usage, for example 6.6 or so)
- `s:#.macd.flat`              - which value of histogram is considered being flat (should be low number, like 0.1 or so)

**Flags:**
- `b:macd.dmz`                 - is MACD in a DMZ zone
- `b:macd.flat`                - is histogram flat
- `b:macd.up.low`              - weak uptrend signal
- `b:macd.up.high`             - strong uptrend signal
- `b:macd.up.impulse`          - impulse uptrend signal
- `b:macd.down.low`            - weak downtrend signal
- `b:macd.down.high`           - strong downtrend signal
- `b:macd.down.impulse`        - impulse downtrend signal
- `b:macd.long-dmz`            - long signal in DMZ zone
- `b:macd.long`                - long signal outside DMZ zone
- `b:macd.short-dmz`           - short signal in DMZ zone
- `b:macd.short`               - short signal outside DMZ zone

### SuperTrend

SuperTrend uses fast and slow ATR to calculate trend; you may pick, which one you want to use.

Both fast and slow are useful to confirm each other's "real" trend.

**Context**: `super-trend`

**Config:**
- `s:#.super-trend.fast-length`        - fast ATR length
- `s:#.super-trend.fast-factor`        - fast factor
- `s:#.super-trend.slow-length`        - slow ATR length
- `s:#.super-trend.slow-factor`        - slow factor

**Flags:**
- `b:super-trend.fast.up`              - fast trend is uptrend
- `b:super-trend.fast.up.start`        - fast trend has started (one time flag)
- `b:super-trend.fast.up.end`          - fast trend has ended (one time flag)
- `b:super-trend.fast.down`            - fast trend is downtrend
- `b:super-trend.fast.down.start`      - fast trend has started (one time flag)
- `b:super-trend.fast.down.end`        - fast trend has ended (one time flag)
- `b:super-trend.slow.up`              - slow trend is uptrend
- `b:super-trend.slow.up.start`        - slow trend has started (one time flag)
- `b:super-trend.slow.up.end`          - slow trend has ended (one time flag)
- `b:super-trend.slow.down`            - slow trend is downtrend
- `b:super-trend.slow.down.start`      - slow trend has started (one time flag)
- `b:super-trend.slow.down.end`        - slow trend has ended (one time flag)
- `b:super-trend.up`                   - both fast and slow are uptrend
- `b:super-trend.down`                 - both fast and slow are downtrend

### CCI

Commodity Channel Index, default TradingView implementation.

**Context**: `cci`

**Config:**
- `s:#.cci.length`            - length (int)

**Flags:**
- `b:cci.below.enter`         - Signal exits (> -100)
- `b:cci.below.exit`          - Signal enters (< -100)
- `b:cci.above.enter`         - Signal exits (> 100)
- `b:cci.above.exit`          - Signal enters (< 100)
- `b:cci.zone`                - Signal is in zone (>= -100, <= 100)

**Values:**
- `b:cci.signal`              - CCI value
