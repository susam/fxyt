FXYT Community Demos
=====================

This document contains a collection of FXYT demos contributed by
community members.  FXYT is a tiny, esoteric, stack-based, postfix,
canvas colouring language with only 36 simple commands.  See
[README.md][] for more details about FXYT.

If you create an interesting demo please [create a new post][post] and
share it.  If the demo looks very interesting, it may be added to this
document.

[README.md]: README.md
[post]: https://github.com/susam/fxyt/issues/new


Contents
--------

* [Dynamic Demos](#dynamic-demos)
  * [Ripples](#ripples)
* [Static Demos](#static-demos)
  * [Mandelbrot](#mandelbrot)
  * [Grid](#grid)
  * [Filled Circle](#filled-circle)
  * [Filled Square](#filled-square)
  * [Rotated Filled Square](#rotated-filled-square)
  * [Empty Square](#empty-square)
  * [Empty Right Triangle](#empty-right-triangle)
  * [Flag of France](#flag-of-france)
  * [Flag of Italy](#flag-of-italy)


Dynamic Demos
------------

### Ripples


Contributed by [TJSomething][] on 22 Dec 2023.

```
MXN127-D*YN127-D*+N5/DN2/NN6[RRSDRDRS/+N2/R]PSPTN2*-N20%N12*
```

<https://susam.net/fxyt.html#MXN127dDpYN127dDpsN5qDN2qNN6bRRSDRDRSqsN2qRcPSPTN2pdN20rN12p>


Static Demos
-------------

### Mandelbrot


Contributed by [Nick Craig-Wood][] on 23 Mar 2024.

```
NNNN7[SDD*N1024/RDD*N1024/R+N4096<[RN1+RR]SDD*N1024/RDD*N1024/R+N4096<![PPN4000N4000]SDD*N1024/RDD*N1024/RS-XN128-N12*N512-+RR*N512/YN128-N12*+]PPN30*
```


### Grid

Contributed by [Greg Oledzki][] on 23 Mar 2024.

```
XN15%0N0=YN15%0N0=|00
```

https://susam.net/fxyt.html#XN15r0N0eYN15r0N0eo00

### Checkers pattern
Contributed by [Greg Oledzki][] on 23 Mar 2024.

```
XN16/N2%N0=YN16/N2%N1=^N255*
```

https://susam.net/fxyt.html#XN16qN2rN0eYN16qN2rN1exN255p


### Filled Circle

Contributed by [Greg Oledzki][] on 23 Mar 2024.

```
XN128-XN128-*YN128-YN128-*+N128/N64<00
```

https://susam.net/fxyt.html#XN128dXN128dpYN128dYN128dpsN128qN64l00


### Filled Square

Contributed by [Greg Oledzki][] on 23 Mar 2024.

```
XN99>XN157<&YN99>&YN157<&N255*
```

https://susam.net/fxyt.html#XN99gXN157laYN99gaYN157laN255p


### Rotated Filled Square

Contributed by [Greg Oledzki][] on 24 Mar 2024.

```
XY+N192>XY-N64<&YX-N64<&XY+N320<&00
```

https://susam.net/fxyt.html#XYsN192gXYdN64laYXdN64laXYsN320la00


### Empty Square

Contributed by [Greg Oledzki][] on 23 Mar 2024.

```
XN100=XN156=|YN100=YN156=||XN99>&XN157<&YN99>&YN157<&N255*
```

https://susam.net/fxyt.html#XN100eXN156eoYN100eYN156eooXN99gaXN157laYN99gaYN157laN255p


### Empty Right Triangle

Contributed by [Greg Oledzki][] on 23 Mar 2024.

```
XY-N0=YN50=|CXN206=|XN49>&XN207<&YN49>YN206<&&00
```

https://susam.net/fxyt.html#XYdN0eYN50eoCXN206eoXN49gaXN207laYN49gYN206laa00


#### Flag of France

Contributed by [Greg Oledzki][] on 24 Mar 2024.

```
XN107>XN192<&N255*XN107>XN151<&N255*XN64>XN151<&N255*
```

https://susam.net/fxyt.html#XN107gXN192laN255pXN107gXN151laN255pXN64gXN151laN255p


#### Flag of Italy

Contributed by [Greg Oledzki][] on 24 Mar 2024.

Continuing with the above example for the national Flag of France,
the one for Italy is just `RR` away.

```
XN107>XN192<&N255*XN107>XN151<&N255*XN64>XN151<&N255*RR
```

https://susam.net/fxyt.html#XN107gXN192laN255pXN107gXN151laN255pXN64gXN151laN255pRR


<!-- Authors -->

[TJSomething]: https://news.ycombinator.com/user?id=TJSomething
[Nick Craig-Wood]: https://github.com/ncw
[Greg Oledzki]: https://github.com/mccartney
