# Svelte Weather Widget

This is a Svelte-based weather widget that allows users to search for cities and get real-time weather data. It fetches weather information using the OpenWeatherMap API and suggests cities using the GeoNames API.

## Features

- Enter a city name to fetch current weather.
- City suggestions while typing.
- Displays weather data such as temperature, humidity, and wind speed.
- Shows an animated weather icon based on the current weather condition.

## Demo

You can view a live demo of the project [here](#).

## Installation

To get started with this project, follow these steps:

### 1. Clone the repository

First, clone this repository to your local machine:

```bash
git clone https://github.com/lakshitha1992/svelte-weather-widget.git
cd svelte-weather-widget

2. Install dependencies
Next, install the required dependencies using npm:

bash
Copy code
npm install

3. Set up your API keys
In order to fetch weather data from OpenWeatherMap and get city suggestions from GeoNames, you'll need to obtain API keys for both services.

OpenWeatherMap API Key

Go to the OpenWeatherMap website.

Sign up or log in to your account.

Navigate to the API section and generate a new API key.

Once you have the key, replace the apiKey variable in src/App.svelte with your newly generated key:

js
Copy code
const apiKey = "YOUR_OPENWEATHERMAP_API_KEY";
GeoNames API Key
Go to the GeoNames website.

Create a free account if you don't already have one.

After logging in, visit the Web Services section, where you'll find your username.

Replace the geonamesApi variable in src/App.svelte with your GeoNames username:

js
Copy code
const geonamesApi = "YOUR_GEONAMES_USERNAME";
4. Run the app locally
After setting up the API keys, you can run the application locally:

bash
Copy code
npm run dev
Your app will be running at http://localhost:5000.

5. Build for production
To build the app for production, run:

bash
Copy code
npm run build
This will create an optimized build of your app in the public folder. You can deploy this build to any static hosting service like Vercel, Netlify, or GitHub Pages.

License
This project is licensed under the MIT License - see the LICENSE file for details.

markdown
Copy code

### Instructions:
- Replace `"YOUR_OPENWEATHERMAP_API_KEY"` with your OpenWeatherMap API key in the `apiKey` variable.
- Replace `"YOUR_GEONAMES_USERNAME"` with your GeoNames username in the `geonamesApi` variable.

This `README.md` file now points to your GitHub repository (`https://github.com/lakshitha1992/svelt