<script>
	export let minWidth = "none";
	export let maxWidth = "none";
</script>

<div
	class={"box"}
	style={{
		minWidth: minWidth,
		maxWidth: maxWidth,
	}}
>
	<slot />
</div>

<style lang="scss">
	.box {
		width: 100%;
		border-radius: 20px;
		border: 1px solid hsla(0, 0%, 100%, 0.3);
		padding: 35px 25px 35px 35px;
		background: linear-gradient(155.14deg, hsla(0, 0%, 100%, 0) -2.13%, hsla(0, 0%, 100%, 0.15) 136.58%);
		filter: drop-shadow(0 3.99902px 48.988px rgba(0, 7, 72, 0.12));
		backdrop-filter: blur(7.49816px);
	}
</style>
