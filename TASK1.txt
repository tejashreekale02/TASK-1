import requests
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns


API_KEY = "76009d813914fb0396a32441e9d779c9"
CITY = "London"  
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

def fetch_weather_data(city, api_key):
    """Fetch weather data from OpenWeatherMap API."""
    url = f"{BASE_URL}?q={city}&appid={api_key}&units=metric"
    response = requests.get(url)
    if response.status_code == 200:
        return response.json()
    else:
        print(f"Error fetching data: {response.status_code}")
        return None

def parse_weather_data(data):
    """Parse relevant weather data from the API response."""
    if not data:
        return None
    weather_info = {
        "City": data["name"],
        "Temperature (°C)": data["main"]["temp"],
        "Humidity (%)": data["main"]["humidity"],
        "Wind Speed (m/s)": data["wind"]["speed"],
        "Weather Condition": data["weather"][0]["main"],
    }
    return weather_info

def create_visualizations(weather_info):
    """Create visualizations using matplotlib and seaborn."""
    if not weather_info:
        return

    # Convert data to a DataFrame for easier plotting
    df = pd.DataFrame([weather_info])

    # Set up the figure and subplots
    plt.figure(figsize=(12, 6))

    # Plot 1: Temperature
    plt.subplot(1, 3, 1)
    sns.barplot(x=df["City"], y=df["Temperature (°C)"], palette="coolwarm")
    plt.title("Temperature in °C")
    plt.ylabel("Temperature (°C)")

    # Plot 2: Humidity
    plt.subplot(1, 3, 2)
    sns.barplot(x=df["City"], y=df["Humidity (%)"], palette="Blues")
    plt.title("Humidity in %")
    plt.ylabel("Humidity (%)")

    # Plot 3: Wind Speed
    plt.subplot(1, 3, 3)
    sns.barplot(x=df["City"], y=df["Wind Speed (m/s)"], palette="Greens")
    plt.title("Wind Speed in m/s")
    plt.ylabel("Wind Speed (m/s)")

    # Adjust layout and display
    plt.tight_layout()
    plt.show()

def main():
    # Fetch weather data
    weather_data = fetch_weather_data(CITY, API_KEY)
    if not weather_data:
        return

    # Parse relevant data
    weather_info = parse_weather_data(weather_data)
    if not weather_info:
        return

    # Print weather info
    print("Weather Information:")
    for key, value in weather_info.items():
        print(f"{key}: {value}")

    # Create visualizations
    create_visualizations(weather_info)

if __name__ == "__main__":
    main()