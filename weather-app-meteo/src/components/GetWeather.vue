//GetWeather.vue
<script lang="ts">
//env
const accessToken = import.meta.env.VITE_OPENWEATHER_API;
//imports
import { Geolocation } from "@capacitor/Geolocation";

//get current position
const getCurrentPosition = async () => {
  const coordinates = await Geolocation.getCurrentPosition();
  const positionInfo = {
    lat: coordinates.coords.latitude,
    lon: coordinates.coords.longitude,
  };
  return positionInfo;
};

//get current weather
const getCurrentWeather = async (lat: any, lon: any, lang = "en", units = "metric") => {
  const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${accessToken}&lang=${lang}&units=${units}`;
  const response = await fetch(url);
  const data = await response.json();
  console.log(data);
  const weatherInfo = {
    name: data.name,
    temp: data.main.temp,
    feels_like: data.main.feels_like,
    temp_min: data.main.temp_min,
    temp_max: data.main.temp_max,
    weather: data.weather[0].main,
    weather_description: data.weather[0].description,
    weather_icon: data.weather[0].icon,
    sunrise: data.sys.sunrise,
    sunset: data.sys.sunset,
  };
  //   console.log(cityInfo);
  return weatherInfo;
};

//geocoding city name
const fetchCity = async (city: String, lang = "en") => {
  const url = `https://api.openweathermap.org/geo/1.0/direct?q=${city}&limit=1&appid=${accessToken}`;
  const response = await fetch(url);
  const data = await response.json();
  console.log(data);
  const cityInfo = {
    name: data[0].name,
    langName: data[0].local_names[lang],
    lat: data[0].lat,
    lon: data[0].lon,
  };
  return cityInfo;
};

export { fetchCity, getCurrentWeather, getCurrentPosition };
</script>
