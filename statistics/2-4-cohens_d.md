[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

>> 
# Examples and Exercises from Think Stats, 2nd Edition

http://thinkstats2.com

Copyright 2016 Allen B. Downey

MIT License: https://opensource.org/licenses/MIT



```python
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import nsfg
import first
```

Given a list of values, there are several ways to count the frequency of each value.


```python
t = [1, 2, 2, 3, 5]
```

You can use a Python dictionary:


```python
hist = {}
for x in t:
    hist[x] = hist.get(x, 0) + 1
    
hist
```




    {1: 1, 2: 2, 3: 1, 5: 1}



You can use a `Counter` (which is a dictionary with additional methods):


```python
from collections import Counter
counter = Counter(t)
counter
```




    Counter({1: 1, 2: 2, 3: 1, 5: 1})



Or you can use the `Hist` object provided by `thinkstats2`:


```python
import thinkstats2
hist = thinkstats2.Hist([1, 2, 2, 3, 5])
hist
```




    Hist({1: 1, 2: 2, 3: 1, 5: 1})



`Hist` provides `Freq`, which looks up the frequency of a value.


```python
hist.Freq(2)
```




    2



You can also use the bracket operator, which does the same thing.


```python
hist[2]
```




    2



If the value does not appear, it has frequency 0.


```python
hist[4]
```




    0



The `Values` method returns the values:


```python
hist.Values()
```




    [1, 2, 3, 5]



So you can iterate the values and their frequencies like this:


```python
for val in sorted(hist.Values()):
    print(val, hist[val])
```

    1 1
    2 2
    3 1
    5 1
    

Or you can use the `Items` method:


```python
for val, freq in hist.Items():
     print(val, freq)
```

    1 1
    2 2
    3 1
    5 1
    

`thinkplot` is a wrapper for `matplotlib` that provides functions that work with the objects in `thinkstats2`.

For example `Hist` plots the values and their frequencies as a bar graph.

`Config` takes parameters that label the x and y axes, among other things.


```python
import thinkplot
thinkplot.Hist(hist)
thinkplot.Config(xlabel='value', ylabel='frequency')
```


![png](output_23_0.png)


As an example, I'll replicate some of the figures from the book.

First, I'll load the data from the pregnancy file and select the records for live births.


```python
preg = nsfg.ReadFemPreg()
live = preg[preg.outcome == 1]
```

Here's the histogram of birth weights in pounds.  Notice that `Hist` works with anything iterable, including a Pandas Series.  The `label` attribute appears in the legend when you plot the `Hist`. 


```python
hist = thinkstats2.Hist(live.birthwgt_lb, label='birthwgt_lb')
thinkplot.Hist(hist)
thinkplot.Config(xlabel='Birth weight (pounds)', ylabel='Count')
```


![png](output_27_0.png)


Before plotting the ages, I'll apply `floor` to round down:


```python
ages = np.floor(live.agepreg)
```


```python
hist = thinkstats2.Hist(ages, label='agepreg')
thinkplot.Hist(hist)
thinkplot.Config(xlabel='years', ylabel='Count')
```


![png](output_30_0.png)


As an exercise, plot the histogram of pregnancy lengths (column `prglngth`).


```python
# Solution

hist = thinkstats2.Hist(live.prglngth, label='prglngth')
thinkplot.Hist(hist)
thinkplot.Config(xlabel='weeks', ylabel='Count')
```


![png](output_32_0.png)


`Hist` provides smallest, which select the lowest values and their frequencies.


```python
for weeks, freq in hist.Smallest(10):
    print(weeks, freq)
```

    0 1
    4 1
    9 1
    13 1
    17 2
    18 1
    19 1
    20 1
    21 2
    22 7
    

Use `Largest` to display the longest pregnancy lengths.


```python
# Solution

for weeks, freq in hist.Largest(10):
    print(weeks, freq)
```

    50 2
    48 7
    47 1
    46 1
    45 10
    44 46
    43 148
    42 328
    41 587
    40 1116
    

From live births, we can selection first babies and others using `birthord`, then compute histograms of pregnancy length for the two groups.


```python
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]

first_hist = thinkstats2.Hist(firsts.prglngth, label='first')
other_hist = thinkstats2.Hist(others.prglngth, label='other')
```

We can use `width` and `align` to plot two histograms side-by-side.


```python
width = 0.45
thinkplot.PrePlot(2)
thinkplot.Hist(first_hist, align='right', width=width)
thinkplot.Hist(other_hist, align='left', width=width)
thinkplot.Config(xlabel='weeks', ylabel='Count', xlim=[27, 46])
```


![png](output_40_0.png)


`Series` provides methods to compute summary statistics:


```python
mean = live.prglngth.mean()
var = live.prglngth.var()
std = live.prglngth.std()
```

Here are the mean and standard deviation:


```python
mean, std
```




    (38.56055968517709, 2.702343810070593)



As an exercise, confirm that `std` is the square root of `var`:


```python
# Solution

np.sqrt(var) == std
```




    True



Here's are the mean pregnancy lengths for first babies and others:


```python
firsts.prglngth.mean(), others.prglngth.mean()
```




    (38.60095173351461, 38.52291446673706)



And here's the difference (in weeks):


```python
firsts.prglngth.mean() - others.prglngth.mean()
```




    0.07803726677754952



This functon computes the Cohen effect size, which is the difference in means expressed in number of standard deviations:


```python
def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d
```

Compute the Cohen effect size for the difference in pregnancy length for first babies and others.


```python
# Solution

CohenEffectSize(firsts.prglngth, others.prglngth)
```




    0.028879044654449883



## Exercises

Using the variable `totalwgt_lb`, investigate whether first babies are lighter or heavier than others. 

Compute Cohen’s effect size to quantify the difference between the groups.  How does it compare to the difference in pregnancy length?


```python
# Solution

firsts.totalwgt_lb.mean(), others.totalwgt_lb.mean()
```




    (7.201094430437772, 7.325855614973262)




```python
# Solution

CohenEffectSize(firsts.totalwgt_lb, others.totalwgt_lb)
```




    -0.088672927072602006



For the next few exercises, we'll load the respondent file:


```python
resp = nsfg.ReadFemResp()
```

Make a histogram of <tt>totincr</tt> the total income for the respondent's family.  To interpret the codes see the [codebook](http://www.icpsr.umich.edu/nsfg6/Controller?displayPage=labelDetails&fileCode=FEM&section=R&subSec=7876&srtLabel=607543).


```python
# Solution

hist = thinkstats2.Hist(resp.totincr)
thinkplot.Hist(hist, label='totincr')
thinkplot.Config(xlabel='income (category)', ylabel='Count')
```


![png](output_62_0.png)


Make a histogram of <tt>age_r</tt>, the respondent's age at the time of interview.


```python
# Solution

hist = thinkstats2.Hist(resp.ager)
thinkplot.Hist(hist, label='ager')
thinkplot.Config(xlabel='age (years)', ylabel='Count')
```


![png](output_64_0.png)


Make a histogram of <tt>numfmhh</tt>, the number of people in the respondent's household.


```python
# Solution

hist = thinkstats2.Hist(resp.numfmhh)
thinkplot.Hist(hist, label='numfmhh')
thinkplot.Config(xlabel='number of people', ylabel='Count')
```


![png](output_66_0.png)


Make a histogram of <tt>parity</tt>, the number of children borne by the respondent.  How would you describe this distribution?


```python
# Solution

# This distribution is positive-valued and skewed to the right.

hist = thinkstats2.Hist(resp.parity)
thinkplot.Hist(hist, label='parity')
thinkplot.Config(xlabel='parity', ylabel='Count')
```


![png](output_68_0.png)


Use Hist.Largest to find the largest values of <tt>parity</tt>.


```python
# Solution

hist.Largest(10)
```




    [(22, 1),
     (16, 1),
     (10, 3),
     (9, 2),
     (8, 8),
     (7, 15),
     (6, 29),
     (5, 95),
     (4, 309),
     (3, 828)]



Let's investigate whether people with higher income have higher parity.  Keep in mind that in this study, we are observing different people at different times during their lives, so this data is not the best choice for answering this question.  But for now let's take it at face value.

Use <tt>totincr</tt> to select the respondents with the highest income (level 14).  Plot the histogram of <tt>parity</tt> for just the high income respondents.


```python
# Solution

rich = resp[resp.totincr == 14]
hist = thinkstats2.Hist(rich.parity)
thinkplot.Hist(hist, label='parity')
thinkplot.Config(xlabel='parity', ylabel='Count')
```


![png](output_72_0.png)


Find the largest parities for high income respondents.


```python
# Solution

hist.Largest(10)
```




    [(8, 1), (7, 1), (5, 5), (4, 19), (3, 123), (2, 267), (1, 229), (0, 515)]



Compare the mean <tt>parity</tt> for high income respondents and others.


```python
# Solution

notrich = resp[resp.totincr < 14]
rich.parity.mean(), notrich.parity.mean()
```




    (1.0758620689655172, 1.2495758136665125)



Compute the Cohen effect size for this difference.  How does it compare with the difference in pregnancy length for first babies and others?


```python
# Solution

# This effect is about 10 times stronger than the difference in pregnancy length.
# But remembering the design of the study, we should not make too much of this
# apparent effect.

CohenEffectSize(rich.parity, notrich.parity)
```




    -0.12511855314660611



