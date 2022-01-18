<template>
	<div id="app">
		<header>
			<h1>Screenshot This</h1>
			
		</header>
		<main>
			<form @submit.prevent="requestScreenshot">
				<input type="url" name="url" id="url" v-model="url" />
				<input type="submit" value="Take Screenshot" />
			</form>
			<div class="screenshot">
				<p v-if="screenshotURL != ''">Click on the image to download it</p>
				<a v-if="screenshotURL != ''" :href="screenshotURL" target="_blank" download >
					<img :src="screenshotURL" alt="">
				</a>
			</div>
		</main>
	</div>
</template>

<script>
import {ref} from 'vue';

export default {
	name: 'App',
	setup() {
		const url = ref("https://");
		const screenshotURL = ref("");

		const requestScreenshot = async () =>{
			const res = await fetch('http://localhost:5000/screenshot', {
				method: 'POST',
				headers: {
					'Content-type': 'application/json'
				},
				body: JSON.stringify({
					url: url.value
				})
			}).then(data => data.json());

			screenshotURL.value = 'http://localhost:5000/static/screenshots/'+ res.ID + '.png';
			console.log(res);
		}
		return{
			url,
			screenshotURL,
			requestScreenshot
		}
	}

}
</script>

<style>
#app {
	
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
.screenshot{
	border-radius:12px;
}
img{
	display: block;
	width: 80%;
	max-width: 100%;;
	margin: 40px auto;
}
</style>
