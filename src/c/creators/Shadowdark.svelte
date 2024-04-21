<script>
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
		<label for="level">Level</label>
		<input name="level" type="number" />
	</div>
	<div class="grid-item">
		<label for="character_name">Class</label>
		<input name="character_name" type="text" />
	</div>
	<div class="grid-item">
		<label for="character_name">Background</label>
		<input name="character_name" type="text" />
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
		<button class="btn st" on:click={roll}>Roll</button>
	</div>
</div>

<style lang="scss">
	.grid {
		min-width: 600px;
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		grid-template-rows: 1fr;
		gap: 15px;
		&.grid-6 {
			grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
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
</style>
