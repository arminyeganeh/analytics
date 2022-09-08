---
description: Week 1, Lecture A1.1, 1500 words, 1 hour to complete
---

# Origins of Data

### What is Data?

Data is "factual information, such as measurements or statistics, used as a basis for reasoning, discussion, or calculation." (Merriam-Webster) An almost universal feature of data is that it rarely comes in a form that can directly help answer our questions.&#x20;

### What is Data Analysis?

Data analysis is a process. It starts with formulating a question and collecting appropriate data or assessing whether the available data can help answer the question. Then comes cleaning and organizing the data, tedious but essential tasks that affect analysis results as much as any other step in the process. Exploratory data analysis gives context to results and helps decide the details of the analytical method to be applied. The primary analysis consists of choosing and implementing the method to answer the question, with potential robustness checks. Along the way, correct interpretation and effective presentation of the results are crucial. Carefully crafted data visualization help summarize our findings and convey key messages. The final task is to answer the original question, with potential qualifications and directions for future inquiries.

Data is most straightforward to analyze if it forms a single data table. A data table consists of observations and variables. Observations are also known as cases. Variables are also called features. A dataset is a broader concept that includes, potentially, multiple data tables with different kinds of information. In a data table, the rows are the observations: each row is a different observation, and whatever is in a row is information about that specific observation. Columns are variables, so that column one is variable one, column two is another variable, and so on.

A common file format for data tables is the csv file (for "comma separated values"). csv files are text files of a data table, with rows and columns. Rows are separated by end of line signs; columns are separated by a character called a delimiter (often a comma or a semicolon). csv files can be imported.

Variables are identified by names. The data table may have variable names already, and analysts are free to use those names or rename the variables. Personal taste plays a role here: some prefer short names that are easier to work with in code; others prefer long names that are more informative; yet others prefer variable names that refer to something other than their content (such as the question number in a survey questionnaire). It is good practice to include the names of the variables in the first row of a csv data table. The observations start with the second row and go on until the end of the file.

Observations are identified by identifier or ID variables. An observation is identified by a single ID variable, or by a combination of multiple ID variables. ID variables, or their combinations, should uniquely identify each observation. They may be numeric or text containing letters or other characters. They are usually contained in the first column of data tables. We use the notation to refer to the value of variable x for observation i, where i typically refers to the position of the observation in the dataset. This way i starts with 1 and goes up to the number of observations in the dataset (often denoted as n or N). In a dataset with n observations, i 1, 2, . . n. (Note that in some programming languages, indexing may start from 0.)

## Data Structures

Observations can have a cross-sectional, time series, or a multi-dimensional structure.

![](file:///C:/Users/armin/AppData/Local/Temp/msohtmlclip1/01/clip\_image002.jpg)Observations in cross-sectional data, often abbreviated as xsec data, come from the same time, and they refer to different units such as different individuals, families, firms, and countries. Ideally, all observations in a cross-sectional dataset are observed at the exact same time. In practice this often means a particular time interval. When that interval is narrow, data analysts treat it as if it were a single point in time.

In most cross-sectional data, the ordering of observations in the dataset does not matter: the first data row may be switched with the second data row, and the information content of the data would be the same. Cross-sectional data has the simplest structure. Therefore we introduce most methods and tools of data analysis using cross-sectional data and turn to other data structures later.

Observations in time series data refer to a single unit observed multiple times, such as a shop's monthly sales values. In time series data, there is a natural ordering of the observations, which is typically important for the analysis. A common abbreviation used for time series data is tseries data. We shall discuss the specific features of time series data in Chapter 1 2, where we introduce time series analysis.

Multi-dimensional data, as its name suggests, has more than one dimension. It is also called panel data. A common type of panel data has many units, each observed multiple times. Such data is called longitudinal data, or cross-section time series data, abbreviated as Xt data. Examples include countries observed repeatedly for several years, data on employees of a firm on a monthly basis, or prices of several company stocks observed on many days.

Multi-dimensional datasets can be represented in table formats in various ways. For Xt data, the most convenient format has one observation representing one unit observed at one time (country— year observations, person—month observations, company-day observations) so that one unit (country, employee, company) is represented by multiple observations. In Xt data tables, observations are identified by two ID variables: one for the cross-sectional units and one for time. Xt data is called balanced if all cross-sectional units have observations for the very same time periods. It is called unbalanced if some cross-sectional units are observed more times than others. We shall discuss other specific features of multi-dimensional data in Chapter 23 where we discuss the analysis of panel data in detail.

Another important feature of data is the level of aggregation of observations. Data with information on people may have observations at different levels: age is at the individual level, home location is at the family level, and real estate prices may be available as averages for zip code areas. Data with information on manufacturing firms may have observations at the level of plants, firms as legal entities (possibly with multiple plants), industries with multiple firms, and so on. Time series data on transactions may have observations for each transaction or for transactions aggregated over some time period.

Sometimes data is available at a level of aggregation that is different from the ideal level. If data is too disaggregated (i.e., by establishments within firms when decisions are made at the firm level), we may want to aggregate all variables to the preferred level. If, however, the data is too aggregated (i.e., industry-level data when we want firm-level data), there isn't much that can be done. Such data misses potentially important information.

### Data Quality

Data analysts should know their data. They should know how the data was born, with all details of measurement that may be relevant for their analysis. They should know their data better than their audience. Few things have more devastating consequences for a data analyst's reputation than someone in the audience pointing out serious measurement issues the analyst didn't consider.

Garbage in - garbage out. This summarizes the prime importance of data quality. The results of an analysis cannot be better than the data it uses. If our data is useless to answer our question, the results of our analysis are bound to be useless, no matter how fancy a method we apply to it. Conversely, with excellent data even the simplest methods may deliver very useful results. Sophisticated data analysis may uncover patterns from complicated and messy data but only if the information is there.

| Aspect             | Description                                                                                                                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Content            | The content of a variable is determined by how it was measured, not by what it was meant to measure. As a consequence, just because a variable is given a particular name, it does not necessarily measure that. |
| Validity           | The content of a variable (actual content) should be as close as possible to what it is meant to measure (intended content).                                                                                     |
| Reliability        | Measurement of a variable should be stable, leading to the same value if measured the same way again.                                                                                                            |
| Comparability      | A variable should be measured the same way for all observations.                                                                                                                                                 |
| Coverage           | Ideally, observations in the collected dataset should include all of those that were intended to be covered (complete coverage). In practice, they may not (incomplete coverage).                                |
| Unbiased selection | If coverage is incomplete, the observations that are included should be similar to all observations that were intended to be covered (and, thus, to those that are left uncovered).                              |

### How Data Is Born: The Big Picture

Data can be collected for the purpose of the analysis, or it can be derived from information collected for other purposes.

![](file:///C:/Users/armin/AppData/Local/Temp/msohtmlclip1/01/clip\_image002.jpg)The structure and content of data purposely collected for the analysis are usually better suited to analysis. Such data is more likely to include variables that are the focus of the analysis, measured in a way that best suits the analysis, and structured in a way that is convenient for the analysis. Frequent methods to collect data include scraping the Web for information (web scraping) or conducting a

survey (see Section 1.5 and Section 1.6).

Data collected for other purposes can be also very useful to answer our inquiries. Data collected for the purpose of administering, monitoring, or controlling processes in business, public administration, or other environments are called administrative data ("admin" data). If they are related to transactions, they are also called transaction data. Examples include payment, promotion, and training data of employees of a firm; transactions using credit cards issued by a bank; and personal income tax forms submitted in a country.

Admin data usually cover a complete population: all employees in a firm, all customers of a bank, or all tax filers in a country. A special case is Big Data, to be discussed in more detail in Section 1.9, which may have its specific promises and issues due to its size and other characteristics.

Often, data collected for other purposes is available at low cost for many observations. At the same time, the structure and content of such data are usually further away from the needs of the analysis compared to purposely collected data. This trade-off has consequences that vary across data, methods, and questions to be answered.

Data quality is determined by how the data was born, and data collection affects various aspects of data quality in different ways. For example, validity of the most important variables tends to be higher in purposely collected data, while coverage tends to be more complete in admin data. However, that's not always the case, and even when it is, we shouldn't think in terms of extremes. Instead, it is best to think of these issues as part of a continuum. For example, we rarely have the variables we ideally want even if we collected the data for the purpose of the analysis, and admin data may have variables with high validity for our purposes. Or, purposely collected data may have

incomplete coverage but without much selection bias, whereas admin data may be closer to complete coverage but may have severe selection bias.
