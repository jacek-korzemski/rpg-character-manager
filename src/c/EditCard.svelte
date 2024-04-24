<script>
	import { onMount } from "svelte";

	let id;
	let mount = false;

	onMount(() => {
		if (location.hash) {
			id = location.hash;
			mount = true;
		}
	});
</script>

{#if mount}
	<h1>Siema! komponent się zamontował</h1>
{/if}
