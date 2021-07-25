## ETL Process

### Table1_all_bike_trips
1. Process Capital Bikeshare bike trip data sets to create one big dataset that contains all 28M trips with followings columns: 'Trip Number,' 'Starts station number,' 'End station number,' 'start date,' 'end date,' and 'member type' to enable us review when and where each bike trip took place and whether it was done by a member or a casual user. 
    - Downloaded the total 65 Capital Bikeshare bike trip dataset available from `Index of bucket "capitalbikeshare-data"` page of Capital Bikeshare website.
    - Create an index of the 65 files as a csv file to allow python code able to call files by each file's directory and file name. Then, process files with index 0-52 and 53-65 separately as those files have different columns. The list can be refered to the csv file <Resources/capitalbikeshare_dataset_index.csv>.
   - The files from index 0-52 have unique column 'Bike number' which define which bike was used for each trip.
   - After cleaning the data and dropping unnecessary columns, output the dataframe as csv file `trips_2010to202003.csv`.
   - The files with indexes 53-65 have unique column called 'rideable_type,' which describes which bike type was used. We extract this information separately to analyze how this variable affects the membership type of users.
   - After cleaning the data and dropping unnecessary columns, output the dataframe as csv file `trips_202005to202105.csv`.
   - Merge the `trips_2010to202003.csv` and `trips_202005to202105.csv` files together 
   - Drop unnecessary columns, clean up NaN rows, remove mixed data type columns, convert datatype, and output the dataframe as `Table1_all_bike_trips.csv`.
 
### Table2_bike_number and Table3_rideable_type
2. Create two additional tables from `trips_2010to202003.csv` and `trips_202005to202105.csv` that represent 'Bike number' and 'rideable_type' of bikes respectively.
   - Extract `Table2_bike_number.csv` dataframe and ouput as csv from the `trips_2010to202003.csv`.
   - Extract `Table3_rideable_type` dataframe and ouput as csv from the `trips_202005to202105.csv`.

### Table4_station_list and Table5_station_active_dates
3. Create a dataset with list of `station_id` and corresponding latitude and longitude by utilizing the datasets from April 2020 to May 2021. Note the most recent file available is May 2021 and Capital Bikeshare started to record geographic information since April 2020.
    - Use the files from index 53 to 65 to create a new dataframe with 'stationnumber' and its corresponding geocode.
    - The output csv file `Table4_station_list` has columns `stationnumber`, `lat`, and `lng` to refer the location of each bike station.
  
4. Output an additional list of bike stations that displays the first day and the last day of their usage to identify if any stations have imbalance in number of data points due its length of operation.
    - Use the `Table1_all_bike_trips.csv` and pick the first time and the last time that each `startstationnumber` appears in the list
    - Output the dataframe with `startstationnumber`, `first`, and `last` as a csv file `Table5_station_active_dates.csv`.

### Table6_number_of_trips
5.  From the `all_bike_trips.csv` file, extract an additional dataframe that displays number of trips took by each staion number.
    - Utilize `value_counts()` method and list how many times does each station appear over the 28M trips.
    - Output the dataframe with `startstationnumber` and its `occurence` as a csv file `Table6_number_of_trips.csv`.

### Table7_member_trips and Table8_casual_trips
7.  Create two separate datasets listing trips done by 'Member' users and 'Casual' users separately.
    - Refer to the `Table1_all_bike_trips.csv` and pick the rows with `membertype` being `Member` and `Casual` separately into two dataframes: `Table7_member_trips.csv` and `Table8_casual_trips.csv`.

### Table9_ratio_df and Table10_dayofweek_ratio
8.  Create a table listing the ratio of trips done by 'Member' users over the all trips have done at each station.
    - Refer to the `Table7_member_trips.csv` and `Table8_casual_trips.csv` files for number of trips done at each station by `Member` and `Casual` using `value_counts()`. Then, calculate the ratio based on those values to figure out the ratio of `Member` usage by each station.
    - Output the resulted dataframe as `Table9_ratio_df.csv` file.
    
9.  Create a table listing the ratio of trips done by 'Member' users over the all trips by each day of week.
    - Refer to the `Table7_member_trips.csv` and `Table8_casual_trips.csv` files and figure out the ratio of `Member` users by each day of week.
    - Output the resulted dataframe as `Table10_dayofweek_ratio.csv` file.
