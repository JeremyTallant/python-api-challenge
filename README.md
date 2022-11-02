# Python API-Challenge
![equatorsign](https://user-images.githubusercontent.com/112406455/199588838-d6db71a7-6026-4185-997a-3277ebbd3ac9.png)
## Background
Data's true power is its ability to definitively answer questions. So, let's take what you've learned about Python requests, APIs, and JSON traversals to answer a fundamental question: "What is the weather like as we approach the equator?"

Now, we know what you may be thinking: “That’s obvious. It gets hotter.” But, if pressed for more information, how would you prove that?

## API Keys
To run the python scripts for both parts, first you will need to obtain two API Keys. The first API key is for the OpenWeatherMap API which is used in part one. Here is the link: "https://home.openweathermap.org/users/sign_up". The second API key is for the Google Maps API which is used in part two. Here is the link: "https://developers.google.com/maps/documentation/geocoding/start". Once you have obtained both keys you will need to place them in the config.py file that you will see in the WeatherPy folder. Then you will have to change the file in both codes after is says # Import API key "from api_keys" to "from config".
## Instructions
This activity is broken down into two deliverables, WeatherPy and VacationPy.
## Part 1: WeatherPy
In this deliverable, you'll create a Python script to visualize the weather of over 500 cities of varying distances from the equator. You'll use the citipy Python library, the OpenWeatherMap API, and your problem-solving skills to create a representative model of weather across cities.

### Requirement 1: Create Plots to Showcase the Relationship Between Weather Variables and Latitude
To fulfill the first requirement, you'll use the OpenWeatherMap API to retrieve weather data from the cities list generated in the starter code. Next, you'll create a series of scatter plots to showcase the following relationships:

Latitude vs. Temperature:

![LatitudeVsTemperaturePlot](https://user-images.githubusercontent.com/112406455/199590532-eee324f1-ff21-46e1-9250-6b6c59b93c6e.png)

Latitude vs. Humidity:

![LatitudeVsHumidtyPlot](https://user-images.githubusercontent.com/112406455/199590598-4c79c218-196a-48f2-a7ef-021978bf6a84.png)

Latitude vs. Cloudiness:

![LatitudeVsCloudinessPlot](https://user-images.githubusercontent.com/112406455/199590690-59ece1dd-4a1f-4a4a-bc7c-8bfbd47fbff9.png)

Latitude vs. Wind Speed:

![LatitudeVsWindSpeedPlot](https://user-images.githubusercontent.com/112406455/199590880-33ba7089-c6a0-4373-8ce1-83c001b1713b.png)

## Requirement 2: Compute Linear Regression for Each Relationship
To fulfill the second requirement, compute the linear regression for each relationship. Separate the plots into Northern Hemisphere (greater than or equal to 0 degrees latitude) and Southern Hemisphere (less than 0 degrees latitude). You may find it helpful to define a function in order to create the linear regression plots.

Next, create a series of scatter plots. Be sure to include the linear regression line, the model's formula, and the r values. After each pair of plots, explain what the linear regression is modeling. Describe any relationships that you notice and any other findings you may uncover.
You should create the following plots:

Northern Hemisphere: Temperature vs. Latitude:

![NorthernHemisphereLatitudeVsTemperaturePlot](https://user-images.githubusercontent.com/112406455/199591696-12d81300-fbc5-4f1d-b8b2-05912c342937.png)

A linear regression model is shown here displaying latitude and maximum temperature of random cities in the northern hemisphere. As can be seen from the plot above, there is a strong, negative correlation between latitude and maximum temperature. In other words, the further a city is from the equator, the lower its maximum temperature will be.

Southern Hemisphere: Temperature vs. Latitude:

![SouthernHemisphereLatitudeVsTemperaturePlot](https://user-images.githubusercontent.com/112406455/199591726-4276d5b2-5dc6-4a34-a081-4453c6086de3.png)

A linear regression model is shown here displaying latitude and maximum temperature of random cities in the southern hemisphere. As can be seen from the plot above, there is a strong, positive correlation between latitude and maximum temperature. Cities closer to the equator, therefore, have a higher maximum temperature.

Northern Hemisphere: Humidity vs. Latitude:

![NorthernHemisphereLatitudeVsHumidityPlot](https://user-images.githubusercontent.com/112406455/199591804-97ae7854-fb55-4de2-8b7c-2216f692c58d.png)

A linear regression model displaying humidity and latitude for random cities in the northern hemisphere is shown above. The two have a very small, positive correlation, but not enough to be significant.

Southern Hemisphere: Humidity vs. Latitude:

![SouthernHemisphereLatitudeVsHumidityPlot](https://user-images.githubusercontent.com/112406455/199591855-0ad7988e-8b0b-4b4c-9044-5564a8296d8d.png)

A linear regression model displaying humidity and latitude for random cities in the southern hemisphere is shown above. Same as the northern hemisphere, the two have a very small, positive correlation, but not enough to be significant.

Northern Hemisphere: Cloudiness vs. Latitude:

![NorthernHemisphereLatitudeVsCloudinessPlot](https://user-images.githubusercontent.com/112406455/199591923-950ceb35-df32-4b9a-b4f4-cd709b300a0e.png)

A linear regression model is shown here displaying the latitude and cloudiness of random cities in the northern hemisphere. As seen from the plot above, there is a weak, positive correlation between the two, but not enough to draw conclusions from.

Southern Hemisphere: Cloudiness vs. Latitude:

![SouthernHemisphereLatitudeVsCloudinessPlot](https://user-images.githubusercontent.com/112406455/199592008-14897a74-6a02-4c7f-83f6-3b7bdb471430.png)

A linear regression model is shown here displaying the latitude and cloudiness of random cities in the southern hemisphere. As seen from the data above, there is a very small, positive correlation between the two, but not enough to draw conclusions from. 

Northern Hemisphere: Wind Speed vs. Latitude:

![NorthernHemisphereLatitudeVsWindSpeedPlot](https://user-images.githubusercontent.com/112406455/199592037-168b31bd-ec9b-420e-ab2f-a0e6f6dea5b7.png)

A linear regression model displaying the latitude and the wind speed for random cities located in the northern hemisphere is shown above. Inferable conclusions cannot be drawn from the data, but there is a very weak, positive correlation between them.

Southern Hemisphere: Wind Speed vs. Latitude:

![SouthernHemisphereLatitudeVsWindSpeedPlot](https://user-images.githubusercontent.com/112406455/199592080-040e69e4-5f1c-4c4b-8399-5a939b7f4e71.png)
 
 A linear regression model displaying the latitude and the wind speed for random cities located in the southern hemisphere is shown above. Inferable conclusions cannot be drawn from the data, but there is a weak, negative correlation between them.
 
 ## Analysis
* Observing the data, we can see that there is a very strong correlation between latitude and max temperature, and that the further away a city is from the equator, the lower the max temperature will be.
* The correlation between humidity, cloudiness, and wind speed vs. latitude did not appear to be significant. As shown in the plots, the linear regression lines were essentially flat. 
* The cloudiness and lattitude plot shows that the majority of cities are either extremely cloudy or not cloudy at all, with fewer cities in between.

## Part 2: VacationPy
In this part you will create two heatmaps using gmaps in jupyter and Google Places API. The first heatmap will contain all the cities that was found in the first part of the assignment and their humidity. The second map will contain markers of cities with ideal weather conditions and display the city's information with a hotel located in a 5,000 mile radius of the city. You might have to run "jupyter nbextension enable --py gmaps" to enable gmaps in your environment. 

First heat map:

![map (2)](https://user-images.githubusercontent.com/112406455/199597003-50740ec6-8f4a-42bc-ae12-e060fff7fabd.png)

Second heat map:

![map (3)](https://user-images.githubusercontent.com/112406455/199597152-960855af-0efc-46fb-a856-327c75628144.png)

## Instructions on how to run the code properly:
* Download the repository onto you computer
* Open the repository in Jupiter Notebook
* First add your api keys to the config.py file 
* Select the WeatherPy.ipynb script located in the WeatherPy folder 
* Select the "Kernel" tab and click "Restart Kernel and Clear all Outputs"
* Then proceed to hit shift + enter to cycle through the script cell by cell
* The code is accompanied by an outline to walk you through the process step by step
* The output could look different from what you see here since the cities are randomly generated every time you cycle through the script. 
* Once finished, you will notice an output_data file with a csv of all the cities and the weather information, and images of all the plots that were created. 
* Next you will want to proceed to the VacationPy.ipynb script, restart the kernel and clear all outputs, and then cycle through the script to generate both heat maps. 

© 2022 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.
