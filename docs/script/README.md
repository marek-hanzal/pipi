# PiPi Script

Here you'll find, how to write scripts for PiPi Trading System.

## Overview

White spaces are not important for the parser, they're only useful for humans, so we can read the code more easily.

> **First rule:** Use TABs. **Second rule:** Don't forgot semicolons!

## Script

The script itself is simple, it consists only of series of simple commands describing which candlestick patterns you want to find
and what to do afterward.

**Let's see an example:**

```
//	Comments are allowed, so you can write anything you want here; keep in mind TradingView has limited number of characters for a script,
//	so you may want to keep it short and simple or strip comments at all.

//	Init section is used for setting up environment and all indicators, so you can be sure your strategy will work as expected.
//		- This section should be present only once
//		- You can set required priority level of your patterns, so you can simply skip some of them
//		- You can enable or disable long or short trades
//		- You *should* set all your defaults for indicators here as I may change defaults in the future breaking your strategy
init
	//	Set required priority level; all patterns with lower priority will be skipped
	priority	15
	//	Enable/Disable long trades
	long		true
	//	Enable/Disable short trades
	short		true
	//	This section allows setting various variables for your strategy
	//		- You can also set your own variables here
	set
		//	There are few types of variables you can set:
		//		- string for text
		//		- bool for true/false
		//		- float for numbers	
		
		//	This is not documentation of individual features more like
		//	how to write the script.
		
		//	Example of setting up string value:
		//		- specify type - [string]
		//		- tell the script, where to put the value - [$ctx.session.time]
		//		- set the value - [0900-2300]
		string	$ctx.session.time				0900-2300
		
		//	Example of setting up bool value:
		//		- specify type - [bool]
		//		- tell the script, where to put the value - [$ctx.session.exclusive]
		//		- set the value - [true]
		bool	$ctx.session.exclusive			true

		//	Example of setting up float value:
		//		- specify type - [float]
		//		- tell the script, where to put the value - [$ctx.factor]
		//		- set the value - [0.1]
		float	$ctx.factor						0.1

		float	$ctx.cci.length					25

		float	$ctx.macd.length				9
		float	$ctx.macd.signal				5
		float	$ctx.macd.dmz					4.5
		float	$ctx.macd.impulse				3
		float	$ctx.macd.flat					0.1

		float	$ctx.ema.length-1				12
		float	$ctx.ema.length-2				50
		float	$ctx.ema.length-3				200

		float	$ctx.super-trend.fast-length	12
		float	$ctx.super-trend.fast-factor	1
		float	$ctx.super-trend.slow-length	24
		float	$ctx.super-trend.slow-factor	5

		float	$ctx.trends.factor				0.2
		float	$ctx.trends.touches				3
		float	$ctx.trends.count				3
		float	$ctx.trends.trend				3

		float	$ctx.take-profit				5
		float	$ctx.stop-loss					5
		float	$ctx.position-size				2
		
	//	Important piece! Semicolon, because this tells parser to end this block
	;
//	Last one is also important, that's the reason tabs are recommended. 
;
```
