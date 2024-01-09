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
```




