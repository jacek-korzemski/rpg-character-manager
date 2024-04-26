<script>
	import { onMount } from "svelte";
	import AddCard from "./AddCard.svelte";

	let id;
	let data;
	let token;
	let mount = false;

	onMount(() => {
		token = sessionStorage.getItem("token");
		if (location.hash) {
			id = location.hash.replace("#", "");
			mount = true;
			loadData();
		}
	});

	async function loadData() {
		await fetch(import.meta.env.PUBLIC_API + "/getCard", {
			method: "POST",
			"Content-Type": "application/json",
			headers: {
				Authorization: `Bearer ${token}`,
			},
			body: JSON.stringify({ id }),
		})
			.then((res) => {
				if (res.ok) {
					return res.json();
				}
			})
			.then((res) => {
				data = res;
				console.log(data);
			})
			.catch((e) => {
				console.error(e);
			});
	}
</script>

{#if mount}
	<AddCard {data} lock={true} />
{/if}
