<script>
	import Heart from "../img/heart.png";
	import Shield from "../img/shield.png";
	import Circle from "../img/circle.png";

	$: hasSpells = false;

	const modCount = (num) => {
		return Math.floor((num - 10) / 2);
	};

	let str = 10;
	let dex = 10;
	let con = 10;
	let int = 10;
	let wis = 10;
	let cha = 10;

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
		<input name="character_name" type="text" />
	</div>
	<div class="grid-item">
		<label for="ancestry">Ancestry</label>
		<input name="ancestry" type="text" />
	</div>
	<div class="grid-item">
		<label for="class">Class</label>
		<input name="class" type="text" />
	</div>
	<div class="grid-item">
		<label for="background">Background</label>
		<input name="background" type="text" />
	</div>
</div>
<div class="grid grid-3">
	<div class="grid-item star-center">
		<label for="level">Level</label>
		<img src={Circle.src} alt="heart" />
		<input name="level" type="number" min="0" max="10" />
	</div>
	<div class="grid-item heart-center">
		<label for="hp">Hit Points</label>
		<img src={Heart.src} alt="heart" />
		<input type="number" name="hp" />
	</div>
	<div class="grid-item shield-center">
		<label for="ac">Armor Class</label>
		<img src={Shield.src} alt="shield" />
		<input type="number" name="ac" />
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
		<input type="text" name="ancestry-talent" />
	</div>
</div>
<div class="grid grid-1">
	<div class="grid-item">
		<label for="class_feats">Class Features and talents</label>
		<textarea name="class_feats"></textarea>
	</div>
</div>
<div class="grid grid-2">
	<div class="grid-item">
		<label for="gear">Gear</label>
		<textarea name="gear"></textarea>
	</div>
	<div class="grid-item">
		<label for="notes">Notes</label>
		<textarea name="notes"></textarea>
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
			<textarea name="spells"></textarea>
		</div>
	{/if}
</div>

<style lang="scss">
	.grid {
		min-width: 600px;
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		grid-template-rows: 1fr;
		gap: 15px;
		&.grid-1 {
			grid-template-columns: 1fr;
		}
		&.grid-2 {
			grid-template-columns: 1fr 1fr;
		}
		&.grid-3 {
			grid-template-columns: 1fr 1fr 1fr;
		}
		&.grid-6 {
			grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
		}
		input[type="text"],
		input[type="number"] {
			width: 100%;
			max-width: none;
		}
	}
	.grid-item {
		display: flex;
		flex-direction: column;
		input[type="number"] {
			padding-right: 0;
		}
	}
	.btn.st {
		margin-top: 24px;
	}
	.flex-row {
		display: flex;
		flex-direction: row;
		justify-content: flex-start;
		align-items: center;
		gap: 15px;
	}
	.star-center,
	.heart-center,
	.shield-center {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		position: relative;
		padding-bottom: 32px;
		img {
			position: absolute;
			top: 16px;
			max-width: 112px;
			max-height: 112px;
		}
		input[type="number"] {
			position: relative;
			margin-top: 16px;
			max-width: 54px;
			max-height: 54px;
			text-align: center;
			background: transparent;
			border: none;
			box-shadow: none;
		}
	}
	.shield-center {
		img {
			top: 24px;
			max-height: 90px;
			max-width: 90px;
		}
	}
</style>
