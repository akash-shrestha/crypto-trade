<script>

	import { onMount } from 'svelte'

	import Modals from './components/layout/Modals.svelte'
	import Toasts from './components/layout/Toasts.svelte'
	import Header from './components/layout/Header.svelte'

	import { monitorOracleResponse } from './lib/monitor'
	import { initWebsocket } from './lib/stream'
	import { component } from './lib/stores'
	import { loadRoute, navigateTo, catchLinks, hidePopoversOnClick } from './lib/utils'

	// localstorage clearance for v2
	if (localStorage.getItem('productId')*1 > 0) {
		localStorage.removeItem('productId');
		localStorage.removeItem('leverage');
		localStorage.removeItem('cachedLeverages');
	}

	onMount(async () => {
		loadRoute(location.hash, true);
		catchLinks((path) => navigateTo(path));
		hidePopoversOnClick();

		// For back button functionality
		window.onpopstate = () => loadRoute(location.hash);

		initWebsocket();
		monitorOracleResponse();
	});

</script>

<style>

	/*Global styles*/

	/*Only keep color vars here, rest can go to global.css reset stylesheet*/


	:global(:root) {

		--red: #f6685e;
		--red-dim: #E04700;
		--red-dark: #421500;
		--green: #66c37b;
		--green-dim: #00C403;
		--green-dark: #004D01;
		--yellow : #ffd60a;
		--yellow-light: #ffe667;
		--orange: rgb(253,167,20);

		--rich-black: #080808;
		--rich-black-fogra: #0F0F0F;
		/* --rich-black: #1A1A1A; */
		--rich-grey : #121212;
		--jet-dim: #212121;
		--jet: #292929;
		--onyx-dim: #303030;
		--onyx: #6e6e6e;
		--dim-gray: #4A4A4A;
		--sonic-silver: #7c7c7c;
		--silver-chalice:  #ADADAD;
		
		--base-padding: 16px;
		--semi-padding: 8px;
		--base-radius: 8px;

		--chart-resolution-height: 40px;
		--chart-height: 430px;
		--header-height: 60px;
		--ticker-height: 60px;
		--grid-gap: 1px;

	}

	@media (max-height: 800px) {
	  :global(:root) {
	    --chart-height: 380px;
	  }
	}

	@media (max-height: 700px) {
	  :global(:root) {
	    --chart-height: 320px;
	  }
	}

	@media (max-height: 600px) {
	  :global(:root) {
	    --chart-height: 240px;
	  }
	}

	@media (max-height: 500px) {
	  :global(:root) {
	    --chart-height: 180px;
	  }
	}

	:global(.pos) {
		color: var(--green);
	}
	:global(.neg) {
		color: var(--red);
	}

</style>

<Modals />
<Toasts />

<Header />
<svelte:component this={$component}/>
