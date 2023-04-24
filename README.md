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

##### Step 4: EDA Analysis

After collecting the necessary data from each API, I began my exploratory data analysis (EDA). As a first step in my EDA,  I decided to compare the number of parks and the number of restaurants that were found in both a 250 M radius of bike stations on Foursquare and Yelp.

![Yelp vs FourSquare Parks and Restaurants Bar Chart](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/Number%20of%20Parks%20on%20Restaurants%20on%20Yelp%20and%20FourSquare.png)

The resulting bar chart (shown above) reveals a large discrepancy between the number of parks listed on Foursquare and Yelp, with approximately 1447% more parks listed on Foursquare than Yelp. Conversely, the difference between the number of restaurants listed on Yelp and Foursquare is smaller, with approximately 18% more restaurants listed on Yelp than Foursquare.

These findings raise interesting questions about the potential biases or limitations of each platform, and will require further exploration and analysis. For example, it is possible that Foursquare is more popular among park-goers, while Yelp is more popular among restaurant enthusiasts. Alternatively, there could be differences in how each platform categorizes and catergorizes their data. Further investigation of each Dataframe from Yelp and FourSquare will be needed to better understand these discrepancies and their implications for our analysis.


##### Exploratory Data Analysis - FourSquare

Upon first glance, the descriptive and summary data from the FourSquare DataFrame appeared to be normal and consistent with what I might find in terms of bike numbers. However, there were no available price or rating statistics to be able to run a regression analysis.

![FourSquare Describe Stats](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare%20Describe%20Stats%20EDA.png)

![FourSquare Summary Stats](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare%20Summary%20Stats%20EDA.png)

To address this concern, I added a "type" column during my initial data review to distinguish between the number of parks and restaurants within a 250-meter radius of bike stations. The intention of adding the type variable was to be able to run a regression analysis on the number of available bikes versus the number of parks and restaurants in each area.

I then looked at a sample from the FourSquare DataFrame to ensure it was formatted correctly and that the data contained inside made sense. However, further examination of the categories table and how different points of interest were categorized revealed that the API was including various categories that were not a good fit for parks and restaurants. For parks this was the inclusion of beer gardens, car parks (i.e. parking lots), playgrounds, corporate business parks, and outdoor food and drink patios. For restaurants it was pulling in parks, hotels, car parks, office buildings, or anywhere that has a form of food service attached. Thus, typifying each row as a park or restaurant could lead to misleading or inaccurate regression results. 

![FourSquare DataFrame Sample](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare_Df_Sample.png)

Although regression analysis is a powerful tool for examining relationships between variables, it relies on having all necessary data is crucial for obtaining accurate and informative results. Therefore, without access to important information such as ratings and prices, and with incorrectly catergorized types of parks, I did not feel that attempting to perform regression analysis would be effective. I also felt that it could lead to potentially misleading results. Therefore, I made the decision to exclude the FourSquare data from the study and focus on alternative sources of information.

To address this issue in the future, it might be helpful to manually review the categories assigned by FourSquare's API and cross-reference them with the expected categories. This could ensure that only relevant points of interest are included in the analysis. Additionally, exploring alternative data sources could provide more comprehensive and accurate data for analysis. 

##### Exploratory Data Analysis - Yelp

I also first looked at a sample from the Yelp DataFrame to ensure it was formatted correctly and that the data contained inside made sense. 

![Yelp DataFrame Sample](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/Yelp%20Parks%20Data%20Sample.png)

I then wanted to see what types of data had been collected. Therefore, I ran a describe query for the Yelp DataFrame, which provided a summary of the data types and non-null count for each column in a Pandas DataFrame. 

![Yelp Describe Stats EDA](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/FourSquare%20Summary%20Stats%20EDA.png)

The Yelp DataFrame has 9 columns, each with a specific data type. The first three columns, "name," "location," and "categories," are object data types, which typically represent strings. The fourth column, "rating," is a float64 data type, which represents floating-point numbers. The fifth column, "Type," is also an object data type. The sixth and seventh and columns, "latitude" and "longitude," are float64 data types. The eighth column "free_bikes," is an integer data type. Finally, the ninth column, "price," is an object data type and has 2300 non-null values, meaning there are 1272 null values.


After running the describe function on the Yelp DataFrame, I was able to generate a summary of the central tendency, dispersion, and shape of the data's distribution, excluding NaN values. This gave me a better understanding of what the data was showing. 

![Yelp Summary Stats EDA](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/Yelp%20Summary%20Stats%20EDA.png)

The Yelp DataFrame contains information on 3572 restaurants and parks in London. The average rating for businesses in the DataFrame is 3.81 out of 5, indicating that overall satisfaction levels are relatively high in the region. While the latitude and longitude values for each entry allow for mapping the locations of the businesses, they are not relevant in terms of summary statistics. The DataFrame also shows that the average number of available bikes is 14, with a maximum of 63 bikes available in a 250m proximity for a single business. Additionally, businesses are priced from 1 to 4, with an average price of 1.98. However, it is important to note that there are only 2300 non-null values, mostly for free parks across the region, so almost a third of the data is NaN or free.

The information provided in the summary statistics can help to identify potential variables to include in a regression analysis. For example, the average rating for businesses in the DataFrame could be a dependent variable, with independent variables such as the number of available bikes, price, and location (latitude and longitude) being tested for their relationship to the dependent variable. The fact that the latitude and longitude values are not important in terms of summary statistics means they may not be good independent variables for the regression analysis.

Additionally, since almost one-third of the data on price is missing, it may be necessary to exclude this variable from the analysis or impute a 0 value, as we know the park locations where no price data is included are because they are free of charge.

Finally, i created a scatter plot comparing the ratings of nearby restaurants and parks and number of available bikes. 

![Yelp Rating vs Available Bikes](https://github.com/Brittanysacha/Statistical-Modelling-with-Python/blob/main/images/yelp_citybikes_scatterplot.png)

From a review of the chart, the does not appear to be a strong correlation between the number of parks and restaurants in a particular location and the number of available bikes at CityBikes. This is because the data points are fairly dispersed, and there are a significant number of bike stations with a low number of nearby restaurants/parks and vice versa.
Nevertheless, the scatter plot still suggests that there is some positive relationship between the number of available bikes and the number of nearby restaurants/parks, as we see a general trend of higher bike availability corresponding with higher numbers of nearby restaurants/parks.


### Step 4: Results

Based on my analysis of the data, the quality of Yelp's API coverage in London appears to be insufficient for providing accurate and reliable predictions of the number of free bikes available. Although the model shows some potential for predicting the number of free bikes available, the results are not particularly reliable or accurate. The low R-squared value of 0.001 indicates that only a tiny fraction of the variance in the dependent variable can be explained by the independent variables used in the model. Furthermore, the F-statistic of 2.568 and its associated probability suggest that the model as a whole is not statistically significant, which further calls into question its overall quality. However, it is worth noting that the coefficient for the price_num variable is statistically significant, indicating that it has a noticeable impact on the number of free bikes available. In contrast, the coefficient for the rating variable is not statistically significant.

The model also underwent three additional tests: the Durbin-Watson test, the Jarque-Bera test, and AIC-BIC values. The Durbin-Watson test checked for the presence of autocorrelation in the residuals of a regression analysis, while the Jarque-Bera test determined whether the residuals of a regression analysis are normally distributed. The AIC and BIC values are measures of the quality of the model fit. Together, these three tests suggest that the model fit is not of high quality and should be approached with caution. 

The low Durbin-Watson statistic suggests that the model's residuals may have correlation, which means that errors may be correlated with each other over time. This violates one of the assumptions of linear regression and may lead to unreliable predictions. The Jarque-Bera test indicates that the residuals are not normally distributed, which means the data is not capturing all of the relevant factors that affect the dependent variable, or that there are other sources of variability that are not accounted for in the model. Lastly, the AIC and BIC values suggest that the model fit is not optimal, which is a further indication that the model should be used with care. Overall, these tests suggest that the model may not be suitable for predicting the number of free bikes available with a high degree of accuracy and that alternative sources of information or more robust models may be needed.


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
