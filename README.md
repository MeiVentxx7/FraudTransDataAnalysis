# Fraud Transaction Data Cleaning & EDA Project


## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Steps](#steps)
- [Key Takeaways](#key-takeaways)  
&ensp;

### Project Overview

This project aims to provide key insights and discover correlations between certain factors and the occurrence of fraudulent transactions.  
&ensp;

### Data Sources

- fraud_data.csv (https://www.kaggle.com/datasets/neharoychoudhury/credit-card-fraud-data/data) :<br/>
  Contains detailed information about each transaction, including date, time, amount, customer information, and location in 2019 and 2020.  
&ensp;

### Tools

MS SQL Server  
&ensp;

### Steps

I took the following steps to perform analysis: 

1. Established specific questions such as:

    - Which factors have a strong correlation with the occurrence of fraudulent transactions?
    - Did the fraud transaction rate decrease compared to previous year?
    - Which purchase categories have the highest percentage of fraud transactions?  
&ensp;

2. Data Cleaning/Preprocessing

    - Made a separate column for date, year, and month based on the `trans_date_trans_time`column.
    - Extracted the age for each person and added it as a new column.

    ```sql
    select datediff(YEAR, dob, cast(trans_date_trans_time as date)) as age 
    from DataCleaningProject.dbo.fraud_data;
    
    alter table dbo.fraud_data
    add age int;

    update dbo.fraud_data
    set age = datediff(YEAR, dob, cast(trans_date_trans_time as date));
    ```
    - Removed duplicates and deleted unused columns.  
&ensp;

3. Exploratory Data Analysis

    - Conducted SQL queries to investigate the correlation between the occurrence of fraud transactions and certain factors such as month, hour, age, and location.<br/>   
      &nbsp;
   For example,
      
   ```sql
   -- Q2. Are older people more likely to be targeted? 
   -- Is there a correlation between age and the occurrence of fraud transactions?

   select age, sum(amt) as total_amt, count(trans_date_trans_time)
   from DataCleaningProject.dbo.fraud_data
   where is_fraud = 'Yes'
   group by age
   order by 1 desc
   ```
      and
   
   ```sql
   -- Q5. Is there a correlation between transaction hours and occurence?

   select DATEPART(HOUR, trans_date_trans_time) as trans_hour, count(is_fraud) as fraud_count
   from DataCleaningProject.dbo.fraud_data
   where is_fraud = 'Yes'
   group by DATEPART(HOUR, trans_date_trans_time)
   order by 2 desc  
   ```
   &ensp; 

### Key Takeaways

- I found that most fraudulent transactions occurred between 10 p.m. and 3 a.m., which is bedtime for most people.
- I initially assumed that older individuals would have more fraudulent transactions because they are more likely to be targeted.<br/>
  However, I was wrong. I created a visualization to explore this in more detail for the next project.
- The other factors did not provide key insights regarding a correlation with the occurrence of fraudulent transactions.
- The total number of fraudulent transactions in 2020 decreased by only 1% compared to 2019.  
&ensp;

**Thank you for reading**!😄




    
