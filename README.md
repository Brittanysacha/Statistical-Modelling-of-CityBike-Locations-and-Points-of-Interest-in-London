Project-Statistical-Modelling-with-Python

## Project/Goals
(fill in your description and goals here)

## Process
This project had several steps aimed at gaining a comprehensive understanding of different points of interest in close proximity to CityBike locations across a certain region. The region selected for this project was the city of London, England. Below are some of the steps involved in the process of completing this project:

### Step 1: Connecting and Retrieving CityBikes API Information
The first step of this project was to connect to the CityBikes API and retrieve information on CityBike locations within London. To achieve this, I wrote a code that retrieves bike station information from the Santander Cycles network by sending HTTP GET requests to the CityBikes API using the requests library. Initially, I ran a request to get general information about the network such as the latitude, longitude, and the number of available bikes at each station. In the second request, I specified the fields to include in the response JSON content, such as the station name, location, latitude, longitude, and the number of available bikes at each station. The code then loops through the list of stations to extract this information. Finally, the retrieved data is saved in a pandas DataFrame and exported to a CSV file to be used later in the project.

### Step 2: Connecting and Retrieving FourSquare and Yelp API Information on CityBikes Locations

The second step of this project involved gathering information about points of interest located within a 1000-metre radius of each CityBike location. I began by looking for restaurants within a 1000-metre radius of each CityBike location using code that utilised the Foursquare API. Firstly, I set up the API credentials using the `os.environ.get()` function and connected to the Greater London API network. Next, I defined a function called `restaurant_foursquare_query()` that takes latitude and longitude coordinates as input and sends an HTTP GET request to the API endpoint URL with the given query parameters. The function then returns the JSON content from the response.

To gather information for each CityBike location, I wrote a function called `bike_radius()` that takes latitude and longitude coordinates as input, calls `restaurant_foursquare_query()`, and returns the results of the query. Finally, I wrote a function called `bike_radius_info()` that takes latitude, longitude, and location name as input, calls `restaurant_foursquare_query()`, extracts relevant information from the query result, and returns the information as a list of dictionaries.

Next, I read in a CSV file containing City Bike data for London and used the `sample()` function to randomly select 250 locations from the dataset. The name, latitude and longitude columns from the sampled data were then extracted and stored as separate variables. This sampling approach was effective because it reduced the computational load and avoided hitting API rate limits, while still providing a representative sample of locations across the city of London.

To store the query results, the `foursquare_restaurant_data_list` list was defined. The code then looped through each location in the sample, called the `bike_radius_info()` function to query the Foursquare API for restaurants within a 1000-metre radius of each location, and appended the results to the `foursquare_restaurant_data_list` list. To avoid exceeding the API rate limit, a 1-second delay was added between each iteration.

Finally, the resulting list of queried data was converted to a pandas DataFrame and exported as a CSV file for further analysis. This approach allows for a comprehensive overview of the restaurant landscape surrounding CityBike locations across London.

The above steps were repeated for three other queries related to points of interest located within a 1000-metre radius of each CityBike location: a further FourSquare parks request, as well as Yelp parks and restaurants requests.

### Step 3:

### Step 4:
### Step 5: 


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