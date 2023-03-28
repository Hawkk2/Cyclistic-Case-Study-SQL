# Cyclistic-Case-Study-SQL
### Using BigQuery to clean, transform and analyze data. 

## Google Data Analytics Capstone Project
### Cyclistic Case Study
---

# Introduction


My name is Caleb Haqq. I am beginning a new career in Data Analytics and this document is one of a series that I have made to demonstrate my learned skills in the data analysis process.  This case study will show my completion of the Google Data Analytics Professional Certificate. The document you are viewing displays my use of SQL queries using Google’s BigQuery. Other post will demonstrate other languages and tools used as a data analyst.

# Scenario

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations. 
Characters and teams

 ● Cyclistic: A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day. 
 
● Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels. 

● Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them. 

● Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program. 

About the company 

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime. 
Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members. 
Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.
 Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends


## 1.	ASK

You will produce a report with the following deliverables:

a.	A clear statement of the business task 
b.	A description of all data sources used
c.	Documentation of any cleaning or manipulation of data 
d.	A summary of your analysis 
e.	Supporting visualizations and key findings 
f.	Your top three recommendations based on your analysis

a.	What is the business task?

Analyze the previous 12 months of historical data for Cyclistic bike rentals in order to answer the following questions.

* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?

## 2.	PREPARE

b.	Where is your data located? How is the data organized? 
*	Data is located in CSV files that is made available by Motivate International Inc. publicly. 
*	The data is broken down into columns that among many things Identifies the a ride_id, start/end time, start/end location, longitude/latitude and whether the rider is a member or casual. 
*	 Are there issues with bias or credibility in this data? Does your data ROCCC (R eliable, O riginal, C omprehensive, C urrent, and C ited.) Reliable? 
*	The data appears to be credible because it comes form an LLC that is contracted to run the City of Chicago’s bicycle sharing service. 
*	The data is missing values under stations…  The reasons could be the following:
*	There are not enough stations to house every bicycle. 
*	Bicycles were acquired and abandoned at riders destinations where stations were not available. 
*	 How are you addressing licensing, privacy, security, and accessibility? 
*	Licensing has been granted for data to be made available for public use
*	No personal information is provided that could be linked to the rider. 
*	Data that has been downloaded does not need to be destroyed upon completion of work because it is published publicly. 
	
*	 How did you verify the data’s integrity? 
*	Divvy program is verifiable through licensing provided on website and confirmed through Chicago.gov. 
*	 How does it help you answer your question? 
*	Becoming familiar with the characters, data and stakeholders provide a better grounding of the environment that the questions if formed from. 
*	 Are there any problems with the data? 
*	Mentioned above, it appears that there are stations missing but the steps taken to prepare have provided sufficient answers given resources available. 
*	If this project were provided directly from an employer we would need to email to confirm that our conclusion on the missing stations was correct and if we it were possible to fill in the gaps missing in the data. 


3.	PROCESS
*	What tools are you choosing and why? 
I am using a cloud SQL querying service through Google called BigQuery.  BigQuery is being used because it was the SQL language taught in the Google Data Analytics Certification Course. 
Tableau will be used following the cleaning and analysis to visualize the data. It was also taught in the Google course. 

*	Have you ensured your data’s integrity? 
I started by making sure the schema is identical for each data set that was downloaded from Divvy. Because each data set only contains information for the corresponding month each will have to be merged before use. 
```sql
SELECT *
 FROM
   `aesthetic-abbey-377903.Cyclistic.`.INFORMATION_SCHEMA.COLUMN_FIELD_PATHS
 WHERE
   table_name = "202203-divvy-tripdata"  OR
   table_name = "202204-divvy-tripdata"  OR
   table_name = "202205-divvy-tripdata"  OR
   table_name = "202206-divvy-tripdata"  OR
   table_name = "202207-divvy-tripdata"  OR
   table_name = "202208-divvy-tripdata"  OR
   table_name = "202209-divvy-publictripdata"  OR
   table_name = "202210-divvy-tripdata"  OR
   table_name = "202211-divvy-tripdata"  OR
   table_name = "202212-divvy-tripdata"  OR
   table_name = "202301-divvy-tripdata"  OR
   table_name = "202302-divvy-tripdata"
 ORDER BY column_name, table_name
 ```

#Need to merge all of the datasets into one dataset using UNION ALL

```sql
CREATE OR REPLACE TABLE Cyclistic.v1_divvytripdata AS 
(
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202203-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202204-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202205-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202206-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202207-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202208-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202209-divvy-publictripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202210-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202211-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202212-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202301-divvy-tripdata`
 UNION ALL
 SELECT * 
 FROM `aesthetic-abbey-377903.Cyclistic.202302-divvy-tripdata`
)
```
#There should only be 2 distinct values in this column. 
```sql
SELECT 
  DISTINCT(member_casual)
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
```

#This gives a count of the distinct values for each column. C
#Can gleam some insite from comparing the locations with the station names. 
```sql
SELECT
  COUNT(DISTINCT ride_id) AS rideId,
  COUNT(DISTINCT start_station_name) AS startStationName,
  COUNT(DISTINCT start_station_id) AS startStationId,
  COUNT(DISTINCT end_station_name) AS endStationName,
  COUNT(DISTINCT end_station_id) AS endStationId,
  COUNT(DISTINCT start_lat) AS startlat,
  COUNT(DISTINCT start_lng) AS startlng,
  COUNT(DISTINCT end_lat) AS endlat,
  COUNT(DISTINCT end_lng) AS endlng,
FROM `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
```

**•	What steps have you taken to ensure that your data is clean?** 
#Begin reviewing data and cleaning. 
#Looking for spaces in the station id's
```sql
SELECT 
  start_station_id, end_station_id
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
WHERE 
  start_station_id IS NOT NULL
  AND end_station_id IS NOT NULL
  AND start_station_id LIKE '% %'
  AND end_station_id LIKE '% %'
 ```
  #After running I discovered something called (LBS-WH-TEST) and Warehouse test station



#IF ALL OF THESE ARE NEGATIVE DURATIONS THEN DELETE BY NAME. 
```sql
SELECT 
  DISTINCT start_station_id
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
WHERE 
  start_station_id = "Hubbard Bike-checking (LBS-WH-TEST)"
```
#This provides us with 1934 cases

```sql
DELETE FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata` 
WHERE 
  start_station_id = "Hubbard Bike-checking (LBS-WH-TEST)"
```

#We removed the Test cases. 

#Need to Count how many instances of %TEST% and %test%
#Hubbard Bike-checking (LBS-WH-TEST)

```sql
SELECT
  start_station_id, end_station_id
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
WHERE 
  start_station_id LIKE '%Test%'
  OR start_station_id LIKE '%test%'
  OR end_station_id LIKE '%Test%'
  OR end_station_id LIKE '%test%'
```

#DIVVY 001 - Warehouse test station is returned 10x

```sql
DELETE FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata` 
WHERE 
  start_station_id = "DIVVY 001 - Warehouse test station"
```

   #Delete remaining test cases

*	How can you verify that your data is clean and ready to analyze? 

   #We are looking for blank spaces in ride_id
```sql
SELECT 
  DISTINCT ride_id
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
WHERE 
  ride_id LIKE '% %'
ORDER BY 
  ride_id ASC```

    #No spaces found
    
   #Verifying there are no duplicate ride_ids
   
```sql
SELECT
  COUNT(DISTINCT ride_id)
FROM
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
```
      # Number of distinct ride_ids matches with the total number of rows in data set.

#Double check no duplicates in ride_id
```sql
SELECT 
  ride_id, COUNT(1) 
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata` 
GROUP BY 
  ride_id HAVING COUNT(1) > 1
```
      #This further confirms that there are no duplicate rides unless the duplicate can have a unique ride_id. 

#The following are queries to find if there are duplicate rentals at the exact same time and location
```sql
SELECT 
  * EXCEPT(row_number)
FROM 
  (SELECT*,ROW_NUMBER() OVER (PARTITION BY (CONCAT(started_at, ended_at))) AS row_number
FROM 
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`)
WHERE 
  row_number > 1
ORDER BY 
  started_at
  ```
      #This results 714 identical start and end times but with the frequency and amount of rentals it is possible for identical start and end times. 
      
#Ran a test of one of the results to see the two identical start and end times
```sql
SELECT * 
    FROM `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
WHERE 
    started_at = "2022-08-12 22:24:52" AND ended_at = "2022-08-12 22:59:31"
```

#Checking to see how many rentals have identical (rideable_type, started_at, ended_at, start_lat, start_lng, member_casual) by concatinating those columns together and querying to see if they appear more than once. 
```sql
SELECT 
  * EXCEPT(row_number)
FROM (
      SELECT
        *,ROW_NUMBER() OVER (PARTITION BY (CONCAT(rideable_type, started_at, ended_at, start_lat, start_lng, member_casual))) AS row_number
      FROM 
        `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`)
WHERE 
  row_number > 1 
ORDER BY 
  start_station_name
```
      #Conclusion is that there are 24 rides that share everything except ride_id with exactly one partner making 48 in total. Because 48 rides makes up less than 0.001 of all ride I have deemed it statistically insignificant regarding the scope of this project. ((48 / 5,829,084) * 100 = 0.000823456%) If this number would have been closer to 1 percent then data would have been removed. It is possible that tourist or school group rented simultaneously. 
      #The ride_id's of these rides will be provided
      
#This tells us how many null values exist in each core column. 
```sql
SELECT
  SUM(CASE WHEN rideable_type IS NULL THEN 1 ELSE 0 END) AS null_rideable_type,
  SUM(CASE WHEN started_at IS NULL THEN 1 ELSE 0 END) AS null_started_at,
  SUM(CASE WHEN ended_at IS NULL THEN 1 ELSE 0 END) AS null_ended_at,
  SUM(CASE WHEN start_station_name IS NULL THEN 1 ELSE 0 END) AS null_start_station,
  SUM(CASE WHEN end_station_name IS NULL THEN 1 ELSE 0 END) AS null_end_station,
  SUM(CASE WHEN start_lat IS NULL THEN 1 ELSE 0 END)AS null_start_lat,
  SUM(CASE WHEN start_lng IS NULL THEN 1 ELSE 0 END) AS null_start_lng,
  SUM(CASE WHEN end_lat IS NULL THEN 1 ELSE 0 END) AS null_end_lat,
  SUM(CASE WHEN end_lng IS NULL THEN 1 ELSE 0 END) AS null_end_lng,
  SUM(CASE WHEN member_casual IS NULL THEN 1 ELSE 0 END) AS null_member_casual
FROM
  `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`
```
      #Station names, and end lat/lng have the only NULL values. 
      #Display results in table.

    
#Cleaning the extra Lat and Lng for each station. This needs to be done because the Latitude and Longitude are recording multiple precise locations within the station. 
```sql
SELECT start_station_name,
  count(distinct start_lat) as count_lat,
   count(distinct start_lng) as count_lng,
FROM 
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
GROUP BY 
  start_station_name
HAVING 
  count(distinct start_lat) > 1000
  AND count(distinct start_lng) > 1000
```

#This shows that there are more than 3000 distinct start_latitudes for the station "Sheffield Ave & Fullerton Ave".
```sql
SELECT 
  DISTINCT start_lat,
FROM 
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
WHERE 
  start_station_name = "Sheffield Ave & Fullerton Ave"
```

```sql
CREATE OR REPLACE TABLE 
    aesthetic-abbey-377903.Cyclistic.vtemp2union_stations AS
  SELECT *
  FROM (
    SELECT  
      start_station_name AS station_name,
      start_lat AS station_lat,
      start_lng AS station_lng
    FROM    
      `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata` UNION ALL
    SELECT  
      end_station_name AS station_name,
      end_lat AS station_lat,
      end_lng AS station_lng,
    FROM    
      `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`)
```

```sql
CREATE TEMP FUNCTION median (arr ANY type) AS (
  IF(MOD(ARRAY_LENGTH(arr), 2) = 0,
    ( arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2) - 1)] +
      arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))])  / 2,
      arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))] )
)
CREATE OR REPLACE TABLE 
    aesthetic-abbey-377903.Cyclistic.vtemp3grouped_stations AS
  SELECT 
    station_name,
    COUNT(1) AS count_rides,
    median(ARRAY_AGG(station_lat ORDER BY station_lat)) AS median_lat,
    median(ARRAY_AGG(station_lng ORDER BY station_lng)) AS median_lng
  FROM
    'aesthetic-abbey-377903.Cyclistic.vtemp2union_stations'
  GROUP BY
    station_name
```

```sql
CREATE OR REPLACE TABLE 
    aesthetic-abbey-377903.Cyclistic.v4_divvytripdata AS
  SELECT
    a.* EXCEPT (start_lat, start_lng, end_lat, end_lng),
    s.median_lat as start_lat,
    s.median_lng as start_lng,
    e.median_lat as end_lat,
    e.median_lng as end_lng
  FROM
    `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata` a
    LEFT JOIN aesthetic-abbey-377903.Cyclistic.vtemp3grouped_stations s on a.start_station_name = s.station_name
    LEFT JOIN aesthetic-abbey-377903.Cyclistic.vtemp3grouped_stations e on a.end_station_name = e.station_name

#Double checking that the consolidation of the lat and lng looks correct. 
```sql
SELECT 
  COUNT(DISTINCT start_station_name) AS start_station_name,
  COUNT(DISTINCT start_station_id) AS start_station_id,
  COUNT(DISTINCT start_lat) AS start_station_latitude,
  COUNT(DISTINCT start_lng) AS start_station_longitude,
  COUNT(DISTINCT end_station_name) AS end_station_name,
  COUNT(DISTINCT start_station_id) AS start_station_id,
  COUNT(DISTINCT end_lat) AS end_station_latitude,
  COUNT(DISTINCT end_lng) AS end_station_longitude
FROM 
  `aesthetic-abbey-377903.Cyclistic.v4_divvytripdata`
WHERE 
  end_lat IS NOT NULL
```

#Finally Cleaning the table of any remaining extra spaces at the beginning or end of key columns
```sql
CREATE OR REPLACE TABLE `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata` AS
  SELECT
      TRIM(ride_id) AS ride_id
      ,TRIM(start_station_name) AS start_station_name
      ,TRIM(end_station_name) AS end_station_name
      ,TRIM(member_casual) AS member_casual
      ,* EXCEPT (ride_id, start_station_name, end_station_name, member_casual)
  FROM
    `aesthetic-abbey-377903.Cyclistic.v3_divvytripdata`
```


*	Have you documented your cleaning process so you can review and share those results?
All documentation of cleaning is recorded and explained above. 

1.	ANALYZE
Before pulling statistical info from the dataset I create a few new columns that will allow me to do more with the set. 

#Create new table with the Duration of each ride as ride_length. 
#Then replace table adding in round_trip. 
#DATETIME_DIFF(date1, date2, interval)
```sql
CREATE OR REPLACE TABLE 
    Cyclistic.v2_divvytripdata AS
  SELECT
    *, DATETIME_DIFF(ended_at, started_at, SECOND) AS ride_length
  FROM 
   `aesthetic-abbey-377903.Cyclistic.v1_divvytripdata`

  CREATE OR REPLACE TABLE 
      Cyclistic.v2_divvytripdata AS
  SELECT 
    *, CASE WHEN UPPER(start_station_name) = UPPER(end_station_name) THEN 1 ELSE 0 END AS round_trip
  FROM 
   `aesthetic-abbey-377903.Cyclistic.v2_divvytripdata`
```
   

#Converts started_at(Date) into Day, DayofWeek, Week, Month Year and start_date. Then save into table. 
```sql
CREATE OR REPLACE TABLE 
    Cyclistic.v3_divvytripdata AS
  SELECT *,
    TIME(EXTRACT(HOUR FROM started_at),0,0) AS start_hour,
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
    EXTRACT(DAY FROM started_at) AS start_day,
    EXTRACT(MONTH FROM started_at) AS start_month,
    EXTRACT(YEAR FROM started_at) AS start_year,
    FORMAT_DATE("%d-%m-%Y", started_at) AS start_date
  FROM 
    `aesthetic-abbey-377903.Cyclistic.v2_divvytripdata`
```

Next I count the number of members and casual users. 
```sql
SELECT 
  COUNT(*)
FROM 
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
WHERE 
  member_casual = "member"
   #3,463,964

#Count the number of casual rides  

```sql
SELECT 
  COUNT(*)
FROM
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
WHERE 
  member_casual = "casual" 
```
     #2,365,120
     
#Finding the Median
```sql
SELECT 
  DISTINCT member_casual, PERCENTILE_DISC(ride_length, 0.5) OVER (PARTITION BY member_casual) AS medi_ride
FROM 
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
ORDER BY
  member_casual DESC
```sql
      #Member=524 or 8.7 min,  Casual=770 or 12.8 min

#Find the Mean (I want to also find the averages for each month)
```sql
SELECT
  member_casual, AVG(ride_length) AS avg_ride
FROM
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
GROUP BY
  member_casual
ORDER BY
  member_casual
```
      #Member = 1736.58413 or 0:28:57 min, Casual = 754.618827 or 0:12:35min
      
 #Count and percentages of rides
```sql
Select
  count(*) AS count_rides, ROUND(COUNT(*) * 100 / SUM(COUNT(*)) OVER (),2) AS Percent, member_casual
FROM
  `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
GROUP BY 
  member_casual
ORDER BY
  count_rides DESC
```
      # 3463964 or 59.43% member, 2365120 or 40.57% casual

#Final function for all above. 
#Also median function in following./// // //
```sql
CREATE TEMP FUNCTION 
    median (arr ANY type) AS (
  IF(MOD(ARRAY_LENGTH(arr), 2) = 0,
    ( arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2) - 1)] +
      arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))])  / 2,
      arr[OFFSET(DIV(ARRAY_LENGTH(arr), 2))] )
)
SELECT
    member_casual
    ,COUNT(1) AS count_rides
    ,SUM(round_trip) AS count_round_trip
    ,(ROUND(SAFE_DIVIDE(SUM(round_trip),COUNT(1)),2) * 100) AS rate_round_percent
    ,COUNT(DISTINCT start_date) AS count_dates
    ,ROUND(AVG(ride_length)/60,1) as mean_mins
    ,ROUND(median(ARRAY_AGG(ride_length ORDER BY ride_length))/60,1) AS median_mins
FROM
    `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
GROUP BY
    member_casual  
```
      
#Bike type usage and percentages. 
```sql
SELECT
    member_casual,
    rideable_type,
    COUNT(1) AS count_rows,
    ROUND(100*(COUNT(1) / 5829084),2) AS count_percent,
    COUNT(DISTINCT start_date) AS count_dates,
FROM
    `aesthetic-abbey-377903.Cyclistic.vtemp_divvytripdata`
GROUP BY
    member_casual
    ,rideable_type
ORDER BY
    member_casual
```
            # Only on one day were electric bikes not used and only by casual renters.     
      

