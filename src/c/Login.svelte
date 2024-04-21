<script lang="ts">
	import { onMount } from "svelte";
	import Box from "./Box.svelte";

	let email: string = "";
	let password: string = "";
	let message: string | undefined = undefined;
	let error: string | undefined = undefined;
	let mount: boolean = false;

	const formData = new FormData();

	const postLogin = async (e: SubmitEvent) => {
		e.preventDefault();
		formData.append("email", email);
		formData.append("password", password);

		await fetch(`${import.meta.env.PUBLIC_API}/login`, {
			method: "post",
			body: formData,
		})
			.then((res) => {
				if (res.ok) {
					return res.json();
				}
			})
			.then((data) => {
				sessionStorage.setItem("token", data.token);
				location.href = "/loginSuccess";
			})
			.catch((e) => {
				console.error(e);
				error = "Wystąpił błąd podczas logowania 😟";
			});
	};

	const logout = () => {
		sessionStorage.removeItem("token");
		message = undefined;
		location.href = "/logoutSuccess";
	};

	const retry = () => {
		message = undefined;
		error = undefined;
	};

	onMount(() => {
		mount = true;
		if (sessionStorage.getItem("token")) {
			message = "Jesteś już zalogowany!";
		}
	});
</script>

{#if mount}
	{#if message}
		<div class="message">
			<h1>{message}</h1>
			<button on:click={logout} class="btn">Wyloguj</button>
		</div>
	{:else if error}
		<div class="error">
			<p>{error}</p>
		</div>
		<button on:click={retry} class="btn">Spróbuj ponownie</button>
	{:else}
		<form id="login" on:submit|preventDefault={postLogin}>
			<label for="email"> Adres email </label>
			<input name="email" type="text" bind:value={email} />
			<label for="password"> Hasło </label>
			<input name="password" type="password" bind:value={password} />
			<button type="submit" class="btn">Login</button>
		</form>
	{/if}
{/if}

<style>
	p {
		margin: 15px auto 0 auto;
		width: fit-content;
	}
	.message {
		text-align: center;
		margin: auto;
		height: fit-content;
		width: fit-content;
	}
	.message h1 {
		margin-bottom: 15px;
	}
	.error {
		font-size: 16px;
		margin: 15px auto;
		border: 1px solid red;
		border-radius: 4px;
		background: white;
		color: red;
	}
	.error p {
		margin-bottom: 15px;
	}
	button {
		display: block;
		margin: 0 auto 15px auto;
	}
</style>
