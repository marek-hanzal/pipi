# PiPi

**This project is under heavy development, even it may look like an abandoned one. I'm still working on it. You may check [updates here](BLOG.md).**

> There will be revision of strategies **every month**; "main" branch will contain **current** month, while previous months will
> be archived in individual branches.

## Disclaimer

> **If you are beginner to trading, this system is not suitable for you**, even you can find pre-made strategies, which *may* work.

Before everything else: try whatever you find here, **test** your own mutations of strategies, but keep in mind - you're on the market and no indicator nor system is _**absolutly perfect**_.

**Keep in mind that this tool is extremely experimental, meaning I'm still trying to find its limits.**

## Intro

**PiPi** is a **_trading system_** which takes quite _different_ approach. Base idea is to _describe_ a strategy using simple custom language, so it behaves all
the time same way and is portable and then run strategy to see the results.

It may be used _only_ as buy/sell indicator using your own rules, or fully-fledged trading system with active backtesting (using TradingView's Strategy Tester).

The name - don't ask me, _even I don't know_ - it's just short and **I like it**.

> For quick reference, use [cheat.md](cheat.md) file.

## The Reason

Yes, this system integrates some commonly used indicators, but the main difference is in the way, how it works: you're targeting individual candlestick
patterns and executing actions on them. The idea was to convert human behavior into the code for consistent results in trading.

Common question **_"How would I do that as a human?"_** is transformed into the language, so you can share, version or quickly switch strategies for various situations.

> _Why language in language?_ You can program all the things here, but it's time consuming and you cannot simply share nor test the results. This is the bare system
> you can use with your rules, which you can simply switch. As en example, you can see this repository, where GIT is used to version strategies.

Also, custom language is used mainly for the ability to copy-paste the code. From there you can simply switch different strategies and test its
results. Yes, it may be a bit cumbersome without TradingView's inputs, but it's worth it - you'll never-ever forget any input.

## Strategies

Example strategies included are somehow optimized for profit, but the goal is not to have _"nice numbers"_ for you, but to find the best strategy possible using this tool.
Also huge emphasize goes to reproducible results, so you can pick a strategy and should see similar results on your own.

> All the times it depends on the market itself, time of the day, news, etc. - so don't expect from strategy same performance all the time.

## How it works

-  Configure the script, so all indicators works as you expect
-  Describe candlestick patterns you're looking for
-  Describe actions you want to do on match (render marker or make a trade)
-  Profit _(maybe)_

## Script

Parser _does not care about whitespaces_, only place with **significance** presence is keyword separation on the line. The advice is to use **tabs** for section identation, but at the end it's up to you.

> **Important note:** Because the language is written in PineScript, which was built for **_absolutely different_** purpose, parser has quite limited ability to detect errors and report them. So you **must** be careful, when writing your own script.

### init

#### Overview

Exactly one occurance: used to set "global" variables for indicators, including your own you may want to use in the script.

#### init - set

This section is a common way, how to set any variable in script. See the **set section** later on.

### pattern

#### Overview

This is the place most magic happens.

Pattern describes single candlestick pattern you want to match and action you want to execute. There may be as many patterns, as you want, only rule is to keep more specific at the top, rest at the bottom.

Good practice is to specify risky patterns with lower SL/TP, more specific and reliable patterns may have wider SL/TP.

One strategy may contain number of patterns using Day Low/High Fibbonachi, MACD, SuperTrend and other indicators, including individual candle shapes, so you can build one strategy based on patterns, MA indicators, or whatever you want.

> Script internally tracks limited number of candles a pattern runs on, so keep it mind if you want to write some extreme rules (really long and complex ones).

#### strict

Patterns in strict mode (with the `strict` keyword present) matches candles one-by-one, meaning if any candle breaks the rule, pattern won't match.

**Example:**
-  You're searching for `red, green, red, green`
-  Candles `green, red, [red, green], blue, green` - won't match
-  Candles `red, red, green, green, [red, green, red, green]` - will match

This mode is useful if you're looking for very specific pattern, for example exact candle shapes (three soldiers or something like that).

#### non-strict

**This is a default mode.**

Non-strict mode looks for patterns in the current buffer until all the rules matches. This is useful if you're looking for behavior of candles (like ok, we're on day's low, then 50, then day's high - go short).

**Example:**
-  You're searching for (fibb. percentage in short) `100, 70, 50, 20`
-  Candles on `70, 50, 70, 100, 50` - it's a surprise, they won't match
-  Candles on `70, [100], [70], 100, [50], 60, 50, [20]` - Yaaay, match!

#### Hacks

Because each pattern may execute an action, for example setting variable, you can create quite complex branching: one pattern sets variable `foo` and another patter may listen (have candle rule) for this flag. With this you can do quite a big magic.

### Scope

The script tracks variables in two scopes - one per-bar and one global.

#### State

Represented by `s:` prefix. State means _"keep and don't change until I want to"_ (or internal indicator wants to). This is useful, for example, if you want set uptrend/downtrend when it starts.

#### Bar

Represented by `b:` prefix. Bar variables are used only temporarily on the current bar, which is useful when you want to render a single marker or keep a value only until the next bar.

### Variable types

Because this system is hacked using PineScript which enforces types, they're propagated to the language itself.

**There are three types exposed:**
-  **flag**   -  boolean value; usually helper stuff from indicators (like if a candle is above a level or is there supertrend)
-  **value**  -  float value; all the values you may think of - candle's OHLC, day's levels, fibbo and much more
-  **name**   -  good old string value (rarely used) - _errrh... not sure, why I've put support for them here_

## Language

The language is extremely simple and somehow even stupid. But it's powerful enough to power your trading strategy, yaaay!

> **Tabs!** Tabs Everywhere, even when you may not like it, because (again...) it's hacked in PineScript, there are limited ways how to parse
> input, thus I had to choose some deliminator. Tabs won.

**There are a few rules:**
-  Tabs (again)
-  Indendation does not matter, but makes things much easier to read
-  Semicolon (`;`) end section; if you forget it, nobody cares, only whole system will mess up _(meaning, there is no error message)_

***

**Example piece by piece**

#### How to use Init section

```
init
    set
        name	s:#.session.time        1000-2300
        flag	s:#.session.exclusive	  true

        value	s:#.factor				      0.3
    ;
;
```

You should setup all values (including internal indicators) you may need, so your strategy remain consistant, when default values changes.

`set` keyword may used in different places, behavior is the same.

***

**Code:**
`name	s:#.session.time        1000-2300`

**Meaning:**
Set type `name` (string) in the global state `s:` in global context `#` with the name `session.time` to a value `1000-2300`; the global context `#` is built in and indicators usually reads values from it.

***

**Code:**
`flag	s:#.session.exclusive	  true`

**Meaning:**
Set type `flag` (boolean) in the global state `s:` in global context `#` with the name `session.exclusive` to `true`.

***

**Code:**
`value	s:#.factor				      0.3`

**Meaning:**
Set type `value` (float) in the global state `s:` in global context `#` with the name `factor` to `0.3`. Here you have to be **very careful** about decimals.
