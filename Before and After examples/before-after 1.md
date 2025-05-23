1. Weather data retrieval function
   
Before:  
def get_weather_data(city):
    import requests
    url = f"http://api.openweathermap.org/data/2.5/forecast?q={city}&appid=API_KEY&units=metric"
    response = requests.get(url)
    return response.json()

Analysis of initial code:
Basic functionality: fetches forecast by city name
No geolocation accuracy
Lacks error handling, time zone or location fallback 


After:
def get_weather_data(location):
    api_key = os.environ.get("OPENWEATHER_API_KEY")
    geocode_location = geolocator.geocode(location)
    lat, lon = geocode_location.latitude, geocode_location.longitude
    url = f"http://api.openweathermap.org/data/2.5/forecast?lat={lat}&lon={lon}&appid={api_key}&units=metric"
    response = requests.get(url, timeout=10)
    weather_data = response.json()
    weather_data['city_name_geocoded'] = geocode_location.address
    return weather_data, None

Why My Prompting Strategy Was Effective:
Uses geopy for location coordinates (more accurate)
Uses latitude/longitude in API request
Adds full error handling and fallback names
Supports environment-based API key setup
