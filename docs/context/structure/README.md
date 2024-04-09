# Structure

Simple dynamic structure detection. This tool tracks low/high price changes in shorter frame then day low/high.

**Context**: `structure`

**Config**:
*	`$ctx.structure.length`     - length of structure detection (number of bar lookback)
*	`$ctx.structure.deviation`  - standard deviation of structure detection

**Flags**:
_No flags provided_

**Values**:
_No values provided_

**Levels**:
*	`$structure.low`  	- low price of structure
*	`$structure.high`    - high price of structure
