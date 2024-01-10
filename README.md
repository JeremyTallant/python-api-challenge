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
Begin by configuring the Jupyter Notebook to display high-quality SVG format plots. Then, import essential libraries: `matplotlib.pyplot` for plotting graphs, `pandas` for data handling, `numpy` for numerical calculations, and `requests` for API calls. For statistical analysis, include `linregress` from `scipy.stats`. Lastly, import a OpenWeatherMap API key from an external file for secure API access and use `citipy` to identify cities based on latitude and longitude.

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
In this step, we'll start by preparing two empty lists: one for latitude-longitude pairs and another for city names. Define the range for latitudes and longitudes to span the entire globe. Generate 1500 random latitude and longitude values within these ranges. Then, iterate through these coordinates, using `citipy` to find the nearest city to each pair. Add each unique city to the city list, ensuring a diverse selection for the weather analysis. After completing the loop, check the number of cities gathered to ensure we have a robust dataset for the next stages of the project.

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
Here, we'll set up an API request to gather weather data for each city in the list. Start by defining the base URL for the OpenWeatherMap API. We'll then loop through each city, dynamically creating the full URL for each API request. For every city, the script makes a GET request to the API, retrieving key weather data such as latitude, longitude, maximum temperature, humidity, cloudiness, wind speed, country, and the date of the weather data. This information is stored in a list. The loop includes error handling to skip any cities that are not found. After processing all cities, a message confirms the completion of data retrieval.

#### Creating DataFrame from Collected Weather Data
```python
# Convert the cities weather data into a Pandas DataFrame
city_data_df = pd.DataFrame(city_data)

# Show Record Count
city_data_df.count()
```
This step involves converting the gathered weather data into a Pandas DataFrame for easier analysis and manipulation. By feeding the `city_data` list into `pd.DataFrame()`, each dictionary in the list becomes a row in the DataFrame. After creating the DataFrame, we can use `city_data_df.count()` to get a count of non-null values for each column.

#### Previewing Weather Data
```python
# Display sample data
city_data_df.head()
```
Now, take a first look at the weather data by displaying the top five entries of your DataFrame. The `city_data_df.head()` function is used to preview the initial rows, giving us a quick snapshot of the dataset structure and the type of weather information collected for each city.

#### Exporting Data to CSV 
```python
# Export the City_Data into a csv
city_data_df.to_csv("output_data/cities.csv", index_label="City_ID")
```
This step involves saving our collected city weather data to a CSV file. Using `city_data_df.to_csv()`, the DataFrame is written to a file named `cities.csv` in the `output_data` directory. The `index_label` parameter is set to "City_ID", which adds a column in the CSV for the DataFrame's index, labeling it as 'City_ID'. This is useful for easy reference and future data processing tasks.

#### Loading Data from CSV
```python
# Read saved data
city_data_df = pd.read_csv("output_data/cities.csv", index_col="City_ID")

# Display sample data
city_data_df.head()
```
In this step, we'll reload the previously saved weather data from the CSV file into a Pandas DataFrame. Use `pd.read_csv()` to read the file, setting `index_col` to "City_ID" to use it as the DataFrame index. After loading, preview the first few rows with `city_data_df.head()` to confirm the data is loaded correctly and to familiarize ourselves with its structure once again.

#### Latitude vs. Temperature Scatter Plot
```python
# Build scatter plot for latitude vs. temperature
plt.scatter(city_data_df['Lat'], city_data_df['Max Temp'], edgecolors='black', marker='o', alpha=0.8)

# Incorporate the other graph properties
plt.title('City Latitude vs. Temperature (%s)' % time.strftime('%x'))
plt.ylabel('Max Temperature (F)')
plt.xlabel('Latitude')
plt.grid(True)

# Save the figure
plt.savefig("output_data/Fig1.png")

# Show plot
plt.show()
```
We're now creating a scatter plot that compares city latitude to their maximum temperature. With black-edged, semi-transparent markers, the plot is both clear and informative. It includes a title reflecting the current date, and labels for both axes, along with a grid for easier interpretation. After configuring these properties, we save the plot as 'Fig1.png' in the 'output_data' directory and then display it on screen.

#### Latitude vs. Humidity Scatter Plot
```python
# Build the scatter plots for latitude vs. humidity
plt.scatter(city_data_df['Lat'], city_data_df['Humidity'], edgecolors='black', marker='o', alpha=0.8)

# Incorporate the other graph properties
plt.title('City Latitude vs. Humidity (%s)' % time.strftime('%x'))
plt.ylabel('Humditity (%)')
plt.xlabel('Latitude')
plt.grid(True)

# Save the figure
plt.savefig("output_data/Fig2.png")

# Show plot
plt.show()
```
We're constructing a scatter plot to illustrate the relationship between city latitude and humidity. This visualization features black-edged, semi-transparent markers for clarity. The plot is enhanced with a current date title and appropriately labeled axes. After fine-tuning these elements, we save the plot as 'Fig2.png' in the 'output_data' folder and then display it for review.

#### Latitude vs. Cloudiness Scatter Plot
```python
# Build the scatter plots for latitude vs. cloudiness
plt.scatter(city_data_df['Lat'], city_data_df['Cloudiness'], edgecolors='black', marker='o', alpha=0.8)

# Incorporate the other graph properties
plt.title('City Latitude vs. Cloudiness (%s)' % time.strftime('%x'))
plt.ylabel('Cloudiness (%)')
plt.xlabel('Latitude')
plt.grid(True)

# Save the figure
plt.savefig("output_data/Fig3.png")

# Show plot
plt.show()
```
In this step, we're creating a scatter plot to examine the correlation between city latitude and cloudiness levels. The plot features black-edged markers with a degree of transparency for easy visualization. It includes a title indicating the current date, along with axes labeled for latitude and cloudiness percentage. Once the plot is tailored with these attributes, we save it as 'Fig3.png' in the 'output_data' directory and then display it on the screen.

#### Latitude vs. Wind Speed Scatter Plot
```python
# Build the scatter plots for latitude vs. wind speed
plt.scatter(city_data_df['Lat'], city_data_df['Wind Speed'], edgecolors='black', marker='o', alpha=0.8)

# Incorporate the other graph properties
plt.title('City Latitude vs. Wind Speed (%s)' % time.strftime('%x'))
plt.ylabel('Wind Speed (m/s)')
plt.xlabel('Latitude')
plt.grid(True)

# Save the figure
plt.savefig("output_data/Fig4.png")

# Show plot
plt.show()
```
We're now creating a scatter plot that maps city latitude against wind speed. The plot is designed with black-edged, semi-transparent markers for easy reading. It's enhanced with a title displaying the current date and axes labeled for latitude and wind speed (in meters per second). After setting these features, we save the plot as 'Fig4.png' in the 'output_data' folder and display it for visualization.

#### Defining a Linear Regression Plot Function
```python
# Define a function to create Linear Regression plots
def plot_linear_regression(x_values, y_values, x_label, y_label, annotation_coordinates):
    (slope, intercept, rvalue, pvalue, stderr) = linregress(x_values, y_values)
    print(f"The r-value is: {rvalue**2}")
    
    # Calculate the regression line "y values" from the slope and intercept
    regress_values = x_values * slope + intercept
    
    # Create a scatter plot of the x and y values
    plt.scatter(x_values, y_values)
    
    # Plot the regression line with the x-values and the calculated y-values
    plt.plot(x_values, regress_values, "r-")
    
    # Annotate the plot with the linear equation
    line_eq = f"y = {slope:.2f}x + {intercept:.2f}"
    plt.annotate(line_eq, annotation_coordinates, fontsize=15, color="red")
    
    # Set the x and y labels
    plt.xlabel(x_label)
    plt.ylabel(y_label)
    
    plt.show()
```
We've defined a function, `plot_linear_regression`, to create and display scatter plots with linear regression lines. This function takes in x and y values, labels for these axes, and coordinates for annotating the regression equation. Inside the function, we calculate the linear regression parameters, plot the data points, draw the regression line, and annotate the plot with the regression equation. This utility makes it easy to visualize relationships between different variables and understand their correlation.

#### Extracting Northern Hemisphere Data
```python
# Create a DataFrame with the Northern Hemisphere data (Latitude >= 0)
northern_hemi_df = city_data_df[city_data_df['Lat'] >= 0]

# Display sample data
northern_hemi_df.head()
```
We're filtering the original dataset to create a new DataFrame, `northern_hemi_df`, containing only cities from the Northern Hemisphere (latitude >= 0). This focused dataset will be used for specific analyses related to the Northern Hemisphere. After the data is filtered, we preview the first few rows using `northern_hemi_df.head()` to verify the selection and to get an initial look at the Northern Hemisphere data.

#### Extracting Southern Hemisphere Data
```python
# Create a DataFrame with the Southern Hemisphere data (Latitude < 0)
southern_hemi_df = city_data_df[city_data_df['Lat'] < 0]

# Display sample data
southern_hemi_df.head()
```
Here, we're creating a DataFrame, `southern_hemi_df`, which includes only cities located in the Southern Hemisphere (latitude < 0). This subset allows for targeted analysis of weather patterns specific to this hemisphere. After filtering the data, a quick preview of the first few entries is displayed using `southern_hemi_df.head()` to confirm the dataset's accuracy and get a glimpse of the Southern Hemisphere cities.

#### Linear Regression for Max Temp in the Northern Hemisphere 
```python
# Linear regression on Northern Hemisphere
plot_linear_regression(northern_hemi_df['Lat'], northern_hemi_df['Max Temp'], 'Latitude', 'Max Temp', (10, -30))
```
We're applying the `plot_linear_regression` function to the Northern Hemisphere data, focusing on the relationship between latitude and maximum temperature. By passing the latitude and maximum temperature data from `northern_hemi_df`, along with appropriate axis labels and annotation coordinates, the function will generate a scatter plot with a linear regression line, helping to visualize and understand how temperature varies with latitude in the Northern Hemisphere.

#### Linear Regression for Max Temp in the Southern Hemisphere 
```python
# Linear regression on Southern Hemisphere
plot_linear_regression(southern_hemi_df['Lat'], southern_hemi_df['Max Temp'], 'Latitude', 'Max Temp', (-50, 90))
```
In this step, the `plot_linear_regression` function is utilized for the Southern Hemisphere data, analyzing the correlation between latitude and maximum temperature. The latitude and maximum temperature columns from `southern_hemi_df` are used as inputs, along with corresponding axis labels. The annotation coordinates are set to ensure clear visibility of the regression line equation, thereby facilitating an insightful understanding of temperature trends relative to latitude in the Southern Hemisphere.

#### Linear Regression for Humidity in the North Hemisphere 
```python
# Northern Hemisphere
plot_linear_regression(northern_hemi_df['Lat'], northern_hemi_df['Humidity'], 'Latitude', 'Humidity', (40, 20))
```
Here, we're employing the `plot_linear_regression` function to examine the relationship between latitude and humidity within the Northern Hemisphere. Latitude values and humidity levels from `northern_hemi_df` are input into the function, along with labels for the respective axes. An annotation is strategically placed to clearly present the regression line equation, aiding in the interpretation of how humidity changes with latitude in the Northern Hemisphere.

#### Linear Regression for Humidity in the Southern Hemisphere 
```python
# Southern Hemisphere
plot_linear_regression(southern_hemi_df['Lat'], southern_hemi_df['Humidity'], 'Latitude', 'Humidity', (-25, 20))
```
In this analysis, we use the `plot_linear_regression` function to explore the link between latitude and humidity in the Southern Hemisphere. By inputting latitude and humidity data from `southern_hemi_df` into the function, along with appropriate axis labels, the function will generate a scatter plot with a regression line. The specified annotation coordinates help to display the regression equation, offering insights into how humidity varies with latitude in this hemisphere.

#### Linear Regression for Cloudiness in the Northern Hemisphere 
```python
# Northern Hemisphere
plot_linear_regression(northern_hemi_df['Lat'], northern_hemi_df['Cloudiness'], 'Latitude', 'Cloudiness', (50, 10))
```
For this step, we're applying the `plot_linear_regression` function to study the relationship between latitude and cloudiness in the Northern Hemisphere. The function takes the latitude and cloudiness data from `northern_hemi_df`, along with corresponding axis labels. The selected annotation coordinates ensure the regression equation is clearly visible on the scatter plot. This visualization aids in understanding how cloudiness trends correlate with latitude across the Northern Hemisphere.

#### Linear Regression for Cloudiness in the Southern Hemisphere 
```python
# Southern Hemisphere
plot_linear_regression(southern_hemi_df['Lat'], southern_hemi_df['Cloudiness'], 'Latitude', 'Cloudiness', (-25, 25))
```
In this analysis, we're using the `plot_linear_regression` function to investigate the relationship between latitude and cloudiness in the Southern Hemisphere. The function is provided with latitude and cloudiness data from `southern_hemi_df`, as well as labels for these axes. By setting the annotation coordinates, the regression line equation is displayed clearly on the plot, offering insights into how cloudiness varies with latitude in the Southern Hemisphere.

#### Linear Regression for Wind Speed in the Northern Hemisphere 
```python
# Northern Hemisphere
plot_linear_regression(northern_hemi_df['Lat'], northern_hemi_df['Wind Speed'], 'Latitude', 'Wind Speed', (0, 30))
```
Here, we employ the `plot_linear_regression` function to analyze the correlation between latitude and wind speed in the Northern Hemisphere. The function uses latitude and wind speed data from `northern_hemi_df` and includes axis labels for clarity. The annotation coordinates are chosen to ensure the regression equation is prominently displayed, aiding in the interpretation of wind speed patterns relative to latitude in the Northern Hemisphere.

#### Linear Regression for Wind speed in the Southern Hemisphere 
```python
# Sourthern Hemisphere
plot_linear_regression(southern_hemi_df['Lat'], southern_hemi_df['Wind Speed'], 'Latitude', 'Wind Speed', (-25, 25))
```
In this step, we're utilizing the `plot_linear_regression` function to explore the relationship between latitude and wind speed in the Southern Hemisphere. The function processes latitude and wind speed data from `southern_hemi_df`, complemented by relevant axis labels. The chosen annotation coordinates help in clearly showcasing the regression line equation on the plot, providing insights into how wind speed varies with latitude across the Southern Hemisphere.

### VacationPy
#### Setting Up for Map Visualization and API Requests
```python
# Dependencies and Setup
import hvplot.pandas
import pandas as pd
import requests

# Import API key
from api_keys import geoapify_key
```
In this phase, we're preparing for map visualization and API interactions. We import `hvplot.pandas` for advanced interactive visualizations, `pandas` for data handling, and `requests` for making HTTP requests. Additionally, the Geoapify API key is imported from an external file, ensuring secure access for geolocation services. This setup is essential for subsequent steps involving geospatial data analysis and map-based visualizations.

