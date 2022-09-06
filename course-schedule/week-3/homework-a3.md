---
description: Week 3, HA3, 450-600 lines, 3 hours to complete
---

# Homework A3

### HA3 Instructions

Read this tutorial and apply the codes in R.

### Measures of variability

The statistics that we’ve discussed so far all relate to central tendency. That is, they all talk about which values are “in the middle” or “popular” in the data. However, central tendency is not the only type of summary statistic that we want to calculate. The second thing that we really want is a measure of the variability of the data. That is, how “spread out” are the data? How “far” away from the mean or median do the observed values tend to be? We’ll continue to use the `afl.margins` data to discuss several different measures of spread, each with different strengths and weaknesses.

### Range

The **range** of a variable is very simple: it’s the largest value minus the smallest value. For the AFL winning margins data, the maximum value is 116, and the minimum value is 0. We can calculate these values in R using the `max()` and `min()` functions:

```
max(afl.margins)
min(afl.margins)
```

```
[1] 116
[1] 0
```

where I’ve omitted the output because it’s not interesting. The other possibility is to use the `range()` function; which outputs both the minimum value and the maximum value in a vector:

```
range( afl.margins )
```

```
[1]   0 116
```

Although the range is the simplest way to quantify the notion of “variability”, it’s one of the worst. Recall from our discussion of the mean that we want our summary measure to be robust. If the data set has one or two extremely bad values in it, we’d like our statistics not to be unduly influenced by these cases. If we look once again at our toy example of a data set containing very extreme outliers…

$$
−100,2,3,4,5,6,7,8,9,10
$$

… it is clear that the range is not robust, since this has a range of 110, but if the outlier were removed we would have a range of only 8.

### Interquartile range

The **interquartile range (IQR)** is like the range, but instead of calculating the difference between the biggest and smallest value, it calculates the difference between the 25th quantile and the 75th quantile. Probably you already know what a **quantile** is (they’re more commonly called **percentiles**), but if not: the 10th percentile of a data set is the smallest number such that 10% of the data is less than that number. The median of a data set is its 50th quantile (i.e., percentile!). R actually provides you with a way of calculating quantiles, using the `quantile()` function. Let’s use it to calculate the median AFL winning margin:

```
quantile(x = afl.margins, probs = .5)
```

```
50% 
30.5
```

And not surprisingly, this agrees with the answer that we saw earlier with the `median()` function. Now, we can actually input lots of quantiles at once, by specifying a vector for the `probs` argument. So let's do that, and get the 25th and 75th percentile:

```
quantile(x = afl.margins, probs = c(.25,.75))
```

```
25%   75% 
12.75 50.50
```

And, by noting that:&#x20;

$$
50.5 - 12.75 = 37.75
$$

we can see that the interquartile range for the 2010 AFL winning margins data is 37.75. Of course, that seems like too much work to do all that typing, so R has a built-in function called `IQR()` that we can use:

```
IQR(x = afl.margins)
```

```
[1] 37.75
```

While it’s obvious how to interpret the range, it’s a little less obvious how to interpret the IQR. The simplest way to think about it is like this: the interquartile range is the range spanned by the “middle half” of the data. That is, one-quarter of the data falls below the 25th percentile, and one-quarter of the data is above the 75th percentile, leaving the “middle half” of the data lying in between the two. And the IQR is the range covered by that middle half.

### Mean absolute deviation

A different approach is to select a meaningful reference point (usually the mean or the median) and then report the “typical” deviations from that reference point. This leads to two different measures, the “mean absolute deviation (from the mean)” and the “median absolute deviation (from the median)”.&#x20;

Since the previous paragraph might sound a little abstract, let’s go through the **mean absolute deviation** from the mean a little more slowly. One useful thing about this measure is that the name actually tells you exactly how to calculate it. Let’s think about our AFL winning margins data, and once again we’ll start by pretending that there are only 5 games in total, with winning margins of 56, 31, 56, 8, and 32. Since our calculations rely on an examination of the deviation from some reference point, the first thing we need to calculate is the mean. For these five observations, our mean:

$$
\bar{X} = 36.6
$$

&#x20;The next step is to convert each of our observations into a deviation score. We do this by calculating the difference between each observation and the mean. The next step in the process is to convert these deviations to absolute deviations. We use the absolute value function here because we don’t really care whether the value is higher than the mean or lower than the mean, we’re just interested in how close it is to the mean. Now that we have calculated the absolute deviation score for every observation in the data set, all that we have to do to calculate the mean of these scores:

$$
\frac{19.4 + 5.6 + 19.4 + 28.6 + 4.6}{5} = 15.52
$$

Here’s the formula that describes what we just calculated:

$$
\mbox{}(X) = \frac{1}{N} \sum_{i = 1}^N |X_i - \bar{X}|
$$

The last thing we need to talk about is how to calculate Average Absolute Deviation (AAD) in R. One possibility would be to do everything using low-level commands:

```
X <- c(56, 31, 56, 8, 32)   # enter the data
X.bar <- mean(X)            # step 1. the mean of the data
AD <- abs(X - X.bar)        # step 2. the absolute deviations from the mean
AAD <- mean(AD)             # step 3. the mean absolute deviations
print(AAD)                  # print the results
```

```
[1] 15.52
```

The `lsr` package has a very simple function called `aad()` that does the calculations for you. If we apply the `aad()` function to our data, we get this:

```
library(lsr)
aad(X)
```

```
[1] 15.52
```

### Variance

Although the mean absolute deviation measure has its uses, it’s not the best measure of variability to use. From a purely mathematical perspective, there are some solid reasons to prefer squared deviations rather than absolute deviations. If we do that, we obtain a measure called the **variance**, which has a lot of really nice statistical properties. Now imagine we want to define a new variable that is the sum of the two others. The variance of the new variable is equal to the sum of the variances of the two others. This useful property is not true of the other measures of variability.  The formula that we use to calculate the variance of a set of observations is as follows:

$$
\mbox{Var}(X) = \frac{1}{N} \sum_{i=1}^N \left( X_i - \bar{X} \right)^2
$$

As you can see, it’s basically the same formula that we used to calculate the mean absolute deviation, except that instead of using “absolute deviations” we use “squared deviations”. It is for this reason that the variance is sometimes referred to as the **mean square deviation**.

We can calculate the variance of a vector of data by using the following command,

```
mean((X - mean(X))^2)
```

Fortunately, R has a built-in function called `var()` which does calculate variances. Let's switch to the full set of 176 games that we’ve got stored in our `afl.margins` vector. First, let’s calculate the variance by using the formula described above and the `var()` function:

```
mean((afl.margins - mean(afl.margins))^2)
var(afl.margins)
```

```
[1] 675.9718
[1] 679.8345
```

What R is doing is evaluating a slightly different formula to the one I showed you above. Instead of averaging the squared deviations, which requires you to divide by the number of data points, R has chosen to divide by the number of data points minus one:

$$
\frac{1}{N-1} \sum_{i=1}^N \left( X_i - \bar{X} \right)^2
$$

The real question is _why?_ There’s a subtle distinction between “describing a sample” and “making guesses about the population from which the sample came”. Regardless of whether you’re describing a sample or drawing inferences about the population, the mean is calculated exactly the same way. Most of the time, however, you’re not terribly interested in the sample _in and of itself_. Rather, the sample exists to tell you something about the world. If so, you’re actually starting to move away from calculating a “sample statistic”, and towards the idea of estimating a “population parameter”. For now, let’s just take it on faith that R knows what it’s doing, and we’ll revisit the question later on when we talk about estimation.

The reason why we haven’t been given a human-friendly interpretation of the variance is that there really isn’t one. This is the most serious problem with the variance. Although it has some elegant mathematical properties that suggest that it really is a fundamental quantity for expressing variation, it’s completely useless if you want to communicate with an actual human. Variances are completely uninterpretable in terms of the original variable! All the numbers have been squared, and they don’t mean anything anymore. This is a huge issue.&#x20;

### Standard deviation

Since you’re a human and not a robot, you’d like to have a measure that is expressed in the same units as the data itself. What should you do? The solution to the problem is obvious: take the square root of the variance, known as the **standard deviation**, also called the **root mean squared deviation**. This solves the problem fairly neatly: while nobody has a clue what “a variance of 324.68 points-squared” really means, it’s much easier to understand “a standard deviation of 18.01 points” since it’s expressed in the original units. Because the standard deviation is equal to the square root of the variance, you probably won’t be surprised to see that the formula is:

$$
\hat\sigma = \sqrt{ \frac{1}{N-1} \sum_{i=1}^N \left( X_i - \bar{X} \right)^2 }
$$

The R function that we use to calculate it is `sd()`:

```
sd(afl.margins) 
```

```
[1] 26.07364
```

Interpreting standard deviations is slightly more complex. Because the standard deviation is derived from the variance, and the variance is a quantity that has little to no meaning that makes sense to us humans, the standard deviation doesn’t have a simple interpretation. As a consequence, most of us just rely on a simple rule of thumb: in general, you should expect 68% of the data to fall within 1 standard deviation of the mean, 95% of the data to fall within 2 standard deviations of the mean, and 99.7% of the data to fall within 3 standard deviations of the mean. This rule tends to work pretty well most of the time, but it’s not exact: it’s actually calculated based on an _assumption_ that the histogram is symmetric and bell-shaped. As you can tell from looking at the AFL winning margins histogram, this isn’t exactly true of our data! Even so, the rule is approximately correct. As it turns out, 65.3% of the AFL margins data fall within one standard deviation of the mean.&#x20;

### Median absolute deviation

The last measure of variability that we want to talk about is the **median absolute deviation**. The basic idea behind MAD is very simple and is pretty much identical to the idea behind the mean absolute deviation. The difference is that you use the median everywhere. If we were to frame this idea as a pair of R commands, they would look like this:

```
# mean absolute deviation from the mean:
mean(abs(afl.margins - mean(afl.margins)))
# *median* absolute deviation from the *median*:
median(abs(afl.margins - median(afl.margins)))
```

```
[1] 21.10124
[1] 19.5
```

This has a straightforward interpretation: every observation in the data set lies some distance away from the typical value (the median). So this is an attempt to describe a typical deviation from a typical value in the data set. It wouldn’t be unreasonable to interpret the MAD value of 19.5 for our AFL data by saying something like this:

> The median winning margin in 2010 was 30.5, indicating that a typical game involved a winning margin of about 30 points. However, there was a fair amount of variation from game to game: the MAD value was 19.5, indicating that a typical winning margin would differ from this median value by about 19-20 points.

As you’d expect, R has a built-in function for calculating MAD, and you will be shocked no doubt to hear that it’s called `mad()`. The command that you need to use specifies two arguments: the data set itself and a constant. For our purposes, the constant is 1, so our command becomes

```
mad(x = afl.margins, constant = 1)
```

```
[1] 19.5
```

Okay, so what exactly is this `constant = 1` argument? In the same way that the mean is very sensitive to extreme values, the standard deviation is vulnerable to the exact same issue. So, in much the same way that people sometimes use the median as a “robust” way of calculating “something that is like the mean”, it’s not uncommon to use MAD as a method for calculating “something that is like the standard deviation”. Unfortunately, the _raw_ MAD value doesn’t do this. Our raw MAD value is 19.5, and our standard deviation was 26.07. However, what some clever person has shown is that, under certain assumptions, you can multiply the raw MAD value by 1.4826 and obtain a number that is directly comparable to the standard deviation. As a consequence, the default value of `constant` is 1.4826, and so when you use the `mad()` command without manually setting a value, here’s what you get:

```
mad(afl.margins)
```

```
[1] 28.9107
```

If you want to use this “corrected” MAD value as a robust version of the standard deviation, you really are relying on the assumption that the data are symmetric and basically shaped like a bell curve. That’s really _not_ true for our `afl.margins` data, so in this case, we wouldn’t try to use the MAD value this way.

### Which measure to use?

We have discussed quite a few measures of spread (range, IQR, MAD, variance, and standard deviation) and hinted at their strengths and weaknesses. Here’s a quick summary:

* _Range_. Gives you the full spread of the data. It’s very vulnerable to outliers, and as a consequence, it isn’t often used unless you have good reasons to care about the extremes in the data.
* _Interquartile range_. Tells you where the “middle half” of the data sits. It’s pretty robust and complements the median nicely. This is used a lot.
* _Mean absolute deviation_. Tells you how far “on average” the observations are from the mean. It’s very interpretable but has a few minor issues (not discussed here) that make it less attractive to statisticians than the standard deviation. Used sometimes, but not often.
* _Variance_. Tells you the average squared deviation from the mean. It’s mathematically elegant and is probably the “right” way to describe variation around the mean, but it’s completely uninterpretable because it doesn’t use the same units as the data. Almost never used except as a mathematical tool, but it’s buried “under the hood” of a very large number of statistical tools.
* _Standard deviation_. This is the square root of the variance. It’s fairly elegant mathematically, and it’s expressed in the same units as the data so it can be interpreted pretty well. In situations where the mean is the measure of central tendency, this is the default. This is by far the most popular measure of variation.
* _Median absolute deviation_. The typical (i.e., median) deviation from the median value. In the raw form, it’s simple and interpretable. In the corrected form it’s a robust way to estimate the standard deviation, for some kinds of data sets. Not used very often, but it does get reported sometimes.

In short, the IQR and the standard deviation are easily the two most common measures used to report the variability of the data.

### Skew and kurtosis

There are two more descriptive statistics that you will sometimes see reported in literature known as skew and kurtosis. In practice, neither one is used anywhere near as frequently as the measures of central tendency and variability that we have been talking about. **Skewness** is basically a measure of asymmetry, and the easiest way to explain it is by drawing some pictures. As **Figure HA3.1** illustrates, if the data tend to have a lot of extremely small values (i.e., the lower tail is “longer” than the upper tail) and not so many extremely large values (left panel), then we say that the data are negatively skewed. On the other hand, if there are more extremely large values than extremely small ones (right panel) we say that the data are positively skewed.

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/skewness-1.png" alt=""><figcaption><p><strong>Figure HA3.1</strong> Illustration of skewness</p></figcaption></figure>

$$
\mbox{skewness}(X) = \frac{1}{N \hat{\sigma}^3} \sum_{i=1}^N (X_i - \bar{X})^3
$$

The final measure that is sometimes referred to, though very rarely in practice, is the **kurtosis** of a data set. Put simply, kurtosis is a measure of the “pointiness” of a data set, as illustrated in **Figure HA3.2**. By convention, we say that the “normal curve” (black lines) has zero kurtosis, so the pointiness of a data set is assessed relative to this curve. In this Figure, the data on the left is not pointy enough, so the kurtosis is negative and we call the data platykurtic. The data on the right are too pointy, so the kurtosis is positive and we say that the data is leptokurtic. But the data in the middle are just pointy enough, so we say that it is mesokurtic and has kurtosis zero.

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/kurtosis-1.png" alt=""><figcaption><p><strong>Figure HA3.2</strong> An illustration of kurtosis</p></figcaption></figure>

$$
\mbox{kurtosis}(X) = \frac{1}{N \hat\sigma^4} \sum_{i=1}^N \left( X_i - \bar{X} \right)^4  - 3
$$

### Getting an overall summary of a variable

Wouldn’t it be nice if R had some helpful functions that would do all these tedious calculations at once? Something like `summary()` or `describe()`, perhaps? The function `summary()` is in the `base` package, so it comes with every installation of R.

The basic idea behind the `summary()` function is that it prints out some useful information about whatever object (i.e., variable, as far as we’re concerned) you specify as the `object` argument. Let’s start by giving it a _numeric_ object:

```
summary(object = afl.margins)  
```

```
Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
0.00   12.75   30.50   35.30   50.50  116.00
```

It gives us the minimum and maximum values (i.e., the range), the first and third quartiles (25th and 75th percentiles; i.e., the IQR), the mean, and the median. In other words, it gives us a pretty good collection of descriptive statistics related to the central tendency and the spread of the data. In the context of logical data, the function `summary()` gives us a count of the number of `TRUE` values, the number of `FALSE` values, and the number of missing values (i.e., the `NA`s).&#x20;

Okay, what about data frames? When you pass a data frame to the `summary()` function, it produces a slightly condensed summary of each variable inside the data frame. To give you a sense of how this can be useful, let’s try this for a new data set, one that you’ve never seen before. The data is stored in the `clinicaltrial.Rdata` . Let’s load it, and see what we’ve got:

{% file src="../../.gitbook/assets/clinicaltrial.Rdata" %}

```
load( "./data/clinicaltrial.Rdata" )
who(TRUE)
```

```
##    -- Name --    -- Class --   -- Size --
##    clin.trial    data.frame    18 x 3    
##     $drug        factor        18        
##     $therapy     factor        18        
##     $mood.gain   numeric       18
```

There’s a single data frame called `clin.trial` which contains three variables, `drug`, `therapy` and `mood.gain`. Presumably, this data is from a clinical trial of some kind, in which people were administered different drugs; and the researchers looked to see what the drugs did to their mood. Let’s see if the `summary()` function sheds a little more light on this situation:

```
summary(clin.trial)
```

```
drug         therapy        mood.gain     
placebo :6   no.therapy:9   Min.   :0.1000  
anxifree:6   CBT       :9   1st Qu.:0.4250  
joyzepam:6                  Median :0.8500  
                            Mean   :0.8833  
                            3rd Qu.:1.3000  
                            Max.   :1.8000
```

Evidently, there were three drugs: a placebo, something called anxifree, and something called joyzepam and there were 6 people administered each drug. There were 9 people treated using cognitive behavioral therapy (CBT) and 9 people who received no psychological treatment. And we can see from looking at the summary of the `mood.gain` variable that most people did show a mood gain, though without knowing what the scale is here it’s hard to say much more than that. Still, that’s not too bad.

### Standard scores

The standard score, also known as the **z score**, is defined as the number of standard deviations above the mean where a score lies. To phrase it in “pseudo-maths” the standard score is calculated like this:

$$
\mbox{standard score} = \frac{\mbox{raw score} - \mbox{mean}}{\mbox{standard deviation}}
$$

In actual maths, the equation for the standard score is

$$
z_i = \frac{X_i - \bar{X}}{\hat\sigma}
$$

Let's say we have a sample including data on grumpiness scores. If Dan's score is 35, the mean is 17, and the standard deviation is 5, then Dan's standardized grumpiness score would be 3.6. To interpret this value, recall the rough heuristic that 99.7% of values are expected to lie within 3 standard deviations of the mean. So the fact that Dan's grumpiness corresponds to a z score of 3.6 indicates that Dan is very grumpy indeed. Later on, we will introduce a function called `pnorm()` that allows us to be a bit more precise than this. Specifically, it allows us to calculate a theoretical percentile rank for grumpiness, as follows:

```
pnorm(3.6)
```

```
[1] 0.9998409
```

At this stage, this command doesn’t make too much sense, but the output is fairly straightforward: it suggests that Dan is grumpier than 99.98% of people. In addition to allowing you to interpret a raw score in relation to a larger population (and thereby allowing you to make sense of variables that lie on arbitrary scales), standard scores serve a second useful function. Standard scores can be compared to one another in situations where the raw scores can’t. Suppose, for instance, Dan's friend had another questionnaire that measured extraversion using a questionnaire. The overall mean for this measure turns out to be 13, with a standard deviation of 4, and Dan's score is 2. As you can imagine, it doesn’t make a lot of sense to try to compare Dan's raw score of 2 on the extraversion questionnaire to his raw score of 35 on the grumpiness questionnaire. The raw scores for the two variables are “about” fundamentally different things, so this would be like comparing apples to oranges.

If we calculate the standard scores, the two numbers can be compared to each other. Dan is much less extraverted than most people and much grumpier than most people, but the extent of his unusualness is much more extreme for grumpiness (since 3.6 is a bigger number than 2.75). Because each standardized score is a statement about where an observation falls relative to its own population, it is possible to compare standardized scores across completely different variables.

### Correlations

Up to this point, we have focused entirely on how to construct descriptive statistics for a single variable. Now, let's talk about how to describe relationships between variables. To do that, we want to talk mostly about **correlation**.

Let’s turn to a topic close to every parent’s heart: sleep. The following data set is fictitious but based on real events. Suppose we are curious to find out how much Dan's infant son’s sleeping habits affect his mood. Let’s say that Dan can rate his grumpiness very precisely, on a scale from 0 (not at all grumpy) to 100 (grumpy as a very, very grumpy old man). And, let's also assume that Dan has been measuring his grumpiness, sleeping patterns, and his son’s sleeping patterns for 100 days, and, being a nerd, Dan has saved the data as a file called `parenthood.Rdata`:

{% file src="../../.gitbook/assets/parenthood.Rdata" %}

```
load( "./data/parenthood.Rdata" )
who(TRUE)
```

```
##    -- Name --     -- Class --   -- Size --
##    parenthood     data.frame    100 x 4   
##     $dan.sleep    numeric       100       
##     $baby.sleep   numeric       100       
##     $dan.grump    numeric       100       
##     $day          integer       100
```

The file contains a single data frame `parenthood`, which contains four variables `dan.sleep`, `baby.sleep`, `dan.grump` , and `day`. If we peek at the data using `head()` out the data, here’s what we get:

```
head(parenthood,10)
```

```
   dan.sleep baby.sleep dan.grump day
1       7.59      10.18        56   1
2       7.91      11.66        60   2
3       5.14       7.92        82   3
4       7.71       9.61        55   4
5       6.68       9.75        67   5
6       5.99       5.04        72   6
7       8.19      10.45        53   7
8       7.19       8.27        60   8
9       7.40       6.06        60   9
10      6.58       7.09        71  10
```

```
summary(parenthood)
```

```
   dan.sleep       baby.sleep       dan.grump          day        
 Min.   :4.840   Min.   : 3.250   Min.   :41.00   Min.   :  1.00  
 1st Qu.:6.293   1st Qu.: 6.425   1st Qu.:57.00   1st Qu.: 25.75  
 Median :7.030   Median : 7.950   Median :62.00   Median : 50.50  
 Mean   :6.965   Mean   : 8.049   Mean   :63.71   Mean   : 50.50  
 3rd Qu.:7.740   3rd Qu.: 9.635   3rd Qu.:71.00   3rd Qu.: 75.25  
 Max.   :9.000   Max.   :12.070   Max.   :91.00   Max.   :100.00  
```

One thing to note: just because R can calculate dozens of different statistics doesn’t mean you should report all of them. If we were writing this up for a report, we would pick out those statistics that are of most interest and then put them into a nice, simple table, giving everything “human readable” names. We can draw scatterplots to give us a general sense of how closely related two variables are. Ideally, we might want to say a bit more about it than that. For instance, let’s compare the relationship between `dan.sleep` and `dan.grump` (**Figure HA3.3**) and see the relationship between `baby.sleep` and `dan.grump` (**Figure HA3.4**). **** &#x20;

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/scatterparent1a-1.png" alt=""><figcaption><p><strong>Figure HA3.3</strong> Scatterplot showing the relationship between <code>dan.sleep</code> and <code>dan.grump</code></p></figcaption></figure>

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/scatterparent1b-1.png" alt=""><figcaption><p><strong>Figure HA3.4</strong> Scatterplot showing the relationship between <code>baby.sleep</code> and <code>dan.grump</code></p></figcaption></figure>



It’s clear that the relationship is qualitatively the same in both cases: more sleep equals less grump! However, it’s also pretty obvious that the relationship between `dan.sleep` and `dan.grump` is stronger than the relationship between `baby.sleep` and `dan.grump`. The plot in **Figure HA3.3** is “neater”. What it feels like is that if you want to predict Dan's mood, it would help you a little bit to know how many hours his son slept, but it’d be more helpful to know how many hours Dan slept.

If we compare the scatterplot of “`baby.sleep` v `dan.grump`” to the scatterplot of “\``baby.sleep` v `dan.sleep`”, the overall strength of the relationship is the same, but the direction is different. If the son sleeps more, Dan gets more sleep (positive relationship, but if the son sleeps more then Dan gets less grumpy (negative relationship).

### The correlation coefficient

The **Correlation coefficient** between two variables is traditionally denoted by r and is a measure that varies from -1 to +1. When r equals -1 it means that we have a perfect negative relationship, and when r is +1 it means we have a perfect positive relationship. When is there no relationship at all? If you look at **Figure HA3.5**, you can see several plots showing what different correlations look like.

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/corr-1.png" alt=""><figcaption><p><strong>Figure HA3.5</strong> The effect of varying the strength and direction of a correlation</p></figcaption></figure>

The formula for Pearson’s correlation coefficient can be written in several different ways. The simplest way to write down the formula is to break it into two steps. Firstly, let’s introduce the idea of a _**covariance**_. The covariance between two variables is a generalization of the notion of the variance; it is a mathematically simple way of describing the relationship between two variables:

$$
\mbox{Cov}(X,Y) = \frac{1}{N-1} \sum_{i=1}^N \left( X_i - \bar{X} \right) \left( Y_i - \bar{Y} \right)
$$

The Pearson correlation coefficient **r** fixes this interpretation problem by standardizing the covariance, similar to the way that the **z**-score standardizes a raw score: by dividing by the standard deviation. However, because we have two variables that contribute to the covariance, the standardization only works if we divide by both standard deviations. In other words:

$$
r_{XY}  = \frac{\mbox{Cov}(X,Y)}{ \hat{\sigma}_X \ \hat{\sigma}_Y}
$$

By doing this standardization, not only do we keep all of the nice properties of the covariance discussed earlier, but the actual values of **r** are on a meaningful scale: **r = 1** implies a perfect positive relationship, and **r = -1** implies a perfect negative relationship. Let’s look at how to calculate correlations in R.

Calculating correlations in R can be done using the `cor()` command:

```
cor( x = parenthood$dan.sleep, y = parenthood$dan.grump )
```

```
[1] -0.903384
```

However, the function `cor()` is more powerful than this simple example suggests. For example, you can also calculate a complete “correlation matrix”, between all pairs of variables in the data frame:

```
# correlate all pairs of variables in "parenthood":
cor(x = parenthood)  
```

```
           dan.sleep   baby.sleep  dan.grump   day
dan.sleep   1.00000000  0.62794934 -0.90338404 -0.09840768
baby.sleep  0.62794934  1.00000000 -0.56596373 -0.01043394
dan.grump  -0.90338404 -0.56596373  1.00000000  0.07647926
day        -0.09840768 -0.01043394  0.07647926  1.00000000
```

### Interpreting a correlation

Interpreting a correlation coefficient depends a lot on the context. Nonetheless, a rough guide is:

| Correlation  | Strength    | Direction |
| ------------ | ----------- | --------- |
| -1.0 to -0.9 | Very strong | Negative  |
| -0.9 to -0.7 | Strong      | Negative  |
| -0.7 to -0.4 | Moderate    | Negative  |
| -0.4 to -0.2 | Weak        | Negative  |
| -0.2 to 0    | Negligible  | Negative  |
| 0 to 0.2     | Negligible  | Positive  |
| 0.2 to 0.4   | Weak        | Positive  |
| 0.4 to 0.7   | Moderate    | Positive  |
| 0.7 to 0.9   | Strong      | Positive  |
| 0.9 to 1.0   | Very strong | Positive  |

However, something that can never be stressed enough is that you should always look at the scatterplot before attaching any interpretation to the data. A correlation might not mean what you think it means. The classic illustration of this is “Anscombe’s Quartet”, a collection of four data sets. Each data set has two variables. In all four datasets, the mean value of the first variable is 9 and the mean for the second variable is 7.5 and the standard deviations of the two variables are almost identical. And in each case, the correlation between the variables is 0.816. You can verify this yourself since the dataset comes distributed with R:

```
cor(anscombe$x1, anscombe$y1)
cor(anscombe$x2, anscombe$y2)
```

```
[1] 0.8164205
[1] 0.8162365
```

You’d think that these four data sets would look pretty similar to one another. They do not:

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/anscombe-1.png" alt=""><figcaption><p><strong>Figure HA3.6</strong> Anscombe’s quartet</p></figcaption></figure>

The lesson here, which so very many people seem to forget in real life is “_always graph your raw data_”.

### Spearman’s rank correlations

The Pearson correlation coefficient is useful for a lot of things, but it does have shortcomings. One issue, in particular, stands out: what it actually measures is the strength of the linear relationship between two variables. In other words, what it gives you is a measure of the extent to which the data all tend to fall on a single, perfectly straight line. Often, this is a pretty good approximation to what we mean when we say “relationship”, and so the Pearson correlation is a good thing to calculate. Sometimes, it isn’t.

One very common situation where the Pearson correlation isn’t quite the right thing to use arises when an increase in one variable really is reflected in an increase in another variable, but the nature of the relationship isn’t necessarily linear. An example of this might be the relationship between effort and reward when studying for an exam. If you put zero effort into learning a subject, then you should expect a grade of 0. However, a little bit of effort will cause a massive improvement: just turning up to lectures means that you learn a fair bit, and if you just turn up to classes and scribble a few things down, your grade might rise to 35%, all without a lot of effort. However, you just don’t get the same effect at the other end of the scale. As everyone knows, it takes _a_ lot more effort to get a grade of 90% than it takes to get a grade of 55%. What this means is that, if I’ve got data looking at study effort and grades, there’s a pretty good chance that Pearson correlations will be misleading.

To illustrate, consider the data plotted in Figure [5.12](https://learningstatisticswithr.com/book/descriptives.html#fig:rankcorrpic), showing the relationship between hours worked and grades received for 10 students. The curious thing about this – highly fictitious – data set is that increasing effort always increases your grade. It might be by a lot or it might be by a little, but the increasing effort will never decrease your grade.&#x20;

###

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/rankcorrpic-1.png" alt=""><figcaption><p><strong>Figure HA3.7</strong> The relationship between hours worked and grade received</p></figcaption></figure>

The dashed line through the middle shows the linear relationship between the two variables. This produces a strong Pearson correlation of r = **0.9**. However, the interesting thing to note here is that there’s actually a perfect monotonic relationship between the two variables: in this toy example at least, increasing the hours worked always increases the grade received, as illustrated by the solid line. This is reflected in a Spearman correlation of **rho = 1**. With such a small data set, however, it’s an open question as to which version better describes the actual relationship involved.

The data are stored in `effort.Rdata`:

```
load( "effort.Rdata" )
who(TRUE)
   -- Name --   -- Class --   -- Size --
    effort      data.frame    10 x 2    
    $hours      numeric       10        
    $grade      numeric       10        
```

The raw data look like this:

```
effort
   hours grade
1      2    13
2     76    91
3     40    79
4      6    14
5     16    21
6     28    74
7     27    47
8     59    85
9     46    84
10    68    88
```

If we run a standard Pearson correlation, it shows a strong relationship between hours worked and grade received,

```
cor(effort$hours, effort$grade)
[1] 0.909402
```

but this doesn’t actually capture the observation that increasing hours worked always increases the grade. There’s a sense here in which we want to be able to say that the correlation is perfect but for a somewhat different notion of what a “relationship” is. What we’re looking for is something that captures the fact that there is a perfect **ordinal relationship** here. That is, if student 1 works more hours than student 2, then we can guarantee that student 1 will get a better grade. That’s not what a correlation of **r = 0.9** says at all.

How should we address this? Actually, it’s really easy: if we’re looking for ordinal relationships, all we have to do is treat the data as if it were an ordinal scale! So, instead of measuring effort in terms of “hours worked”, let's rank all 10 of our students in order of hours worked. That is, student 1 did the least work out of anyone (2 hours) so they get the lowest rank (rank = 1). Student 4 was the next laziest, putting in only 6 hours of work in over the whole semester, so they get the next lowest rank (rank = 2). Notice that using “rank =1” means “low rank”, because that’s the default way that R does it. But in real life, it’s really easy to forget which way you set things up, so you have to put a bit of effort into remembering! Okay, so let’s have a look at our students when we rank them from worst to best in terms of effort and reward:

|         | rank (hours worked) | rank (grade received) |
| ------- | ------------------- | --------------------- |
| student | 1                   | 1                     |
| student | 2                   | 10                    |
| student | 3                   | 6                     |
| student | 4                   | 2                     |
| student | 5                   | 3                     |
| student | 6                   | 5                     |
| student | 7                   | 4                     |
| student | 8                   | 8                     |
| student | 9                   | 7                     |
| student | 10                  | 9                     |

Hm. These are identical. The student who put in the most effort got the best grade, the student with the least effort got the worst grade, etc. We can get R to construct these rankings using the `rank()` function, like this:

```
hours.rank <- rank( effort$hours )   # rank students by hours worked
grade.rank <- rank( effort$grade )   # rank students by grade received
```

As the table above shows, these two rankings are identical, so if we now correlate them we get a perfect relationship:

```
cor( hours.rank, grade.rank )
[1] 1
```

What we’ve just re-invented is **Spearman’s rank order correlation**. We can calculate Spearman’s correlation coefficient using R in two different ways. Firstly we could do it the way I just showed, using the `rank()` function to construct the rankings, and then calculate the Pearson correlation on these ranks. However, that’s way too much effort to do every time. It’s much easier to just specify the `method` argument of the `cor()` function.

```
cor(effort$hours, effort$grade, method = "spearman")
[1] 1
```

The default value of the `method` argument is `"pearson"`, which is why we didn’t have to specify it earlier on when we were doing Pearson correlations.

### Handling missing values

Real data sets very frequently turn out to have missing values: perhaps someone forgot to fill in a particular survey question, for instance. You need to understand the basics of handling missing data in R.

Let’s start with the simplest case, in which you’re trying to calculate descriptive statistics for a single variable that has missing data. In R, this means that there will be `NA` values in your data vector:

```
partial <- c(10, 20, NA, 30)
```

Let’s assume that you want to calculate the mean of this variable. By default, R assumes that you want to calculate the mean using all four elements of this vector, which is probably the safest thing for a dumb automaton to do, but it’s rarely what you actually want. Why not? Well, remember that the basic interpretation of `NA` is “I don’t know what this number is”. As a consequence, if you don’t explicitly tell R to ignore the missing values, then the output will itself be a missing value. Here’s what happens:

```
mean(x = partial)
[1] NA
```

Most descriptive statistics functions discussed have an optional argument called `na.rm`, which is shorthand for “remove NA values”. By default, `na.rm = FALSE` , so R does nothing about the missing data problem. Let’s try setting `na.rm = TRUE` and see what happens:

```
mean(x = partial, na.rm = TRUE)
[1] 20
```

The function `cor()` is a special case. It doesn’t have an `na.rm` argument, because the story becomes a lot more complicated when more than one variable is involved. What it does have is an argument called `use` which does roughly the same thing, but you need to think a little more carefully about what you want this time. To make R behave more sensibly in this situation, you need to specify the `use` argument to the `cor()` function. There are several different values that you can specify for this, but the two that we care most about in practice tend to be `"complete.obs"` and `"pairwise.complete.obs"`. If we specify `use = "complete.obs"`, R will completely ignore all cases (i.e., all rows in our data frame) that have any missing values at all. The other possibility that we care about, and the one that tends to get used more often in practice, is to set `use = "pairwise.complete.obs"`. When we do that, R only looks at the variables that it’s trying to correlate when determining what to drop.&#x20;

So which method should you use? It depends a lot on _why_ you think your values are missing, and probably depends a little on how paranoid you are. For instance, if you think that the missing values were “chosen” completely randomly then you’ll probably want to use the pairwise method. If you think that missing data are a cue to thinking that the whole observation might be rubbish (e.g., someone just selecting arbitrary responses in your questionnaire), but that there’s no pattern to which observations are “rubbish” then it’s probably safer to keep only those observations that are complete. If you think there’s something systematic going on, in that some observations are more likely to be missing than others, then you have a much trickier problem to solve, and one that is beyond the scope of this book.

You are a data analyst, not a statistical software package. Part of your job is to take these _statistics_ and turn them into a description. When you analyze data, it is not sufficient to list off a collection of numbers. Always remember that what you’re really trying to do is communicate with a human audience. The numbers are important, but they need to be put together into a meaningful story that your audience can interpret. That means you need to think about framing. You need to think about context. And you need to think about the individual events that your statistics are summarising.

\
