# Structure Fibbonachi Levels

This indicator provides Fibbonachi levels for the structure based on lowest / highest prices.

**Context**: `structure-fibbonachi`

**Config**:
*	`$ctx.factor`  - factor used to compute structure's factor based on low and high prices (defines fibbonachi level size)

**Flags**:
_No flags provided_

**Values**:
*  `$structure-fibbonachi.factor`   - based on `$ctx.factor` and low / high prices of the structure

**Levels**:
*	`$structure-fibbonachi.0`  	- 0% level (highest price)
*	`$structure-fibbonachi.20`   - 23.6% level
*   `$structure-fibbonachi.30`    - 38.2% level
*   `$structure-fibbonachi.50`    - 50% level
*   `$structure-fibbonachi.60`    - 61.8% level
*   `$structure-fibbonachi.70`    - 76.4% level
*   `$structure-fibbonachi.100`   - 100% level (lowest price)
