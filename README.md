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