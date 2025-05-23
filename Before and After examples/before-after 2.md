2. Forecast Query Parser
   
Before:
def parse_forecast_details(query):
    if "rain" in query:
        return "rain"
    elif "temp" in query:
        return "temperature"
    return None

Analysis of initial code:
Simple keyword match
No support for multiple weather attributes or date
Not extensible or flexible

After:
def parse_forecast_details(query):
    q_lower = query.lower()
    if "today" in q_lower: date_spec = "today"
    elif "tomorrow" in q_lower: date_spec = "tomorrow"
    elif "weekend" in q_lower: date_spec = "weekend"

    keywords = {
        "temperature": ["temperature", "temp", "hot", "cold"],
        "rain": ["rain", "umbrella"],
        "humidity": ["humidity", "humid"],
        "wind": ["wind", "breeze"],
        "condition": ["weather", "forecast"],
        "sunrise_sunset": ["sunrise", "sunset"]
    }

    for key, words in keywords.items():
        for word in words:
            if word in q_lower:
                attribute = key
                break

    return {"date_spec": date_spec, "attribute": attribute}

Why my prompting was effective:
Extracts both weather attribute and date
Handles flexible phrases
Easily expandable for more keywords
