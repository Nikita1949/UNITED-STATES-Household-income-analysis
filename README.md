# US Household Income Analysis

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Findings](#findings)
- [Visualizations](#visualizations)
- [Limitations](#limitations)
  
## Project Overview
Through this project, I delved into a comprehensive dataset capturing household income metrics as well as the land and water areas across various states in the United States. Leveraging statistical techniques and data visualization tools, I explored the distribution, trends, and disparities in household income levels.
## Data Source
US Houshold Income Data: The primary dataset utilized for this analysis is the "UShousholdincome.csv" file, encompassing various details about the state demographic, the area of water and land, the mean and median.

## Tools
- SQL Server- Data Cleaning
- SQL Server- Exploratory Data Analysis
- Tableau- Report Generation and Visualization [Click here](https://public.tableau.com/app/profile/abimbola.ajayi8433/vizzes)

## Exploratory Data Analysis
EDA involve exploring the data to answer the key questions, such as:
- The top 10 largest state by land areas 
```sql
SELECT state_name, SUM(Aland)
FROM ushouseholdincome
GROUP BY state_name
ORDER BY SUM(Aland) DESC
LIMIT 10;
```
- The top 10 largest state by water areas
```sql
SELECT state_name,SUM(Awater)
FROM ushouseholdincome
GROUP BY state_name
ORDER BY SUM(Awater) DESC
LIMIT 10;
```
- What is the Distribution of area of water and land across different states 
```sql
SELECT state_name, city, ROUND(SUM(Aland),0) AS sum_of_land, ROUND(SUM(Awater),0) AS sum_of_water 
FROM ushouseholdincome
GROUP BY state_name, city
HAVING sum_of_land <> 0 
AND sum_of_water <> 0
ORDER BY sum_of_land DESC;
```
- What is the distribution of household incomes across different states, cities, and counties?
```sql
SELECT u.state_name, u.city, u.county, ROUND(AVG(median),0) AS Avg_median, ROUND(AVG(mean),0) AS Avg_mean
FROM ushouseholdincome u
JOIN ushouseholdincome_statistics s ON
u.id = s.id
GROUP BY u.state_name, u.city, u.county
HAVING AVG(median) <> 0
AND AVG(mean) <> 0
ORDER BY Avg_mean DESC;
```
- Which states exhibit the highest and lowest household incomes?
```sql
SELECT u.state_name, AVG(mean)
FROM ushouseholdincome u
JOIN ushouseholdincome_statistics s ON
u.id = s.id
GROUP BY u.state_name
ORDER BY AVG(mean) DESC ;

SELECT u.state_name, AVG(mean)
FROM ushouseholdincome u
JOIN ushouseholdincome_statistics s ON
u.id = s.id
GROUP BY u.state_name
ORDER BY AVG(mean) DESC ;
```
- What regions have the highest income

```sql
SELECT u.Type,ROUND(AVG(median),1) AS Avg_median, ROUND(AVG(mean),1) AS Avg_mean
FROM ushouseholdincome u
INNER JOIN ushouseholdincome_statistics s ON
u.id = s.id
GROUP BY u.Type
HAVING AVG(mean) <> 0
ORDER BY 3 DESC
LIMIT 10;
```
## Findings
According to the dataset, the top 5 states by land area are Texas, California, Missouri, Minnesota, and Illinois. These states boast extensive land areas, with Texas and California being particularly notable for their large geographical footprint.
Michigan, Texas, Florida, and Minnesota emerge as the top states by water area, with significant bodies of water contributing to their overall geographic composition. The inclusion of Texas and Florida underscores the importance of coastal regions in the distribution of water resources.


Visualizations of household income distribution across states reveal notable disparities. States such as New Jersey, Alaska, California and New York exhibit higher median household incomes, reflective of their large urban centers and diverse economies. Conversely, states like Mississippi and West Virginia have lower median incomes, indicative of socioeconomic challenges in rural areas. The findings also indicate that the Municipality region boasts the highest income levels.
