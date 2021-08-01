### Description of Data Exploration Phase
The data exploration phase started with a review of the websites of the known bike share services in different cities in the United States. We were able to find available data for the cities of New York, Chicago, Pittsburgh, Boston and Washington D.C. 

Even though all cities showed consistent data we decided to stay with Washington D.C. considering this is the city where all team members live and therefore, we could understand better. 

The data retrieved in the capital bike share website was consistent and showed quality. Each CSV file had the same features/columns aside from 2020 and 2021 where we found a couple of new columns added. For the purposes of our analysis, we disregarded those new addings to maintain the same consistency of the information.

The size of the data collected for each year was considerably large. The amount increased our reliability to the data as we were able to draw some time series in our work. 

## DATABASE INTEGRATION

Before arriving to the ETL Process, and once our data was reviewed and understood, a database, and its relationships, were established to facilitate the transformation process of the data. 

After identifying and organizing the raw data extracted from the capital bike share website, we established five tables to integrate our database: 

![Final_ERD](https://user-images.githubusercontent.com/78656720/126044898-bf43e200-e51a-45a5-83e4-865fc1d466df.png)

Based on the above ERD, we continued on transforming datasets further to suit them to visuzalization and Machine Learning processing. The created csv files are available in Google Drive as link below:

[link to Google Drive](https://drive.google.com/drive/folders/1RhGPE1DgXtZ2eRaMr09bNaQkYnRQHske)


## ETL Process
Once with the information identified, our analysis started with an ETL process which aimed to create one big dataset from all the individual CSV files. 
 
The final dataset contained all 28M trips and was conformed by the following columns: 'Trip Number,' 'Starts station number,' 'End station number,' 'start date,' 'end date,' and 'member type'.

From this unified dataset, a series of tables were extracted. In conclusion, we utilized below lists of csv files for analysis:
1. Table 1: all_bike_trips - Lists the 28 million bike trips recorded from 2011 to 2021 May. 
2. Table 4: station_list - the table lists the 650 bike station and their latitude and longitude.
3. Table 9: ratio_df - the table provides each station's ratio of 'member' user over the all trips initiated from the station.
4. Table 10: dayofweek_ratio - the table provides the ratio of 'member' user for each day of week.
5. Table 15: merge_covid - the table organzies each station's ratio of 'member' user and the number of trips took place by using the multi-indexing of whether it was before or after COVID19 and Weekday or Weekend.  
 
## Visual representation of the output:
Table #1 all_bikes_trips

![image](https://user-images.githubusercontent.com/78698456/125867516-cb3773d8-bef6-4fa0-bc48-196176f0c837.png) 

Table #4 station_list

![image](Image/4_station_list_table.png)

Table #9 ratio_df 

![image](Image/9_ratio_df.png)

Table #10 dayofweek_ratio

![image](Image/10_dayofweek_ratio.png)

Table #15 merge_covid

![image](Image/15_merge_covid.png)

Column Term | Definition
------------ | -------------
Trip_number	| an arbitrary sequential number assigned to each bike trip in order of `startdate`.
startstationnumber		| the station id number of a bike station that a trip is initiated.
endstationnumber			| the station id number of a bike station that a trip is ended.
startdate		| the time that bike trips has started in YYYY-MM-DD HH:MM:SS format.
enddate	| the time that bike trips has ended in YYYY-MM-DD HH:MM:SS format.
membertype	| the binary column that tells whether the bike trip was done by 'Member' user signed up in the membership or 'Casual' users who rented a bike without signing up the memberhsip.  
weekday		| labels which day of the week the bike trip took place.
station_id		| the same value as `startstationnumber` that identifies specific bike station.
station_name| The name of bike station. Each station name is paired with an unique `station_id`.
lat	| The latitude of a specific bike station.
lng | The longitude of a specific bike station.
ratio		| The ratio of bike trip done by 'Member' users over the all trips took place there.
number_of_members	| The number of times that "Member' users used bikes.
number_of_casuals	| The number of times that "Casual' users used bikes.
trip_total	| The number of times that both 'Member' and 'Casual' users used bikes.
status 	| Labels whether te data refers to `pre_covid` as before the COVID19 or `post_covid` as after the COVID19 pandemic started. In this analysis, the data from 2019/01-2019/12 are grouped as **pre-COVID** and that from 2020/01-2021/05 as **post-Covid**.
WEEKDAY	| the column binary labels **0** 

