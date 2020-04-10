<template>
	<div id="weather" class="col-md-8 offset-md-2 text-center">
		<h1>Weather App</h1>
		<div class="col-md-8 offset-md-2">
			<v-text-field
				label="Enter the location"
				width="300"
				v-model="searchedLocation"
			></v-text-field>
		</div>
		<v-btn small color="primary" @click="searchDate" class="mb-5"
			>Search</v-btn
		>
		<div v-show="weatherCityList != ''">
			<v-expansion-panels>
				<v-expansion-panel
					@click="details(weatherCity.woeid)"
					v-for="weatherCity in weatherCityList"
					:key="weatherCity.woeid"
				>
					<v-expansion-panel-header>
						{{ weatherCity.title }}
					</v-expansion-panel-header>
					<v-expansion-panel-content
						v-for="weatherData in weatherDataList"
						:key="weatherData.id"
					>
						<p>Date: {{ weatherData.applicable_date }}</p>
						<p>
							Weather State: {{ weatherData.weather_state_name }}
						</p>
						<p>Min Temp: {{ Math.round(weatherData.min_temp) }}</p>
						<p>Max Temp: {{ Math.round(weatherData.max_temp) }}</p>
						<p>Humidity: {{ weatherData.humidity }}</p>
						<hr />
					</v-expansion-panel-content>
				</v-expansion-panel>
			</v-expansion-panels>
		</div>
	</div>
</template>

<script>
import axios from "axios";
export default {
	name: "Weather",
	data() {
		return {
			searchedLocation: this.value,
			weatherCityList: [],
			weatherDataList: [],
		};
	},
	methods: {
		searchDate() {
			console.log(this.searchedLocation);
			axios
				.get(
					`https://www.metaweather.com/api/location/search/?query=` +
						this.searchedLocation
				)
				.then((res) => {
					this.weatherCityList = res.data;
					console.table(this.weatherCityList);
				})
				.catch((e) => {
					this.errors.push(e);
				});
		},
		details(woeId) {
			console.log(woeId);
			axios
				.get(`https://www.metaweather.com/api/location/` + woeId)
				.then((res) => {
					this.weatherDataList = res.data.consolidated_weather;
					console.table(this.weatherDataList);
				})
				.catch((e) => {
					this.errors.push(e);
				});
		},
	},
};
</script>
<style lang="scss" scoped>
input {
	width: 50%;
	height: 20px;
	border: 2px solid #23a6b8;
}
button {
	margin: 0 10px;
}
</style>
