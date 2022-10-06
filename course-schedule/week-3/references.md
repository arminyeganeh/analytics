# References

Agresti, A. (2018). Statistical Methods for the Social Sciences.

* Chapter 5 Statistical Inference: Estimation
* Chapter 6 Statistical Inference: Significance Tests

Bekez G. & Kezdi B. (2021). Data Analysis for Business, Economics, and Analysis. &#x20;

* Chapter 5: Generalization
* Chapter 6: Hypothesis testing

Navarro, D. (2019) Learning Stats with R

* Chapter 5 Descriptive Statistics

### A real-life example

To try to get a sense of why you need to pay attention to the differences between the mean and the median, let’s consider a real-life example. This is from an excellent article on the ABC news website, dated 24 September 2010:

> Senior Commonwealth Bank executives have travelled the world in the past couple of weeks with a presentation showing how Australian house prices, and the key price to income ratios, compare favourably with similar countries. “Housing affordability has actually been going sideways for the last five to six years,” said Craig James, the chief economist of the bank’s trading arm, CommSec.

This probably comes as a huge surprise to anyone with a mortgage, or who wants a mortgage, or pays rent, or isn’t completely oblivious to what’s been going on in the Australian housing market over the last several years. Back to the article:

> CBA has waged its war against what it believes are housing doomsayers with graphs, numbers and international comparisons. In its presentation, the bank rejects arguments that Australia’s housing is relatively expensive compared to incomes. It says Australia’s house price to household income ratio of 5.6 in the major cities, and 4.3 nationwide, is comparable to many other developed nations. It says San Francisco and New York have ratios of 7, Auckland’s is 6.7, and Vancouver comes in at 9.3.

More excellent news! Except, the article goes on to make the observation that…

> Many analysts say that has led the bank to use misleading figures and comparisons. If you go to page four of CBA’s presentation and read the source information at the bottom of the graph and table, you would notice there is an additional source on the international comparison – Demographia. However, if the Commonwealth Bank had also used Demographia’s analysis of Australia’s house price to income ratio, it would have come up with a figure closer to 9 rather than 5.6 or 4.3

That’s, um, a rather serious discrepancy. One group of people says 9, another says 4-5. Should we just split the difference, and say the truth lies somewhere in between? Absolutely not: this is a situation where there is a right answer and a wrong answer. Demographia is correct, and the Commonwealth Bank is incorrect. As the article points out

> \[An] obvious problem with the Commonwealth Bank’s domestic price to income figures is they compare average incomes with median house prices (unlike the Demographia figures that compare median incomes to median prices). The median is the mid-point, effectively cutting out the highs and lows, and that means the average is generally higher when it comes to incomes and asset prices, because it includes the earnings of Australia’s wealthiest people. To put it another way: the Commonwealth Bank’s figures count Ralph Norris’ multi-million dollar pay packet on the income side, but not his (no doubt) very expensive house in the property price figures, thus understating the house price to income ratio for middle-income Australians.

The way that Demographia calculated the ratio is the right thing to do. The way that the Bank did it is incorrect. The article itself does happen to mention the following facts, which may or may not be relevant:

> \[As] Australia’s largest home lender, the Commonwealth Bank has one of the biggest vested interests in house prices rising. It effectively owns a massive swathe of Australian housing as security for its home loans as well as many small business loans.

**How to Lie with Statistics** is a book written by Darrell Huff in 1954, presenting an introduction to statistics for the general reader. The book is a brief, breezy illustrated volume outlining the misuse of statistics and errors in the interpretation of statistics, and how these errors may create incorrect conclusions.
