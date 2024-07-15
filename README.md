# laslos
import requests
import pandas as pd

API_KEY = 'ваш_ключ_API'
CITIES = ['Beijing', 'Tokyo', 'Seoul', 'Shanghai', 'Hong Kong']

def get_weather_data(city):
    url = f'http://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}'
    response = requests.get(url)
    return response.json()

def categorize_weather(weather_description):
    if 'clear' in weather_description:
        return 'Sunny'
    elif 'cloud' in weather_description:
        return 'Cloudy'
    else:
        return 'Other'

def main():
    weather_data = []
    
    for city in CITIES:
        city_weather = get_weather_data(city)
        weather_description = city_weather['weather'][0]['description']
        category = categorize_weather(weather_description)
        weather_data.append({'City': city, 'Weather': category})

    df = pd.DataFrame(weather_data)
    print(df)

    sunny_days = df[df['Weather'] == 'Sunny'].shape[0]
    cloudy_days = df[df['Weather'] == 'Cloudy'].shape[0]
    
    print(f'Total Sunny Days: {sunny_days}')
    print(f'Total Cloudy Days: {cloudy_days}')

if __name__ == "__main__":
    main()
