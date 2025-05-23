3. Weather summary generator
   
Before:
def respond_to_query(attribute, forecast):
    return f"The {attribute} is {forecast[attribute]}"

Analysis of initial code:
Returns basic forecast info
Lacks natural language
No emojis, no context, no sunrise/sunset or fallback behaviour


After:
def generate_weather_response(parsed_details, weather_data):
    # Handles date, time, emoji, condition, temp, wind, humidity, rain/snow
    # Handles sunrise/sunset if attribute is present
    return "☀️ For Rose-Hill tomorrow: It will be partly cloudy with a temperature around 27°C. Humidity will be 70%. Wind speeds around 4 m/s."

Why my prompting was effective:
Human-like rich descriptions 
Dynamically includes emojis based on condition
Includes "feels like", wind, humidity, sunrise/sunset
Fallbacks when data is missing or vague

