<script lang="ts">
	import Ad from "../lib/Ad.svelte";
    import { createSelect } from '@melt-ui/svelte';
    import { ArrowRight } from 'lucide-svelte';
    import "./AdPage.css";
	
	let walletAddress = '';
	let walletAddressSubmitted = '';
	let selectedNetwork: string;

    const networks = [
        { value: "Near", label: "Near" },
        { value: "Ethereum", label: "Ethereum" },
        { value: "Polygon", label: "Polygon" },
        { value: "Optimism", label: "Optimism" },
        { value: "Arbitrum", label: "Arbitrum" },
        { value: "Base", label: "Base" }
    ];

	const {
		elements: { trigger, menu, option },
		states: { selectedLabel, open },
        helpers: { isSelected },
	} = createSelect<string>({
		defaultSelected: { value: "Near", label: "Near" }
	});

	$: selectedNetwork = $selectedLabel;
</script>

<div class="ad-page">
	<div class="main">
		<h1>
			{#if walletAddressSubmitted}
				GM, {walletAddressSubmitted}!
			{:else}
				No more boring ads!
			{/if}
		</h1>
		<div>
			Our ad units only serve relevant ads that are based on your users recent on-chain activity.
			<br />
			This is an example implementation of a GrowthMate ad unit.
			<a
				class="more-info"
				href="https://github.com/growth-mate/react-example/tree/main"
			>
				More info
			</a>
			<br />
			<br />
            <div class="inputs">
                <div class="network-input-container">
                    <button
                        use:trigger
                        class="network-input-trigger"
                    >
                        <span>{selectedNetwork}</span>
                        <svg width="9" height="9" viewBox="0 0 9 9" fill="currentcolor" xmlns="http://www.w3.org/2000/svg" class="rt-SelectIcon" aria-hidden="true"><path d="M0.135232 3.15803C0.324102 2.95657 0.640521 2.94637 0.841971 3.13523L4.5 6.56464L8.158 3.13523C8.3595 2.94637 8.6759 2.95657 8.8648 3.15803C9.0536 3.35949 9.0434 3.67591 8.842 3.86477L4.84197 7.6148C4.64964 7.7951 4.35036 7.7951 4.15803 7.6148L0.158031 3.86477C-0.0434285 3.67591 -0.0536285 3.35949 0.135232 3.15803Z"></path></svg>
                    </button>
                    
                    {#if $open}
                        <div use:menu class="network-input-menu-container" 
                        style="--data-index: {networks.findIndex(network => network.value === selectedNetwork)}">
                            <div class="network-input-menu">
                                {#each networks as network}
                                    <div
                                        {...$option({ label: network.label, value: network.value })}
                                        use:option
                                        class="network-input-item-container"
                                    >
                                        <div class="{$isSelected(network.value) ? 'network-input-item--selected' : 'network-input-item'}">
                                            <span><svg width="9" height="9" viewBox="0 0 9 9" fill="currentcolor" xmlns="http://www.w3.org/2000/svg" class="rt-SelectItemIndicatorIcon"><path fill-rule="evenodd" clip-rule="evenodd" d="M8.53547 0.62293C8.88226 0.849446 8.97976 1.3142 8.75325 1.66099L4.5083 8.1599C4.38833 8.34356 4.19397 8.4655 3.9764 8.49358C3.75883 8.52167 3.53987 8.45309 3.3772 8.30591L0.616113 5.80777C0.308959 5.52987 0.285246 5.05559 0.563148 4.74844C0.84105 4.44128 1.31533 4.41757 1.62249 4.69547L3.73256 6.60459L7.49741 0.840706C7.72393 0.493916 8.18868 0.396414 8.53547 0.62293Z"></path></svg></span>
                                            {network.label}
                                        </div>
                                    </div>
                                {/each}
                            </div>
                        </div>
                    {/if}
                </div>
                <input
                    type="text"
                    class="account-input"
                    placeholder="Enter your wallet address"
                    bind:value={walletAddress}
                    on:blur={() => {
                        walletAddressSubmitted = walletAddress;
                    }}
                    on:keydown={(e) => {
                        if (e.key === 'Enter') {
                            walletAddressSubmitted = walletAddress;
                        }
                    }}
                />
                <button
                    class="submit-button"
                    on:click={() => {
                        walletAddressSubmitted = walletAddress;
                    }}
                >
                    <ArrowRight />
                </button>
            </div>
		</div>
	</div>
	<div class="ads">
		<div class="overflow-scroll">
			<div class="resizable">
				<Ad
					unitId="1W7xUZDbrIAMhKseC6tSUA=="
					format="Leaderboard"
					network={selectedNetwork}
                    accountId={walletAddressSubmitted}
				/>
			</div>
		</div>
		<div class="overflow-scroll">
			<div class="resizable">
				<Ad
					unitId="OhHdiR6xO06CdOhrlnmr/A=="
					format="Small Rectangle"
					network={selectedNetwork}
                    accountId={walletAddressSubmitted}
				/>
			</div>
		</div>
	</div>
</div>