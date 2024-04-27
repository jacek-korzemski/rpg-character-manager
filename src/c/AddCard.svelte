<script>
	import Shadowdark from "./creators/Shadowdark.svelte";
	import Box from "./Box.svelte";
	import { onMount } from "svelte";
	import BasicFantasy from "./creators/BasicFantasy.svelte";

	export let data = undefined;

	$: system = "none";

	let token = undefined;
	let mount = false;

	let isLoading = false;
	let error = undefined;

	let btnClass = isLoading ? "btn loading" : "btn";

	const postCard = async (e) => {
		error = undefined;
		isLoading = true;
		e.preventDefault();
		const form = document.getElementById("add-card-form");
		const formData = new FormData(form);

		await fetch(`${import.meta.env.PUBLIC_API}/addCard`, {
			method: "post",
			body: formData,
			headers: {
				Authorization: `Bearer ${token}`,
			},
		})
			.then((res) => {
				if (res.ok) {
					isLoading = false;
					location.href = "/addCardSuccess";
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
		if ($$props.system) {
			system = $$props.system;
		}
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
					<div class="grid-item">
						<label for="name"> Nazwa karty lub postaci </label>
						<input name="name" id="name" type="text" value={data?.name || ""} />
					</div>
					<div class="grid-item">
						<label for="system">
							System
							{#if $$props.lock}
								<small
									class="i"
									data-text="Możliwość zmiany systemu jest niemożliwa, 
									ze względu na różnorodność pól. 
									Aby zmienić system, proszę 
									uwtórz nową kartę postaci.">❔</small
								>
							{/if}
						</label>
						<select name="system" bind:value={system} disabled={$$props.lock}>
							<option value="none">None (just a note)</option>
							<option value="bf">Basic Fantasy 3e</option>
							<option value="shd">Shadowdark</option>
						</select>
					</div>
					<div class="grid-item span-2">
						<label for="description"> Opis karty postaci </label>
						<input name="description" id="description" type="text" value={data?.description || ""} />
					</div>
				</div>
			</Box>
			{#if system === "shd"}
				<h1>Wybrany system: Shadowdark</h1>
				<Box>
					<Shadowdark content={data?.content && JSON.parse(data?.content)} />
				</Box>
			{:else if system === "bf"}
				<h1>Wybrany system: Basic Fantasy</h1>
				<Box>
					<BasicFantasy content={data?.content && JSON.parse(data?.content)} />
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

	/* tooltip */
	.i {
		cursor: pointer;
		position: relative;
	}
	.i:before {
		content: attr(data-text);
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		left: 100%;
		margin-left: 15px;
		width: 200px;
		padding: 10px;
		border-radius: 10px;
		background: #000;
		color: #fff;
		text-align: center;
		display: none;
		z-index: 9999;
	}
	.i:hover:before {
		display: block;
	}
</style>
