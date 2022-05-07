<template>
	<div :style="'background-image: url('+require(`./assets/${data.background}.png`)+')'">
		<input type="text" v-model="data.location" @change="updateData">
		<main>
			<GlassComponent :main="data.temperature" :side="data.weather">°C</GlassComponent>
			<section>
				<GlassComponent :main="data.feelsLike" side="Feels Like">°C</GlassComponent>
				<GlassComponent :main="data.humidity" side="Humidity">%</GlassComponent>
				<GlassComponent :main="data.pressure" side="Pressure">mbar</GlassComponent>
				<GlassComponent :main="data.windspeed" side="Wind Speed">Km/h</GlassComponent>
			</section>
		</main>
	</div>
</template>

<script>

import GlassComponent from './components/GlassComponent.vue'

	export default {
		name: 'App',
		components: {
			GlassComponent
		},
		data() {
			return {
				data: {
					location: 'Kozhikode',
					background: 'Clear_Day',
					temperature: null,
					weather: null,
					feelsLike: null,
					humidity: null,
					pressure: null,
					windspeed: null
				}
			}
		},
		beforeMount() {
			this.updateData()
		},
		methods: {
			updateData() {
				if (this.data.location.trim().length === 0)
					return
				fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.data.location}&units=metric&appid=7638a6a66e6374d974d5bc9ed55e1ced`)
				.then(response => response.json())
				.then(data => {
					this.data.location = data.name
					this.data.temperature = data.main.temp
					this.data.weather = data.weather[0].main
					this.data.feelsLike = data.main.feels_like
					this.data.humidity = data.main.humidity
					this.data.pressure = data.main.pressure
					this.data.windspeed = data.wind.speed

					const weatherCode = Number(data.weather[0].icon.slice(0, 2))
					const time = data.weather[0].icon.slice(2) === 'd' ? 'Day': 'Night'
					if (weatherCode < 3)
						this.data.background = `Clear_${time}`
					else if (weatherCode < 5)
						this.data.background = `Cloud_${time}`
					else if (weatherCode < 12)
						this.data.background = `Rain_${time}`
					else
						this.data.background = `Snow_${time}`
				})
				.catch(() => alert("Location Not Found"))
			}
		}
	}

</script>

<style>

	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
	}

	#app > div {
		height: 100vh;
		background-size: cover;
		background-repeat: no-repeat;
		background-position: 50%;
	}

	input {
		position: fixed;
		top: 5%;
		left: 50%;
		height: max(5%, 75px);
		width: min(80%, 500px);
		margin: auto;
		background-color: transparent;
		transform: translateX(-50%);
		font-size: 36px;
		font-weight: bold;
		color: #FFF;
		text-shadow: 0 4px 4px #0004;
		border: none;
		outline: none;
		text-align: center;
	}
	input:hover,
	input:focus,
	input:active {
		background-color: #FFF3;
	}

	main {
		position: fixed;
		bottom: 5vh;
		left: 5vw;
		display: flex;
		flex-direction: column;
		justify-content: space-evenly;
		align-items: center;
		width: 90vw;
		height: 75vh;
	}
	main > div {
		aspect-ratio: 1 !important;
		transform: scale(1.2);
	}

	section {
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		row-gap: 5vh;
		flex-wrap: wrap;
		width: 80%;
	}
	.glass {
		width: 40%;
		aspect-ratio: 18/19;
	}

	@media screen and (orientation: landscape) {
		#app > div {
		background-size: cover;
		}
		
		input {
			color: #000;
		}

		main {
			left: 10%;
			flex-direction: row;
			justify-content: space-between;
			width: 80%;
		}
		section {
			width: 45%;
		}
		main > .glass {
			width: 25%;
		}
	}

</style>