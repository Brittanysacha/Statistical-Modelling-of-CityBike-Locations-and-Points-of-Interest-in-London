# Statistical Modelling of CityBike Locations and Points of Interest in London

## Project/Goals
The goal of this project was to retrieve API data from CityBikes, Yelp, and FourSquare to determine points of interest (POI) locations such as restaurants and parks within a 250m radius of each bike point. Regression analysis was conducted to examine the relationship between the types of POI locations nearby and CityBike locations, specifically looking for correlations between higher ratings or prices and the number of available bikes.

## Process
### Step 1: Connecting and Retrieving CityBikes API Information
- Connected to the CityBikes API to retrieve information on CityBike locations in London.
- Sent HTTP GET requests to the API to obtain data on bike station information, including latitude, longitude, and the number of available bikes.
- Extracted the required information, saved it in a pandas DataFrame, and exported it as a CSV file.

### Step 2: Connecting and Retrieving FourSquare and Yelp API Information on CityBikes Locations
- Utilized the Foursquare and Yelp APIs to gather information on points of interest within a 250m radius of each CityBike location.
- Wrote functions to query the APIs for restaurants and parks based on latitude and longitude coordinates.
- Sampled CityBike locations and retrieved data for each location using the APIs, storing the results in separate CSV files for analysis.

### Step 3: Joining CityBikes, Yelp, and FourSquare Data
- Imported the CSV files containing CityBike, Yelp, and FourSquare data.
- Renamed and organized columns to facilitate data merging.
- Merged the Yelp and Foursquare datasets with the CityBikes dataset separately.
- Removed unnecessary columns and exported the merged datasets for further analysis.

### Step 3B: Exploratory Data Analysis (EDA)
- Conducted EDA on the data to gain insights and identify potential variables for regression analysis.
- Examined the number of parks and restaurants listed on Foursquare and Yelp using bar charts.
- Analyzed summary statistics and data distribution in the Yelp DataFrame.
- Created scatter plots to visualize the relationship between POI ratings, prices, and the number of available bikes.

### Step 4: Regression Analysis and Results
- Built regression models to examine the relationship between POI ratings, prices, and the number of available bikes.
- Evaluated the models using R-squared, F-statistic, and p-values.
- Conducted tests for autocorrelation and normality of residuals.
- Plotted regression analysis results to visualize the relationships between variables.

## Results
- The regression model using Yelp data showed a weak relationship between POI ratings, prices, and the number of available bikes.
- The model's R-squared value was low, indicating that only a small portion of the variance in the number of available bikes could be explained by the independent variables.
- The F-statistic suggested that the model as a whole was not statistically significant.
- The scatter plots showed a minimal effect of POI ratings and prices on the number of available bikes.

## Challenges
- Faced rate-limiting issues when retrieving data from the FourSquare API, which required implementing measures to avoid being rate-limited.
- Initially struggled to understand the different APIs and their data retrieval capabilities.
- Encountered limitations in the data from the FourSquare API, such as missing numerical data and incorrect categorization of POI types.

## Future Goals
- Explore additional APIs and data sources to gather more comprehensive and accurate data for analysis.
- Consider including data from platforms like TripAdvisor for a more robust comparison.
- Conduct separate regression analyses for parks and restaurants to obtain more accurate results.
- Manually review API-assigned categories to ensure relevant points of interest are included.
- Further

For a more comprehensive analysis please see [here](https://github.com/Brittanysacha/London-UK-Citybikes-and-Attractions---Statistical-Modelling-with-Python/blob/main/Comprehensive%20Analysis)
