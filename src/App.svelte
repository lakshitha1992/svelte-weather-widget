<script>
	import axios from "axios";
	import { onMount } from "svelte";
	import { fade, slide } from "svelte/transition";

	let weatherData = null;
	let loading = true;
	let errorMessage = "";
	let city = "";
	let suggestions = [];
	const apiKey = "YOUR_OPENWEATHERMAP_API_KEY";
	const geonamesApi = "YOUR_GEONAMES_USERNAME";

	const fetchWeather = async () => {
		if (!city) {
			errorMessage = "Please enter a valid city.";
			return;
		}
		loading = true;
		errorMessage = "";
		weatherData = null;

		try {
			const response = await axios.get(
				`https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`
			);
			weatherData = response.data;
		} catch (error) {
			errorMessage = error.response?.data?.message || "Could not fetch weather data. Please try again.";
		} finally {
			loading = false;
		}
	};

	const fetchCities = async (query) => {
		if (!query) {
			suggestions = [];
			return;
		}

		try {
			const response = await axios.get(
				`http://api.geonames.org/searchJSON?name_startsWith=${query}&maxRows=10&username=${geonamesApi}`
			);

			if (response.data.geonames.length === 0) {
				errorMessage = "No cities found. Please try again with a valid city name.";
			} else {
				suggestions = response.data.geonames.map((city) => city.name);
			}
		} catch (error) {
			console.error("City suggestion error:", error);
			errorMessage = "Error fetching city suggestions. Please try again.";
			suggestions = [];
		}
	};

	const selectCity = (selectedCity) => {
		city = selectedCity;
		suggestions = [];
		fetchWeather();
	};

	const getWeatherAnimation = (condition) => {
		const conditionMap = {
			clear: "gif/sunny.gif",
			rain: "gif/rain.gif",
			clouds: "gif/cloudy.gif",
			thunderstorm: "gif/storm.gif",
			snow: "gif/snow.gif",
			drizzle: "gif/drizzle.gif",
			mist: "gif/mist.gif",
			fog: "gif/foggy.gif",
		};
		return conditionMap[condition.toLowerCase()] || "gif/default.gif";
	};

	const handleCityInput = (e) => fetchCities(e.target.value);

	onMount(() => {
		if (city) fetchWeather();
	});
</script>

<style>
	.widget {
		max-width: 350px;
		margin: 20px auto;
		padding: 20px;
		background: #ffffff;
		color: #333333;
		border: 1px solid #e0e0e0;
		border-radius: 10px;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
		text-align: center;
		font-family: Arial, sans-serif;
	}

	.loading,
	.error {
		color: #ff4d4f;
		font-size: 1.2rem;
		margin-top: 20px;
	}

	input {
		width: calc(100% - 20px);
		padding: 10px;
		margin-bottom: 10px;
		border: 1px solid #cccccc;
		border-radius: 5px;
		font-size: 1rem;
	}

	.autocomplete {
		position: relative;
	}

	.suggestions {
		position: absolute;
		background: #ffffff;
		color: #333333;
		list-style: none;
		margin: 0;
		padding: 0;
		width: 100%;
		border-radius: 5px;
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
		z-index: 10;
	}

	.suggestion {
		padding: 10px;
		cursor: pointer;
	}

	.suggestion:hover {
		background: #007bff;
		color: #ffffff;
	}

	button {
		background-color: #007bff;
		color: #ffffff;
		border: none;
		padding: 10px 20px;
		font-size: 1rem;
		border-radius: 5px;
		cursor: pointer;
		margin-top: 10px;
	}

	button:hover {
		background-color: #0056b3;
	}

	.weather-icon {
		width: 150px;
		margin: 20px auto;
	}
</style>

<div class="widget">
	<h2>Weather Widget</h2>
	<div class="autocomplete">
		<input
			type="text"
			bind:value={city}
			placeholder="Enter city name"
			on:input={(e) => fetchCities(e.target.value)}
		/>
		{#if suggestions.length}
			<ul class="suggestions">
				{#each suggestions as suggestion}
					<li
						class="suggestion"
						on:click={() => selectCity(suggestion)}
					>
						{suggestion}
					</li>
				{/each}
			</ul>
		{/if}
	</div>
	<button on:click={fetchWeather}>Get Weather</button>

	{#if loading}
		<div class="loading" in:fade>
			Loading...
		</div>
	{:else if errorMessage}
		<div class="error" in:fade>
			{errorMessage}
		</div>
	{:else if weatherData}
		<div in:slide>
			<h3>{weatherData.name}</h3>
			<p>{weatherData.weather[0].description}</p>
			<img
				src={`./${getWeatherAnimation(weatherData.weather[0].main)}`}
				alt="weather animation"
				class="weather-icon"
			/>
			<h1>{Math.round(weatherData.main.temp)}°C</h1>
			<p>Humidity: {weatherData.main.humidity}%</p>
			<p>Wind: {weatherData.wind.speed} m/s</p>
		</div>
	{/if}
</div>
