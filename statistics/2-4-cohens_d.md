[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

>> 

```python
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import nsfg
import first
import thinkstats2
import thinkplot
```

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
# The difference in means of the newborns' weight is 0.088 standard deviations, which is higher than the difference in pregnancy length # but yet it's a relatively small difference.

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

![alt text](https://github.com/marineveits/dsp/blob/master/img/1_01.png)


Make a histogram of <tt>age_r</tt>, the respondent's age at the time of interview.


```python
# Solution

hist = thinkstats2.Hist(resp.ager)
thinkplot.Hist(hist, label='ager')
thinkplot.Config(xlabel='age (years)', ylabel='Count')
```



Make a histogram of <tt>numfmhh</tt>, the number of people in the respondent's household.


```python
# Solution

hist = thinkstats2.Hist(resp.numfmhh)
thinkplot.Hist(hist, label='numfmhh')
thinkplot.Config(xlabel='number of people', ylabel='Count')
```



Make a histogram of <tt>parity</tt>, the number of children borne by the respondent.  How would you describe this distribution?


```python
# Solution

# This distribution is skewed to the right.

hist = thinkstats2.Hist(resp.parity)
thinkplot.Hist(hist, label='parity')
thinkplot.Config(xlabel='parity', ylabel='Count')
```



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

# This effect is stronger than the difference in pregnancy length, but given the design factors of the study we could not make any 
# signifficant conclusions

CohenEffectSize(rich.parity, notrich.parity)
```




    -0.12511855314660611



