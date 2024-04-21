<script>
	import Shadowdark from "./creators/Shadowdark.svelte";
	import Box from "./Box.svelte";

	let system = "dnd";

	const postCard = async (e) => {
		e.preventDefault();
	};

	// const test = () => {
	// 	const form = document.getElementById("add-card-form");
	// 	const formData = new FormData(form);
	// };
</script>

<form on:submit|preventDefault={postCard} id="add-card-form">
	<Box>
		<div class="intro-data">
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
	<button type="submit" class="btn">Zapisz</button>
</form>

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
</style>
