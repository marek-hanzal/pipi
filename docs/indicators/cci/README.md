## CCI
This is default implementation of CCI (Commodity Channel Index) without any further modifications.

Example:

![img.png](img.png)

### How it works

Because CCI is an oscillator, I've used markers on the chart to show the signals.

Basically you can read what's happening from the markers - if there is overbought/oversold state and also where the signal is going (from overbought to zero
to oversold, or jumping around or whatever).

It's possible to get the reversal point using this indicator (with a price action).

The signals are:

Signal enters < -100 zone:

![below-minus-100.png](below-minus-100.png)

Signal exits < -100 zone:

![exit-minus-100.png](exit-minus-100.png)

Signal crosses over zero:

![over-zero.png](over-zero.png)

Signal crosses under zero:

![under-zero.png](under-zero.png)

Signal enters > 100 zone:

![over-100.png](over-100.png)

Signal exits > 100 zone:

![exit-100.png](exit-100.png)
