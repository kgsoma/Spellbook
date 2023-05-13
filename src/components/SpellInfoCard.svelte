<script lang="ts">
	import type { spellInfo } from '../lib/spellInterface';
	import { createEventDispatcher } from 'svelte';
	export let spellInfo: spellInfo;

	const dispatch = createEventDispatcher();
	//function that will close the spellcard when the 'X' button is pressed
	const closeCard = () => {
		dispatch('closeSpellInfoCard');
	};
</script>

<div>
	<div class="card cursor-pointer mx-4">
		<header class="card-header rounded-t-3xl bg-cyan-950">
			<div class="flex flex-row items-center justify-center">
				<div class="flex text-center">{spellInfo.name}</div>
				<div class="flex ml-auto">
					<button type="button" class="btn btn-sm redButton" on:click={closeCard}> X </button>
				</div>
			</div>
		</header>

		<section class="bg-cyan-700 p-4">
			<em>{spellInfo.level.replace('-', ' ')} {spellInfo.school}</em>
			<br />
			<div class="flex flex-row">
				<b class="mr-2">Casting Time: </b>
				<p>{spellInfo.casting_time}</p>
			</div>
			<div class="flex flex-row">
				<b class="mr-2">Range: </b>
				<p>{spellInfo.range}</p>
			</div>
			<div class="flex flex-row">
				<b class="mr-2">Components: </b>
				<p>{spellInfo.components}</p>
			</div>
			<div class="flex flex-row">
				<b class="mr-2">Duration: </b>
				<p>{spellInfo.duration}</p>
			</div>
			<div class="flex flex-row">
				<b class="mr-2">Classes: </b>
				<p>{spellInfo.dnd_class}</p>
			</div>
			<div class="flex flex-row">
				<p>{spellInfo.desc}</p>
			</div>
			{#if spellInfo.higher_level}
				<div class="">
					<b class="mr-2">At Higher Levels: </b>
					<span>{spellInfo.higher_level}</span>
				</div>
			{/if}
		</section>
		<footer class="card-footer rounded-b-3xl bg-cyan-950" />
	</div>
</div>

<style>
	.redButton {
		background-color: rgb(61, 0, 12);
	}
</style>
