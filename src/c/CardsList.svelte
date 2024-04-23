<script>
	import { onMount } from "svelte";
	import Box from "./Box.svelte";

	let mount = false;
	let token = "";
	let cardsArray = [];

	onMount(() => {
		mount = true;
		token = sessionStorage.getItem("token");
		loadData();
	});

	async function loadData() {
		await fetch(import.meta.env.PUBLIC_API + "/allCards", {
			method: "post",
			headers: {
				Authorization: `Bearer ${token}`,
			},
		});
	}
</script>

{#if mount}
	<Box>
		<h1>Lista Twoich kart postaci</h1>
		{#if cardsArray.length > 0}
			<table>
				<tr>
					<td> Lp. </td>
					<td> Nazwa karty </td>
					<td> Opis </td>
					<td> Akcje </td>
				</tr>
				{#each cardsArray as [name, description, id], index}
					<tr>
						<td>{index}</td>
						<td>{name}</td>
						<td>{description}</td>
						<td><button on:click={() => alert(id)}>test</button></td>
					</tr>
				{/each}
			</table>
		{/if}
	</Box>
{/if}
