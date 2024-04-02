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

## Day High/Low

Tracks daily high and low prices.

**Context**: `day`

**Config:**
- `#.factor`          - factor used to calculate day's factor (used for level size calculation)

**Flags:**
- `b:day.change`      - new day/new low/new high
- `b:day.new-day`     - new day
- `b:day.higher-high` - new day's higher high
- `b:day.lower-low`   - new day's lower low

**Values:**
- `s:day.factor`      - day's factor used to calculate level size (for example for fibbo and so on);
  						the reason is to keep levels somehow in sync with day's price range 
- `s:day.size`        - day's price range

## Day Fibbonachi

This one works in cooperation with day's low/high. Tracks fibbonachi levels during the day.

**Config:**
- `#.factor`          - affected by this config, through the day's factor

**Levels:**
- `s:day-fibbonachi.0`	 - day's high
- `s:day-fibbonachi.20`  - 23.6%
- `s:day-fibbonachi.30`  - 38.2%
- `s:day-fibbonachi.50`  - 50%
- `s:day-fibbonachi.60`  - 61.8%
- `s:day-fibbonachi.70`  - 78.6%
- `s:day-fibbonachi.100` - day's low

## Candle

Tracks a lot of information about a candle.

**Context**: `candle`

**Config:**
_No config required._

**Flags:**
- `b:candle.red`       - red candle
- `b:candle.green`     - green candle

**Values:**
- `b:candle.[open/high/low/close]`      - candle's OHLC; for example `b:candle.low`

