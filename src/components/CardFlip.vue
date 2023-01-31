<template>
	<div class="card" @click="onToggleFlipCard">
		<div class="card__inner" :class="{ 'is-flipped': isFlipped }">
			<div class="card__face card__face--font">
				<div class="card__content">front</div>
			</div>
			<div class="card__face card__face--back">
				<div class="card__content">back</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "CardFlip",
	data() {
		return {
			isFlipped: false,
		};
	},
	methods: {
		onToggleFlipCard() {
			this.isFlipped = !this.isFlipped;
		},
	},
};
</script>

<style scoped>
.card {
	display: inline-block;
	margin-right: 1rem;
	margin-bottom: 1rem;
	width: 90px;
	height: 120px;
}

.card__inner {
	width: 100%;
	height: 100%;
	transition: transform 1s;
	transform-style: preserve-3d;
	cursor: pointer;
	position: relative;
}

.card__inner.is-flipped {
	transform: rotateY(-180deg);
}

.card__face {
	position: absolute;
	width: 100%;
	height: 100%;
	backface-visibility: hidden;
	overflow: hidden;
	border-radius: 1rem;
	padding: 1rem;
	box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.card__face--back {
	transform: rotateY(-180deg);
	background-color: var(--light);
}
</style>
