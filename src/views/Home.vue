<template>
	<div class="weather">
		<p class="weather__title">  {{ title }} </p>
		<h1 class="weather__location"> insert city here (bergen) </h1>

	<div class="weather_days">
		<section class="weather__today">
			today: 
			{{ weather.temperature }}
			{{ weather.wind }}
			{{ weather.description }}
		</section>
		<section class="weather__tomorrow">
			tomorrow: 
			{{ weather.forecast[0] }}
		</section>
		<section class="weather__afterTomorrow">
			day after tomorrow: 
			{{ wa }}
		</section>
	</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			title: 'Whats the weather in:',
			weather: {},
			error: '',
			days: [
			"today", "tomorrow", "afterTomorrow"
			]
		}
	},

	created() {
			this.fetchData();
	},

	methods: {
		async fetchData() {
			const url = `https://goweather.herokuapp.com/weather/bergen`;
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
			}
		}
	}
}
</script>

