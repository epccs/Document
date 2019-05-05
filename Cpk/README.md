# How to calculate Cp, Cpk and Ppk

## Overview

The Design limits include Upper Specification Limit USL and Lower Specification Limit LSL. The measurements include sample mean "m'," lot average "m," estimated standard deviation "s'" and standard deviation "s."


## Process indexs

Calculate Cp which is a process index that relates the specification range to process variation.

```
        Cp = (USL-LSL)/6s'
```

Cpk is a process index that relates the process mean to the nominal value of the two-sided specification. First, determine the difference between the process mean and the specification limits.

```
        XUSL = (USL-m')

        XLSL = (LSL-m')
```

Select the minimum of the two values.

```
        XMIN = min[XUSL, - XLSL]
```

Cpk is found by the following.

```
        Cpk = XMIN/3s'
```

Relation between Cp and Cpk

```
        Cpk = Cp(1-K)
```

K is found with Target value T, which is the center of the specification range (e.g., from upper specification limit and lower specification limit), and Specification width W, which is the specification range.

```
        T = (USL - LSL)/2

        W = USL-LSL

        K = (m' - T)/(W/2)
```

Ppk is the same as Cpk, however replace "m'" and "s'" with m and s. Its a difference of sample verse lot. 


## Ideal Test Limits

You might think that big Cpk numbers mean proper test limits, but that's wrong. Cpk= 1.33 (4 StdDev) is about the ideal; a little more is often needed in electronics to allow component variability that can't be controlled. Consider reducing the test limits if Cpk is 1.67 (5 StdDev) or larger. Some Quality people would say I was nut's for saying this, but the job of the test is to find things that are likely to be wrong or put another way unlikely to occur. I recall verifying problems on several telecom power converters that tested outside 4 StdDev but passed 5 StdDev. The rate at which the failures occurred was higher than statistics said they should happen, so those units were statistically likely to have a problem.


## Production Outliers
 
What are production outliers? Outliers are units that fall in the gray area of being statistically unlikely but within the design limits. Outliers are detected with probability. 

Some time back I did an automatic test system that tested and passed over 12,000 power processing devices a week, and in one week it passed 63 units outside the four standard deviation range. Assuming a normal distribution only 0.0063 percent should have failed, that is less than one part. The customer explained about reliability problems inherent in outliers and requested that we look at some, so I performed analysis on a batch of these units and saw things like parallel inductor windings where one was open, partial solder on power connections, and some wrong value parts. 

A well tested product looks at characteristics that maximize the use of statistics. One of the best examples I know is using bias (e.g., input current) in circuits that come into play only under cretin conditions like light load, thermal shutdown, current limit, input lockout, and output lockout. These bias level tests reveal circuit problems and often show outliers; they are easy to test, and statistical methods enhance them significantly. 



## Probability Between StdDev Ranges

```
    +/- 1 standard deviation 68.27%
    +/- 2 standard deviation 95.45%
    +/- 3 standard deviation 99.73%
    +/- 4 standard deviation 99.9936%
    +/- 5 standard deviation 99.999943%
```


## Reference

    Statistical Quality Design and Control 1992 Prentice-Hall, Inc.
    Process Measurement By Hawthorne SMT February 1997
