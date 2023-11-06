<script>
	import { onMount } from 'svelte'

	import Button from './layout/Button.svelte'

	import { CIRCLE_ICON } from '../lib/icons'

	import { checkMetamaskSession, switchChains } from '../lib/wallet'

	import { address, wrongNetwork, currentPage } from '../lib/stores'

	import { shortAddress, showModal } from '../lib/utils'

	onMount(async () => {
		await checkMetamaskSession();
	});

</script>

<style>

	.wallet {
		display: flex;
		align-items: center;
	}

	.address {
		display: flex;
		align-items: center;
		justify-content: flex-end;
		text-align: right;
		white-space: nowrap;
	}

	.address :global(svg) {
		fill:  var(--yellow);
		height: 10px;
		margin-right: 8px;
	}

	.wrong-network {
		color: var(--orange);
		padding-right: var(--base-padding);
		white-space: nowrap;
		cursor: pointer;
	}

	.btn-wallet {
		color: #000;
		background-color: blue;
	}

	@media (max-width: 600px) {
		.wrong-network {
			display: none;
		}
	}

</style>

<div class='wallet'>

	{#if $address && $wrongNetwork}
	<div class='wrong-network' on:click={() => {switchChains()}}>Switch to Aurora</div>
	{/if}

	{#if $address}
		<a href="https://aurora.dev/faucet" target="_blank" style="margin-right:20px">Aurora Faucet</a>
		<div class='address'>	
			{@html CIRCLE_ICON}
			{shortAddress($address)}
		</div>
	{:else}
		{#if $currentPage != 'home'}
		<Button isYellow={true} small={true} onClick={() => {showModal('Connect')}} label={`Connect Wallet`} />
		{/if}
	{/if}

</div>