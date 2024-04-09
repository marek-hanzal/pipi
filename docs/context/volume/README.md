# Volume

This indicator tracks volume of orders and gives some little insight, what's happening.

**Context**: `volume`

**Config**:
_No config needed_

**Flags**:
*	`$volume.lower`  - volume is lower than previous bar
*	`$volume.higher` - volume is higher than previous bar
*	`$volume.min`    - new day minimum
*	`$volume.max`    - new day maximum

**Values**:
*	`$volume.size`     - actual volume
*	`$volume.range`    - volume range (difference between min and max)
*	`$volume.percent`  - volume percent of day range
