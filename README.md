# Cyclistic Bike Share : Case study with R
## Rounak Saha
### 2024-07-18
  
# Introduction 
This is a capstone project as a part my [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics) course. In this project i have used R programming language and [R studio IDE](https://posit.co/download/rstudio-desktop/) as a data analysis tool, for the simplicity of its data analysis and data visualization.

**For this project following steps should be followed:**

* Ask
* Prepare
* Process
* Analyze 
* Share 
* Act 

### About the company
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are
geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to
any other station in the system anytime.
Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One
approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and
annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who
purchase annual memberships are Cyclistic members.

#### Scenario 
In the given scenario, I am a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company's future success depends on maximizing the number of annual memberships. Therefore, my
team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, our team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve my
recommendations, so they must be backed up with compelling data insights and professional data visualizations.

My report will include the following deliverables:

* A clear summary of the business task
* A description of all data sources used
* Documentation of any cleaning or manipulation of data
* A summary of your analysis
* Supporting visualizations and key findings
* Your top high-level content recommendations based on your analysis

## Step 1: Ask 
#### Business Task
Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

#### Key stakeholders
* **Cyclistic:** A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart
by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities
and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use
the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each
day.
* **Lily Moreno:** The director of marketing and your manager. Moreno is responsible for the development of campaigns and
initiatives to promote the bike-share program. These may include email, social media, and other channels.
* **Cyclistic marketing analytics team:** A team of data analysts who are responsible for collecting, analyzing, and reporting
data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about
Cyclistic’s mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them.
* **Cyclistic executive team:** The notoriously detail-oriented executive team will decide whether to approve the
recommended marketing program.

#### Questions to explore for the analysis 
* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?

## Step 2: Prepare 
I will use Cyclistic’s historical trip data (July 2023 to June 2024) to analyze and identify trends. The data has been made available by Motivate International Inc. under the [license](https://divvybikes.com/data-license-agreement). The dataset is available [here](https://divvy-tripdata.s3.amazonaws.com/index.html).

The data set contains 12 CSV files organized in long format. Below is a breakdown of the data using the ROCCC approach:

* **Reliable**- Yes, not biased, but not sure if vetted, as its given by a certification course 
* **Original**- Yes, located at original public data
* **Comprehensive**- Yes, not missing any important information 
* **Current**- Yes, the data is updated monthly 
* **Cited**- Yes

## Step 3: Process
First, we will install and load the required packages 

```{r, echo = TRUE, eval = TRUE, results="hide", fig.keep = "none"}
## Installing the requird packages
install.packages("tidyverse", repos="https://cloud.r-project.org/")
install.packages("readr", repos="https://cloud.r-project.org/")
install.packages("dplyr", repos="https://cloud.r-project.org/")
install.packages("tidyr", repos="https://cloud.r-project.org/")
install.packages("ggplot2", repos="https://cloud.r-project.org/")
install.packages("lubridate",repos="https://cloud.r-project.org/")
install.packages("geosphere",repos="https://cloud.r-project.org/")
install.packages("ggmap",repos="https://cloud.r-project.org/")
install.packages("sqldf",repos="https://cloud.r-project.org/")
install.packages("scales", repos="https://cloud.r-project.org/")
```
```{r, echo = TRUE, eval = TRUE, results="hide", fig.keep = "none"}
## Loading the packages
library(tidyverse)
library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
library(lubridate)
library(geosphere)
library(ggmap)
library(sqldf)
library(scales)
```
We will now load the dataframes using `read_csv()` function 
```{r}
## Import data into R Studio 
jul23 <- read_csv("data-June2023:June2024/202307-divvy-tripdata.csv")

aug23 <- read_csv("data-June2023:June2024/202308-divvy-tripdata.csv")

sep23 <- read_csv("data-June2023:June2024/202309-divvy-tripdata.csv")

oct23 <- read_csv("data-June2023:June2024/202310-divvy-tripdata.csv")

nov23 <- read_csv("data-June2023:June2024/202311-divvy-tripdata.csv")

dec23 <- read_csv("data-June2023:June2024/202312-divvy-tripdata.csv")

jan24 <- read_csv("data-June2023:June2024/202401-divvy-tripdata.csv")

feb24 <- read_csv("data-June2023:June2024/202402-divvy-tripdata.csv")

mar24 <- read_csv("data-June2023:June2024/202403-divvy-tripdata.csv")

apr24 <- read_csv("data-June2023:June2024/202404-divvy-tripdata.csv")

may24 <- read_csv("data-June2023:June2024/202405-divvy-tripdata.csv")

jun24 <- read_csv("data-June2023:June2024/202406-divvy-tripdata.csv")
```

Examining each of the datasets
```{r}
glimpse(jul23)
glimpse(aug23)
glimpse(sep23)
glimpse(oct23)
glimpse(nov23)
glimpse(dec23)
glimpse(jan24)
glimpse(feb24)
glimpse(mar24)
glimpse(apr24)
glimpse(may24)
glimpse(jun24)
```

Column names for all the dataframes are consistent and are in correct data format.
Now, combining 12 dataframes into one large dataframe
```{r}
trip_data <- rbind(jul23,aug23,sep23,oct23,nov23,dec23,jan24,feb24,mar24,apr24,may24,jun24)

# Removing individual dataframes 
rm(jul23,aug23,sep23,oct23,nov23,dec23,jan24,feb24,mar24,apr24,may24,jun24)
```

Check the structure and get a glimpse of the consolidated data 
```{r}
glimpse(trip_data)

str(trip_data,give.attr = FALSE)

head(trip_data)
```

```{r}
# Check rideable_type and member_casual column for any discrepancies 

unique(trip_data$rideable_type)
unique(trip_data$member_casual)
```

```{r}
# Check for duplicates in ride_id 
sum(duplicated(trip_data$ride_id))

# Removing duplicates 
trip_data <- trip_data[!duplicated(trip_data$ride_id),]
```

```{r}
# Check for NA's
sum(is.na(trip_data))

# Removing NA's
trip_data <- drop_na(trip_data)
```

```{r}
# Checking for test stations
unique(trip_data$start_station_name[grep("TEST", trip_data$start_station_name)])
unique(trip_data$start_station_name[grep("Test", trip_data$start_station_name)])
```

Adding column for Month, Year, day of week, hour of ride, ride length, ride distance 
```{r}
# ride length 
trip_data$ride_length <- round(difftime(trip_data$ended_at, trip_data$started_at, units = "mins"),1)

# Month, Year, Day of Week, Hour of ride
trip_data <- trip_data %>% 
  mutate(month = format(as.Date(started_at), "%B")) %>% 
  mutate(year = format(as.Date(started_at), "%Y")) %>% 
  mutate(day_of_week = format(as.Date(started_at), "%A")) %>% 
  mutate(hour = format(as_datetime(started_at), "%H"))

# Calculate the ride distance in Kms
trip_data <- trip_data %>% 
  rowwise() %>% 
  mutate(ride_distance = distGeo(c(start_lng,start_lat), c(end_lng,end_lat))/ 1000)

# Convert ride length to numeric 
trip_data$ride_length <- as.numeric(trip_data$ride_length) 
is.numeric(trip_data$ride_length)
```

```{r}
# Removing the trips where ride_length <= 0 or more than 24hrs (24*60= 1440 mins)
trip_data <- trip_data[!(trip_data$ride_length <= 0 | trip_data$ride_length> 1440),]
```

## Step 4: Analyze 
Now, all the required information are in one place to begin our analysis phase

```{r}
# Lets first check our cleaned dataframe
str(trip_data, give.attr = FALSE)
head(trip_data)
summary(trip_data)
```

Conduct descriptive analysis:
Compare Members vs casual on total number of rides taken 
```{r}
trip_data %>% 
  group_by(member_casual) %>% 
  summarise(no_of_rides = n(), ride_percentage = (no_of_rides = n()/ nrow(trip_data))*100)
```

Descriptive analysis of ride length(Min,Max,Median,Mean)
```{r}
# min = shortest ride in mins
aggregate(trip_data$ride_length ~ trip_data$member_casual, FUN = min) 
# max = longest ride in mins
aggregate(trip_data$ride_length ~ trip_data$member_casual, FUN = max)
# median = midpoint of ride length array in asc order
aggregate(trip_data$ride_length ~ trip_data$member_casual, FUN = median) 
# mean= straight average of ride length in mins 
aggregate(trip_data$ride_length ~ trip_data$member_casual, FUN = mean)
```

Next, we need to compare the average ride length and number of rides takes by members and casuals on each day of the week
```{r}
# We need to order the day of week first 
trip_data$day_of_week <- ordered(trip_data$day_of_week, 
                                 levels = c("Monday", "Tuesday", "Wednesday", 
                                            "Thursday", "Friday", "Saturday", "Sunday"))

# Compare the average ride length and number of rides takes by members and casuals 
#on each day of the week
trip_data %>% 
  group_by(member_casual, day_of_week) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  arrange(day_of_week)
```

Next, we need to compare the average ride length and number of rides taken by members and casuals on each Month and Year 
```{r}
# We need to order the month first 
trip_data$month <- ordered(trip_data$month, 
                           levels = c("July","August","September","October",
                                      "November","December","January","February",
                                      "March","April","May","June"))

# Average ride length and number of rides taken each month 
trip_data %>% 
  group_by(member_casual, month) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  arrange(month)

# Average ride length and number of rides taken each Year
trip_data %>% 
  group_by(member_casual, year) %>% 
  summarise(no_of_rides_taken = n()) %>% 
  arrange(year)
```

Next, we need to compare the number of rides takes by members and casuals with each type of bike 
```{r}
# Compare the average ride length and number of rides 
#takes by members and casuals with each type of bike 
trip_data %>% 
  group_by(member_casual, rideable_type) %>% 
  summarise(no_of_rides_taken = n()) %>% 
  arrange(rideable_type,member_casual)
```

Comparison between Members and Casual riders depending on ride distance
```{r}
# Avg ride distance in kms
trip_data %>% 
  group_by(member_casual) %>% 
  summarise(average_ride_distance = mean(ride_distance))
```

Analyzing the top 10 most travelled routes for Member and Casual riders
```{r}
trip_data_v1 <- trip_data %>% 
  unite("most_common_travel_route", start_station_name,end_station_name, sep = "--")

#top 10 most travelled routes for Member
r_member <- sqldf("SELECT most_common_travel_route, count (ride_id) AS total_number_of_ride
                  FROM trip_data_v1
                  WHERE member_casual = 'member'
                  GROUP BY most_common_travel_route
                  ORDER BY total_number_of_ride DESC
                  LIMIT 10")
r_member

#top 10 most travelled routes for Casual riders
r_casual <- sqldf("SELECT most_common_travel_route, count (ride_id) AS total_number_of_ride
                  FROM trip_data_v1
                  WHERE member_casual = 'casual'
                  GROUP BY most_common_travel_route
                  ORDER BY total_number_of_ride DESC
                  LIMIT 10")
r_casual
```

## Step 5: Share 
In this step we will visualize the trends and relationship between different variables 

Viz 1- Total number of rides taken by member and casual riders 

```{r}
mindate<- as.Date(min(trip_data$started_at))
maxdate<- as.Date(max(trip_data$started_at))

# Rides Distribution: Casual vs Member
trip_data %>% 
  group_by(member_casual) %>% 
  summarise(no_of_rides = n(), 
            ride_percentage = (no_of_rides = n()/ nrow(trip_data))*100) %>% 
  ggplot() + geom_col(mapping = aes(x= member_casual, 
                                    y = no_of_rides, 
                                    fill = member_casual),width=0.6)+ 
  theme_classic()+
  scale_fill_manual(values = c("casual"= "#99dcc1", "member"= "#B6D5EB"))+
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))+ 
  labs(x= '', y= 'Number of Rides' , 
       title = 'Rides Distribution: Casual vs Member', 
  caption = paste("Data from:", mindate, "to", maxdate)) + 
  geom_text(aes(x= member_casual, y = no_of_rides, 
                label = percent(ride_percentage/100), hjust= 0.5))+
   coord_flip()  
```

* We can see on the member vs casual ride distribution chart, 65% of the rides are taken by members and 35% of the rides were taken by casual rider. So its clearly visible that in July 23 to June 24, members used ride share ~30% more than the casual riders  

Viz 2 & 3- Average ride duration and total number of rides taken per day between member and casual riders

```{r}
# Average ride length per day
trip_data %>% 
  group_by(member_casual, day_of_week) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  ggplot() + geom_col(mapping = aes(x= day_of_week, y= average_ride_length, 
                                    fill= member_casual),position = "dodge", 
                      width = 0.6)+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= '', y= 'Average Duration (mins)' , 
       title = 'Average Duration of Rides per Day: Member Vs Casual', 
  caption = paste("Data from:", mindate, "to", maxdate), fill= "Member/Casual")

# Number of rides taken per day 
trip_data %>% 
  group_by(member_casual, day_of_week) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  ggplot() + geom_col(mapping = aes(x= day_of_week, y= no_of_rides_taken, 
                                    fill= member_casual),position = "dodge", width = 0.6)+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= '', y= 'No of Rides' , title = 'Rides per Day: Casual vs Member', 
  caption = paste("Data from:", mindate, "to", maxdate), fill= "Member/Casual")+
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

* From the first chart above, average ride duration for casual riders are much more than the members. It can be seen weekend rides has more duration is higher for casual riders. It is also seen a consistent ride duration for the member through the week (having less than 15 mins)
* Members took more rides on weekdays, starting a decline on weekends. Whereas, the casual riders took rides more on Friday, Saturday & Sunday than the other days. 

Viz 4 & 5- Average ride duration and total number of rides taken each month between member and casual riders

```{r}
# Number of rides taken each month 
trip_data %>% 
  group_by(member_casual, month) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  ggplot() + geom_col(mapping = aes(x= month, y= no_of_rides_taken, 
                                    fill = member_casual), position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  theme(axis.text.x = element_text(angle = 45, hjust = 0.55, vjust = 0.65))+
  labs(x= "", y= "No of Rides", 
       title = "Rides per Month: Member Vs Casual",
       fill= "Member/Casual", caption =  paste("Data from:", mindate, "to", maxdate))+
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))

# Average ride length each month 
trip_data %>% 
  group_by(member_casual, month) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  ggplot() + geom_col(mapping = aes(x= month, y= average_ride_length, 
                                    fill = member_casual), position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  theme(axis.text.x = element_text(angle = 45, hjust = 0.55, vjust = 0.65))+
  labs(x= "", y= "Average Duration (mins)", 
       title = "Average Duration of Rides per Month: Member Vs Casual", 
       fill= "Member/Casual", caption = paste("Data from:", mindate, "to", maxdate))
```

* From the first viz, we can see the number of rides taken by both is decreasing from July to Jan and increasing from Feb to June, with member have more rides than casual riders. It is possible due to winter there is significant drop in the rides.
* Members still have a ride duration less than 15 mins. The trend of the chart is similar to the previous chart.

Viz 6- Total number of rides taken per year between member and casual riders

```{r}
# Number of rides taken each Year
trip_data %>% 
  group_by(member_casual, year) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  arrange(year) %>% 
  ggplot() + geom_col(mapping = aes(x= year, y= no_of_rides_taken, 
                                    fill = member_casual), width = 0.5, position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= "", y= "No of Rides", title = "Rides per Year: Member Vs Casual", 
       fill= "Member/Casual", caption = paste("Data from:", mindate, "to", maxdate))+
  coord_flip()  
```

* Overall rides taken in 2023 was more than the total number of rides taken in 2024, with member having more ride share than casual riders.

Viz 7- Total number of rides taken per hour between member and casual riders

```{r}
# Count of Hourly Rides 
trip_data %>% 
  group_by(member_casual, hour) %>% 
  summarise(average_ride_length = mean(ride_length), no_of_rides_taken = n()) %>% 
  ggplot() + geom_col(mapping = aes(x= hour, y= no_of_rides_taken, fill = member_casual), position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= "Hour of the day", y= "Count of Rides", 
       title = "Count of Hourly Rides: Member Vs Casual", fill= "Member/Casual", 
       caption = paste("Data from:", mindate, "to", maxdate))+
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

* From the above graph, we can see that the number of rides taken increases from 10am to 5pm for casual riders. Also there is a bigger volume rise from 3pm to 6pm for the member. This need to check further on daily basis. 

Viz 8- Hourly ride distribution each weekday between member and casual riders

```{r}
#Analysis and visualization on cyclistic's bike demand per hour by day of the week
trip_data %>% 
  ggplot() + geom_bar(mapping = aes(hour, fill = member_casual), position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= "Hour of the day", y= "Count of Rides", 
       title = "Cyclistic's bike demand per hour by day of the week", fill= "Member/Casual", 
       caption = paste("Data from:", mindate, "to", maxdate))+
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))+
  facet_wrap(~day_of_week)+ theme(axis.text.x = element_text(angle = 90, size = 5, face = "bold"))
```

* There is alot of difference between the weekdays and weekends. A big volume of rides increase on weekdays from 6am to 9am and another big volume increase from 4pm to 7pm. This can be due to riders going to office in the morning and returning back at the evening. Whereas, on weekends most of the rides are taken between 9am to 6pm, from this we can hypothesize that both the riders use their bike share for leisure purpose on weekends.  

Viz 9- Comparison between Members and Casual riders depending on ride distance

```{r}
# Avg ride distance in kms
trip_data %>% 
  group_by(member_casual) %>% 
  summarise(average_ride_distance = mean(ride_distance)) %>% 
   ggplot() + geom_col(mapping = aes(x= member_casual, y= average_ride_distance, 
                                     fill = member_casual), width = 0.5, position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= "", y= "Distance (Km)", 
       title = "Mean travel distance by Members and Casual riders", 
       fill= "Member/Casual", caption = paste("Data from:", mindate, "to", maxdate))+
  geom_text(aes(x= member_casual, y = average_ride_distance, 
                label = round(average_ride_distance,2), vjust = -0.5))
```

* No major difference in distance can be seen between members and casual riders.

Viz 10- Usage of different bike by rider type 

```{r}
trip_data %>% 
  group_by(member_casual,rideable_type) %>% 
  summarise(no_of_rides_taken = n()) %>% 
  arrange(rideable_type,member_casual) %>% 
  ggplot() + geom_col(mapping = aes(x= rideable_type, y= no_of_rides_taken, 
                                    fill = member_casual), width = 0.5, position = "dodge")+
  theme_classic()+ scale_fill_manual(values = c("casual"= "#005682", "member"= "#B6D5EB"))+
  labs(x= "", y= "No of Rides", title = "Rides per Year: Member Vs Casual", 
       fill= "Member/Casual", caption = paste("Data from:", mindate, "to", maxdate))+
  coord_flip()
```

* From the above chart we can see, members and casual riders mostly use classic bikes, followed by electric bikes. Docked bikes are only used by casual riders.

Viz 11- Top 10 Station routes taken by Casual Riders 

```{r}
# top 10 routes for casual riders
ggplot(r_casual)+ geom_col(mapping = aes(x= reorder(most_common_travel_route,
                                                    total_number_of_ride), 
                                         y= total_number_of_ride),fill = "#004328")+ 
  coord_flip()+ theme_classic()+
  labs(x= "", y= "No of Rides", title = "Top 10 Station Routes for Casual Riders", 
       fill= "Member/Casual", caption = paste("Data from:", mindate, "to", maxdate))+
  scale_x_discrete(labels = wrap_format(40))
```

* From the above viz, Streeter Dr & Grand Ave--Streeter Dr & Grand Ave is the most popular route for the casual riders.

Viz 12- Top 10 Station routes taken by members 

```{r}
# top 10 routes for members
r_member %>% 
 ggplot()+ geom_col(mapping = aes(x= reorder(most_common_travel_route,total_number_of_ride), 
                                  y= total_number_of_ride),fill = "#5d495d")+ 
  coord_flip()+ theme_classic()+
  labs(x= "", y= "No of Rides", title = "Top 10 Station Routes for Members", 
       fill= "Member/Casual", caption = paste("Data from:", mindate, "to", maxdate))+
   scale_x_discrete(labels = wrap_format(40))
```

* From the above viz, Calumet Ave & 33rd St--State St & 33rd St is the most popular route taken by members

Key findings:

* Members hold biggest proportion of total rides, ~30% more than the casual riders.
* Casual riders have more number of rides on weekends, whereas, members have more on the weekdays. The avg ride duration for casual is significantly more than the members. And avg ride time increases slightly on the weekends.
* Number of rides taken and ride duration decreases during cold season. 
* We have more rides from 6am to 9am and  4pm to 7pm on weekdays. On weekdays the maximum rides are taken from 9am to 6pm.
* Top 3 routes taken by casual riders are 1) Streeter Dr & Grand Ave--Streeter Dr & Grand Ave	2) DuSable Lake Shore Dr & Monroe St--DuSable Lake Shore Dr & Monroe St 3) DuSable Lake Shore Dr & Monroe St--Streeter Dr & Grand Ave.
* Similar to members, casual riders used classic bike more frequently. Docked bikes are the least popular bike types.

## Step 6: Act

Three recommendations:

* Since the members hold the biggest proportion of total rides, the marketing team can provide promotional offers and discount for Yearly and monthly memberships. 
* The marketing team should give additional discounts on summer months and run campaigns from May to October, which can help converting one-time customers to loyal members and increase visibility to attract new customers. Moreover, the team can target the stations for the most route taken by casual riders for running advertisement and campaigns.
* The company should increase the number of electric and docked bikes and reduce the price for those passes. This could benefit the company as electric bikes are already in trends. More docking stations can be introduced for more organised pick-up and dropping system for the users. 
