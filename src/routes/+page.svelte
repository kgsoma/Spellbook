<script lang="ts">
	import SpellCard from '../components/SpellCard.svelte';
	import type { spellInfo } from '../lib/spellInterface';
	import SpellInfoCard from '../components/SpellInfoCard.svelte';
	import { Autocomplete } from '@skeletonlabs/skeleton';
	import type { AutocompleteOption } from '@skeletonlabs/skeleton';

	interface apiResponse {
		count: number;
		next?: string;
		previous?: string;
		results: spellInfo[];
	}

	let selectedCard: spellInfo | undefined;
	let searchBarIsActive = false;
	let spellsLoaded = false;
	let searchInput = '';
	let spellsArray: spellInfo[] = [];
	let autoCompleteArray: AutocompleteOption[] = [];

	function onSpellSelection(event: any): void {
		selectedCard = spellsArray.find((spell) => {
			return spell.name === event.detail.label;
		});
		searchInput = '';
		searchBarIsActive = false;
	}

	function generateAutoCompleteSpells() {
		for (let spell of spellsArray) {
			let autoCompleteSpell = {
				label: spell.name,
				value: spell.name.toLowerCase(),
				keywords: spell.dnd_class,
				meta: {
					spell_level: spell.spell_level
				}
			};
			autoCompleteArray.push(autoCompleteSpell);
		}
		spellsLoaded = true;
		autoCompleteArray = autoCompleteArray;
	}

	const getSpells = async (url: string) => {
		const response = await fetch(url);
		const data: apiResponse = await response.json();
		spellsArray.push(...data.results);
		if (data.next) {
			getSpells(data.next);
		} else {
			generateAutoCompleteSpells();
		}
	};
	getSpells('https://api.open5e.com/spells/');
	console.log(autoCompleteArray);

	function clickOutsideSearchBar(event: any) {
		console.log(event.target.id);
		if (!event.target.closest('#search_bar')) {
			//close the search dropdown
			searchBarIsActive = false;
		}
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div on:click={clickOutsideSearchBar} class="flex justify-center">
	<div id="search_bar" class="grid grid-cols-1 p-6 min-w-full md:min-w-[750px]">
		<div class="card p-4 w-full text-token space-y-4">
			<label class="label relative">
				<input
					class="input"
					type="text"
					placeholder="Search for spell..."
					disabled={!spellsLoaded}
					bind:value={searchInput}
					on:focusin={() => (searchBarIsActive = true)}
				/>
				{#if spellsLoaded && searchBarIsActive}
					<div class="card w-full max-h-48 absolute p-4 overflow-y-auto">
						<Autocomplete
							bind:input={searchInput}
							options={autoCompleteArray}
							on:selection={onSpellSelection}
						/>
					</div>
				{/if}
			</label>
		</div>
	</div>
</div>
{#if selectedCard}
	<SpellInfoCard spellInfo={selectedCard} />
{/if}
<div>
	<h3 class="text-left p-4 py-2">Cantrips</h3>
	<SpellCard name="Fire Bolt" />
</div>
<div>
	<div class="flex flex-row items-center text-left p-4 py-2">
		<h3>Level 1</h3>
		<div class="space-y-2">
			<label class="flex flex-row mx-2 items-center justify-center p-4 py-2">
				<input class="flex m-1 checkbox" type="checkbox" />
				<input class="flex m-1 checkbox" type="checkbox" />
				<input class="flex m-1 checkbox" type="checkbox" />
			</label>
		</div>
	</div>
	<SpellCard name="Shield" />
</div>
<div>
	<h3 class="text-left p-4 py-2">Level 2</h3>
	<SpellCard name="Rope Trick" />
</div>
<div>
	<h3 class="text-left p-4 py-2">Level 3</h3>
	<SpellCard name="Fireball" />
</div>
<div>
	<h3 class="text-left p-4 py-2">Level 4</h3>
	<SpellCard name="Polymorph" />
</div>
<div>
	<h3 class="text-left p-4 py-2">Level 5</h3>
	<SpellCard name="Bigby's Hand" />
</div>
