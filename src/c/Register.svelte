<script lang="ts">
	import Box from "./Box.svelte";

	let isLoading: boolean = false;
	$: btnClass = isLoading ? "btn loading" : "btn";
	let email: string = "";
	let password: string = "";
	let confirm_password: string = "";
	let message: string | undefined = undefined;
	let error: string | undefined = undefined;

	const postRegister = async () => {
		isLoading = true;
		const formData = new FormData();
		formData.append("email", email);
		formData.append("password", password);
		formData.append("confirm_password", confirm_password);

		await fetch(`${import.meta.env.PUBLIC_API}/register`, {
			method: "POST",
			body: formData,
		})
			.then((res) => {
				if (res.ok) {
					return res.json();
				}
			})
			.then(() => {
				message = "Rejestracja przebiegła pomyślnie! Możesz się teraz zalogować.";
				isLoading = false;
			})
			.catch((e) => {
				error = "Podczas rejestracji wystąpił błąd. Spróbuj ponownie później.";
				isLoading = false;
				console.error(e);
			});
	};

	const reset = () => {
		error = undefined;
		message = undefined;
	};
</script>

<Box>
	{#if message}
		<p>{message}</p>
		<p><a href="/login">Zaloguj się</a></p>
	{:else if error}
		<p>{error}</p>
		<button class="btn" on:click={reset}>Spróbuj ponownie</button>
	{:else}
		<form id="register" on:submit|preventDefault={postRegister}>
			<h1>Załóż konto</h1>
			<label for="email"> Adres email </label>
			<input name="email" type="text" disabled={isLoading} />
			<label for="password"> Hasło </label>
			<input name="password" type="password" disabled={isLoading} />
			<label for="confirm_password"> Potwierdź hasło </label>
			<input name="confirm_password" type="password" disabled={isLoading} />
			<button type="submit" class={btnClass}>Zarejestruj</button>
		</form>
	{/if}
</Box>
