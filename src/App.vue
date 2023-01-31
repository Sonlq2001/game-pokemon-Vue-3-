<template>
	<main-screen
		v-if="statusMatch === 'default'"
		@onStart="onHandleBeforeStart"
	/>
	<interact-screen v-if="statusMatch === 'match'" />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";

export default {
	name: "App",
	components: { MainScreen, InteractScreen },
	data() {
		return {
			statusMatch: "match",
			settings: {
				totalOfBlocks: 0,
				cardsContext: [],
			},
		};
	},
	methods: {
		onHandleBeforeStart(config) {
			console.log(config);
			this.statusMatch = "match";
		},
	},
};
</script>

<style></style>
