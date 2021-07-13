
# bikesharing_finalproject

## Group Members

Team 5: Thomas Shane, Paola Escamilla, Takuma Koide, Habtamu Tikuye, Mair Manson, Derrick Amegashie

## Selected Topic:


An Analysis of Public Bike Sharing (PBS) Services to uncover growing trends for potential business expansion. And also analyzing bike share data from DC to help us make a predicted forecast of which areas (zip codes) to focus are marketing strategies/invest in.




## Reasons for selecting Topic:

We want to offer a deeper analysis to investors and community on the growth trends of Public Bike Sharing Services in order to motivate investors to expand the business and increase trust from community to their usage

## Variables to Consider:
-       Population, gender, age range
-       Weather
-       Popular / Unpopular routes
-       Weekday / Weekend comparison
-       Time of the day analysis
-       Zipcode
-       location coardinates

## Description of sources for Project:
-       CitiBike (NYC)
-       BlueBikes (Boston)
-       Healthy Ride (Pittsburgh)
-       Divvy Bikes (Chicago)

-       Climate Information: < https://developer.accuweather.com/>, < https://www.ncdc.noaa.gov/cdo-web/search>
-       Climate Information:
-         < https://developer.accuweather.com/>
-         < https://www.ncdc.noaa.gov/cdo-web/search>
## Questions to answers:
-       Who are the main users of PBS and why?
-       What are the important factors that cause high demand of PBS in certain city/area/time period?
-       Are there any factors that cause negative impact on the growth of PBS in city? (weather, location of bike docks, etc…)

## Role Definition
The team has found a natural work dynamic. We will be taking advantage of the already used channels, such as slack and zoom website.
## Methodology
We will use Visualization using Tableau and R. 
This project is going to use **ARIMA model** to predict the next station that the invester is going to open

- **Ŷt = μ + ϕ1Yt-1**
- - Y: represents the dependent variable PBS(Number of station)
- μ: is the intercept 
- ϕ: is the slope, and
- t-1: is laged period
We are going to use time series data and predict where is the best location to to open the next station.

Below image shows the mock conceptual relationship of the databases across cities and the main variables we are going to analyse whether they affect the PBS positively or negativley and their significance aswell. The result will give an insight if we have to recommend the stakeholder to invest on PBS or not and where to invest it.


![ConceptualERD-export](https://user-images.githubusercontent.com/78656720/124362240-c9951200-dc01-11eb-88fb-67bad7efb827.png)



## Communication Protocols

The group communicates regularly over slack, sharing ideas and datasets as well as discussing next steps. Additionally, the group utilizes Zoom to meet outside of class hours. 

## Methodology

- We will use Visualization using Tableau and R. 

## Provisional Model for Machine Learning

- Run a series of regression on correlation between the factors such as weather, seasonality, characteristics of area (office, leisure, academic, etc…) with the usage of PBS.

- Visualize the maps of each city and  show popular routes. Combine the map image with layering and pop-up highlighting the major stations, landmarks to assess the main incentives of riders choosing that route.
- Visualize the maps of each city and  show popular routes. Combine the map image with layering and pop-up highlighting the major stations, landmarks to assess the main incentives of riders choosing that routes.

## Database Integration




