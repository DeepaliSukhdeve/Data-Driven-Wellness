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
The dataset, obtained from Kaggle, has been imported into  Microsoft SQL Server Management Studio to help process and analyze 
For visualization purposes, I've used Tableau Public, employing its dynamic features to create compelling visual representations of the analyzed data. 
Considerations about the Dataset:
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

--Preview the dataset 
SELECT *
FROM dbo.dailyActivity_merged



