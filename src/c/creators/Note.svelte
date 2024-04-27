<script>
	export let content;
</script>

<div class="grid grid-1">
	<div class="grid-item">
		<label for="gear">Content of the note</label>
		{#if content && content.note}
			<textarea name="note">{content.note}</textarea>
		{:else}
			<textarea name="note"></textarea>
		{/if}
	</div>
</div>
