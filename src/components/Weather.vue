<template>
	<div>
		<h1>Enter the Location</h1>
		<input type="text" name="searchInput" v-model="searchedLocation" />
		<button type="submit" @click="searchDate">Search</button>
		<p>{{ searchedLocation }}</p>
		<p>Year: {{dateYear}}</p>
		<p>Month: {{dateMonth}}</p>
		<p>Day: {{dateDay}}</p>
		<p>Data: {{data}}</p>
	</div>
</template>

<script>
import axios from "axios";
export default {
	name: "Weather",
	data() {
		return {
			searchedLocation: this.value,
			data: [],
			dateYear: (new Date()).getFullYear(),
			dateMonth: (new Date()).getMonth() + 1,
			dateDay: (new Date()).getDate()
		};
	},
	methods: {
		searchDate() {
			console.log(this.searchedLocation);
			axios
			.get(`https://www.metaweather.com/api/location/search/?query=` + this.searchedLocation)
			.then((response) => {
				this.data = response.data;
			})
			.catch((e) => {
				this.errors.push(e);
			});
		}
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
