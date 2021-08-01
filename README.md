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

Table #4 Station List

![image](Image/4_station_list_table.png)

Table #9 Ratio df

![image](Image/9_ratio_df.png)

Table #10 Dayofweek Ratio

![image](Image/10_dayofweek_ratio.png)

Table #15 Merge Covid

![image](Image/15_merge_covid.png)
