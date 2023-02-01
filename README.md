# Capstone_Bellabeat

### INTRODUCTION

This case stud is the Capstone Project for the Google Data Analytics Professional Certificate. I will be using the ***6 steps of Data Analysis*** to present this data.


Title: ****Bellabeat Case Study****

Author: Caleb Shorter

Date: September, 2022

![Unknown](https://user-images.githubusercontent.com/112402643/200191189-c222e396-7442-439e-a891-3224f71c079c.png)

## Table of Contents
- [Step 1: ASK](#step-1-ask)
- [Step 2: PREPARE](#step-2-prepare)
- [Step 3: PROCESS](#step-3-process)
- [Step 4: ANALYZE](#step-4-analyze)
- [Step 5: SHARE](#step-5-share)
- [Step 6: ACT](#step-6-act)

## Step 1: ASK

***1.0 Backgroud***

Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that manufactures health-focused smart products.
Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around
the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with
knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly
positioned itself as a tech-driven wellness company for women.
By 2016, Bellabeat had opened offices around the world and launched multiple products. Bellabeat products became available
through a growing number of online retailers in addition to their own e-commerce channel on their website. The company
has invested in traditional advertising media, such as radio, out-of-home billboards, print, and television, but focuses on digital
marketing extensively. Bellabeat invests year-round in Google Search, maintaining active Facebook and Instagram pages, and
consistently engages consumers on Twitter. Additionally, Bellabeat runs video ads on Youtube and display ads on the Google
Display Network to support campaigns around key marketing dates.

***1.1 Business Tasks***

Analyze smart device usage data in order to gain insight into how people are already using their smart devices. Then, using this information, she would like high-level recommendations for how these trends can inform Bellabeat marketing strategy.

Sršen asks you to analyze smart device usage data in order to gain insight into how consumers use non-Bellabeat smart
devices. She then wants you to select one Bellabeat product to apply these insights to in your presentation.

***1.2 Objectives***

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

***1.3 Deliverables***

1. A clear summary of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Your top high-level content recommendations based on your analysis

***1.4 Key Stakeholders***

- Urška Sršen and Sando Mur co-founders of Bellabeat
- Bellabeats Marketing and analytics team

## Step 2: PREPARE

***2.0 Dataset Information***

- There are 18 CSV files that are available from FitBit Fitness Tracker Data stored on a Public Domain that are made available by Mobius.
- The data set on Kaggle contains personal fitness tracking data from 30 Fitbit users who have consented to have their personal data/information tracked by their fitbit.
- The Data is mostly in wide formate and is organized by user ID  
- Data was sampled in 2016
- The data doesn't doesn't track everything that bellabeats products track. 
- Sample size is small lowing the validity of our findings 
- Not all participants tracked sleep, so for an already small sample size sleep is even smaller 

***2.1 Data Limitations***

1. Data is out of date
2. Data does not cover all the offerings that Bellabeats tracks
3. Data was obtained from a third party source 
4. Data does not specify male vs female participants 


**_R.O.C.C.C._**

- Reliable - is limited. population size is to small to determine true trends 
- Original - unclear
- Comprehensive - This is not very comprehensive. Data is limited to activity while bellabeats focus on womens health. Fitbit only focuses on activity and sleep.
- Current - out of date. These are taken from participants in 2016
- Cited - unclear
 
I will be using both excel and R to clean, explore, process, and visualize the datasets. 

## Step 3: PROCESS

Excel cleaning/modifcations:
- split date and time in one cell to multiple cells 
- cleaned data to ensure no added spaces, null values, duplicate entries
- checked unique user ID in each csv for accuracy 
- change all column names to lowercase 
- change names of all date columns to date vs activity_date, sleedday
- removed sleep time and left it as just the date 

## Step 4: ANALYZE

**load clean data to R:**
```
daily_activity_clean <- read.csv("Fitabase Data 4.12.16-5.12.16/daily_activity_clean.csv")
sleep_daily_clean <- read.csv("Fitabase Data 4.12.16-5.12.16/sleep_day_clean.csv")
hourly_int_clean <- read.csv("Fitabase Data 4.12.16-5.12.16/hourly_int_clean.csv")
sleep_day_clean <- read_csv("Fitabase Data 4.12.16-5.12.16/sleep_day_clean.csv")
2
```

**Merge Data**
```
merged_data <- merge(daily_activity_clean, sleep_day_clean, by = c('Id','Date'))
```

```ggplot(data = merged_data, aes(x=(TotalMinutesAsleep/60), y=TotalSteps))+
geom_point()+
geom_smooth()+
labs(title = "Total Steps vs Total Sleep")
```
## Step 5: SHARE

## Step 6: ACT
