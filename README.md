# India General Elections 2024 Data Analysis Using SQL

## Overview 

This project involves an in-depth analysis of the 2024 India General Elections results using SQL. The aim is to extract insights related to the election results across different constituencies and states. The project addresses key business questions such as the distribution of seats won by political alliances, vote margins, and the performance of political parties.

##  Objectives

* Analyze the number of seats won by political alliances (NDA, I.N.D.I.A, OTHER).
* Identify the top candidates based on EVM votes and the closest races.
* Study content distribution based on geographic and demographic factors.
* Analyze party-wise and state-wise election outcomes.

##  Business Problems and Solutions
### Total Seats
```sql
SELECT 
    type,
    COUNT(*)
FROM netflix
GROUP BY 1;
```
