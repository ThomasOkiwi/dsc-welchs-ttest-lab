# Welch's T-Test

## Introduction
Thus far, you've seen the traditional Student's t-test for hypothesis testing between two sample means. Recall that z-tests are also appropriate for statistics, such as the mean, which can be assumed to be normally distributed. However, when sample sizes are low (n_observations < 30), the t-test is more appropriate, as the t-distribution has heavier tails. Even with this modification, remember that there are still several assumptions to the model. Most notably, traditional t-tests assume that sample sizes and sample variances between the two groups are equal. When these assumptions are not met, Welch's t-test is generally a more reliable test.

## Objectives
You will be able to:

- List the conditions needed to require a Welch's t-test
- Calculate the degrees of freedom for a Welch's t-test
- Calculate p-values using Welch's t-test
## T-test review
Recall that t-tests are a useful method for determining whether the mean of two small samples indicate different underlying population parameters. The reasoning behind this begins with the use of z-tests to calculate the likelihood of sampling a particular value from a normal distribution. Furthermore, by the central limit theorem, the mean of a sample is a normally distributed variable centered around the actual underlying population mean. That said, t-tests are more appropriate for small samples (n_observations < 30), due to disproportionate tails. Finally, recall that the t-distribution actually converges to a normal distribution as the degrees of freedom continues to increase.



A normal distribution vs. t-distributions with varying degrees of freedom. Note how the t-distribution approaches the normal distribution as the degrees of freedom increases. Recall that when performing a two-sample t-test, assuming that sample variances are equal, the degrees of freedom equals the total number of observations in the samples minus two.

## Welch's t-test
Just as Student's t-test is a useful adaptation of the normal distribution which can lead to better likelihood estimates under certain conditions, the Welch's t-test is a further adaptation that accounts for additional perturbations in the underlying assumptions of the model. Specifically, the Student's t-test assumes that the samples are of equal size and equal variance. When these assumptions are not met, then Welch's t-test provides a more accurate p-value.

Here is how you calculate it:



where

 - mean of sample i
 - variance of sample i
 - sample size of sample i
The modification is related to the degrees of freedom in the t-test, which tends to increase the test power for samples with unequal variance. When two groups have equal sample sizes and variances, Welch’s t-test tends to give the same result as the Student’s t-test. However, when sample sizes and variances are unequal, Student’s t-test is quite unreliable, whereas Welch’s tends perform better.

## Calculate the degrees of freedom
Once the t-score has been calculated for the experiment using the above formula, you then must calculate the degrees of freedom for the t-distribution. Under the two-sample Student's t-test, this is simply the total number of observations in the samples size minus two, but given that the sample sizes may vary using the Welch's t-test, the calculation is a bit more complex:



## Calculate p-values
Finally, as with the Student's t-test (or a z-test for that matter), you convert the calculated score into a p-value in order to confirm or reject the null-hypothesis of your statistical experiment. For example, you might be using a one-sided t-test to determine whether a new drug had a positive effect on patient outcomes. The p-value for the experiment is equivalent to the area under the t-distribution with the degrees of freedom, as calculated above, and the corresponding t-score.



The easiest method for determining said p-values is to use the .cdf() method from scipy.stats to find the complement and subtracting this from 1.



Here's the relevant code snippet:

import scipy.stats as stats

p = 1 - stats.t.cdf(t, df)
## Summary
This lesson briefly introduced you to another statistical test for comparing the means of two samples: Welch's t-test. Remember that when your samples are not of equal size or do not have equal variances, it is a more appropriate statistical test than the Student's t-test!





# Welch's T-test - Lab

## Introduction 

Now that you've gotten a brief introduction to Welch's t-test, it's time to practice your NumPy and math programming skills to write your own Welch's T-test function! 

## Objectives

In this lab you will: 

- Write a function to calculate Welch's t-score 
- Calculate the degrees of freedom for a Welch's t-test   
- Calculate p-values using Welch's t-test


### Welch's t-test

Recall that Welch's t-Test is given by  

```math
t = \frac{\bar{X_1}-\bar{X_2}}{\sqrt{\frac{{s_1}^2}{N_1} + \frac{{s_2}^2}{N_2}}} = \frac{\bar{X_1}-\bar{X_2}}{\sqrt{{se_1}^2+{se_2}^2}}
```

where $\bar{X_i}$ , ${s_i}^2$, and $N_i$ are the sample mean, sample variance, and sample size, respectively, for sample i.

Write a function for calculating Welch's t-statistic using two samples a and b. To help, 2 potential samples are defined below.

> **Important Note**: While the formula does not indicate it, it is appropriate to take the absolute value of the t-value.


```python
import numpy as np

np.random.seed(82)
control = np.random.normal(loc=10, scale=1, size=8)
treatment = np.random.normal(loc=10.5, scale=1.2, size=12)
```


```python
control
```


```python
treatment
```


```python
def welch_t(a, b):
    
    """ Calculate Welch's t-statistic for two samples. """

    
    return # Return the t-score!

welch_t(control, treatment)
# 2.0997990691576858
```

## Degrees of freedom

Once you have the t-score, you also need to calculate the degrees of freedom to determine the appropriate t-distribution and convert this score into a p-value. The effective degrees of freedom can be calculated using the formula:

# $ v \approx \frac{\left( \frac{{s_1}^2}{N_1} + \frac{{s_2}^2}{N_2}\right)^2}{\frac{{s_1}^4}{{{N_1}^2}v_1} + \frac{{s_2}^4}{{{N_2}^2}v_2}} $

$N_i$ - sample size of sample i  
${s_i}^2$ - variance of sample i  
$v_i$ - degrees of freedom for sample i (equivalent to $N_i-1$)  
  
Write a second function to calculate degree of freedom for above samples:


```python
def welch_df(a, b):
    
    """ Calculate the effective degrees of freedom for two samples. """
    return # Return the degrees of freedom

welch_df(control, treatment)
# 17.673079085111
```

Now calculate the welch t-score and degrees of freedom from the samples, a and b, using your functions.


```python
# Your code here; calculate t-score and the degrees of freedom for the two samples, a and b
t = None
df = None
print(t, df)
# 2.0997990691576858 17.673079085111
```

## Convert to a p-value

Great! Now that you have the t-score and the degrees of freedom, it's time to convert those values into a p-value (for a one-sided t-test). Remember that the easiest way to do this is to use the cumulative distribution function associated with your particular t-distribution.  

Calculate the p-value associated with this experiment.


```python
# Your code here; calculate the p-value for the two samples defined above

p = None
print(p)
# 0.025191666225846454
```

In this case, there is a 2.5% probability you would see a t-score equal to or greater than what you saw from the data. Given that alpha was set at 0.05, this would constitute sufficient evidence to reject the null hypothesis.

Building on this, you can also write a function that calculates the p-value for given samples with a two-sided test by taking advantage of the symmetry of the t-distribution to calculate only one side. The two-tailed p-value is simply twice the one-tailed value because you want the probability:  
> $t<−|t̂|$ and  $t>|t̂|$ , where t̂  is the t-statistic calculated from our data  

With that, define a summative function `p_val_welch(a, b, two_sided=False)` which takes in two samples a and b, as  well as an optional binary variable to allow you to toggle between a one and two-sided Welch's t-test.   

> The default behavior should be set to one-sided as indicated above. If the parameter two_sided is set to True, the function should return the p-value for a two-sided t-test, as opposed to a one-sided t-test.


```python
def p_value(a, b, two_sided=False):
    # Your code here
    return # Return the p-value!
```

Now briefly test your function; no need to write any code just run the cells below to ensure your function is operating properly. The output should match the commented values.


```python
p_value(treatment, control)
# 0.025191666225846454
```


```python
p_value(treatment, control, two_side=True)
# 0.05038333245169291
```

## Summary

Nice work! In this lab, you practiced implementing functions for Welch's t-test when sample variances or sample sizes differ in your experimental groups. You also got to review converting t-scores into p-values. All of this should continue to build on your abilities to effectively design and carry out statistically valid hypothesis tests.
