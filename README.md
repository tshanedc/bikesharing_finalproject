

# FINAL PROJECT
## BIKESHARING SERVICES
An Analysis of Public Bike Sharing Services in Washington DC to uncover growing membership trends for potential business expansion

## REASONS FOR SELECTING TOPIC
We want to offer a deeper analysis to investors and the community on the growing membership trends of Public Bike Sharing Services in Washington DC in order to motivate investors to expand the availability of bike stations and also understand the usage needs of the community

## VARIABLES TO CONSIDER
- Location of Bike Station (Latitude, Longitude, and Zip code)
- User types: Member and Casual
- Bike routes: Popular and Unpopular
- User number: Weekday and Weekend
- Time of day of the ride

## DESCRIPTION OF THE SOURCE DATA
For our analysis, we are using secondary data which has already been collected by the capital bike share website (https://www.capitalbikeshare.com/system-data). The data was stored in CSV format and we were able to find data from 2010 to 2021. 

For the years 2010 and 2011 the daily bike ride information of the whole year was gathered in a unique CSV file. From 2012 to 2017, the daily bike ride information was gathered in quarterly arranged CSV files and from 2018 to 2021 the bike ride information was collected in monthly CSV files. Data for the month of april 2020 was not available and year 2021 cut in the month of may.

In total, our final data was 66 CSV files with over 26,000,000 rows and 8 columns each.

### BikeSharing Data:
- Capital Bikshare (Washington D.C.) from 2010 - 05/2021. Retrieved from: https://s3.amazonaws.com/capitalbikeshare-data/index.html

## QUESTIONS TO ANSWER
Q. How does time of day influences the member type of riders in Washington D.C.?

H0: The member type of riders is not related to the time of the day of the ride. 

## ETL Process
1. Process Capital Bikeshare bike trip data sets to create one big dataset that contains all 28M trips with followings columns: 'Trip Number,' 'Starts station number,' 'End station number,' 'start date,' 'end date,' and 'member type.'
    - Load the total 65 Capital Bikeshare bike trip dataset from `Index of bucket "capitalbikeshare-data"` page of Capital Bikeshare website.
    - Create an index of the 65 files as a csv file to allow python code able to call files by each file's directory and file name.
    - Process files with index 0-52 and 53-65 separately as those files have different columns. The list can be refered to the csv file <Resources/capitalbikeshare_dataset_index.csv>.
   ### Processing the files from Index 0-52
    ```
    # import dependencies
    import os
    import pandas as pd

    # import index csv for datasets
    #file_path = "../Datasets/Washington DC/capitalbikeshare_dataset_index.xlsx"
    folder_path = '../Datasets/Washington DC/'
    file_name = 'capitalbikeshare_dataset_index.csv'
    index_df=pd.read_csv(f"{folder_path}{file_name}")

    # Adjust the "Year" column as string data to add 
    index_df['Year'] = index_df['Year'].astype(str)
    index_df.dtypes
   
    # variables
    i = 0
    Year = index_df['Year'][i]+'/'
    file_name = index_df['File Name'][i]+'.csv'
    file_name = f'{file_name}'
    file_name

    # Create the list of csv file paths we want to read
    csv_file_list = []
    # loop through the range of index we are interested to create a list of file path of csv files
    for i in range(0,52):
        Year = index_df['Year'][i]+'/'
        file_name = index_df['File Name'][i]+'.csv'
        file_name = f'{file_name}'
        folder_path = '../Datasets/Washington DC/'
        folder_path = f"{folder_path}{Year}{file_name}"
        csv_file_list.append(f"{folder_path}")
    csv_file_list
    
    # Read the listed file paths as DataFrame and merge
    list_of_datasets = []
    # loop through pd.read_csv method with the list of file path we created earlier 
    for filepath in csv_file_list:
        list_of_datasets.append(pd.read_csv(filepath, low_memory=False))

    # merge the dataframes we read into one dataframe
    merge_df = pd.concat(list_of_datasets)
    merge_df
    
    # Drop unnecessary columns for geo data table and check data types
    df_dropped = merge_df[['Start station number','End station number','Start date','End date','Member type','Bike number']]
    
    # Take rows of 'Bike number' which are not NaN.
    df_cleaned = df_dropped[df_dropped['Bike number'].notna()]
    
    # Convert the 'started_at' and 'ended_at' columns to datetime data type
    df_cleaned['Start date'] = pd.to_datetime(df_cleaned['Start date'])
    df_cleaned['End date'] = pd.to_datetime(df_cleaned['End date'])

    # Rename columns match with the other table
    df_renamed = df_cleaned.rename(columns={'Start station number':'startsstationnumber',
                                            'End station number':'endstationnumber',
                                            'Start date':'startdate',
                                            'End date':'enddate',
                                            'Member type':'membertype',
                                            'Bike number':'bikenumber',
                                           })
    df_renamed = df_renamed[['startsstationnumber','endstationnumber',
                             'startdate','enddate','membertype','bikenumber']]
                             
    # Sort the table by the order of 'startdate' to list the trips in order of their ocuurence
    # then choose columns to keep for two tables: trip_later and rideable_type
    df = df_renamed.sort_values(by='startdate', ascending=True, na_position='first')
    df_reset = df.reset_index(drop=True)
    df_reset.index = df_reset.index + 1
    df_reset['Trip_number'] = df_reset.index
    columnsTitles = ['Trip_number','startsstationnumber','endstationnumber',
                    'startdate','enddate','membertype']
    trips_2010to202003 = df_reset.reindex(columns = columnsTitles)
      
    # Add column with Day of the Week
    trips_2010to202003['weekday'] = trips_2010to202003['startdate'].dt.day_name()
    
    # Output to csv
    folder_path = '../Datasets/Washington DC/tables/'
    trips_2010to202003.to_csv(os.path.join(folder_path,'trips_2010to202003.csv'),index=False)
    ```
    ### Output 'bike_number' table 
    - Extract `bike_number` dataframe and ouput as csv from the previous extraction code.
    ```
    columnsTitlesBikeNumber = ['Trip_number','bikenumber','startsstationnumber',
                             'endstationnumber','membertype']
    bike_number = df_reset.reindex(columns = columnsTitlesBikeNumber)
    
    # Output to csv
    folder_path = '../Datasets/Washington DC/tables'
    bike_number.to_csv(os.path.join(folder_path,'Table2_bike_number.csv'),index=False)
    ```
   ### Processing the files from Index 53-65
    ```
    # Create the list of csv file paths we want to read
    csv_file_list = []
    # loop through the range of index we are interested to create a list of file path of csv files
    for i in range(53,65):
        Year = index_df['Year'][i]+'/'
        file_name = index_df['File Name'][i]+'.csv'
        file_name = f'{file_name}'
        folder_path = '../Datasets/Washington DC/'
        folder_path = f"{folder_path}{Year}{file_name}"
        csv_file_list.append(f"{folder_path}")
    csv_file_list
    
    # Read the listed file paths as DataFrame and merge
    list_of_datasets = []
    # loop through pd.read_csv method with the list of file path we created earlier 
    for filepath in csv_file_list:
        list_of_datasets.append(pd.read_csv(filepath, low_memory=False))

    # merge the dataframes we read into one dataframe
    merge_df = pd.concat(list_of_datasets)
    
    # Drop unnecessary columns for geo data table and check data types
    df_dropped = merge_df[['rideable_type','start_station_id','end_station_id','started_at','ended_at','member_casual']]

    # Identify any NaN in the dataset
    df_dropped.isnull().sum()
    
    # Take rows of 'start_station_id' and 'end_station_id' which are not NaN.
    df_cleaned = df_dropped[df_dropped['start_station_id'].notna()]
    df_cleaned = df_cleaned[df_cleaned['end_station_id'].notna()]
   
   # Remove a row with mixed data 
    df_cleaned = df_cleaned[~(df_cleaned == "MTL-ECO5-03").any(axis=1)]
    
    # Convert the 'started_at' and 'ended_at' columns to datetime data type
    df_cleaned['started_at'] = pd.to_datetime(df_cleaned['started_at'])
    df_cleaned['ended_at'] = pd.to_datetime(df_cleaned['ended_at'])
    df_cleaned['start_station_id'] = df_cleaned['start_station_id'].astype(int)
    df_cleaned['end_station_id'] = df_cleaned['end_station_id'].astype(int)
    
    # Rename columns match with the other table
    df_renamed = df_cleaned.rename(columns={'start_station_id':'startsstationnumber',
                                            'end_station_id':'endstationnumber',
                                            'started_at':'startdate',
                                            'ended_at':'enddate',
                                            'member_casual':'membertype'
                                           })
    df_renamed = df_renamed[['rideable_type','startsstationnumber','endstationnumber',
                             'startdate','enddate','membertype']]
    # Change the first letter of 'membertype' to upper case to match with the other tables
    df_renamed['membertype'] = df_renamed['membertype'].str.capitalize()
    
    # Sort the table by the order of 'startdate' to list the trips in order of their ocuurence
    # then choose columns to keep for two tables: trip_later and rideable_type
    df = df_renamed.sort_values(by='startdate', ascending=True, na_position='first')
    df_reset = df.reset_index(drop=True)
    df_reset.index = df_reset.index + 26433601
    df_reset['Trip_number'] = df_reset.index
    columnsTitles = ['Trip_number','startsstationnumber','endstationnumber',
                    'startdate','enddate','membertype']
    columnsTitlesRideableType = ['Trip_number','rideable_type','startsstationnumber',
                                 'endstationnumber','membertype']
    trips_202005to202105 = df_reset.reindex(columns = columnsTitles)
    rideable_type = df_reset.reindex(columns = columnsTitlesRideableType )

    # Add column with Day of the Week
    trips_202005to202105['weekday'] = trips_202005to202105['startdate'].dt.day_name()
    
    # Output to csv
    folder_path = '../Datasets/Washington DC/tables/'
    trips_202005to202105.to_csv(os.path.join(folder_path,'trips_202005to202105.csv'),index=False)
    folder_path = '../Datasets/Washington DC/tables/'
    rideable_type.to_csv(os.path.join(folder_path,'Table3_rideable_type.csv'),index=False)
    ```
    ### Output 'rideable_type' table 
    - Extract `rideable_type` dataframe and ouput as csv from the previous extraction code.
    ```
    columnsTitlesRideableType = ['Trip_number','rideable_type','startsstationnumber',
                             'endstationnumber','membertype']
    rideable_type = df_reset.reindex(columns = columnsTitlesRideableType)
    
    # Output to csv
    folder_path = '../Datasets/Washington DC/tables/'
    rideable_type.to_csv(os.path.join(folder_path,'Table3_rideable_type.csv'),index=False)
    ```    
    
 2. Create a dataset with list of `station_id` and corresponding latitude and longitude by utilizing the datasets from April 2020 to May 2021. Note the most recent file available is May 2021 and Capital Bikeshare started to record geographic information since April 2020.
    - Use the `merge_df` dataframe containing the files from index 53 to 65 to create a new dataframe with 'stationnumber' and its corresponding geocode.
   ```
   # Choose columns to keep for station_list table
    merge_df_dropped = merge_df[['start_station_id','end_station_id',
                                 'start_station_name','end_station_name',
                                 'start_lat','start_lng','end_lat','end_lng']]
   
   # Rename the dataframe to df for simplicity
    df = merge_df_dropped
    
    # Take rows of 'start_station_id' and 'start_lat' which are not NaN.
    df_cleaned = df[df['start_station_id'].notna()]
    df_cleaned = df_cleaned[df_cleaned['end_station_id'].notna()]
    df_cleaned = df_cleaned[df_cleaned['start_lat'].notna()]
    df_cleaned['start_station_id'].isnull().sum()
    
    # convert to station_id to integer and frop unnecessary columns
    df_cleaned['start_station_id'] = df_cleaned['start_station_id'].astype(int)
    df_cleaned['end_station_id'] = df_cleaned['end_station_id'].astype(int)
    station_table =     df_cleaned[['start_station_id','end_station_id','start_station_name','end_station_name','start_lat','start_lng','end_lat','end_lng']]
    station_table.dtypes
    
    # Splitting data by start stations and end stations
    start_stations = station_table[['start_station_id','start_station_name','start_lat','start_lng']]
    end_stations = station_table[['end_station_id','end_station_name','end_lat','end_lng']]
    
    # Rename columns to remove 'start' and 'end'
    start_stations = start_stations.rename(columns={'start_station_id':'station_id','start_station_name':'station_name','start_lat':'lat','start_lng':'lng'})
    end_stations = end_stations.rename(columns={'end_station_id':'station_id','end_station_name':'station_name','end_lat':'lat','end_lng':'lng'})
    
    # Combine the two tables together
    frames = [start_stations,end_stations]
    station_list = pd.concat(frames)
    station_list.sort_values(by='station_id', ascending=True, na_position='first')
    
    # Pick rows with only values
    station_list = station_list[station_list['station_id'].notna()]
    station_list['station_id'].isnull().sum()
    
    station_list.sort_values(by='station_id', ascending=True, na_position='first')
    
    # Drop the duplicate entries of 'station_id'
    station_list_cleaned = station_list.drop_duplicates(subset=['station_id'],
                                 keep = 'first')
    station_list_cleaned = station_list_cleaned.sort_values(by='station_id', 
                                                            ascending=True).reset_index().drop(columns=['index'])
                                                            
    # Output to csv
    folder_path = '../Datasets/Washington DC/tables/'
    station_list_cleaned.to_csv(os.path.join(folder_path,'Table4_station_list.csv'),index=False)
   ```


- Convert time date feature into a day of week column. 
![image](https://user-images.githubusercontent.com/78698456/125535528-357a0d62-ebbe-4b9b-bff5-ff3186481461.png)

Table #1 all_bikes_trips

![image](https://user-images.githubusercontent.com/78698456/125867516-cb3773d8-bef6-4fa0-bc48-196176f0c837.png)

Table #2 bike_number

![image](Image/2_bike_number_table.png)

Table #3 Rideable_type

![image](Image/3_rideable_type_table.png)

Table #4 Station List

![image](Image/4_station_list_table.png)

Table #5 Station List with its active dates

![image](Image/5_station_activedates_table.png)


## Database Integration

Our database has been refined and currently, holds the following schema:

![image](https://user-images.githubusercontent.com/78698456/125697070-0860aaa8-99b0-452d-b74d-cf30d090c097.png)

![Final_ERD](https://user-images.githubusercontent.com/78656720/126044898-bf43e200-e51a-45a5-83e4-865fc1d466df.png)

The connection between tables, the fields that have been kept and the data types are displayed in the image above. After cleaned and organized the raw data extracted from the citibike website, we created five tables that are ready for our analysis and load the in to PostgerSQL database. While we are working on our project we have also extracted different tables using SQL join query. Looking at the image below we can see the Table created by Inner join from our *station_list* and *station_list_active*.

![Inner_Join](https://user-images.githubusercontent.com/78656720/126045190-2021a9e1-b07f-40b8-9c1a-0a5cddf2b1ea.png)

## MACHINE LEARNING
- We will develop Visualization using Tableau and R. 
- Visualize the maps of each city and show popular routes. 
- Combine the map image with layering and pop-up highlighting the major stations, landmarks to assess the main incentives of riders choosing that routes
- We are also thinking about multivariate regression to identify which factor are affecting the PBS or random forest model to predict the availability of bikes in the stations.

ARIMA Model:
Part 1: Preprocessing (Completed)
1) Read in 'all_bike' csv
- Change Trip_number col to lowercase
1) Read in 'bike_type' csv
- Encode bike types
- Rename cols
1) Merge 'all_bike' and 'bike_type'
2) Drop unnecessary cols
3) Change 'startdate' from object to datetime
4) Set index to 'startdate'

Part 2: Split Train_Test

Part 3: Check for Stationarity

Part 4: Modeling/Forecasting

Part 5: Evaluate/Improve Forecasting/Predictions 

## DASHBOARD
The image below shows the comaparision of number of trips by memebrship status and the bike type. The table contains the 10 years data of bike trip in Washington DC. As we can see in the image the mebers bike morethan the casual in all bike types. 
![trip_biketype](https://user-images.githubusercontent.com/78656720/126076728-15f14cb9-ced2-4afb-9c75-78831ec4003a.png)

Visualization of 'stationnumber', 'occurrence', 'latitude', and 'longitude' to display bike stations and how many trips where initiated in each.
![image](https://user-images.githubusercontent.com/78698456/126231066-b6978c64-9c18-4749-bda7-03dca0d7054e.png)

