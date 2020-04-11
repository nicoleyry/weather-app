<script>
import { Pie } from "vue-chartjs";

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
			required: true
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
				legend: {
					display: false,
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
						pointBackgroundColor: "white",
						pointBorderColor: "#249EBF",
						backgroundColor: "#249EBF",
						data: this.chartdata,
					},
				],
			},
			this.options
		);
	},
};
</script>
