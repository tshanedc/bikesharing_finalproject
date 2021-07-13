
## BIKESHARING
An Analysis of Public Bike Sharing Services to uncover growing trends for potential business expansion
## REASONS FOR SELECTING TOPIC
We want to offer a deeper analysis to investors and community on the growth trends of Public Bike Sharing Services in order to motivate investors to expand the business and increase trust from community to their usage
## VARIABLES TO CONSIDER
- Location of Bike Station (Latitude, Longitude, and Zip code)
- Member and Casual user types 
- Popular and Unpopular Bike routes
- Weather
- Weekday and Weekend User number 
- Time of day analysis
## DESCRIPTION OF SOURCE DATA
We will be using datasources from Capitol Bikshare tripdata datasets to analyze the growth of its business through the number of trips taken, locations of bike stops, and membership ratio of users.
### BikeSharing Data:
Â·      Capitol Bikshare (Washington D.C.) from 2010 - 05/2021 
`<https://s3.amazonaws.com/capitalbikeshare-data/index.html>`
### Climate Information:
- https://developer.accuweather.com/
- https://www.ncdc.noaa.gov/cdo-web/search
### Census Data:
Â·      U.S census data (Information around gender, occupation, age , type of usage)
## QUESTIONS TO ANSWER
Â·      What kind of factors of location do impact on high demand of PBS?
Â·      What are the reason in imbalance in ratio of member users and casual users by different bike station, if any?
Â·      When 
Â·      Are there any factors that cause negative impact on the growth of PBS in the city?(weather, location of bick docks etc)
Â·      

## ETL Process
1. Prepare a Dataset of Capital Bikeshare bike trip data by eliminating  
    - Load the total 65 Capital Bikeshare bike trip dataset from `Index of bucket "capitalbikeshare-data"` page of Capital Bikeshare website.ã€€
    - Create an index of the 65 files as a csv file to allow python code able to call files by each file's directory and file name.
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

    # loop through the 0 to 63 indexes of dataset files we have to import csv files into dataframe
    def read_files(i):
        # for i in range(0,65):
            i = 4
            Year = index_df['Year'][i]+'/'
            file_name = index_df['File Name'][i]+'.csv'
            file_name = f'{file_name}'
            folder_path = '../Datasets/Washington DC/'
            folder_path = f"{folder_path}{Year}{file_name}"
            df = pd.read_csv(f"{folder_path}")
            return df
    read_files(i)
    ```
 2. Create a dataset with list of `station_id` and corresponding latitude and longitude by utilizing the datasets from April 2020 to May 2021. Note the most recent file available is May 2021 and Capital Bikeshare started to record geographic information since April 2020.
- Prepare an empty list to keep the list of cvs files we are interested (index 53 to 65).
```
   # Create the list of csv file paths we want to read
   def list_files(i):
       csv_file_list = []
       # loop through the range of index we are interested to create a list of file path of csv files
       for i in range(53,65):
           Year = index_df['Year'][i]+'/'
           file_name = index_df['File Name'][i]+'.csv'
           file_name = f'{file_name}'
           folder_path = '../Datasets/Washington DC/'
           folder_path = f"{folder_path}{Year}{file_name}"
           csv_file_list.append(f"{folder_path}")
       return csv_file_list
   list_files(i)
 ```
 - Merge the listed files by passing through `pd.read_csv()` function.
 ```
   # Read the listed file paths as DataFrame and merge
   list_of_datasets = []
   # loop through pd.read_csv method with the list of file path we created earlier 
   for filepath in csv_file_list:
       list_of_datasets.append(pd.read_csv(filepath, low_memory=False))

   # merge the dataframes we read into one dataframe
   merge_df = pd.concat(list_of_datasets)
   merge_df
 ```
 - Clean up the dataset by dropping unnecessary columns, converting column data types as needed, dropping rows with NaN, to keep the data with non-zero values related to bike station information.
```
   # Drop unnecessary columns for geo data table and check data types
   merge_df_dropped = merge_df.drop(columns = ['ride_id','rideable_type','started_at','ended_at','member_casual'])
   merge_df_dropped.dtypes

   # Identify any NaN in start_station_id
   merge_df_dropped['start_station_id'].isnull().sum()

   # Take rows of 'start_station_id' and 'start_lat' which are not NaN.
   merge_cleaned = merge_df_dropped[merge_df_dropped['start_station_id'].notna()]
   merge_cleaned = merge_cleaned[merge_cleaned['end_station_id'].notna()]
   merge_cleaned = merge_cleaned[merge_cleaned['start_lat'].notna()]
   merge_cleaned['start_station_id'].isnull().sum()

   # convert to integer
   merge_cleaned['start_station_id'] = merge_cleaned['start_station_id'].astype(int)
   merge_cleaned['end_station_id'] = merge_cleaned['end_station_id'].astype(int)
   merge_cleaned.dtypes

```
 - Create a csv file with list of active bike stations with their latitude and longitude information by stucking all `start_station` information with `end_station` information, eliminating any duplicate entries, and outputting the resulted dataframe as csv file.
 ```
   # Take rows of 'start_station_id' and 'start_lat' which are not NaN.
   merge_cleaned = merge_df_dropped[merge_df_dropped['start_station_id'].notna()]
   merge_cleaned = merge_cleaned[merge_cleaned['end_station_id'].notna()]
   merge_cleaned = merge_cleaned[merge_cleaned['start_lat'].notna()]
   merge_cleaned['start_station_id'].isnull().sum()
   
   # convert to integer
   merge_cleaned['start_station_id'] = merge_cleaned['start_station_id'].astype(int)
   merge_cleaned['end_station_id'] = merge_cleaned['end_station_id'].astype(int)
   merge_cleaned.dtypes
   
   # Organize the table by columns and sort by 'start_station_id'
   station_table = merge_cleaned[['start_station_id','end_station_id','start_station_name','end_station_name','start_lat','start_lng','end_lat','end_lng']]
   station_table = station_table.sort_values(by='start_station_id', ascending=True, na_position='first')
   station_table
   
   # Splitting data
   start_stations = station_table[['start_station_id','start_station_name','start_lat','start_lng']]
   end_stations = station_table[['end_station_id','end_station_name','end_lat','end_lng']]

   # Rename columns to remove 'start' and 'end'
   start_stations = start_stations.rename(columns={'start_station_id':'station_id','start_station_name':'station_name','start_lat':'lat','start_lng':'lng'})
   end_stations = end_stations.rename(columns={'end_station_id':'station_id','end_station_name':'station_name','end_lat':'lat','end_lng':'lng'})

   # Combine the two tables together
   frames = [start_stations,end_stations]
   station_list = pd.concat(frames)
   station_list.sort_values(by='station_id', ascending=True, na_position='first')

   # Check the number of rows with NaN
   station_list['station_id'].isnull().sum()

   # Pick rows with only values
   station_list = station_list[station_list['station_id'].notna()]
   station_list['station_id'].isnull().sum()
   station_list.sort_values(by='station_id', ascending=True, na_position='first')

   # Drop the duplicate entries of 'station_id'
   station_list_cleaned = station_list.drop_duplicates(subset=['station_id'],
                                keep = 'first')
   station_list_cleaned = station_list_cleaned.sort_values(by='station_id', 
                                                           ascending=True).reset_index().drop(columns=['index'])
   station_list_cleaned

   # Output to csv
   folder_path = '../Datasets/Washington DC/'
   station_list_cleaned.to_csv(os.path.join(folder_path,'station_list.csv'),index=False)
 ```
## Mockup of Database

## Machine Learning
Â·       We will use Visualization using Tableau and R. Visualize the maps of each city and show popular routes. Combine the map image with layering and pop-up highlighting the major stations, landmarks to assess the main incentives of riders choosing that routes
Â·       We are also thinking about multivariate regression to identify which factor are affecting the PBS. Or random forest model to predict the availability of bikes in the stations

