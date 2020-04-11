<script>
import { Bar } from "vue-chartjs";

export default {
	extends: Bar,
	props: {
		chartdata: {
			type: Array,
			required: true,
		},
		chartlabels: {
			type: Array,
			required: true,
		},
		label: {
			type: String,
			required: true
		}
	},
	data() {
		return {
			options: {
				scales: {
					yAxes: [
						{
							ticks: {
								beginAtZero: true,
								stepSize: 1, // 設定 y 軸的數字級距
							},
							gridLines: {
								display: true,
							},
						},
					],
					xAxes: [
						{
							gridLines: {
								display: false,
							},
						},
					],
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
						label: this.label,
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
