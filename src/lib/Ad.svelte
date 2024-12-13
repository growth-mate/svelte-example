<script lang="ts">
	export let unitId: string;
	export let format: string;
	export let accountId: string | undefined = undefined;
	export let network: string | undefined = undefined;
	export let className: string | undefined = undefined;

	import { onMount, onDestroy } from 'svelte';

	$: {
        void accountId, void network;
		if ((window as any).growthmate !== undefined) (window as any).growthmate.register(unitId);
	}

	onMount(() => {
		if ((window as any).growthmate !== undefined) (window as any).growthmate.register(unitId);

		let script: HTMLScriptElement | null = document.querySelector("#gm-ad-script");
		if (!script) {
			script = document.createElement("script");
			script.src = "https://cdn.growthmate.xyz/scripts/ad-unit-manager.react.js";
			script.id = "gm-ad-script";
			document.head.appendChild(script);
		}

		script.addEventListener("load", () => (window as any).growthmate.register(unitId));
	});

	onDestroy(() => {
		(window as any).growthmate?.unregister(unitId);
	});
</script>

<a
	class="gm-ad-unit {className ?? ''}"
	data-gm-id={unitId}
	data-gm-format={format}
	data-gm-account-id={accountId ?? null}
	data-gm-network={network ?? null}
></a>