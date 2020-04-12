<template>
	<div id="footer">
		<p id="footer__text">Made by <a href="https://nicoley.netlify.com/" target="_blank">Nicole</a> with ❤️</p>
	</div>
</template>

<script>
export default {
	name: "Footer"
};
</script>

<style lang="scss" scoped>
#footer {
	text-align: center;
	background: #112d4e;
	&__text {
		color: #f9f7f7;
		margin-bottom: 0px;
	}
	a {
		text-decoration: none;
    color: #a6e3e9;
	}
}
</style>
