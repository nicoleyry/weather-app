<script>
import { Pie } from "vue-chartjs";
import patternomaly from 'patternomaly';

const chartColors = ['#41b883','#35495e','#54457f','#f5ee9e','#fe5f55','#ea8c55','#5f0f40'];

export default {
	extends: Pie,
	props: {
		chartdata: {
			type: Array,
			required: true,
		},
		chartlabels: {
			type: Array,
			required: true,
		},
		title: {
			type: String,
			required: true,
		},
	},
	data() {
		return {
			options: {
				title: {
					display: true,
					text: this.title,
					fontSize: 16,
				},
				responsive: true,
				maintainAspectRatio: false,
			},
		};
	},
	mounted() {
		this.renderChart(
			{
				labels: this.chartlabels,
				datasets: [
					{
						data: this.chartdata,
						pointBackgroundColor: "white",
						backgroundColor: patternomaly.generate(chartColors),
					},
				],
			},
			this.options
		);
	},
};
</script>
