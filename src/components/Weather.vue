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
		<v-row v-show="weatherCityList != ''">
			<v-expansion-panels>
				<v-expansion-panel
					@click="details(weatherCity.woeid)"
					v-for="weatherCity in weatherCityList"
					:key="weatherCity.woeid"
				>
					<v-expansion-panel-header>
						{{ weatherCity.title }}
					</v-expansion-panel-header>
					<v-expansion-panel-content>
						<v-row>
							<v-col
								cols="2"
								class="details-block"
								v-for="weatherData in weatherDataList"
								:key="weatherData.id"
							>
								<h4>{{ weatherData.applicable_date }}</h4>
								<img :src="`https://www.metaweather.com/static/img/weather/${weatherData.weather_state_abbr}.svg`">
								<p>{{ Math.round(weatherData.min_temp) }}°C ~ {{ Math.round(weatherData.max_temp) }}°C</p>
								<p>Humidity:{{ weatherData.humidity }}%</p>
							</v-col>
						</v-row>
						<div class="chart-block">
							<bar-chart
								v-if="loaded"
								:chartdata="maxTempData"
								:chartlabels="chartlabels"
								label="Max Temp (°C)"
								title="Max Tempture (°C)"
							/>
						</div>
						<div class="chart-block">
							<bar-chart
								v-if="loaded"
								:chartdata="minTempData"
								:chartlabels="chartlabels"
								label="Min Temp (°C)"
								title="Min Tempture (°C)"
							/>
						</div>
						<div class="chart-block">
							<pie-chart
								v-if="loaded"
								:chartdata="humidityData"
								:chartlabels="chartlabels"
								title="Humidity (%)"
							/>
						</div>
					</v-expansion-panel-content>
				</v-expansion-panel>
			</v-expansion-panels>
		</v-row>
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
		PieChart,
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
.details-block {
	margin: 10px 0;
}
.chart-block {
	max-width: 300px;
	margin: 50px 10px;
	display: inline-block;
}
</style>
