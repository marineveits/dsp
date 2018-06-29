[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

>> 
# 

```python
from __future__ import print_function, division

%matplotlib inline

import numpy as np

import nsfg
import first
import thinkstats2
import thinkplot
```


This function computes the biased PMF we would get if we surveyed students and asked about the size of the classes they are in.


```python
def BiasPmf(pmf, label):
    new_pmf = pmf.Copy(label=label)

    for x, p in pmf.Items():
        new_pmf.Mult(x, x)
        
    new_pmf.Normalize()
    return new_pmf
```


**Exercise:** Something like the class size paradox appears if you survey children and ask how many children are in their family. Families with many children are more likely to appear in your sample, and families with no children have no chance to be in the sample.

Use the NSFG respondent variable `numkdhh` to construct the actual distribution for the number of children under 18 in the respondents' households.

Now compute the biased distribution we would see if we surveyed the children and asked them how many children under 18 (including themselves) are in their household.

Plot the actual and biased distributions, and compute their means.


```python
resp = nsfg.ReadFemResp()
```


```python
# Solution goes here

pmf = thinkstats2.Pmf(resp.numkdhh, label='numkdhh')
```


```python
# Solution goes here

thinkplot.Pmf(pmf)
thinkplot.Config(xlabel='Number of kids', ylabel='PMF')
```


![alt text](https://github.com/marineveits/dsp/blob/master/img/2_01.png)



```python
# Solution goes here
biased=BiasPmf(pmf, label='biased')
```


```python
# Solution goes here
thinkplot.PrePlot(2)
thinkplot.Pmfs([pmf, biased])
thinkplot.Show(xlabel='Number of kids', ylabel='PMF')
```


![alt text](https://github.com/marineveits/dsp/blob/master/img/2_02.png)



    <matplotlib.figure.Figure at 0x254a2009ef0>



```python
# Solution goes here
pmf.Mean()
```




    1.024205155043831




```python
# Solution goes here
biased.Mean()
```




    2.403679100664282

