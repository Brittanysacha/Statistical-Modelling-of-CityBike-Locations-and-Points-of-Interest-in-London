Project-Statistical-Modelling-with-Python

## Project/Goals
(fill in your description and goals here)

## Process
This project had several steps aimed at gaining a comprehensive understanding of different points of interest in close proximity to CityBike locations across a certain region. The region selected for this project was the city of London, England. Below are some of the steps involved in the process of completing this project:

### Step 1: Connecting and Retrieving CityBikes API Information
The first step of this project was to connect to the CityBikes API and retrieve information on CityBike locations within London. To achieve this, I wrote a code that retrieves bike station information from the Santander Cycles network by sending HTTP GET requests to the CityBikes API using the requests library. Initially, I ran a request to get general information about the network such as the latitude, longitude, and the number of available bikes at each station. In the second request, I specified the fields to include in the response JSON content, such as the station name, location, latitude, longitude, and the number of available bikes at each station. The code then loops through the list of stations to extract this information. Finally, the retrieved data is saved in a pandas DataFrame and exported to a CSV file to be used later in the project.

### Step 2: Connecting and Retrieving FourSquare and Yelp API Information on CityBikes Locations

The second step of this project involved gathering information about points of interest located within a 100-metre radius of each CityBike location. However, I made the decision to lower my radius of each CityBike points, due to the size of the city I was studying and the close proximity of my bike points. 

![London City Bike Cluster](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/CityBike%20Locations_London_Cluster.png)

Therefore, I began by looking for restaurants within a 250-metre radius of each CityBike location using code that utilised the Foursquare API. Firstly, I set up the API credentials using the `os.environ.get()` function and connected to the Greater London API network. Next, I defined a function called `restaurant_foursquare_query()` that takes latitude and longitude coordinates as input and sends an HTTP GET request to the API endpoint URL with the given query parameters. The function then returns the JSON content from the response.

To gather information for each CityBike location, I wrote a function called `bike_radius()` that takes latitude and longitude coordinates as input, calls `restaurant_foursquare_query()`, and returns the results of the query. Finally, I wrote a function called `bike_radius_info()` that takes latitude, longitude, and location name as input, calls `restaurant_foursquare_query()`, extracts relevant information from the query result, and returns the information as a list of dictionaries.

Next, I read in a CSV file containing City Bike data for London and used the `sample()` function to randomly select 250 locations from the dataset. The name, latitude and longitude columns from the sampled data were then extracted and stored as separate variables. This sampling approach was effective because it reduced the computational load and avoided hitting API rate limits, while still providing a representative sample of locations across the city of London.

To store the query results, the `foursquare_restaurant_data_list` list was defined. The code then looped through each location in the sample, called the `bike_radius_info()` function to query the Foursquare API for restaurants within a 250-metre radius of each location, and appended the results to the `foursquare_restaurant_data_list` list. To avoid exceeding the API rate limit, a 1-second delay was added between each iteration.

Finally, the resulting list of queried data was converted to a pandas DataFrame and exported as a CSV file for further analysis. This approach allows for a comprehensive overview of the restaurant landscape surrounding CityBike locations across London.

The above steps were repeated for three other queries related to points of interest located within a 1025000-metre radius of each CityBike location: a further FourSquare parks request, as well as Yelp parks and restaurants requests.

### Step 3: Joining CityBikes, Yelp, and FourSquare

##### Data Joining, Primary Cleaning, and Review
The third phase of this project involved joining the data collected from the City Bikes, Yelp, and Foursquare API requests. As the data from the Foursquare API request was not very comprehensive, I decided not to join the Yelp and Foursquare API requests together, but to have two separate groups of data. The first group consisted of the Yelp and City Bikes information, while the second group consisted of the City Bikes and Foursquare information.

To start, I imported the five CSV files created in the first two phases. These files contained data about London CityBikes, two Yelp datasets (one for parks and one for restaurants), and two Foursquare datasets (one for parks and one for restaurants). To differentiate between the two types of locations, I added a new column called "Type" to each of the Yelp and Foursquare datasets. This column indicated whether the location was a park or a restaurant. I then renamed the "name" column in the CityBikes DataFrame to "location" so that it could be merged with the Foursquare datasets. Similarly, I renamed the "location_name" column in the Foursquare datasets to "location" to make the merging process easier.

The next step was to merge the Yelp and Foursquare datasets based on the "location" column and concatenate them together. After this, I repeated the process with the City Bikes dataset, resulting in two separate merged datasets: one for Yelp and City Bikes and one for Foursquare and City Bikes. Before proceeding, I removed the "id" and "fsq_id" columns from the merged datasets as they did not add any useful information. Finally, I exported the merged datasets to CSV files and loaded them back in as new DataFrames for further analysis.

##### Exploratory Data Analysis - FourSquare
After collecting the necessary data, I began my exploratory data analysis (EDA). I started with my FourSqaure and CityBikes API Data. However, upon examining the data retireved from the FourSquare API request, it became apparent that the information available about restaurants and parks within a 250m radius of the CityBike locations was insufficient for a thorough analysis.

![FourSquare Describe Stats](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare%20Describe%20Stats%20EDA.png)

![FourSquare Summary Stats](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare%20Summary%20Stats%20EDA.png)

To address this issue, I had added a "type" column during my initial data review to distinguish between parks and restaurants, with the intention of then running a regression analysis on the number of available bikes versus the number of parks and restaurants. However, further examination of the categories table and how different points of interest were categorized revealed that the API was including various categories that were not a good fit for parks and restaurants. For parks this was the inclusion of beer gardens, car parks (i.e. parking lots), playgrounds, corporate business parks, and outdoor food and drink patios. For restaurants it was pulling in parks, hotels, car parks, office buildings, or anywhere that has a form of food service attached. Thus, typifying each row as a park or restaurant could lead to misleading or inaccurate regression results. 

![FourSquare DataFrame Sample](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare_Df_Sample.png)

Regression analysis is a powerful tool for examining relationships between variables, but having all necessary data is crucial for obtaining accurate and informative results. Without access to important information such as ratings, prices, IDs, and categories, attempting to perform regression analysis would be ineffective and potentially misleading. Therefore, the decision was made to exclude the FourSquare data from the study and focus on alternative sources of information.

To address this issue in the future, it might be helpful to manually review the categories assigned by FourSquare's API and cross-reference them with the expected categories. This could ensure that only relevant points of interest are included in the analysis. Additionally, exploring alternative data sources could provide more comprehensive and accurate data for analysis. 

Exploratory Data Analysis - Yelp
I first looked at a sample from the Yelp DataFrame to ensure it was formatted correctly and that the data contained inside made sense. 

![FourSquare DataFrame Sample](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/Yelp%20Parks%20Data%20Sample.png)

I then wanted to see what types of data had been collected. Therefore, I ran a describe query for the Yelp DataFrame, which provided a summary of the data types and non-null count for each column in a Pandas DataFrame. The Yelp DataFrame has 9 columns, each with a specific data type. The first three columns, "name," "location," and "categories," are object data types, which typically represent strings. The fourth column, "rating," is a float64 data type, which represents floating-point numbers. The fifth column, "Type," is also an object data type. The sixth and seventh and columns, "latitude" and "longitude," are float64 data types. The eighth column "free_bikes," is an integer data type. Finally, the ninth column, "price," is an object data type and has 2300 non-null values, meaning there are 1272 null values.

![Yelp Describe Stats EDA](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare%20Summary%20Stats%20EDA.png)

After running the describe function on the Yelp DataFrame, I was able to generate a summary of the central tendency, dispersion, and shape of the data's distribution, excluding NaN values. This gave me a better understanding of what the data was showing. The Yelp DataFrame contains information on 3572 restaurants and parks in London. The average rating for businesses in the DataFrame is 3.81 out of 5, indicating that overall satisfaction levels are relatively high in the region. While the latitude and longitude values for each entry allow for mapping the locations of the businesses, they are not relevant in terms of summary statistics. The DataFrame also shows that the average number of available bikes is 14, with a maximum of 63 bikes available in a 250m proximity for a single business. Additionally, businesses are priced from 1 to 4, with an average price of 1.98. However, it is important to note that there are only 2300 non-null values, mostly for free parks across the region, so almost a third of the data is NaN or free.

![Yelp Summary Stats EDA](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/Yelp%20Summary%20Stats%20EDA.png)

The information provided in the summary statistics can help to identify potential variables to include in a regression analysis. For example, the average rating for businesses in the DataFrame could be a dependent variable, with independent variables such as the number of available bikes, price, and location (latitude and longitude) being tested for their relationship to the dependent variable. The fact that the latitude and longitude values are not important in terms of summary statistics means they may not be good independent variables for the regression analysis.

Additionally, since almost one-third of the data on price is missing, it may be necessary to exclude this variable from the analysis or impute a 0 value, as we know the park locations where no price data is included are because they are free of charge.

Based on a scatter plot analysis comparing rating of nearby restaurants and parks and number of available bikes, it appears that there is no strong correlation between the number of parks and restaurants in a particular location and the number of available bikes at CityBikes. The plot does not show any clear pattern or trend that would suggest a strong relationship between these variables. However, it is possible that other factors not included in this analysis could be influencing the availability of bikes at CityBikes.

![Yelp Rating vs Available Bikes](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/yelp_citybikes_scatterplot.png)

Further information may be gathered in step four as part of regression analysis. 


### Step 4:


## Results
(fill in what you found about the comparative quality of API coverage in your chosen area and the results of your model.)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)





#Presentation Guideline - Make ppt. (Or figure out how to add in images and graphs to GitHub)

- Min project flow
- 2-3 min results
- 1 min challenges and other goals
