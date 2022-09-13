# Linking Data Tables

"The process of pulling different variables from different data tables for well-identified entities to create a new data table is called linking, joining, merging, or matching data tables. To do so, data analysts start with one data table, and they merge with it observations from another data table. The most straightforward linkage is **one-to-one (1:1) matching**: merging tables with the same type of observations. For example, a data table with customers as observations with their age and income as variables may be merged with another data table on customers with a variable describing how they rated their last visit to the retail store. Other times we may want to do many-to-one (m:1) matching. For example, we may want to link average family income in zip code locations, a data table with observations on zip codes, to a data table with customers as observations whose zip code of residence is a variable. In the customer-level data table, multiple customers may live in the same zip code area, thus the same zip code information may be merged with many customers. An alternative is **one-to-many (1:m) matching**. One-to-many matching can be thought of as the same as **many-to-one (m:1) matching** but changing the order of the data tables: starting with the data table with the more aggregated observations (zip codes, countries) and merging with it the observations from the other dataset (customers, country-year observations)." (Bekes, 2021)&#x20;

"The most complex case is **many-to-many (m:m) matching**: one observation in the first table may be matched with many observations in the second table, and an observation in the second data table may be matched with many observations in the first data table. In such an instance, we may need to have a separate table that connects IDs. For example, we may want to link information on companies and information on managers. Each company may have had more than one manager during the time span of our data; each manager may have worked for more than one company. In such complicated cases, we often need to think a bit and rephrase the problem as **a set of 1:m and m:1 matching** problems." (Bekes, 2021)