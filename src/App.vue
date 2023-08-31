<script setup lang="ts">
import {ref} from 'vue';

import heroPicker from './components/heroPicker.vue';

import type {Hero} from './types';

import AchillesAvatar from '/heros/achilles.png?url';
import OdysseusAvatar from '/heros/odysseus.png?url';
import HerculesAvatar from '/heros/hercules.png?url';

const heros = ref<Hero[]>([
	{
		name: 'Achilles',
		avatar: AchillesAvatar,
		speed: 10,
		strength: 4,
		intelligence: 6,
	},
	{
		name: 'Odysseus',
		avatar: OdysseusAvatar,
		speed: 6,
		strength: 5,
		intelligence: 9,
	},
	{
		name: 'Hercules',
		avatar: HerculesAvatar,
		speed: 6,
		strength: 10,
		intelligence: 4,
	},
]);
const hero = ref<Hero | null>(null);

let bonus = ref(0);
let bonusLimit = 5;
let bonusSpeed = 0;
let bonusStrength = 0;
let bonusIntelligence = 0;
function doBonus() {
	if(bonus.value >= bonusLimit) {
		return alert('Only 5 bonus allowed!');
	}
	bonus.value++; // increase bonus!

	// increase a random stat
	let randomStat = Math.floor(Math.random() * 3);
	if(randomStat === 0) {
		bonusSpeed++;
	} else if(randomStat === 1) {
		bonusStrength++;
	} else if(randomStat === 2) {
		bonusIntelligence++;
	}
}

function handleUpdate(input) {
	console.log('change', input);
	hero.value = input;
	if(!hero.value) {
		bonus.value = 0;
	}
}
</script>

<template>
	<div class="h-screen flex items-center justify-center w-full">
	<div
		class="
			bg-slate-800
			rounded
			text-white
			flex flex-col
			gap-4
			p-4
			w-[400px]
		"
	>
		<h1>Hero Stats:</h1>
		<div class="flex gap-4">
			<heroPicker
				class="grow"
				v-bind:value="hero"
				v-bind:options="heros"
				@selected="handleUpdate"
			></heroPicker>
			<button
				class="
					border-green-500
					text-sm text-green-400
					border
					p-4
					px-4
					py-2
					rounded
					max-w-[50%]
					min-w-[30%]
				"
				@click="doBonus"
			>
				BONUS {{ bonus > 0 ? `(${bonus})` : '' }} ✨
			</button>
		</div>
		<div v-if="hero" class="bg-slate-400 text-black rounded flex gap-2">
			<img v-bind:src="hero.avatar" class="w-52 aspect-square rounded">
			<div>
				<h2 class="uppercase text-xs mb-4">
					Hero Summary
				</h2>
				<p>
					<span v-text="hero.name"></span>
				</p>
				<dl>
					<dt class="uppercase text-sm">
						Speed:
					</dt>
					<dd v-text="hero.speed + bonusSpeed"></dd>
					<dt class="uppercase text-sm">
						Strength:
					</dt>
					<dd v-text="hero.strength + bonusStrength"></dd>
					<dt class="uppercase text-sm">
						Intelligence:
					</dt>
					<dd v-text="hero.intelligence + bonusIntelligence"></dd>
				</dl>
			</div>
		</div>
	</div>
	</div>
</template>
