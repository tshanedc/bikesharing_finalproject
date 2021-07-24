## ETL Process
1. Process Capital Bikeshare bike trip data sets to create one big dataset that contains all 28M trips with followings columns: 'Trip Number,' 'Starts station number,' 'End station number,' 'start date,' 'end date,' and 'member type' to enable us review when and where each bike trip took place and whether it was done by a member or a casual user. 
    - Downloaded the total 65 Capital Bikeshare bike trip dataset available from `Index of bucket "capitalbikeshare-data"` page of Capital Bikeshare website.
    - Create an index of the 65 files as a csv file to allow python code able to call files by each file's directory and file name. Then, process files with index 0-52 and 53-65 separately as those files have different columns. The list can be refered to the csv file <Resources/capitalbikeshare_dataset_index.csv>.
   ### Processing the files from Index 0-52
   - The files have unique column 'Bike number' which define which bike was used for each trip.
  
   ### Processing the files from Index 53-65
   - These files have unique column called 'rideable_type,' which describes which bike type was used. We extract this information separately to analyze how this variable affects the membership type of users.
    
   ### Merge the two dataframes together to make all_bike_trips file
   - Merge the `trips_2010to202003.csv` and `trips_202005to202105.csv` files together 
   - Drop unnecessary columns, clean up NaN rows, remove mixed data type columns, convert datatype, and output the dataframe as `Table1_all_bike_trips.csv`.
 
2. Create two additional tables from `trips_2010to202003.csv` and `trips_202005to202105.csv` that represent 'Bike number' and 'rideable_type' of bikes respectively.
   ### Output 'bike_number' table 
   - Extract `Table2_bike_number.csv` dataframe and ouput as csv from the `trips_2010to202003.csv`.
   
   ### Output 'rideable_type' table 
   - Extract `Table3_rideable_type` dataframe and ouput as csv from the `trips_202005to202105.csv`.
    
3. Create a dataset with list of `station_id` and corresponding latitude and longitude by utilizing the datasets from April 2020 to May 2021. Note the most recent file available is May 2021 and Capital Bikeshare started to record geographic information since April 2020.
    - Use the files from index 53 to 65 to create a new dataframe with 'stationnumber' and its corresponding geocode.
    - The output csv file `Table4_station_list` has columns `stationnumber`, `lat`, and `lng` to refer the location of each bike station.
  
4. Output an additional list of bike stations that displays the first day and the last day of their usage to identify if any stations have imbalance in number of data points due its length of operation.
    - Use the `Table1_all_bike_trips.csv`
  
5.  From the `all_bike_trips.csv` file, extract an additional dataframe that displays number of trips took by each staion number.
   
