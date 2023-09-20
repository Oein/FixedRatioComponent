<script lang="ts">
	import { onMount } from 'svelte';

	export let wrapperClass: string = '';
	export let ratioClass: string = '';
	export let wrapperSyle: string = '';
	export let ratioStyle: string = '';
	export let width_radio: number = 16;
	export let height_radio: number = 9;

	let wrapperRef: HTMLDivElement;

	let width: number = 0;
	let height: number = 0;

	const renderByWidth = () => {
		const { width: bwidth } = wrapperRef.getBoundingClientRect();
		width = bwidth;
		height = (bwidth * height_radio) / width_radio;
	};
	const renderByHeight = () => {
		const { height: bheight } = wrapperRef.getBoundingClientRect();
		height = bheight;
		width = (bheight * width_radio) / height_radio;
	};

	const handleGaro = () => {
		const { width, height } = wrapperRef.getBoundingClientRect();
		const ratioedHeightByWidth = (width * height_radio) / width_radio;
		if (height > ratioedHeightByWidth) renderByWidth();
		else renderByHeight();
	};

	const onReize = () => {
		handleGaro();
	};

	onMount(() => {
		if (typeof ResizeObserver === 'undefined') {
			const interval2check = setInterval(() => {
				onReize();
			}, 200);
			return () => {
				clearInterval(interval2check);
			};
		}
		const resizeObserver = new ResizeObserver((entries) => {
			for (const entry of entries) {
				if (entry.target == wrapperRef) onReize();
			}
		});

		onReize();
		resizeObserver.observe(wrapperRef);

		return () => {
			resizeObserver.unobserve(wrapperRef);
		};
	});
</script>

<div
	class={'fixedRatioWrapper ' + wrapperClass}
	on:resize={onReize}
	bind:this={wrapperRef}
	style={wrapperSyle}
>
	<div
		class={'fixedRatio ' + ratioClass}
		style={`width: ${width}px; height: ${height}px; ${ratioStyle}`}
	>
		<slot />
	</div>
</div>

<style lang="scss">
	.fixedRatioWrapper {
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
		box-sizing: border-box;

		.fixedRatio {
			position: absolute;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
		}
	}
</style>
