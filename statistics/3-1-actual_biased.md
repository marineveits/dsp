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



**Exercise:** Something like the class size paradox appears if you survey children and ask how many children are in their family. Families with many children are more likely to appear in your sample, and families with no children have no chance to be in the sample.

Use the NSFG respondent variable `numkdhh` to construct the actual distribution for the number of children under 18 in the respondents' households.

Now compute the biased distribution we would see if we surveyed the children and asked them how many children under 18 (including themselves) are in their household.

Plot the actual and biased distributions, and compute their means.


```python
resp = nsfg.ReadFemResp()
```


```python
# Solution 

pmf = thinkstats2.Pmf(resp.numkdhh, label='numkdhh')
```


```python
# Solution 

thinkplot.Pmf(pmf)
thinkplot.Config(xlabel='Number of kids', ylabel='PMF')
```


![alt text](https://github.com/marineveits/dsp/blob/master/img/2_01.png)



```python
# Solution
biased=BiasPmf(pmf, label='biased')
```


```python
# Solution 
thinkplot.PrePlot(2)
thinkplot.Pmfs([pmf, biased])
thinkplot.Show(xlabel='Number of kids', ylabel='PMF')
```


![alt text](https://github.com/marineveits/dsp/blob/master/img/2_02.png)



   



```python
# Solution 
pmf.Mean()
```




    1.024205155043831




```python
# Solution 
biased.Mean()
```




    2.403679100664282

