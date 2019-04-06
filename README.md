
# One-Sample z-Test

## Introduction

A one-sample z test is the most basic type of hypothesis test and is performed when the population means and standard deviation are known. This makes the analysis very simple. The main takeaway from this lesson and next lab is to have an idea around the process of hypothesis testing and understanding test statistics and p-values. 

## Objectives:
You will be able to:
* Understand and explain use cases for a 1-sample z-test
* Set up null and alternative hypotheses
* Calculate z statistic using z-tables and cdf functions
* Calculate and interpret p-value for significance of results

## One-Sample z-test

**The one-sample z-test is best suited for situations where we we want to investigate whether a given "sample" comes from a particular "population".**

The best way to explain how 1-sample z-tests work is by using an example. 
Let's set up a problem scenario (known as a research question or analytical question) and apply a 1-sample z-test, while explaining all the steps required to call our results "statistically significant".

## The Analytical Question 

A researcher wants to study the effects of mentoring on intelligence scores. He wants to know as a baseline what the average intelligence of his students were relative to the general population. He used a standardized IQ test which has a mean of 100 and standard deviation of 16. The 50 students in his study scored an average of 102 on the IQ test. He wants to investigate  whether the increase is IQ for the sample students is because of mentoring. 

## Step 1: State Your Hypotheses

### The Alternative Hypothesis ($H_a$)

The alternative hypothesis always reflects the the idea or theory that needs to be tested. For this problem, you want to test if the mentoring has resulted in a significant increase in student IQ. So, you would write it down as:

> The sample mean is **significantly** bigger than the population mean

Again, significance is the key here. If we denote sample mean as $M$, and population mean as mu ($\mu$), you can write the alternative hypothesis as:

$$\large H_a\text{:   }\mu < M$$

The alternative hypothesis here is that $\mu$ is less than $M$. In other situations, you could check for both possibilities of be $\mu$ being smaller OR bigger than by checking  $\mu \neq M$. 

Maybe the mentoring results as a lower IQ... Who knows!

<img src="images/baby.jpeg" width=400>

For now, you'll just check for the **siginificant increase** for now to keep the process simple.

### The Null Hypothesis ($H_0$)

For a one-sample z-test, you define your null hypothesis as there being **no significant difference** between specified sample and population. This means that under the null hypothesis, you assume that any observed (generally small) difference may be present due to sampling or experimental error. Considering this, for this problem, you can define a null hypothesis ($H_0$) as:

> There is **no significant difference** between sample mean and population mean 

Remember the emphasis is on a _significant_ difference, rather than just any difference as a natural result of taking samples.

Denoting the sample mean as $M$, and population mean as mu ($\mu$), you can write the null hypothesis as:

$$\large H_0\text{:   }\mu \geq M$$


## Step 2: Specify a Significance Level (alpha)

Now that your hypotheses are in place, you have to decide on your significance level alpha ($\alpha$) as a cut-off value to define whether you can reject your null hypothesis or not.

As discussed previously, often, $\alpha$ is set to 0.05, which also has as a side-effect that there is a 5 percent chance that you will reject the null hypothesis when it is true.
Later, you'll see that using alpha, you'll formulate your test result as: "with a confidence level of 95%, we can state that...". For a z-distribution, this can be shown as below:

<img src="images/hypothesis_test.jpg" width=670>


If you test both sides of the distribution ($\mu \neq M$, when $\mu$ can either be smaller OR bigger), you need to perform a 2-tail test to see if mentoring lowers OR highers the IQ of student.

Each red region would calculated as $\dfrac{\alpha}{2}$. When testing of single side (as in the example) i.e. just higher OR just lower, you can use a one-tail test as shown in the first and second images. The $\alpha$ value we use is 0.05 or $5\%$.

## Step 3: Calculate the test statistic

For z-tests, a z-statistic is used as our test statistic. You'll see other statistics suitable for other tests later. A one-sample z-statistic is calculated as:

$$ \large \text{z-statistic} = \dfrac{\bar x - \mu_0}{{\sigma}/{\sqrt{n}}} $$

This formula slightly differs from the standard score formula. It includes the square square root of n to reflect that we are dealing with the sample variance here. 

Now, all you need to do, is use this formula given your sample mean $\bar x$, the population standard deviation $\sigma$, and the number of items in the sample ($n$). $\mu_0$ is the mean you're testing the hypothesis for, or the "hypothesized mean". 

Let's use Python to calculate this. 


```python
import scipy.stats as stats
from math import sqrt
x_bar = 102 # sample mean 
n = 50 # number of students
sigma = 16 # sd of population
mu = 100 # Population mean 

z = (x_bar - mu)/(sigma/sqrt(n))
z
```




    0.8838834764831844



Let's try to plot this z value on a standard normal distribution to see what it means. 


```python
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn')
plt.fill_between(x=np.arange(-4,0.88,0.01),
                 y1= stats.norm.pdf(np.arange(-4,0.88,0.01)) ,
                 facecolor='red',
                 alpha=0.35,
                 label= 'Area below z-statistic'
                 )

plt.fill_between(x=np.arange(0.88,4,0.01), 
                 y1= stats.norm.pdf(np.arange(0.88,4,0.01)) ,
                 facecolor='blue',
                 alpha=0.35, 
                 label= 'Area above z-statistic')
plt.legend()
plt.title ('z-statistic = 0.88');
```


![png](index_files/index_13_0.png)


## Step 4:  Calculate the p-Value

Remember z values in a standard normal distribution represent standard deviations. the standard deviation. A we did before, we shall look up related probability values in a z table, or use scipy.stats to calculate it directly. So cumulative probability upto z-value can be calculated as:


```python
stats.norm.cdf(z)
```




    0.8116204410942089



The percent of area under the normal curve from negative infinity to .88 z score is 81.2% (from z-table and calculations), meaning the average intelligence of this set of students is greater than 81.2% of the population. But we wanted it to be greater than 95% to prove our hypothesis to be significantly correct. 


And we get our p value probability by subtracting z value from 1 , as sum of probabilities in a normal distribution is always 1


```python
pval = 1 - stats.norm.cdf(z)
pval
```




    0.18837955890579106



## Step 5: Interpret p-value

So our p value (0.18) is much larger than our set alpha of 0.05. So what does that mean ? Have we failed ? and iq increase has nothing to do with mentoring ? 

Well we cant say that for sure. What we can say is that there is a weak evidence to reject the null hypothesis with given sample. There are ways to scale such experiments up and collect more data, apply sampling techniques to be sure about the real impact. 

When the sample data helps in rejecting null hypothesis, we still cant be too sure of the outcome, however we can say that given the evidence our results show a SIGNIFICANT increase in the IQ as a result of mentoring - instead of saying - "Mentoring improves IQ".

## Summary 

In this lesson we saw how to run a 1-sample z-test to compare sample and population where the population mean and standard deviation are known. This is the most basic test in statistics as in real world, true population means and sd are rarely identifiable and you have to work with sample statistics. Thats where most advanced tests come in to play. We shall look at those in next statistics section. 
