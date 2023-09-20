<script>
	import FixedRatio from '@/lib/FixedRatio.svelte';
</script>

<div class="wrapper">
	<FixedRatio
		width_radio={21}
		height_radio={9}
		wrapperSyle="background: black;"
		ratioStyle="background: coral;"
	>
		<div>It's me!</div>
	</FixedRatio>
</div>

<style lang="scss">
	.wrapper {
		width: 100vw;
		height: 100vh;
	}
</style>
