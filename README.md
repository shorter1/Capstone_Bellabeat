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

```
ggplot(data = merged_data, aes(x=(TotalMinutesAsleep/60), y=TotalSteps))+
geom_point()+
geom_smooth()+
labs(title = "Total Steps vs Total Sleep")
```
![image](https://user-images.githubusercontent.com/112402643/216103029-c9ad303f-7065-40a5-b1df-390f30e2c4ff.png)

This shows that participants that slept between 5-8 hours were more likly to achieve 10,000 steps. 
worthy note: if you slept more then 10 hours you were less active than any other group 

```{r}
int_new <- hourly_int_clean %>%
  group_by(activity_time) %>%
  drop_na() %>%
  summarise(average_intensity = mean(total_intensity))

view(int_new)
```

```{r}
ggplot(data=int_new, aes(x=activity_time, y=average_intensity))+ 
  geom_histogram(stat="identity", fill= 'darkblue')+
  theme(axis.text.x = element_text(angle = 45))
```
![image](https://user-images.githubusercontent.com/112402643/216103451-7c3435d7-f116-4231-9775-1bf5034800a5.png)

for this we are looking at the average intensity and when participants are likely to conduct their workouts/activity during a given 24 hours.
Two major time frames to focus on lunch time and after work 5-8pm.

```{r}
int_id <- hourly_int_clean %>%
  group_by(Id) %>%
  drop_na() %>%
  summarise(avg_intensity = mean(total_intensity))

view(int_id)

sleep_id <- merged_data %>%
  group_by(Id) %>%
  drop_na() %>%
  summarise(avg_sleep = mean(TotalMinutesAsleep))

view(sleep_id)

sleep_int_merge <- merge(sleep_id, int_id, by=('Id'))

view(sleep_int_merge)

ggplot(data = sleep_int_merge, aes(x=avg_sleep,y=avg_intensity,color=Id))+
  geom_point()+ 
  geom_smooth()
```
![image](https://user-images.githubusercontent.com/112402643/216103675-4da61226-2cfb-417f-a2a2-ea8187ab52a1.png)

even tho members that obtain 5-8 hours of sleep are more active member that only get 5-6 have more intense activity 

```{r}
  
  hist_int_new <- hourly_int_clean %>%
  group_by(activity_time) %>%
  drop_na() %>%
  summarise(mean_total_int = mean(total_intensity))

ggplot(data = hist_int_new, aes(x=activity_time, y=mean_total_int))+
  geom_histogram(stat = "identity",fill='darkblue')+ 
  theme(axis.text.x = element_text(angle = 45))+
  labs(title = "Average Total Intensity vs Time")  
```
![image](https://user-images.githubusercontent.com/112402643/216103831-9e7629b0-738b-4080-a7b2-0ae3f5a6818c.png)

Taking a look at the average total intensity over time of all participants to see if there is a specific time of day that participants are more likely to workout at.  


```{r}

  new_daily_activity <- daily_activity_clean %>%
  group_by(Date) %>%
  drop_na() %>%
  summarise(total_act_time = sum(VeryActiveMinutes,FairlyActiveMinutes,LightlyActiveMinutes),total_vam = sum(VeryActiveMinutes), total_fam = sum(FairlyActiveMinutes), total_lam = sum(LightlyActiveMinutes))
```


```{r}
ggplot(data = new_daily_activity, aes(x=Date, y=total_act_time))+
  geom_histogram(stat = "identity",fill='darkblue')+ 
  theme(axis.text.x = element_text(angle = 90))+
  labs(title = "Total Active Time Per Day")
```

![image](https://user-images.githubusercontent.com/112402643/216104012-f07dcef1-1a97-4542-aed1-bb26fa0973ee.png)

Taking a look at the average total intensity per day of all participants to see if there is a specific day in the week that participants are more likely to workout at. 

```{r}
ggplot(data = new_daily_activity, aes(x=Date, y=total_vam))+
  geom_histogram(stat = "identity",fill='darkred')+ 
  theme(axis.text.x = element_text(angle = 90))+
  labs(title = "Total Very Active Time Per Day")
```
![image](https://user-images.githubusercontent.com/112402643/216104135-a726c998-46a4-4f7d-b3b7-1b91be251f82.png)


```{r}
ggplot(data = new_daily_activity, aes(x=Date, y=total_fam))+
  geom_histogram(stat = "identity",fill='darkorange')+ 
  theme(axis.text.x = element_text(angle = 90))+
  labs(title = "Total Fairly Active Time Per Day")
```
![image](https://user-images.githubusercontent.com/112402643/216104236-cd6cfb9b-935e-44bf-9d09-2b7a566f81f1.png)


```{r}
ggplot(data = new_daily_activity, aes(x=Date, y=total_lam))+
  geom_histogram(stat = "identity",fill='lightblue')+ 
  theme(axis.text.x = element_text(angle = 90))+
  labs(title = "Total lightly Active Time Per Day")
```
![image](https://user-images.githubusercontent.com/112402643/216104292-e864f6fa-2d71-4923-8768-efd728497088.png)

Based on the above data the majority of participants partake in lightly active activity. These are defined by regular daily activity and walks. followed by very active activity and then fairly active activity. 

```{r}
sum_activity <- daily_activity_clean%>%
  summarise(light_active = sum(LightlyActiveMinutes), fairly_active = sum(FairlyActiveMinutes),very_active = sum(VeryActiveMinutes))
 
view(sum_activity)         
```

```{r}
df <- data.frame(
     activity = c("light_active","fairly_active", "very_active"),
     value = c(181244, 12751, 19895))

view(df)
```
     
```{r}
blank_theme <- theme_minimal()+
  theme(
    axis.title.x = element_blank(),
    axis.title.y = element_blank(),
    panel.border = element_blank(),
    panel.grid=element_blank(),
    axis.ticks = element_blank(),
    plot.title=element_text(size= 15, face="bold"))
```

```{r}
ggplot(df, aes(x=1, y=value, fill=activity)) +
  geom_col() +
  scale_fill_brewer("Activity")+
  geom_label_repel(aes(label =percent(value/sum(value), size=5)), position = position_stack(vjust = 0.5), show.legend = FALSE)+
  coord_polar(theta = "y") + 
  theme_void()
```
![image](https://user-images.githubusercontent.com/112402643/216104501-7b9d2a9b-b97d-40e9-9ad3-24e6c22e8b2f.png)

above chart shows that the majority (excluding sedentary activity) is light activity followed by very active activity and close behind that is fairly active activity. It is a good thing to note that sedentary activity is the largest type of activity recorded and was excluded to show a better comparison for the other three types of activity. 


```{r}
dfs <- data.frame(
  activity = c("sedentary", "light_active", "fairly_active", "very_active"),
  value = c(931738, 181244, 12751, 19895))

```

```{r}
head(dfs)

ggplot(dfs, aes(x=1, y=value, fill=activity)) +
  geom_col() +
  scale_fill_brewer("Activity")+
  geom_label_repel(aes(y=value, label =percent(value/sum(value), size=5)), position = position_stack(vjust = 0.5), show.legend = FALSE)+
  coord_polar(theta = "y") + 
  theme_void()
```
![image](https://user-images.githubusercontent.com/112402643/216104701-605f4461-3b91-4118-9daf-d26cb693276b.png)





## Step 5: SHARE

## Step 6: ACT
