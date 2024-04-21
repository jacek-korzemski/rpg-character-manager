<script>
	import { onMount } from "svelte";
	let render = false;
	onMount(() => {
		if (sessionStorage.getItem("token")) {
			render = true;
		}
	});
</script>

{#if render}
	<li>
		<a href="/cards">Moje Karty</a>
	</li>
	<li>
		<a href="/games">Moje Gry</a>
	</li>
	<li>
		<a href="/newCard">Nowa karta</a>
	</li>
	<li>
		<a href="/newGame">Nowa gra</a>
	</li>
{/if}
