<script>
	import "./style.css";
	import Heart from "../img/heart.png";
	import Shield from "../img/shield.png";
	import Circle from "../img/circle.png";
	import { onMount } from "svelte";

	export let content;

	$: hasSpells = false;

	const modCount = (num) => {
		if (num <= 3) {
			return -3;
		} else if (num <= 5) {
			return -2;
		} else if (num <= 8) {
			return -1;
		} else if (num <= 12) {
			return 0;
		} else if (num <= 15) {
			return 1;
		} else if (num <= 17) {
			return 2;
		} else {
			return 3;
		}
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
		let dice = [getRandomInt(1, 6), getRandomInt(1, 6), getRandomInt(1, 6)];
		// dice.sort();
		// dice.shift();
		return dice.reduce((acc, curr) => acc + curr, 0);
	};

	function getRandomInt(min, max) {
		const minCeiled = Math.ceil(min);
		const maxFloored = Math.floor(max + 1); // The maximum is exclusive and the minimum is inclusive
		return Math.floor(Math.random() * (maxFloored - minCeiled) + minCeiled);
	}

	onMount(() => {
		if (content) {
			hasSpells = content?.has_spells === "on" ? true : false;
		}
	});
</script>

<div class="grid grid-4">
	<div class="grid-item">
		<label for="character_name">Character Name</label>
		<input name="character_name" type="text" value={content?.character_name || ""} />
	</div>
	<div class="grid-item">
		<label for="race">Race</label>
		<input name="race" type="text" value={content?.race || ""} />
	</div>
	<div class="grid-item">
		<label for="class">Class</label>
		<input name="class" type="text" value={content?.class || ""} />
	</div>
	<div class="grid-item">
		<label for="background">XP</label>
		<input name="xp" type="number" value={content?.xp || 0} />
	</div>
</div>
<div class="grid grid-3">
	<div class="grid-item heart-center">
		<label for="hp">Hit Points</label>
		<img src={Heart.src} alt="heart" />
		<input type="number" name="hp" value={content?.hp || 1} />
	</div>
	<div class="grid-item shield-center">
		<label for="ac">Armor Class</label>
		<img src={Shield.src} alt="shield" />
		<input type="number" name="ac" value={content?.ac || 10} />
	</div>
	<div class="grid-item star-center">
		<label for="attack"><b>Attack</b> Bonus</label>
		<img src={Circle.src} alt="circle" />
		<input name="attack" type="number" value={content?.attack || 0} />
	</div>
</div>
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
<h2 style="text-align: center; margin-bottom: 15px;">Saving Throws</h2>
<div class="grid grid-5">
	<div class="grid-item">
		<label for="st_death">Death | Poison</label>
		<input name="st_death" type="number" value={content?.st_death || 10} />
	</div>
	<div class="grid-item">
		<label for="st_wands">Wands</label>
		<input name="st_wands" type="number" value={content?.st_wands || 10} />
	</div>
	<div class="grid-item">
		<label for="st_para">Paralize | Stone</label>
		<input name="st_para" type="number" value={content?.st_para || 10} />
	</div>
	<div class="grid-item">
		<label for="st_dragonb">Dragon Breath</label>
		<input name="st_dragonb" type="number" value={content?.st_dragonb || 10} />
	</div>
	<div class="grid-item">
		<label for="st_spells">Spells</label>
		<input name="st_spells" type="number" value={content?.st_spells || 10} />
	</div>
</div>
<div class="grid grid-2">
	<div class="grid-item">
		<label for="gear">Gear <small>(describe weapons and armor here)</small></label>
		{#if content?.gear}
			<textarea name="gear">{content.gear}</textarea>
		{:else}
			<textarea name="gear"></textarea>
		{/if}
	</div>
	<div class="grid-item">
		<label for="notes">Notes <small>(describe race and class features here)</small></label>
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
			<label for="spells">Spells list <small>(and description)</small></label>
			{#if content?.spells}
				<textarea name="spells">{content.spells}</textarea>
			{:else}
				<textarea name="spells"></textarea>
			{/if}
		</div>
	{/if}
</div>

<style>
	small {
		font-size: 0.75rem;
		opacity: 0.6;
	}
</style>
