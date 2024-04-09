# Day

This indicator tracks day low and high prices.

**Context**: `day`

**Config**:
*	`$ctx.factor`  - factor used to compute day's factor based on low and high prices

**Flags**:
*	`$day.change`       - any change - low, high, new day
*	`$day.new-day`      - new day
*	`$day.higher-high`  - new day high
*   `$day.lower-high`   - new day low

**Values**:
*	`$day.factor` 		- based on `$ctx.factor` and low / high prices
*	`$day.size`         - low / high price range

**Levels**:
*	`$day.level`        - level with low / high of day price range
