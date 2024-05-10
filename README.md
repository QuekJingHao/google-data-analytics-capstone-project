# Google Data Analytics Professional Certificate Capstone Case Study

#### -- Project Status: [Completed]

## Project Intro / Objective

The purpose of this project is to analyze the historical revenue of top 100 defense contractors since 2005, by employing the _Ask, Prepare, Process, Analyze, Share, Act_ framework introduced in the course. In order to users to view the analysis for each company, we will also build an interactive dashboard using Looker Studio. This analysis will help government agencies to identify who are the best performing contractors to outsource sensitive procurement projects to. 

### Methods Used
* Inferential Statistics
* Data Wranging
* Data Aggregation
* Data Visualization

### Technologies
* Python
* Pandas, Numpy
* Jupter
* Looker Studio

## Project Description

Imagine you are working as a Business Intelligence Analyst at a defense agency in the government. Recently, your agency is outsourcing an important project to an external defense contractor. The Project Manager has tasked you to conduct an analysis of the historical data of the top 100 defense companies' revenue. Your job is to identify the most suitable firm(s) to be awarded the contract. The metric of success for a company will be its gross revenue from defense contracting.

The dataset used in this project can be assessed via the following link:

https://www.kaggle.com/datasets/onurduman/defence-companies-top-100-for-each-year-from-2005

This repository contains the following directories:
1. Data: contains the raw and processed dataset
2. Information: some pdfs I saved from the Coursera course
3. Notebook: contains only the main notebook used in this analysis


## Details of Project

This section wil summarize the main sections in the ```main.ipynb```:

### Prepare

Looking at the dataset, we have the following columns

<pre>
* Year : year
* Rank : position of the company relative to other within that year
* Company : name of the defense contractor
* Leadership : name of the CEO / President of the firm
* Country : country where the company is based
* Defense_Revenue_From_A_Year_Ago(in millions) : revenue from defense production 1 year ago
* Defense_Revenue_From_Two_Years_Ago(in millions) : revenue from defense production 2 years ago
* %Defense Revenue Change : percentage change of revenue from defense production 1 year ago
* Total Revenue(in millions) : total revenue generated 1 year ago
* %of Revenue from Defence : percentage of revenue from defense production between the last 2 years
</pre>

The datatset gives the revenue obtained from defense contracting 1 year ago. Nonetheless the ranking of the company in the current year is based on its perfromance 1 year ago. That is, for the current year for each company, the defense revenue last year is computed from its total revenue and its percentage of revenue from defense. Their ranking is sorted according to this value in the present year.

### Process

This is the longest section in the notebook. Mainly, we want to manipulate the dataset into something we can work with. We perform the following cleaning steps such as:

* Sort by the unique country names, we standardize the duplicated company names, and correct the mispelled one. This step involves a a good amount of googling! We'll stick to the latest company name as at 2022. These errors are very likely caused by changes in the company name
* Pay special attention to companies that sound similar but are actually separate entities
* Select only rows where the percentage of defense contracting revenue is more than 70%
* Scale the 2013 year revenue accordingly
* We subtract 1 from the Year column, drop the Rank, Leadership and Defense_Revenue_From_A_Year_Ago(in millions) columns. This will reflect the current year's defense revenue income

For example, We want to apply function to scale the numbers by a million:

```python
# scale the numbers by a million
df_cleaned['Present Year Revenue'] = df_cleaned['Present Year Revenue'].apply(lambda x : x / 1000000 if len(str(x)) >= 10 else x)
df_cleaned['Last Year Revenue'] = df_cleaned['Last Year Revenue'].apply(lambda x : x / 1000000 if len(str(x)) >= 10 else x)
df_cleaned['Present Year Total Revenue'] = df_cleaned['Present Year Total Revenue'].apply(lambda x : x / 1000000 if len(str(x)) >= 10 else x)
```


### Analyze

We will perform several aggregations on the cleaned dataframe to derive summary numbers for our analysis. From the aggregated values, we can perform calculations to derive some summary statistics. Lastly, from the numbers and standing of the companies, we can identify trends and relationships.

For example, if we want to aggregate the present year's defense revenue by country, we can use the ```.groupby``` method:


```python
df_cleaned.groupby(['Country']).agg('sum', numeric_only = True).sort_values(by = 'Present Year Revenue', ascending = False)[:10]
```

Senior Management would generally want to restrict their attention to the Top 10 defense firms. We can calculate summary statistics as follows:

```python
df_top_10.groupby(['Company']).agg({'Present Year Revenue'      : ['sum', 'mean', 'median'], 
                                    'Percentage Change'         : ['sum', 'mean', 'median'],
                                    'Percentage Defense Revenue': ['sum', 'mean', 'median'], 
                                    }).sort_values(by = ('Present Year Revenue', 'median'), ascending = False)
```

### Share

From our analysis and the dashboard in Looker Studio, we summarize our analysis here.

The top five countries with the largest defense contracting market are: 

1. USA 
2. UK
3. France 
4. Russia
5. Netherlands

with a combined market share of <u>**5.11 trillion USD**</u>, over 2004 to 2019.

The top performer in terms of yearly defense revenue is <u>**Lockheed Martin Corporation**</u>, with a mean yearly defense profit of <u>**41.72 billion USD**</u>.

The top five defense companies - ranked in terms of median yearly defense revenue - are:

1. Lockheed Martin Corporation 
2. Boeing
3. BAE Systems
4. Raytheon Company
5. Northrop Grumman Corporation

with a combined yearly defense revenue of <u>**2.28 trillion USD**</u> over the period 2004 to 2019. With the exception of BAE Systems - which is British - the rest of the four companies are American. These companies are consistently ranked as top five most profitable companies during this period.

If we look at the growth for these five companies, the one with the most steady growth is <u>**Raytheon Company**</u>. This can be seen from the Growth against Year graph. Raytheon displays the least fluctuation compared to the rest.

### Act

From our findings in the previous steps, I present the following recommendations:

- Lockheed Martin Corporation should be awarded the project tender. With a median yearly defense revenue of 41.72 billion USD, it is the top defense firm. Furthermore, by working with Lockheed Martin, we have access to the lucrative US defense contracting market - with a total market valuation of 3.67 trilion USD, it represents the world's largest share of contractors.

- Besides Lockheed Martin, other top five companies such as Boeing, Raytheon and Northrop Grumman can also be taken into consideration. These companies also allow us to gain access to the US market.

- Now that we have identified the best firm to be awarded the tender, we can ask further specific questions such as:
   1. Which division are we working with? 
   2. How many key deliverables are there in our project? 
   3. Do we work with more than one division in Lockheed Martin?

- Lastly, we can dig deeper into accesing the financial performance of the company by looking at various financial indicators, such as their cash flow and income statements.



## Featured Deliverables

You can access the Dashboard using the following link: [Dashboard](https://lookerstudio.google.com/reporting/30010fde-8f3a-4c33-ade6-aaa86d07e159)


The dashboard has two views - an _Overall_ view:

https://github.com/[username]/[reponame]/blob/[branch]/image.jpg

<img src="https://your-image-url.type" width="100" height="100">


as well as an _Analytics_ view:


https://github.com/[username]/[reponame]/blob/[branch]/image.jpg

<img src="https://your-image-url.type" width="100" height="100">


