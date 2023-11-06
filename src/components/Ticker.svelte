<script>

	import Volume from './Volume.svelte'

	import { showModal, showToast, shortSymbol, displayPricePercentChange, formatToDisplay } from '../lib/utils'
	import { address, productId, product, prices, prices24h } from '../lib/stores'
	import { CARET_DOWN } from '../lib/icons'

	let change = 0;
	$: change = displayPricePercentChange($prices[$productId], $prices24h[$productId]);

</script>

<style>

	.ticker {
		padding: 0 var(--base-padding);
		height: var(--ticker-height);
		display: flex;
		align-items: center;
		background-color: var(--rich-black);
		justify-content: space-between;
		background-color: var(--rich-grey);
		font-size: 15px;
	}

	.product-info {
		display: flex;
		align-items: center;
	}

	.price-change {
		font-size: 13px;
	}

	.selector {
		display: flex;
		align-items: center;
		cursor: pointer;
		padding: 6px 0;
	}

	.selector:hover {
		color: #ddd;
	}

	.selector :global(svg) {
		margin-left: 10px;
		height: 8px;
		fill: transparent;
		stroke: currentColor;
	}

	.selector img {
		width: 24px;
		margin-right: 10px;
	  animation: rotateAnimation 4s linear 2.5s alternate infinite;
	}

	.item {
		margin-right: calc(2 * var(--base-padding));
	}



	.volume {
		text-align: right;
	}

	.label {
		font-size: 80%;
		color: var(--onyx);
	}

	@keyframes rotateAnimation {
	from {transform: rotateY(60deg);}
	to {transform: rotateY(300deg);}
	}

	@media (max-width: 780px) {

		.volume {
			display: none;
		}

		.item {
			margin-right: var(--base-padding);
		}

	}

</style>


<div class='ticker'>
	
	{#if $product && $product.symbol}
	<div class='product-info'>

		<div class='item selector selector-product' on:click={() => {if ($address) {showModal('Products')} else {showToast('Connect your wallet to trade.')}}} data-intercept="true">
			
			{#if $product.symbol == "BTC-USD"}
				<img src={$product.logo} alt={`Yuga`}>
				<span>Yuga-Index</span>
				{@html CARET_DOWN}
			{:else}
				<img src={$product.logo} alt={`${$product.symbol} logo`}>
				<span>{$product.symbol || ''}</span>
				{@html CARET_DOWN}
			{/if}

		</div>

		{#if $productId == "BTC-USD"}
			<div class='item price'>
				${$prices[$productId] ? (5.2*$prices[$productId]).toFixed(2) : ''}
				{#if change}
					<span class={`price-change ${change * 1 < 0 ? 'neg' : 'pos'}`}>
						({change}%)
					</span>
				{/if}
				<div class='label'>Mark Price</div>
			</div>

			<div class='item price'>
				${$prices[$productId] ?(5.2*$prices[$productId]).toFixed(2) : ''}
				<div class='label'>Index Price</div>
			</div>

			<div class={`${change * 1 < 0 ? 'neg' : 'pos'}`}>
				{$prices[$productId] ? (5.2*$prices[$productId] / 100000 ).toFixed(4) : ''}%
				<div class='label'>Funding Rate</div>
			</div>
		{:else}
			<div class='item price'>
				${$prices[$productId] ? $prices[$productId].toFixed(2) : ''}
				{#if change}
					<span class={`price-change ${change * 1 < 0 ? 'neg' : 'pos'}`}>
						({change}%)
					</span>
				{/if}
				<div class='label'>Mark Price</div>
			</div>

			<div class='item price'>
				${$prices[$productId] ? $prices[$productId].toFixed(2) : ''}
				<div class='label'>Index Price</div>
			</div>

			<div class={`${change * 1 < 0 ? 'neg' : 'pos'}`}>
				{$prices[$productId] ? ($prices[$productId] / 100000 ).toFixed(4) : ''}%
				<div class='label'>Funding Rate</div>
			</div>
		{/if}

	</div>
	{/if}

	<!-- <div class='volume'>
		<div class='value'><Volume/></div>
		<div class='label'>Volume</div>
	</div> -->

</div>