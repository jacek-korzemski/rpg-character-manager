<script>
	import { onMount } from "svelte";

	let mount = false;
	let token = "";
	let cardsArray = [];
	let isEmpty = false;

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
		})
			.then((res) => {
				if (res.ok) {
					return res.json();
				}
			})
			.then((data) => {
				cardsArray = [...data].sort((a, b) => b.id - a.id);
				if (cardsArray.length === 0) {
					isEmpty = true;
				}
			})
			.catch((e) => {
				console.error(e);
			});
	}
</script>

{#if mount}
	{#if cardsArray.length > 0}
		<table>
			<tr>
				<td> Lp. </td>
				<td> Nazwa karty </td>
				<td> Opis </td>
				<td> Akcje </td>
			</tr>
			{#each cardsArray as card, index}
				<tr>
					<td>{index + 1}</td>
					<td>{card.name}</td>
					<td>{card.description}</td>
					<td><a href={`/editCard/#${card.id}`}>📝</a></td>
				</tr>
			{/each}
		</table>
	{:else if isEmpty}
		<p>Jeszcze nie masz żadnych kart. <a href="/newCard">Utwórz</a> swoją pierwszą kartę!</p>
	{/if}
{:else}
	<p>Wczytywanie...</p>
{/if}

<style>
	table {
		width: calc(100% - 5px);
		border-collapse: collapse;
		margin-top: 15px;
	}

	tr {
		border-bottom: 1px solid #ccc;
	}
	td {
		border-right: 1px solid #ccc;
		padding: 8px;
	}
	td:first-child {
		border-left: 1px solid #ccc;
	}

	tr:first-child {
		background-color: darkblue;
		font-weight: bold;
		border-top: 1px solid #ccc;
	}
	a:hover {
		border-bottom: transparent;
	}
</style>
