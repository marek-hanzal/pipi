# Cheat Sheet

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
