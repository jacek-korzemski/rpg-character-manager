<script>
	import { onMount } from "svelte";
	import AddCard from "./AddCard.svelte";

	let id;
	let data;
	let token;
	let system;
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
				system = JSON.parse(res.content).system;
			})
			.catch((e) => {
				console.error(e);
			});
	}
</script>

{#if mount && data}
	<AddCard {data} lock={true} {system} />
{/if}
