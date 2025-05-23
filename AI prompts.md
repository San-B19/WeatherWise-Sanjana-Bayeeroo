Tool: ChatGPT
Date: 17-05-2025

Prompt 1 
Me:
Hi, I want to create a Python function that gets a 5-day weather forecast from OpenWeatherMap. Can you help me start with that?

AI:
Sure! To get started, you'll need to use the OpenWeatherMap API and make a request to the 5-day forecast endpoint. Here's a basic function to get the weather data using a city name:

```python
import requests

def get_weather_data(city, api_key):
    url = f"http://api.openweathermap.org/data/2.5/forecast?q={city}&appid={api_key}&units=metric"
    response = requests.get(url)
    data = response.json()
    return data


Prompt 2

Me:
Now I want the weather forecast to work with any location name, even small towns. Can we convert the city name into coordinates first?

AI:
Yes, to improve accuracy, it's better to geocode the location name into latitude and longitude. You can use geopy with Nominatim to do this. Here's how you can integrate geolocation into your weather function:

from geopy.geocoders import Nominatim

geolocator = Nominatim(user_agent="weather_app")

def get_coordinates(location_name):
    location = geolocator.geocode(location_name)
    if location:
        return location.latitude, location.longitude
    return None, None

url = f"http://api.openweathermap.org/data/2.5/forecast?lat={lat}&lon={lon}&appid={api_key}&units=metric"


Prompt 3
Me:
I want my app to accept natural language queries like “Is it going to rain tomorrow in Paris?” Can you help me extract useful parts like the date and weather type?

AI:
Yes! You can use a simple keyword-based parser using regular expressions to detect keywords like "tomorrow", "rain", "temperature", etc. Here's an example:

import re

def parse_forecast_details(query):
    date_spec = None
    attribute = None

    if "today" in query.lower():
        date_spec = "today"
    elif "tomorrow" in query.lower():
        date_spec = "tomorrow"

    if "rain" in query.lower():
        attribute = "rain"
    elif "temperature" in query.lower():
        attribute = "temperature"

    return {"date_spec": date_spec, "attribute": attribute}


Prompt 4
Me:
Can you help me make two charts: one for temperature and one for precipitation over the next 5 days using matplotlib?

AI:
Certainly! You'll first need to convert the forecast JSON into a pandas DataFrame. Then you can plot the temperature and rain/snow values. Here's a basic example for the temperature chart:
import matplotlib.pyplot as plt
import pandas as pd
from datetime import datetime

def prepare_viz_data(weather_data):
    data_list = []
    for entry in weather_data['list']:
        data_list.append({
            'time_utc': datetime.strptime(entry['dt_txt'], '%Y-%m-%d %H:%M:%S'),
            'temperature': entry['main']['temp'],
            'rain_3h': entry.get('rain', {}).get('3h', 0),
            'snow_3h': entry.get('snow', {}).get('3h', 0),
        })
    df = pd.DataFrame(data_list).set_index('time_utc')
    return df

def create_temperature_visualisation(df):
    plt.plot(df.index, df['temperature'], marker='o', linestyle='-')
    plt.title("5-Day Temperature Forecast")
    plt.xlabel("Date")
    plt.ylabel("Temperature (°C)")
    plt.grid(True)
    plt.show()


Prompt 5
Me:
I want to make my app interactive. It should ask the user for a location, then what kind of weather info they want, and finally respond with both text and chart. Can we build this loop?

AI:
Absolutely! You can use pyinputplus for better input handling. Here's how you could structure your main() loop:

import pyinputplus as pyip

def main():
    print("Welcome to WeatherWise!")
    while True:
        location = pyip.inputStr("Enter a location (or type 'exit' to quit): ")
        if location.lower() == "exit":
            break
        weather_data, error = get_weather_data(location)
        if error:
            print("Error:", error)
            continue
        query = pyip.inputStr("What do you want to know? (e.g., temperature tomorrow): ")
        parsed = parse_forecast_details(query)
        response = generate_weather_response(parsed, weather_data)
        print(response)
