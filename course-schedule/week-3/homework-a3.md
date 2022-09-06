---
description: Week 3, HA3, 450 lines, 3 hours to complete
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

The standard score is defined as the number of standard deviations above the mean that my grumpiness score lies. To phrase it in “pseudo-maths” the standard score is calculated like this:$$standard score=raw score−meanstandard deviationstandard score=raw score−meanstandard deviation$$In actual maths, the equation for the $$zz$$-score is$$zi=Xi−¯X^σzi=Xi−X¯σ^$$So, going back to the grumpiness data, we can now transform Dan’s raw grumpiness into a standardised grumpiness score.[77](https://learningstatisticswithr.com/book/descriptives.html#fn77) If the mean is 17 and the standard deviation is 5 then my standardised grumpiness score would be[78](https://learningstatisticswithr.com/book/descriptives.html#fn78)$$z=35−175=3.6z=35−175=3.6$$To interpret this value, recall the rough heuristic that I provided in Section [5.2.5](https://learningstatisticswithr.com/book/descriptives.html#sd), in which I noted that 99.7% of values are expected to lie within 3 standard deviations of the mean. So the fact that my grumpiness corresponds to a $$zz$$ score of 3.6 indicates that I’m very grumpy indeed. Later on, in Section [9.5](https://learningstatisticswithr.com/book/probability.html#normal), I’ll introduce a function called `pnorm()` that allows us to be a bit more precise than this. Specifically, it allows us to calculate a theoretical percentile rank for my grumpiness, as follows:

```
pnorm( 3.6 )
```

```
## [1] 0.9998409
```

At this stage, this command doesn’t make too much sense, but don’t worry too much about it. It’s not important for now. But the output is fairly straightforward: it suggests that I’m grumpier than 99.98% of people. Sounds about right.

In addition to allowing you to interpret a raw score in relation to a larger population (and thereby allowing you to make sense of variables that lie on arbitrary scales), standard scores serve a second useful function. Standard scores can be compared to one another in situations where the raw scores can’t. Suppose, for instance, my friend also had another questionnaire that measured extraversion using a 24 items questionnaire. The overall mean for this measure turns out to be 13 with standard deviation 4; and I scored a 2. As you can imagine, it doesn’t make a lot of sense to try to compare my raw score of 2 on the extraversion questionnaire to my raw score of 35 on the grumpiness questionnaire. The raw scores for the two variables are “about” fundamentally different things, so this would be like comparing apples to oranges.

What about the standard scores? Well, this is a little different. If we calculate the standard scores, we get $$z=(35−17)/5=3.6z=(35−17)/5=3.6$$ for grumpiness and $$z=(2−13)/4=−2.75z=(2−13)/4=−2.75$$ for extraversion. These two numbers _can_ be compared to each other.[79](https://learningstatisticswithr.com/book/descriptives.html#fn79) I’m much less extraverted than most people ($$z=−2.75z=−2.75$$) and much grumpier than most people ($$z=3.6z=3.6$$): but the extent of my unusualness is much more extreme for grumpiness (since 3.6 is a bigger number than 2.75). Because each standardised score is a statement about where an observation falls _relative to its own population_, it _is_ possible to compare standardised scores across completely different variables.
