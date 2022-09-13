# Conditional Probabilities

"We introduced probability as the generalization of relative frequency. The probability of a value of a variable in a dataset is its relative frequency (percentage). In more abstract settings, the probability of an event is the likelihood that it occurs. In this section, we discuss comparing the probability of events, or the probability of values of variables, by another event, or by values of another variable. Conditional probability is the probability of one event if another event happens. The event the conditional probability is about is called the **conditional event**; the other event is called the **conditioning event**." (Bekes, 2021)&#x20;

**In data, the events refer to values of variables.** Conditional probabilities are denoted as P(event1 I event2): the probability of event 1 conditional on event 2. Note that the conditional probability is generally not symmetrical: P(event 1 | event2) != P(event2 | event 1). Pairs of probabilities of this sort are called **inverse conditional probabilities**. Thus, inverse conditional probabilities are not equal in general. In fact, they are related to each other in a somewhat complicated way.&#x20;

Joint probabilities are related to conditional probabilities. The joint probability of two events is the probability that both occur: P(event 1 & event2). When two events are mutually exclusive, their joint probability is zero. Another probability related to two events denotes the likelihood that one event or the other happens. This is the sum of the two probabilities minus their joint probability: P(event 1 OR event 2) = P(event 1) + P(event 2) - P(event 1 & event 2).&#x20;

The conditional probability can be expressed as the corresponding joint probability divided by the probability of the conditioning event: (P(event 1 I event2) - P(event 1 & event 2)) / P(event 2)&#x20;

Two events are independent if the probability of one of the events is the same regardless of whether or not the other event occurs. In the language of conditional probabilities, this means that the conditional probabilities are the same as the unconditional probabilities: P(event 1 I event 2) = P(event 1) and P(event2 | event 1) = P(event2). Less intuitive, but also true is that the joint probability of independent events equals the product of their individual probabilities: P(event 1 & event 2) = P(event 1) x P(event 2).

Often, the conditional variable, y, is a binary variable: y = 0 or y 1. Then the conditional probability is the probability that y = 1 if x has some value: P(y = 1 | x= value). Since y is binary, we know the probability of y = 0 if we know the probability of y = 1, be it a conditional or unconditional probability; e.g.,&#x20;

P(y = 0 l x = value) = 1 - P(y=1|x=value)

When x is binary too, there are two conditional probabilities:

P(y=1|x=1)

P(y=1|x=0)

With more values for either of the two variables (y, x), we have more numbers to compare. With relatively few values, visualization often helps. There are many options for using standard graphs as well as creating individualized graphs. A good solution is the stacked bar chart. It presents the relative frequencies within bars that are on top of each other and thus always add up to a height of 100 %. To visualize conditional probabilities if both y and x have few values, a good visualization is to show stacked bar charts of y for the values of x.
