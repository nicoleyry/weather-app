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
		<v-btn large color="primary" @click="searchDate" class="mb-5">
			Search
		</v-btn>
		<v-row v-if="loadingCities">
			<v-col cols="2" offset="5">
				<div class="loading_box">
					<p>Loading...</p>
					<div class="line"></div>
					<div class="line"></div>
					<div class="line"></div>
				</div>
			</v-col>
		</v-row>
		<!-- <v-row v-show="weatherCityList != ''"> -->
		<v-row v-if="!loadingCities">
			<v-expansion-panels>
				<v-expansion-panel
					@click="details(weatherCity.woeid)"
					v-for="weatherCity in weatherCityList"
					:key="weatherCity.woeid"
					class="results__panel"
				>
					<v-expansion-panel-header
						class="results__header"
					>
						{{ weatherCity.title }}
					</v-expansion-panel-header>
					<v-expansion-panel-content v-if="loading">
						<div class="loading_box">
							<p>Loading...</p>
							<div class="line"></div>
							<div class="line"></div>
							<div class="line"></div>
						</div>
					</v-expansion-panel-content>
					<v-expansion-panel-content v-else>
						<v-row>
							<v-col
								cols="2"
								class="details__block"
								v-for="weatherData in weatherDataList"
								:key="weatherData.id"
							>
								<h3 class="details__date">
									{{ weatherData.applicable_date }}
								</h3>
								<img
									:src="
										`https://www.metaweather.com/static/img/weather/${weatherData.weather_state_abbr}.svg`
									"
								/>
								<p class="details__text">
									<span class="details__label">Max:</span>
									{{ Math.round(weatherData.min_temp) }}°C
								</p>
								<p class="details__text">
									<span class="details__label">Min:</span>
									{{ Math.round(weatherData.max_temp) }}°C
								</p>
								<p>{{ weatherData.humidity }}%</p>
							</v-col>
						</v-row>
						<div class="chart-block">
							<bar-chart
								v-if="!loading"
								:chartdata="maxTempData"
								:chartlabels="chartlabels"
								label="Max Temp (°C)"
								title="Max Tempture (°C)"
								color="#f38181"
							/>
						</div>
						<div class="chart-block">
							<bar-chart
								v-if="!loading"
								:chartdata="minTempData"
								:chartlabels="chartlabels"
								label="Min Temp (°C)"
								title="Min Tempture (°C)"
								color="#3f72af"
							/>
						</div>
						<div class="chart-block">
							<pie-chart
								v-if="!loading"
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

const cors = "https://cors-anywhere.herokuapp.com/"; // use cors-anywhere to fetch api data
const url = "https://www.metaweather.com/"; // origin api url

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
			loading: true,
			loadingCities: false,
			maxTempData: null,
			minTempData: null,
			humidityData: null,
			chartlabels: []
		};
	},
	methods: {
		searchDate() {
			this.loadingCities = true;
			axios
				.get(
					`${cors}${url}api/location/search/?query=` +
						this.searchedLocation
				)
				.then(res => {
					this.weatherCityList = res.data;
					this.loadingCities = false;
				})
				.catch(error => {
					console.log(error);
				});
		},
		details(woeId) {
			this.loading = true;
			axios
				.get(`${cors}${url}/api/location/` + woeId)
				.then(res => {
					this.weatherDataList = res.data.consolidated_weather;
					this.maxTempData = this.weatherDataList.map(
						e => `${Math.round(e.max_temp)}`
					);
					this.minTempData = this.weatherDataList.map(
						e => `${Math.round(e.min_temp)}`
					);
					this.humidityData = this.weatherDataList.map(
						e => `${e.humidity}`
					);
					this.chartlabels = this.weatherDataList.map(
						e => `${e.applicable_date}`
					);
					this.loading = false;
				})
				.catch(error => {
					console.log(error);
				});
		}
	}
};
</script>
<style lang="scss" scoped>
#weather {
	h1 {
		color: #112d4e;
	}
}
input {
	width: 50%;
	height: 20px;
	border: 2px solid #23a6b8;
}
button {
	margin: 0 10px;
}
.results {
	&__panel {
		background-color: #d1f7fa !important;
	}
	&__header {
		font-size: 24px; 
		color: #112d4e;
	}
}
.details {
	&__block {
		margin: 10px 0;
	}
	&__date {
		color: #112d4e;
	}
	&__label {
		font-weight: bold;
		color: #112d4e;
	}
	&__text {
		margin-bottom: 5px;
	}
}
.chart-block {
	max-width: 200px;
	margin: 10px;
	display: inline-block;
}
.loading_box {
	margin: 20px;
	p {
		font-weight: bold;
		color: #112d4e;
		font-size: 20px;
	}
}
.line {
	display: inline-block;
	width: 15px;
	height: 15px;
	border-radius: 15px;
	background-color: #4b9cdb;
}
.loading_box .line:nth-last-child(1) {
	animation: loadingAni 0.6s 0.1s linear infinite;
}
.loading_box .line:nth-last-child(2) {
	animation: loadingAni 0.6s 0.2s linear infinite;
}
.loading_box .line:nth-last-child(3) {
	animation: loadingAni 0.6s 0.3s linear infinite;
}
@keyframes loadingAni {
	0% {
		transform: translate(0, 0);
	}
	50% {
		transform: translate(0, 15px);
	}
	100% {
		transform: translate(0, 0);
	}
}
</style>
