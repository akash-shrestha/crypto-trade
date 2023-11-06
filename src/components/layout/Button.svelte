<script>
	export let isRed = false;
	export let isLoading = false;
	export let onClick = null;
	export let label;
	export let wrap = false;
	export let small = false;
	export let isYellow = false;

</script>

<style>

	button {
		padding: 0 var(--base-padding);
		position: relative;
		height: 48px;
		border-radius: var(--base-radius);
		font-weight: 500;
		color: white;
		background-color: var(--green);
		white-space: nowrap;
		border: 1px solid var(--silver-chalice);
		transition: all 0.2s ease;
		overflow: hidden;
	}
	button:after {
		content: '';
		position: absolute;
		top: 0;
		left: -200%;
		width: 100%;
		height: 100%;
		transform: skewX(-20deg);
		background-image: linear-gradient(to right, transparent, rgba(255,255,255,0.3), transparent);
}
	button:hover:after {
  	animation: shine 1.6s ease;
}
	button:active {
		transform: translateY(1px);
	}

	button.small {
		height: 36px;
		padding: 0 12px;
	}

	button.red {
		color: white;
		background-color: var(--red);
	}

	button.yellow { 
		color: var(--rich-black);
		background-color: var(--yellow);
	}
	button.yellow:hover {
		color: white;
	}

	button:not(.loading):hover {
		background-color: var(--green-dim);
	}
	button.red:not(.loading):hover {
		background-color: var(--red-dim);
	}

	button.yellow:not(.loading):hover {
		background-color: var(--orange);
	}

	button.loading {
		pointer-events: none;
		cursor: not-allowed !important;
		color: var(--sonic-silver);
		background-color: var(--onyx);
	}

	.wrap {
		padding: var(--base-padding);
		border-top: 1px solid var(--rich-black);
		background-color: var(--jet-dim);
	}

	@keyframes shine {
  100% {
    left: 200%
	}
}

</style>

{#if wrap}
<div class='wrap'>
	<button class:loading={isLoading} class:red={isRed} class:yellow={isYellow} on:click={onClick} data-intercept='true'>{label}</button>
</div>
{:else}
<button class:small={small} class:loading={isLoading} class:red={isRed} class:yellow={isYellow} on:click={onClick} data-intercept='true'>{label}</button>
{/if}