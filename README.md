# Global Weather Trends Analysis
![image](images/globe.png)
## Background
In the Python API project, our exploration takes us through the intricate world of data analytics, combining Python programming, API utilization, and sophisticated data visualization. The core of this project lies in analyzing weather patterns across a diverse range of over 500 cities globally, particularly focusing on their spatial relationship to the equator. Leveraging the OpenWeatherMap API and the citipy Python library, we embark on a journey to gather and interpret extensive weather data. The project unfolds in two pivotal segments: WeatherPy and VacationPy. While WeatherPy is committed to assembling and examining weather data to unearth the interplay between various weather parameters and latitude, VacationPy brings an additional dimension to this analysis. In VacationPy, we utilize the Geoapify API, a critical tool that aids in identifying ideal vacation spots based on specific weather preferences, effectively integrating geographical mapping into our data-driven narrative. This endeavor is not only a testament to the practical application of data analytics skills but also highlights the importance of integrating different APIs and robust data visualization in extracting and presenting meaningful insights from complex and diverse datasets.
## Objective
The objective of this project is twofold, encompassing both WeatherPy and VacationPy.
### WeatherPy
In WeatherPy, the goal is to develop a Python script that visualizes weather data from over 500 cities at varying distances from the equator, utilizing the citipy Python library and the OpenWeatherMap API. This script, developed using Jupyter notebook, aims to create a comprehensive model representing weather patterns across these cities. The primary tasks include generating random geographic coordinates, identifying the nearest cities, and then plotting key weather variables (such as temperature, humidity, cloudiness, and wind speed) against latitude. Additionally, this part involves computing and interpreting linear regressions for each weather variable, separated into Northern and Southern Hemispheres, to uncover any significant trends or correlations.
### VacationPy
VacationPy, the second part of the project, shifts the focus to utilizing weather data for vacation planning. Using Jupyter notebooks, the geoViews Python library, and the Geoapify API, the objective is to create map visualizations representing potential vacation destinations based on specific weather criteria. This involves filtering the weather data for ideal conditions, like preferred temperature range, wind speed, and cloudiness, and then using the Geoapify API to locate hotels within these selected cities. The final deliverable is an interactive map displaying each city and its corresponding hotel, enriched with additional data such as humidity levels and geographical coordinates, to facilitate informed vacation planning.
## API Key Acquisition and Setup
To fully utilize the functionalities of this project, you'll need to obtain API keys from OpenWeatherMap and Geoapify. Here's a step-by-step guide to help you through the process:

### OpenWeatherMap API Key
1. **Register on OpenWeatherMap**: Visit [OpenWeatherMap's sign-up page](https://home.openweathermap.org/users/sign_up) and create an account.
2. **Generate API Key**: Once logged in, navigate to the `API keys` tab on your dashboard. You can generate a new key here. Note that it might take a few hours for the new key to become active.
3. **Storing Your API Key**: For security, it's recommended not to hardcode your API key into your scripts. Instead, you can store it in a separate file (e.g., `config.py`) and import it into your main script.
### Geoapify API Key
1. **Sign Up on Geoapify**: Go to the [Geoapify registration page](https://www.geoapify.com/) and create an account.
2. **Create an API Key**: After logging in, navigate to your dashboard and create a new API Key specifically for this project.
3. **Securely Using the API Key**: Similar to the OpenWeatherMap key, store this key in a separate file and import it as needed in your scripts.

### Best Practices for API Key Security
* **Never commit your API keys to public repositories**: Always add the file containing your API keys to `.gitignore` to prevent accidental upload to GitHub or other version control systems.
* **Environment Variables**: For an extra layer of security, consider using environment variables to store your API keys.

By following these steps, you'll have the necessary API keys to successfully run and interact with the APIs used in this project. Remember to keep these keys confidential to protect your account and avoid unauthorized usage.
## Implementation
In the implementation section, we'll delve into the specifics of how we utilized Python, various APIs, and geospatial analysis tools to methodically gather, process, and visualize weather data, culminating in the identification of ideal vacation destinations.
### WeatherPy
#### Environment and Libraries Initialization
```python
%config InlineBackend.figure_format = 'svg'

# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time
from scipy.stats import linregress

# Import the OpenWeatherMap API key
from api_keys import weather_api_key

# Import citipy to determine the cities based on latitude and longitude
from citipy import citipy
```
Begin by configuring your Jupyter Notebook to display high-quality SVG format plots. Then, import essential libraries: `matplotlib.pyplot` for plotting graphs, `pandas` for data handling, `numpy` for numerical calculations, and `requests` for API calls. For statistical analysis, include `linregress` from `scipy.stats`. Lastly, import your OpenWeatherMap API key from an external file for secure API access and use `citipy` to identify cities based on latitude and longitude.
#### Generating Random City Coordinates 
```python
# Empty list for holding the latitude and longitude combinations
lat_lngs = []

# Empty list for holding the cities names
cities = []

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)

# Create a set of random lat and lng combinations
lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
print(f"Number of cities in the list: {len(cities)}")
```
#### Generating Random City Coordinates
```python
# Empty list for holding the latitude and longitude combinations
lat_lngs = []

# Empty list for holding the cities names
cities = []

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)

# Create a set of random lat and lng combinations
lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
print(f"Number of cities in the list: {len(cities)}")
```
In this step, you'll start by preparing two empty lists: one for latitude-longitude pairs and another for city names. Define the range for latitudes and longitudes to span the entire globe. Generate 1500 random latitude and longitude values within these ranges. Then, iterate through these coordinates, using `citipy` to find the nearest city to each pair. Add each unique city to your city list, ensuring a diverse selection for your weather analysis. After completing the loop, check the number of cities gathered to ensure you have a robust dataset for the next stages of your project.
#### Weather Data Collection from API
```python
# Set the API base URL
url =  "http://api.openweathermap.org/data/2.5/weather?"

# Define an empty list to fetch the weather data for each city
city_data = []

# Print to logger
print("Beginning Data Retrieval     ")
print("-----------------------------")

# Create counters
record_count = 1
set_count = 1

# Loop through all the cities in our list to fetch weather data
for i, city in enumerate(cities):
        
    # Group cities in sets of 50 for logging purposes
    if (i % 50 == 0 and i >= 50):
        set_count += 1
        record_count = 0

    # Create endpoint URL with each city
    city_url = f"{url}q={city}&appid={weather_api_key}&units=imperial"
    
    # Log the url, record, and set numbers
    print("Processing Record %s of Set %s | %s" % (record_count, set_count, city))

    # Add 1 to the record count
    record_count += 1

    # Run an API request for each of the cities
    try:
        # Parse the JSON and retrieve data
        city_weather = requests.get(city_url).json()

        # Parse out latitude, longitude, max temp, humidity, cloudiness, wind speed, country, and date
        city_lat = city_weather['coord']['lat']
        city_lng = city_weather['coord']['lon']
        city_max_temp = city_weather['main']['temp_max']
        city_humidity = city_weather['main']['humidity']
        city_clouds = city_weather['clouds']['all']
        city_wind = city_weather['wind']['speed']
        city_country = city_weather['sys']['country']
        city_date = city_weather['dt']

        # Append the City information into city_data list
        city_data.append({"City": city, 
                          "Lat": city_lat, 
                          "Lng": city_lng, 
                          "Max Temp": city_max_temp,
                          "Humidity": city_humidity,
                          "Cloudiness": city_clouds,
                          "Wind Speed": city_wind,
                          "Country": city_country,
                          "Date": city_date})

    # If an error is experienced, skip the city
    except:
        print("City not found. Skipping...")
        pass
              
# Indicate that Data Loading is complete 
print("-----------------------------")
print("Data Retrieval Complete      ")
print("-----------------------------")
```
Here, you'll set up an API request to gather weather data for each city in your list. Start by defining the base URL for the OpenWeatherMap API. You'll then loop through each city, dynamically creating the full URL for each API request. For every city, the script makes a GET request to the API, retrieving key weather data such as latitude, longitude, maximum temperature, humidity, cloudiness, wind speed, country, and the date of the weather data. This information is stored in a list. The loop includes error handling to skip any cities that are not found. After processing all cities, a message confirms the completion of data retrieval.
#### Creating DataFrame from Collected Weather Data
```python
# Convert the cities weather data into a Pandas DataFrame
city_data_df = pd.DataFrame(city_data)

# Show Record Count
city_data_df.count()
```
This step involves converting the gathered weather data into a Pandas DataFrame for easier analysis and manipulation. By feeding the `city_data` list into `pd.DataFrame()`, each dictionary in the list becomes a row in the DataFrame. After creating the DataFrame, you can use `city_data_df.count()` to get a count of non-null values for each column.