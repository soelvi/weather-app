<template>
	<main>
		<p class="weather__title">  {{ title }} </p>
		<div class="weather__location">
			<select v-model="currentCity">
				<!-- v-for fetches the data for one chosen city -->
				<option v-for="city in allCities" :key="city">
					{{ city }}
				</option>
			</select> 
			<br>
			<button @click="fetchData">choose city</button>
		</div>
		
		<div class="weather_days">
			<section class="weather__today">
				<p class="weather__days--title"> today: </p> 
					<!-- v-if displays random weather icon every time page is reloaded -->
				  <p v-if="selectedImage"><img :src="selectedImage" alt="icon skies" class="weather__windIcon-bigger"></p>
				<div>{{ weather.temperature }} </div>
				<div>{{ weather.description }} </div>
					<img src="/public/images/fluent_weather-duststorm-20-regular.png" alt="icon wind" class="weather__windIcon-smaller">
				<div>{{ weather.wind }}</div>
			</section>
			<p class="weather__days--title">tomorrow:</p>
			<section class="weather__forecast">
				{{ weather.forecast[0].temperature }} 
				/
				{{ weather.forecast[0].wind }} 
			</section>
			<p class="weather__days--title">day after tomorrow:</p> 
			<section class="weather__forecast">
				{{ weather.forecast[1].temperature }}
				/
				{{ weather.forecast[1].wind }} 
			</section>
		</div>
	</main>
</template>

<script>
export default {
	data() {
		return {
			title: 'Whats the weather in:',
			weather: {},
			error: '',
			currentCity: 'Oslo',
			allCities: [ "Oslo", "Bergen", "Tromsø", "Kristiansand", "Stavanger", "Kautokeino", "Trondheim", "Lillehammer" ],
			images: [
				'/images/fluent_weather-cloudy-20-regular.png',
				'/images/fluent_weather-drizzle-20-regular.png',
				'/images/fluent_weather-fog-20-regular.png',
				'/images/fluent_weather-hail-day-20-regular.png',
				'/images/fluent_weather-haze-20-regular.png',
				'/images/fluent_weather-partly-cloudy-day-20-regular.png',
				'/images/fluent_weather-rain-20-regular.png',
				'/images/fluent_weather-rain-showers-day-20-regular.png',
				'/images/fluent_weather-rain-snow-20-regular.png',
				'/images/fluent_weather-snow-20-regular.png',
				'/images/fluent_weather-snow-shower-day-20-regular.png',
				'/images/fluent_weather-sunny-20-regular.png',
				'/images/fluent_weather-thunderstorm-20-regular.png',
			],
    		selectedImage: null
		}
	},

	created() {
		this.fetchData();
		this.selectedImage = this.randomIcon(this.images); // fetches the images from the array and selected image will randomly generate an icon 
	},

	methods: {
		async fetchData() {
			const url = `https://goweather.herokuapp.com/weather/${this.currentCity}`; // currentCity is chosen in menu from data allCities
			const response = await fetch(url);
			try {
				await this.handleResponse(response); 
			} catch (error) {
				this.error = error.message;
			}
		},

		async handleResponse(response) {
			if (response.status >= 200 && response.status < 300) {
				const results = await response.json();
				this.weather = results;
				return true; 
			}	else	{
				if(response.status === 404) {
					throw new Error('Url ikke funnet..');
				}
				if(response.status === 401) {
					throw new Error('Ikke autorisert ');
				}
				if(response.status > 500) {
					throw new Error('Server error!');
				}
				throw new Error (' Woops, noe gikk galt!');
			}
		},

		randomIcon(icons) {
      	return icons[Math.floor(Math.random()*icons.length)]; //returns random icon from images array passed to this function
    	}
	}
}
</script>

<style>

div {
	padding-bottom: var(--padding-bottom-small);
}

main {
	width: 100vw;
	display: flex;
	flex-direction: column;
	justify-content: space-evenly;
	text-align: center;
	align-content: center;
}

select {
	margin: 10px;
	padding: 5px;
	font-size: var(--font-size);
	font-family: var(--font-family);
	color: var(--title-color);
	border-radius: var(--border-radius);
	text-align: center;
	background-color: var(--foreground);
	border-style: none;
	box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
}

button {
	padding: var(--padding-small);
	border-radius: var(--border-radius);
	background-color: var(--colored-details);
	margin-bottom: var(--margin-bottom-small);
	box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
}

.weather__title {
	margin-top: var(--margin-top-big);
	color: var(--colored-details);
}

.weather__days--title {
	padding-bottom: var(--padding-bottom-small);
}

.weather__today {
	margin-right: var(--margin-left-right);
	margin-left: var(--margin-left-right);
	background: var(--foreground);
	margin-bottom: var(--margin-bottom-small);
	border-radius: var(--border-radius);
	padding: var(--padding-big);
	box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px, rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
}

.weather__forecast {
	padding: var(--padding-big);
	margin-right: var(--margin-left-right);
	margin-left: var(--margin-left-right);
	background: var(--foreground);
	margin-bottom: var(--margin-bottom-small);
	border-radius: var(--border-radius);
	box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px, rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
}

.weather__windIcon-bigger {
	width: 100px;
	height: 100px;
}

.weather__windIcon-smaller {
	width: 50px;
	height: 50px;
}

/* 800px tablet and smaller -- mobile */
@media screen and (max-width: 800px) { 
	.weather__today {
		width: 60%;
		margin-left: 20%;
	}

	.weather__forecast {
		width: 60%;
		margin-left: 20%;
	}
}
</style>

