---
description: Lab 3, 150-250 lines, 1 hour to complete
---

# Descriptives in R P1

## Descriptives in R P.1

### Instructions

Read this tutorial and apply the codes in R.

### Descriptive statistics

Any time that you get a new data set to look at, one of the first tasks that you have to do is find ways of summarising the data in a compact, easily understood fashion. This is what **descriptive statistics** (as opposed to inferential statistics) is all about. Let’s load **File LA3.1** `aflsmall.Rdata`, and use the `who()` function in the `lsr` package to see what variables are stored in the file. From now on, let's separate the command block and remove the sign `>` so you can easily copy and paste the codes.

```
load( "./data/aflsmall.Rdata" )
library(lsr)
who()       
```

```
-- Name --             -- Class --     -- Size --
afl.finalists          factor          400       
afl.margins            numeric         176       
```

There are two variables here, `afl.finalists` and `afl.margins`. These are actually real data, relating to the Australian Football League. The `afl.margins` variable contains the winning margin (number of points) for all 176 home and away games played during the 2010 season. The `afl.finalists` variable contains the names of all 400 teams that played in all 200 finals matches played during the period 1987 to 2010. Let’s have a look at the `afl.margins` variable:

```
print(afl.margins)
```

```
  [1]  56  31  56   8  32  14  36  56  19   1   3 104  43  44  72   9  28
 [18]  25  27  55  20  16  16   7  23  40  48  64  22  55  95  15  49  52
 [35]  50  10  65  12  39  36   3  26  23  20  43 108  53  38   4   8   3
 [52]  13  66  67  50  61  36  38  29   9  81   3  26  12  36  37  70   1
 [69]  35  12  50  35   9  54  47   8  47   2  29  61  38  41  23  24   1
 [86]   9  11  10  29  47  71  38  49  65  18   0  16   9  19  36  60  24
[103]  25  44  55   3  57  83  84  35   4  35  26  22   2  14  19  30  19
[120]  68  11  75  48  32  36  39  50  11   0  63  82  26   3  82  73  19
[137]  33  48   8  10  53  20  71  75  76  54  44   5  22  94  29   8  98
[154]   9  89   1 101   7  21  52  42  21 116   3  44  29  27  16   6  44
[171]   3  28  38  29  10  10
```

Just “looking at the data” isn’t a terribly effective way of understanding data. In order to get some idea about what’s going on, we need to calculate some descriptive statistics and draw some nice pictures. Since descriptive statistics are the easier of the two topics, we will start with those, but, nevertheless, we will create a histogram of the `afl.margins` data, since it should help get a sense of what the data we are trying to describe actually look like. This histogram was generated using the `hist()` function. We’ll talk a lot more about how to draw histograms. For now, it’s enough to look at the histogram and note that it provides a fairly interpretable representation of the `afl.margins` data.

<figure><img src="https://learningstatisticswithr.com/book/lsr_files/figure-html/histogram1-1.png" alt=""><figcaption><p><strong>Figure LA3.1</strong> A histogram of the AFL 2010 winning margin data</p></figcaption></figure>

### Measures of central tendency

In most situations, the first thing that you’ll want to calculate is a measure of **central tendency**. That is, you’d like to know something about the “average” or “middle” of your data lies. The most commonly used measures are the mean, median, mode, and, occasionally, trimmed mean.

### The mean

The **mean** of a set of observations is just a normal, old-fashioned average: add all of the values up, and then divide by the total number of values. The first five AFL margins were 56, 31, 56, 8, and 32, so the mean of these observations is just:

$$
\frac{56 + 31 + 56 + 8 + 32}{5} = \frac{183}{5} = 36.60
$$

By tradition, we use $$\bar{X}$$ as the notation for the mean. So the calculation for the mean could be expressed using the formula:

$$
\bar{X} = \frac{X_1 + X_2 + ... + X_{N-1} + X_N}{N}
$$

This formula is entirely correct, but it’s terribly long, so we make use of the **summation symbol** $$∑$$ to shorten it:

$$
\bar{X} = \frac{1}{N} \sum_{i=1}^N X_i
$$

If you really wanted to, you could do this calculation directly in R. For the first 5 AFL scores, do this just by typing it in as if R were a calculator:

```
(56 + 31 + 56 + 8 + 32) / 5
[1] 36.6
```

In almost every real-world scenario, you’ve already got the actual numbers stored in a variable of some kind, just like we have with the `afl.margins` variable. Under those circumstances, what you want is a function that will just add up all the values stored in a numeric vector. That’s what the `sum()` function does. If we want to add up all 176 winning margins in the data set, we can do so. If we only want the sum of the first five observations, then we can use square brackets to pull out only the first five elements of the vector:

```
sum(afl.margins)
[1] 6213
sum(afl.margins[1:5])
[1] 183
```

To calculate the mean, we now tell R to divide the output of this summation by five, so the command that we need to type now becomes the following:

```
sum(afl.margins[1:5]) / 5
[1] 36.6
```

Although it’s pretty easy to calculate the mean using the `sum()` function, we can do it in an even easier way, since R also provides us with the `mean()` function. To calculate the mean for all 176 games, we would use the following command:

```
mean(x = afl.margins)
[1] 35.30114
mean(afl.margins[1:5])
[1] 36.6
```

However, since `x` is the first argument to the function, we could have omitted the argument name.

### The median

The second measure of central tendency that people use a lot is the **median**, the middle value. As before let’s imagine we were interested only in the first 5 AFL winning margins: 56, 31, 56, 8, and 32. To figure out the median, we sort these numbers into ascending order:

$$
8,31,32,56,56
$$

From inspection, it’s obvious that the median value of these 5 observations is 32 since that’s the middle one in the sorted list. But what should we do if we were interested in the first 6 games rather than the first 5?

$$
8,14,31,32,56,56
$$

There are two middle numbers, 31 and 32. The median is defined as the average of those two numbers, which is of course 31.5. To illustrate this, here’s what happens when you use R to sort all 176 winning margins:

```
sort(x = afl.margins)
```

```
  [1]   0   0   1   1   1   1   2   2   3   3   3   3   3   3   3   3   4
 [18]   4   5   6   7   7   8   8   8   8   8   9   9   9   9   9   9  10
 [35]  10  10  10  10  11  11  11  12  12  12  13  14  14  15  16  16  16
 [52]  16  18  19  19  19  19  19  20  20  20  21  21  22  22  22  23  23
 [69]  23  24  24  25  25  26  26  26  26  27  27  28  28  29  29  29  29
 [86]  29  29  30  31  32  32  33  35  35  35  35  36  36  36  36  36  36
[103]  37  38  38  38  38  38  39  39  40  41  42  43  43  44  44  44  44
[120]  44  47  47  47  48  48  48  49  49  50  50  50  50  52  52  53  53
[137]  54  54  55  55  55  56  56  56  57  60  61  61  63  64  65  65  66
[154]  67  68  70  71  71  72  73  75  75  76  81  82  82  83  84  89  94
[171]  95  98 101 104 108 116
```

In real life, we use the median command:

```
median(x = afl.margins)
[1] 30.5
```

which outputs the median value of 30.5.

### Mean or median? What’s the difference?

<figure><img src="https://learningstatisticswithr.com/book/img/descriptives2/meanmedian.png" alt=""><figcaption><p>Interpreting the mean and the median</p></figcaption></figure>

The mean is basically the “center of gravity” of the data set: if you imagine that the histogram of the data is a solid object, then the point on which you could balance it (as if on a see-saw) is the mean. In contrast, the median is the middle observation. Half of the observations are smaller, and half of the observations are larger. One consequence is that there are systematic differences between the mean and the median when the histogram is asymmetric (skewed). This is illustrated in **Figure**. Notice that the median (right-hand side) is located closer to the “body” of the histogram, whereas the mean (left-hand side) gets dragged towards the “tail” (where the extreme values are). To give a concrete example, suppose Bob (income $50,000), Kate (income $60,000) and Jane (income $65,000) are sitting at a table: the average income at the table is $58,333 and the median income is $60,000. Then Bill sits down with them (income $100,000,000). The average income has now jumped to $25,043,750 but the median rises only to $62,500. If you’re interested in looking at the overall income at the table, the mean might be the right answer; but if you’re interested in what counts as a typical income at the table, the median would be a better choice here.

### Trimmed mean

Real life is never simple, and so the data sets that you obtain are never as straightforward as the statistical theory says. To illustrate, consider this rather strange-looking data set:

$$
-100,2,3,4,5,6,7,8,9,10
$$

If you were to observe this in a real-life data set, you’d probably suspect that something funny was going on with the value $$−100$$. It’s probably an **outlier**, a value that doesn’t really belong with the others. You might consider removing it from the data set entirely. In real life, however, you don’t always get such cut-and-dried examples. For instance, you might get this instead:

$$
−15,2,3,4,5,6,7,8,9,12
$$

The $$−15$$ looks a bit suspicious, but not anywhere near as much as that $$−100$$ did. In this case, it’s a little trickier. It might be a legitimate observation, it might not. When faced with a situation where some of the most extreme-valued observations might not be quite trustworthy, the mean is not necessarily a good measure of central tendency. It is highly **sensitive** to one or two extreme values, and is thus not considered to be a **robust** measure. One remedy that we’ve seen is to use the median. A more general solution is to use a “trimmed mean”.

To calculate a trimmed mean, what you do is “discard” the most extreme examples on both ends (i.e., the largest and the smallest), and then take the mean of everything else. The goal is to preserve the best characteristics of the mean and the median: just like a median, you aren’t highly influenced by extreme outliers, but like the mean, you “use” more than one of the observations. Generally, we describe a trimmed mean in terms of the percentage of observation on either side that are discarded. So, for instance, a 10% trimmed mean discards the largest 10% of the observations _and_ the smallest 10% of the observations, and then takes the mean of the remaining 80% of the observations.

For our toy example above, we have 10 observations, and so a 10% trimmed mean is calculated by ignoring the largest value (i.e., `12`) and the smallest value (i.e., `-15`) and taking the mean of the remaining values:

```
dataset <- c(-15,2,3,4,5,6,7,8,9,12)
mean(x = dataset)
median(x = dataset)
```

```
[1] 4.1
[1] 5.5
```

If we take a 10% trimmed mean, we’ll drop the extreme values on either side and take the mean of the rest:

```
mean( x = dataset, trim = .1)
```

```
[1] 5.5
```

### Mode

The mode of a sample is very simple: it is the value that occurs most frequently. To illustrate the mode using the AFL data, let’s examine a different aspect of the data set. Who has played in the most finals? The `afl.finalists` variable is a factor that contains the name of every team that played in any AFL final from 1987-2010, so let’s have a look at it. To do this we will use the `head()` command. `head()` is useful when you’re working with a data frame with a lot of rows since you can use it to tell you how many rows to return. The command below tells R we just want the first 25 rows of the data frame:

```
head(afl.finalists, 25)
```

There are actually 400 entries (aren’t you glad we didn’t print them all?). Let’s use the `table()` function to create a **frequency table**:

```
table(afl.finalists)
```

```
afl.finalists
        Adelaide         Brisbane          Carlton      Collingwood 
              26               25               26               28 
        Essendon          Fitzroy        Fremantle          Geelong 
              32                0                6               39 
        Hawthorn        Melbourne  North Melbourne    Port Adelaide 
              27               28               28               17 
        Richmond         St Kilda           Sydney       West Coast 
               6               24               26               38 
Western Bulldogs 
              24
```

Now that we have our frequency table, we can just look at it and see that, over the 24 years for which we have data, Geelong has played in more finals than any other team. Thus, the mode of the `finalists` data is `"Geelong"`. The core packages in R don’t have a function for calculating the mode. There a function called `maxFreq()` that tells you what the modal frequency is:

```
maxFreq(x = afl.finalists)
[1] 39
```

Taken together, we observe that Geelong (39 finals) played in more finals than any other team during the 1987-2010 period.
