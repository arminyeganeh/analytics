# Proxy Variables

"Often, the y or x variables we have in question are abstract concepts: the quality of management of a firm, skills of an employee, the risk tolerance of an investor, the health of a person, the wealth of a country. Typically, such variables are not parts of an actual dataset, and they can't be because they are too abstract. Such variables are called **latent variables**. Data analysis can help answer questions involving latent variables only by substituting observed variables for them. Those observed variables are called **proxy variables**, where "proxy" means substitute ... The quality of management may be proxied by answers to survey questions on management practices, as in our case study; employee skills may be proxied by qualifications or measures of past performance; and so on. The important thing here is that data analysis compares values of measured variables. Even if those variables are supposed to measure abstract concepts, it's never the abstract concepts themselves that we have in our data. Thus we can never examine things such as skills or attitudes or health; instead, we examine proxies such as measures of performance, answers to survey questions, or results of doctors' diagnoses.&#x20;

The content of a variable is determined by how it is measured — not by what name somebody attached to it. A specific issue arises when our data contains not one but more variables that could serve as proxies to the latent variable we want to examine. The question here is how to combine multiple observed variables. Data analysts use one of three main approaches:&#x20;

* Use one of the observed variables&#x20;
* Take the average (or sum) of the observed variables&#x20;
* Use principal component analysis (PCA) to combine the observed variables&#x20;

Using one measured variable and excluding the rest has the advantage of easy interpretation. It has the disadvantage of discarding potentially useful information contained in the other measured variables. Taking the average of all measured variables makes use of all information in a simple way. When all of those variables are measured using the same scale, this approach yields a combined measure with a natural interpretation. When the variables are measured at different scales, we need to bring the observed variables to the same scale. Usually, we do that by standardizing them: subtracting the mean and dividing by the standard deviation. This **standardized measure** is also called a **z-score**. By taking a simple average of all these variables we give them equal weight. This may be a disadvantage if some of the variables are better measures of the latent variable than others. The third approach remedies that problem. Principal component analysis (PCA) is a method to give higher weights to the observable variables that better measure the latent variable. PCA finds those weights by examining how strongly they would be related to the weighted average. The logic is an iterative process: create an average, examine how each variable is related to it by looking at their correlations, give higher weights to those with stronger correlations, and start over.&#x20;

Of the three approaches, we recommend the second one: taking a simple average of the observed variables after making sure that they are measured at the same scale. This is the simplest way to combine all variables in a meaningful way. In principle, PCA produces a better measure, but it is more complicated to produce and harder to present to non-expert audiences. Moreover, the results are often similar to a simple average. Thus we recommend using PCA as a robustness check if used at all. If the results seem to be very different, some of our observed variables are very differently related to the average measure than others. It is good practice to go back and understand the content of those variables and, perhaps, discard some of them from the analysis." (Bekes, 2021)