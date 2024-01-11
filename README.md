# Data-Driven-Wellness
A Smart Data Analysis for Bellabeat

# Summary
Explore the intersection of technology, wellness, and data with this in-depth case study focused on Bellabeat, a high-tech manufacturer of health-focused products for women. The dataset at the heart of this analysis comes from the FitBit Fitness Tracker, capturing minute-level output for physical activity, heart rate, and sleep monitoring from thirty Fitbit users. In the era of smart devices, Bellabeat has positioned itself as a leader in providing health-focused technology for women. The dataset aims to uncover valuable insights into how users interact with Bellabeat's products, with a focus on one key product, guiding marketing strategies to further enhance user experiences. The dataset I will be using for the analysis (https://www.kaggle.com/datasets/arashnic/fitbit) is made available through Mobius and licensed under CC0: Public Domain. This Dataset presents a unique opportunity to delve into the daily habits of individuals using fitness trackers. The inspiration for this analysis comes from Bellabeat's commitment to empowering women with knowledge about their health and habits.

# Business Task
Sršen,Bellabeat's cofounder, would like an analysis of Bellabeat’s available consumer data to identify opportunities for growth. She has asked the marketing analytics team to analyze smart device usage data in order to gain insight into how people are already using their smart devices. Then, using this information, she would like recommendations for how these trends can inform Bellabeat marketing strategy. Therefore, in this case study, I will answer the following questions:
-What are some trends in smart device usage?
-How could these trends apply to Bellabeat customers?
-How can these trends help influence Bellabeat marketing strategy?

# Prepare
The dataset, obtained from Kaggle, has been imported into  Microsoft SQL Server Management Studio to help store,organize,process and analyze. 
For visualization purposes, I've used Tableau Public, employing its dynamic features to create compelling visual representations of the analyzed data. 
Limitations observed about the Dataset:
-It is unclear how participants are choosen
-The dataset does not contain any demographic information about the users, including gender, age, or location, which would be beneficial for marketing purposes to target specific customers
-The dataset is not current.The time limitation for the survey is two months long.

# Process
The organization of the data is essential for effective analysis. In SQL Server, the data is likely organized in a tabular format, with each row representing a specific record (e.g., daily activity) and each column representing a different attribute or variable. 
In the Process phase I have organized data by adding columns, extracting information and removing bad data and duplicates. For the sake of simplicity, I have centralized all of the data into a Relational Database that is connected using SQL Server. This allowed me to easily manage the entirety of the files and make relevant queries, as the CSV files can be transformed into tables which I have linked by joining common attributes.

--For importing the dataset to Microsoft SQL Server Management Studio,Go to Databases -> New Database -> Create a New Database 
--Then Right Click on the Database Created -> Click on Tasks -> Click on Import Data 
--Follow along 
--Select the CSV file from your device and import 

```
--Preview Data in dailyActivity
select *
from dbo.dailyActivity_merged

```
```

--Identify Number of Users
SELECT DISTINCT Id
FROM DBO.dailyActivity_merged
GROUP BY Id

```
```

--Cleaning and Preparing Data
--Identify Missing Values 
SELECT COUNT(*) AS MissingCount
FROM  dbo.dailyActivity_merged
WHERE Id IS NULL

```
```

--Identifying for duplicates in DailyActivity
SELECT Id, ActivityDate, TotalSteps, Count(*)
FROM dbo.dailyActivity_merged
GROUP BY id, ActivityDate, TotalSteps
HAVING Count(*) > 1

```
```

-- Check data types of columns
SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'dailyactivity_merged'

```
```

--Modify Data Types as required
ALTER TABLE dbo.dailyActivity_merged
ALTER COLUMN activitydate date

ALTER TABLE dbo.dailyActivity_merged
ALTER COLUMN totalsteps int

```
```
-- Add day_0f_week column in daily_activities
Alter Table  dbo.dailyActivity_merged
ADD day_of_week nvarchar(50)

```
```

--Extract datename from ActivityDate
Update dbo.dailyActivity_merged
SET day_of_week = DATENAME(DW, ActivityDate)

```
```

--Preview Data in sleepDay_merged
SELECT *
FROM DBO.sleepDay_merged

```
```

--Modify data type of SleepDay
ALTER TABLE DBO.sleepDay_merged
ALTER COLUMN SleepDay Date

```
```
--Preview Data in dbo.hourlyCalories_merged
select *
from dbo.hourlyCalories_merged

```
```

--Modify hourlyCalories_merged by splitting Time and date
Alter Table dbo.hourlyCalories_merged
ADD time_new int, date_new DATE;

Update dbo.hourlyCalories_merged
Set time_new = DATEPART(hh, ActivityHour);

Update dbo.hourlyCalories_merged
Set date_new = CAST(ActivityHour AS DATE);

```
```

--Preview Data in hourlyIntensities_merged
SELECT *
FROM DBO.hourlyIntensities_merged

```
```

--Modify hourlyIntensities_merged by splitting Time and date
Alter Table DBO.hourlyIntensities_merged
ADD time_new int, date_new DATE;

Update DBO.hourlyIntensities_merged
Set time_new = DATEPART(hh, ActivityHour);

Update DBO.hourlyIntensities_merged
Set date_new = CAST(ActivityHour AS DATE);

```
```

--Preview Data in hourlySteps_merged
SELECT *
FROM DBO.hourlySteps_merged

```
```

--Modify hourlySteps_merged by splitting Time and date
Alter Table DBO.hourlySteps_merged
ADD time_new int, date_new DATE


Update DBO.hourlySteps_merged
Set time_new = DATEPART(hh, ActivityHour)


Update DBO.hourlySteps_merged
Set date_new = CAST(ActivityHour AS DATE)

```
```

--Preview Data in minuteMETsNarrow_merged
SELECT *
FROM DBO.minuteMETsNarrow_merged

```
```

--Modify minuteMETsNarrow_merged by splitting Time and date
Alter Table DBO.minuteMETsNarrow_merged
ADD time_new TIME, date_new DATE

Update DBO.minuteMETsNarrow_merged
Set time_new = CAST(ActivityMinute as time)

Update DBO.minuteMETsNarrow_merged
Set date_new = CAST(ActivityMinute AS DATE)

```


# Analyze
Now that our data is stored appropriately and has been prepared for analysis, let's start putting it to work.


```

--Daily Average Analysis
SELECT AVG(TotalSteps) as avg_steps,
AVG(TotalDistance) as avg_distance,
AVG(Calories) as avg_calories,
day_of_week
FROM dbo.dailyActivity_merged
GROUP BY  day_of_week

```

### A daily average analysis based on the provided smart device usage data, we can calculate the average values for relevant metrics for each day. 

<img width="299" alt="Daily Average Analysis" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/c84075de-d2a9-4c08-85b5-89624a6ea90f">

#### During which hour of the day were the more calories burned?
 It provides insights into how users engage with their devices over time.

<img width="484" alt="Average Calories Burned per hour" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/938b1bcd-bca5-47e4-a32b-6926fdd32fa4">


-From the graph above, we can see that the most desired time people are active throughout the day is between 8:00 AM - 7:00PM

#### Compare Different Metrics 
Duration of each Activity and Calories Burned Per User

```

Select Id,
SUM(TotalSteps) as total_steps,
SUM(VeryActiveMinutes) as total_very_active_mins,
Sum(FairlyActiveMinutes) as total_fairly_active_mins,
SUM(LightlyActiveMinutes) as total_lightly_active_mins,
SUM(Calories) as total_calories
From [dbo].[dailyActivity_merged]
Group By Id

```




<img width="452" alt="Activities and Calories Comparison" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/f58442f6-ae57-4040-bc90-bc50c1bf2838">



#### Activity Time and Calories Burned





<img width="315" alt="Steps Vs Calories" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/d5eb556e-7910-4816-965f-dab1fc30ebb9">


The total steps vary significantly across users, ranging from as low as 12,352 to as high as 702,840. This indicates diverse levels of physical activity among users.






<img width="321" alt="Very Active Mins Vs Calories" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/77ac6ff7-ab2f-4773-8745-8dbfa386721b">



Users spend varying amounts of time in very active minutes. Some users have high values, suggesting they engage in intense physical activities, while others have lower values.








<img width="309" alt="Fairly Active Mins Vs Calories" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/643b3709-a1f0-46bd-8115-bef9be5119a7">



 Some users have a significant portion of their activity categorized as fairly active, indicating a moderate level of intensity.








<img width="285" alt="Lightly Active Mins Vs Calories" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/0885428f-baa0-45e7-a872-b4e7eb943bd4">



The distribution of lightly active minutes also varies among users. This metric may include activities such as walking or light exercises.

+ The total calories burned during the tracked activities show a wide range, from 33,972 to 106,534. This reflects differences in energy expenditure based on the intensity and duration of physical activities.

+ Users with higher total steps, very active minutes, and total calories burned can be considered more engaged in physical activities tracked by the device. Conversely, users with lower values may have a less active lifestyle.
  
+ Some users exhibit balanced activity patterns with significant time spent in very active, fairly active, and lightly active minutes, contributing to a higher total calorie burn.
  
+ The data highlights the diversity in user profiles and activity levels. Understanding these differences can help Bellabeat tailor its products and marketing strategies to cater to a wide range of user needs and preferences.

+ Potential target groups based on activity patterns can be identified. For example, users with a high focus on very active minutes may be interested in fitness-oriented features, while users with more balanced activity may benefit from holistic wellness offerings.

+ These insights provide a foundation for further analysis and can guide decision-making in terms of product development, marketing strategies, and user engagement for Bellabeat.


 #### Average Steps Per Hour

This query calculates the average number of steps for each hour, providing insights into the typical step count at different times of the day.


 ```

-- Average Steps per Hour
SELECT
    AVG(StepTotal) as avg_steps,TIME_NEW   
FROM
    [Bellabeat].[dbo].[hourlySteps_merged]
GROUP BY
 	TIME_NEW
ORDER BY
   	TIME_NEW

```





<img width="436" alt="Average Steps per hour" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/613e12e8-ccdb-4b87-b48a-78845e4417be">






+ The average number of steps varies across different hours of the day, indicating distinct patterns in user activity.
+ During the early morning hours (12 AM to 6 AM), the average steps are relatively low, suggesting that users tend to be less active during these hours, possibly due to sleeping.
+ There is a noticeable increase in average steps from 6 AM to 10 AM, indicating a peak in activity during the morning hours. This could be attributed to activities such as morning walks or commutes.
+ The average steps remain relatively consistent from 10 AM to 2 PM, suggesting that users maintain a certain level of activity during this midday period.
+ The average steps decrease again in the late evening, suggesting reduced activity during the nighttime hours.
+ Peak hours of user activity can be identified to target specific time slots for engagement, promotions, or notifications related to wellness activities.
+ These insights provide a temporal understanding of user activity patterns throughout the day, allowing Bellabeat to tailor its product features or marketing strategies to align with users' daily routines.


###  Metrics Comparison Over Time

This query can help understand if there are consistent patterns or if certain metrics are more influential in different periods.
```

-- Metrics Comparison Over Time
SELECT
    AVG(TotalSteps) as avg_steps,
    AVG(TotalDistance) as avg_distance,
    AVG(Calories) as avg_calories,
    ActivityDate
FROM
    dbo.dailyActivity_merged
GROUP BY
    ActivityDate
ORDER BY
    ActivityDate

```






<img width="529" alt="Metrics Comparison Over Time" src="https://github.com/DeepaliSukhdeve/Data-Driven-Wellness/assets/145950963/32a92d0d-bccd-4097-8050-5ac758427d58">












