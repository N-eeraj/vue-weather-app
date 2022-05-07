<template>
	<div class="glass">
		<div>
			<strong>
				{{ main }}
			</strong>
			<slot></slot>
		</div>
		<span>{{ side }}</span>
	</div>
</template>

<script>
	export default {
		props: {
			main: Number,
			side: String
		}
	}
</script>

<style scoped>

	.glass {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 10px;
		background-color: #0003;
		backdrop-filter: blur(10px);
		color: white;
		text-shadow: 5px 5px 10px #000A;
		border: 1px solid #FFF;
		border-radius: 10px;
	}

	.glass div {
		display: flex;
		justify-content: space-around;
		align-items: baseline;
		flex-wrap: wrap;
		font-size: clamp(16pt, 5vw, 22pt);
	}
	span {
		text-align: center;
		font-size: clamp(10pt, 3vw, 16pt);
	}

</style>