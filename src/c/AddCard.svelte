<script>
	import Shadowdark from "./creators/Shadowdark.svelte";
	import Box from "./Box.svelte";
	import { onMount } from "svelte";

	let token = "";
	let mount = false;
	let system = "shd";
	let isLoading = false;
	let error = undefined;
	let btnClass = isLoading ? "btn loading" : "btn";

	const postCard = async (e) => {
		isLoading = true;
		e.preventDefault();
		const form = document.getElementById("add-card-form");
		const formData = new FormData(form);

		await fetch(`${import.meta.env.PUBLIC_API}/addCard`, {
			method: "post",
			body: formData,
		})
			.then((res) => {
				if (res.ok) {
					// location.href = "/addCardSuccess";
					isLoading = false;
				}
			})
			.catch((e) => {
				error = "Wystpił błąd 😟 Spróbuj ponownie później.";
				isLoading = false;
				console.error(e);
			});
	};

	onMount(() => {
		mount = true;
		token = sessionStorage.getItem("token");
	});
</script>

{#if mount}
	<div class="loading-protector">
		{#if isLoading}
			<div class="hider"></div>
		{/if}
		<form on:submit|preventDefault={postCard} id="add-card-form">
			<Box>
				<div class="intro-data">
					<input type="text" name="token" bind:value={token} style="display: none;" />
					<div class="grid-item">
						<label for="name"> Nazwa karty lub postaci </label>
						<input name="name" id="name" type="text" />
					</div>
					<div class="grid-item">
						<label for="system"> System </label>
						<select name="system" bind:value={system}>
							<option value="dnd">Dungeons and Dragons 5e</option>
							<option value="bf">Basic Fantasy 3e</option>
							<option value="shd">Shadowdark</option>
						</select>
					</div>
					<div class="grid-item span-2">
						<label for="description"> Opis karty postaci </label>
						<input name="description" id="description" type="text" />
					</div>
				</div>
			</Box>
			{#if system === "shd"}
				<h1>Wybrany system: Shadowdark</h1>
				<Box>
					<Shadowdark />
				</Box>
			{/if}
			<div class="spacer"></div>
			{#if error}
				<div class="error-box">{error}</div>
			{/if}
			<button type="submit" class={btnClass}>Zapisz</button>
		</form>
	</div>
{/if}

<style lang="scss">
	.spacer {
		margin-bottom: 15px;
	}
	.intro-data {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr;
		gap: 15px;
		.grid-item {
			display: flex;
			flex-direction: column;
			input,
			select {
				max-width: none;
				width: 100%;
			}
		}
		.span-2 {
			grid-column: span 2;
		}
	}
	.loading-protector {
		position: relative;
		.hider {
			position: absolute;
			width: 100%;
			height: calc(100% - 62px);
			z-index: 99;
			background: rgba(0, 0, 0, 0.5);
			border-radius: 20px;
		}
	}
</style>
