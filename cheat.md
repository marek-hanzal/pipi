## Intro

This is a cheat sheet for the PiPi Script Language. For examples, see for example [US500 strategies](strategies/us500/15min).

## Action

This describes available options in actions.

### profit

Profit sets profit price. By itself it does nothing else (for entering a trade, you have to specify `long` or `short` keyword).

> At least value must be set, the rest is optional.

**Code:**
`profit		[value]([modifier]) [+ | -] [value]([modifier])`

**Description:**
- `profit`	     - the keyword
- `[value]`	     - may be constant (`0.3`) or value from a context (`day-fibbonachi.100.low`)
- `([modifier])` - optional modifier (for example `day.size(0.5)`); this multiplies the value by the modifier
- `[+ | -]`	     - optionally you can calculate values (for example `day.size(0.5) + 0.3`)

**Example:**
`profit		day-fibbonachi.100.low	-	day.size(0.5)`

**Meaning:**
Set `profit` to a base value of 100% fibbonachi level's low (`day-fibbonachi.100.low`), but subtracted by a modifier of half (`(0.5)`) of the day's size (`day.size`). 

**Example:**
`profit		candle.top	+	1.5`

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

You'll see `x-cross` and close order description `CbA` (Closed by Action).

> Keep in mind that `close` may be quite suboptimal as it closes the trade on the next candle's open.

### set

Set various variables, when a pattern matches; see [Set section](#set-variables) for more details.

###	hint

Set action hint, which is displayed in the orders.

## Set (variables)

// TODO: Sorry

## Context

- `` for state - this is a state, which persists between bars
- `` for bar - this is a state, which is calculated for each bar

### Global Context

**Context**: `$ctx`

There is a special global context, which indicators use for configuration and you may use it for...
...whatever you want.

## Levels

**Flag**
- `above`                        - candle is above the level
- `below`                        - candle is below the level
- `inside-[open/high/low/close]` - candle's OHLC is inside the level (for example `inside-open`)
- `body-over`                    - candle's body is over the level (both green and red are handled correctly)
- `body-inside`                  - candle's body is inside the level
- `wicks-over`                   - candle's wicks are over the level

**Value**
- `low`		- low value
- `high`	- high value
- `size`	- size
- `level`   - middle value - `(high + low) / 2`

**Usage - Flag**
- Format on the candle: `flag	candle.[level].[flag]`
- Find level name (for example `day-fibbonachi.30`)
- On a candle, use `candle.day-fibbonachi.30.above`
- This means - candle must be above fibb 38.2% level
- Be careful - level name has prefix `` or ``, candle uses dot notation (`s.` or `b.`)

**Usage - Value**
- Format on the candle: `value	[left]	[> | >= | < | <= | == | !=]		[right]`
- [left] or [right] [level].[value]; for example `value candle.low >= day-fibbonachi.100.low`
- This means - candle's low must be greater or equal to fibb 100% level's low

## Indicators

Not everything is an indicator in the true sense, it could be just value provider (for example Trade only provides information about current
position).

### Day High/Low

Tracks daily high and low prices.

**Context**: `day`

**Config:**
- `$ctx.factor`        - factor used to calculate day's factor (used for level size calculation)

**Flag**
- `$day.change`      - new day/new low/new high
- `$day.new-day`     - new day
- `$day.higher-high` - new day's higher high
- `$day.lower-low`   - new day's lower low

**Value**
- `$day.factor`      - day's factor used to calculate level size (for example for fibbo and so on);
  						the reason is to keep levels somehow in sync with day's price range 
- `$day.size`        - day's price range

**Level**
- `$day.level`       - day's level with low and high

### Day Fibbonachi

This one works in cooperation with day's low/high. Tracks fibbonachi levels during the day.

**Context**: `$day-fibbonachi`

**Config:**
- `$ctx.factor`       - affected by this config, through the day's factor

**Level**
- `day-fibbonachi.0`	 - day's high
- `day-fibbonachi.20`  - 23.6%
- `day-fibbonachi.30`  - 38.2%
- `day-fibbonachi.50`  - 50%
- `day-fibbonachi.60`  - 61.8%
- `day-fibbonachi.70`  - 78.6%
- `day-fibbonachi.100` - day's low

### Session

Tracks session open/close (to limit trades only during the day or whatever you want).

**Context**: `$session`

**Config:**
- `$ctx.session.time`        - time range (for example `1000-2300`)
- `$ctx.session.exclusive`   - if true, only trades during the session time

**Flag**
- `session.open`          - Tells, if we're in the session you specified
- `session.is-open`       - One time flag marking opened session
- `session.is-close`      - One time flag marking closed session

**Value**
_No values provided._

### Candle

Tracks a lot of information about a candle.

**Context**: `$candle`

**Config:**
_No config required._

**Flag**
- `$candle.red`       			- red candle
- `$candle.green`     		    - green candle
- `$candle.bull-engulfing`     - green candle with high > previous high and larger body
- `$candle.bear-engulfing`     - red candle with low < previous low and larger body
- `$candle.[open | high | low | close]-[lt | gt]-[open | high | low | close]`       - `[OHLC]` > previous `[OHLC]`; must be same pair (like `close-gt-close` or `high-lt`high`)
- `$candle.body-gt-body`       - current body is larger than previous body
- `$candle.body-lt-body`       - current body is smaller than previous body

**Value**
- `$candle.index`      				      - bar index of the candle
- `$candle.body`      				      - candle body size
- `$candle.length`   				      - high - low
- `$candle.top`      				      - candle's top (open/close, regardless color)
- `$candle.bottom`      				  - candle's bottom (open/close, regardless color)
- `$candle.[open | high | low | close]`   - candle's OHLC; for example `candle.low`

### Trade

Here are interesting things about current trade.

**Context**: `$trade`

**Config:**
_No config needed._

**Flag**
- `$trade.entry`       		- order is placed
- `$trade.exit`       		    - order is closed
- `$trade.closed`    		    - an action has closed the trade
- `$trade.long`      		    - long position
- `$trade.short`      		    - short position

**Value**
- `$trade.profit`              - profit price
- `$trade.loss`                - loss price
- `$trade.length`              - duration of a trade
- `$trade.pnl`                 - current profit/loss; if you want to close the trade by this rule, keep in mind your position size (so you may have a lot of small trades with bigger positions)

### MACD

You know this one.

**Context**: `$macd`

**Config:**
- `$ctx.macd.length`            - length (int)
- `$ctx.macd.signal`            - signal length (int)
- `$ctx.macd.impulse`           - how many bars must be in "flat" zone to trigger impulse
- `$ctx.macd.dmz`               - when in DMZ, long/short signals are not triggered (this may be quite large number, depending on your usage, for example 6.6 or so)
- `$ctx.macd.flat`              - which value of histogram is considered being flat (should be low number, like 0.1 or so)

**Flag**
- `$macd.dmz`                 - is MACD in a DMZ zone
- `$macd.flat`                - is histogram flat
- `$macd.up.low`              - weak uptrend signal
- `$macd.up.high`             - strong uptrend signal
- `$macd.up.impulse`          - impulse uptrend signal
- `$macd.down.low`            - weak downtrend signal
- `$macd.down.high`           - strong downtrend signal
- `$macd.down.impulse`        - impulse downtrend signal
- `$macd.long-dmz`            - long signal in DMZ zone
- `$macd.long`                - long signal outside DMZ zone
- `$macd.short-dmz`           - short signal in DMZ zone
- `$macd.short`               - short signal outside DMZ zone

### SuperTrend

SuperTrend uses fast and slow ATR to calculate trend; you may pick, which one you want to use.

Both fast and slow are useful to confirm each other's "real" trend.

**Context**: `$super-trend`

**Config:**
- `$ctx.super-trend.fast-length`        - fast ATR length
- `$ctx.super-trend.fast-factor`        - fast factor
- `$ctx.super-trend.slow-length`        - slow ATR length
- `$ctx.super-trend.slow-factor`        - slow factor

**Flag**
- `$super-trend.fast.up`              - fast trend is uptrend
- `$super-trend.fast.up.start`        - fast trend has started (one time flag)
- `$super-trend.fast.up.end`          - fast trend has ended (one time flag)
- `$super-trend.fast.down`            - fast trend is downtrend
- `$super-trend.fast.down.start`      - fast trend has started (one time flag)
- `$super-trend.fast.down.end`        - fast trend has ended (one time flag)
- `$super-trend.slow.up`              - slow trend is uptrend
- `$super-trend.slow.up.start`        - slow trend has started (one time flag)
- `$super-trend.slow.up.end`          - slow trend has ended (one time flag)
- `$super-trend.slow.down`            - slow trend is downtrend
- `$super-trend.slow.down.start`      - slow trend has started (one time flag)
- `$super-trend.slow.down.end`        - slow trend has ended (one time flag)
- `$super-trend.up`                   - both fast and slow are uptrend
- `$super-trend.down`                 - both fast and slow are downtrend

### CCI

Commodity Channel Index, default TradingView implementation.

**Context**: `$cci`

**Config:**
- `$ctx.cci.length`            - length (int)

**Flag**
- `$cci.below.enter`         - Signal exits (> -100)
- `$cci.below.exit`          - Signal enters (< -100)
- `$cci.above.enter`         - Signal exits (> 100)
- `$cci.above.exit`          - Signal enters (< 100)
- `$cci.zone`                - Signal is in zone (>= -100, <= 100)

**Value**
- `$cci.signal`              - CCI value

### EMA

Default TradingView EMA using three lengths (if you want).

**Context**: `$ema`

**Config:**
- `$ctx.factor`               - affected by this config, through the day's factor
- `$ctx.ema.length-1`         - length 1 (int)
- `$ctx.ema.length-2`         - length 2 (int)
- `$ctx.ema.length-3`         - length 3 (int)

**Level**
- `$ema.1`              	 - EMA level for length 1
- `$ema.2`              	 - EMA level for length 2
- `$ema.3`              	 - EMA level for length 3

### Structure

This is simple structure detection; it's more dynamic, than day/low high, also supports it's fibbonachi levels; this indicator
may be somehow less accurate as it depends on user's input.

Structures enables tracking candles "in the void" - those in current extremes (above/below structure).

**Context**: `$structure`

**Config:**
- `$ctx.structure.length`		- lookback length
- `$ctx.structure.deviation`	- how/low deviation for structure change detection

**Flag**
- `$structure.change`			- structures' low/high has changed

**Value**
- `$structure.factor`			- current structure size (high - low) factor; similar to the day low/high factor

**Level**
- `$structure.low`     	 		- current structure's low
- `$structure.high`        	 	- current structure's high

### Current Trends

Simple tool, which tracks low/high of candles and dynamically updates low/high level based on number of touches candles has. This could
be useful to find interesting prices with more candles in the low/high range.

**Context**: `$trends`

**Config:**
- `$ctx.trends.factor`			- price tolerance factor (how wide the high/low level would be to count candle as a touch)
- `$ctx.trends.touches`			- how many touches are needed to update low/high level
- `$ctx.trends.trend`			- how many candles in-between must be to count a touch

**Flag**
- `$trends.up`				    - detected uptrend
- `$trends.down`				- detected downtrend

**Value**
- `$trends.up.count`			- uptrend length (number of candles)
- `$trends.down.count`			- downtrend length (number of candles)

**Level**
- `$trends.level`      	 		- point-of-interest level (level with most touches)
