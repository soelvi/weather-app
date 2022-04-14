<template>
	<div class="weather">
		<p class="weather__title">  {{ title }} </p>
		<h1 class="weather__location"> Oslo </h1>

		<section class="weather__today">

		</section>
		
	</div>
</template>

<script>
export default {
	data() {
		return {
			title: 'Whats the weather in:'
		}
	},

	created() {
			this.fetchData();
	},

	methods: {
		async fetchData() {
			const url = ``
		}
	}
}
</script>

