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
					<!-- <v-expansion-panel-content
						v-for="weatherData in weatherDataList"
						:key="weatherData.id"
					> -->
					<v-expansion-panel-content>
						<div class="chart-block">
							<bar-chart
								v-if="loaded"
								:chartdata="maxTempData"
								:chartlabels="chartlabels"
								label="Max Temp"
								title="Max Tempture"
							/>
						</div>
						<div class="chart-block">
							<bar-chart
								v-if="loaded"
								:chartdata="minTempData"
								:chartlabels="chartlabels"
								label="Min Temp"
								title="Min Tempture"
							/>
						</div>
						<div class="chart-block">
							<pie-chart 
								v-if="loaded"
								:chartdata="humidityData"
								:chartlabels="chartlabels"
								label="Humidity"
								title="Humidity"
							/>
						</div>
						<!-- <p>Date: {{ weatherData.applicable_date }}</p>
						<p>
							Weather State: {{ weatherData.weather_state_name }}
						</p>
						<p>Min Temp: {{ Math.round(weatherData.min_temp) }}</p>
						<p>Max Temp: {{ Math.round(weatherData.max_temp) }}</p>
						<p>Humidity: {{ weatherData.humidity }}</p> -->
					</v-expansion-panel-content>
				</v-expansion-panel>
			</v-expansion-panels>
		</div>
	</div>
</template>

<script>
import axios from "axios";
import BarChart from "@/components/BarChart";
import PieChart from "@/components/PieChart";
export default {
	name: "Weather",
	components: {
		BarChart,
		PieChart
	},
	data() {
		return {
			searchedLocation: this.value,
			weatherCityList: [],
			weatherDataList: [],
			loaded: false,
			maxTempData: null,
			minTempData: null,
			humidityData: null,
			chartlabels: [],
		};
	},
	methods: {
		searchDate() {
			axios
				.get(
					`https://www.metaweather.com/api/location/search/?query=` +
						this.searchedLocation
				)
				.then((res) => {
					this.weatherCityList = res.data;
				})
				.catch((e) => {
					this.errors.push(e);
				});
		},
		details(woeId) {
			this.loaded = false;
			axios
				.get(`https://www.metaweather.com/api/location/` + woeId)
				.then((res) => {
					this.weatherDataList = res.data.consolidated_weather;
					this.maxTempData = this.weatherDataList.map(
						(e) => `${Math.round(e.max_temp)}`
					);
					this.minTempData = this.weatherDataList.map(
						(e) => `${Math.round(e.min_temp)}`
					);
					this.humidityData = this.weatherDataList.map(
						(e) => `${e.humidity}`
					);
					this.chartlabels = this.weatherDataList.map(
						(e) => `${e.applicable_date}`
					);
					this.loaded = true;
				})
				.catch((error) => {
					this.errors.push(error);
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
.chart-block {
	max-width: 300px;
	margin: 50px 10px;
	display: inline-block;
}
</style>
