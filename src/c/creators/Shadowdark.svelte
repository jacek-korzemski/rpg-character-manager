<script>
	import "./style.css";
	import Heart from "../img/heart.png";
	import Shield from "../img/shield.png";
	import Circle from "../img/circle.png";

	export let content;

	$: hasSpells = content?.has_spells === "on" ? true : false;

	const modCount = (num) => {
		return Math.floor((num - 10) / 2);
	};

	$: str = content?.str || 10;
	$: dex = content?.dex || 10;
	$: con = content?.con || 10;
	$: int = content?.int || 10;
	$: wis = content?.wis || 10;
	$: cha = content?.cha || 10;

	const roll = () => {
		str = statRoll();
		dex = statRoll();
		con = statRoll();
		int = statRoll();
		wis = statRoll();
		cha = statRoll();
	};

	const statRoll = () => {
		return getRandomInt(1, 6) + getRandomInt(1, 6) + getRandomInt(1, 6);
	};

	function getRandomInt(min, max) {
		const minCeiled = Math.ceil(min);
		const maxFloored = Math.floor(max + 1); // The maximum is exclusive and the minimum is inclusive
		return Math.floor(Math.random() * (maxFloored - minCeiled) + minCeiled);
	}
</script>

<div class="grid">
	<div class="grid-item">
		<label for="character_name">Character Name</label>
		<input name="character_name" type="text" value={content?.character_name || ""} />
	</div>
	<div class="grid-item">
		<label for="ancestry">Ancestry</label>
		<input name="ancestry" type="text" value={content?.ancestry || ""} />
	</div>
	<div class="grid-item">
		<label for="class">Class</label>
		<input name="class" type="text" value={content?.class || ""} />
	</div>
	<div class="grid-item">
		<label for="background">Background</label>
		<input name="background" type="text" value={content?.background || ""} />
	</div>
</div>
<div class="grid grid-3">
	<div class="grid-item star-center">
		<label for="level">Level</label>
		<img src={Circle.src} alt="circle" />
		<input name="level" type="number" min="0" max="10" value={content?.level || ""} />
	</div>
	<div class="grid-item heart-center">
		<label for="hp">Hit Points</label>
		<img src={Heart.src} alt="heart" />
		<input type="number" name="hp" value={content?.hp || ""} />
	</div>
	<div class="grid-item shield-center">
		<label for="ac">Armor Class</label>
		<img src={Shield.src} alt="shield" />
		<input type="number" name="ac" value={content?.ac || ""} />
	</div>
</div>
<hr />
<div class="grid grid-6">
	<div class="grid-item">
		<label for="str">STR | <span class="mod">{modCount(str)}</span></label>
		<input name="str" type="number" bind:value={str} />
	</div>
	<div class="grid-item">
		<label for="dex">DEX | <span class="mod">{modCount(dex)}</span></label>
		<input name="dex" type="number" bind:value={dex} />
	</div>
	<div class="grid-item">
		<label for="con">CON | <span class="mod">{modCount(con)}</span></label>
		<input name="con" type="number" bind:value={con} />
	</div>
	<div class="grid-item">
		<label for="int">INT | <span class="mod">{modCount(int)}</span></label>
		<input name="int" type="number" bind:value={int} />
	</div>
	<div class="grid-item">
		<label for="wis">WIS | <span class="mod">{modCount(wis)}</span></label>
		<input name="wis" type="number" bind:value={wis} />
	</div>
	<div class="grid-item">
		<label for="cha">CHA | <span class="mod">{modCount(cha)}</span></label>
		<input name="cha" type="number" bind:value={cha} />
	</div>
	<div class="grid-item">
		<button class="btn st" on:click|preventDefault={roll}>Roll</button>
	</div>
</div>
<div class="grid grid-1">
	<div class="grid-item">
		<label for="ancestry-talent">Ancestry talent:</label>
		<input type="text" name="ancestry-talent" value={content?.["ancestry-talent"] || ""} />
	</div>
</div>
<div class="grid grid-1">
	<div class="grid-item">
		<label for="class_feats">Class Features and talents</label>
		{#if content?.class_feats}
			<textarea name="class_feats">{content.class_feats}</textarea>
		{:else}
			<textarea name="class_feats"></textarea>
		{/if}
	</div>
</div>
<div class="grid grid-2">
	<div class="grid-item">
		<label for="gear">Gear</label>
		{#if content?.gear}
			<textarea name="gear">{content.gear}</textarea>
		{:else}
			<textarea name="gear"></textarea>
		{/if}
	</div>
	<div class="grid-item">
		<label for="notes">Notes</label>
		{#if content?.notes}
			<textarea name="notes">{content.notes}</textarea>
		{:else}
			<textarea name="notes"></textarea>
		{/if}
	</div>
</div>
<div class="grid grid-1">
	<div class="grid-item flex-row">
		<input type="checkbox" name="has_spells" bind:checked={hasSpells} />
		<label for="has_spells">Has spells?</label>
	</div>
	{#if hasSpells}
		<div class="grid-item">
			<label for="spells">Spells list</label>
			{#if content?.spells}
				<textarea name="spells">{content.spells}</textarea>
			{:else}
				<textarea name="spells"></textarea>
			{/if}
		</div>
	{/if}
</div>
